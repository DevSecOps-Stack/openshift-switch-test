
### DNS Records for Active-Standby Setup

| Field            | Record 1 (Primary - Active Region)                              | Record 2 (Standby Region)                                  |
|------------------|------------------------------------------------------------------|-------------------------------------------------------------|
| **Record Name**  | `app.softekh.com`                                               | `app.softekh.com`                                           |
| **Record Type**  | `CNAME`                                                         | `CNAME`                                                     |
| **Value**        | `route_url`     | `route_url` |
| **Routing Policy**| `Weighted`                                                     | `Weighted`                                                  |
| **Weight**       | `100` (active traffic)                                          | `0` (no traffic until switch)                               |
| **Set ID**       | `useast1`                                                       | `useast2`                                                   |
| **TTL**          | `60` (or less for quick propagation)                            | `60`                                                        |
----------------------------------------------------------------------------------------------------------------------------------------

#apply below command for both routes

oc patch route switch-app-route -n switch-app -p '{"spec":{"host":"app.softekh.com"}}'
