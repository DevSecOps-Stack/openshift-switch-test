| CNAME             | Value (Target)                                             | Weight | Region      |
| ----------------- | ---------------------------------------------------------- | ------ | ----------- |
| `app.softekh.com` | `my-app-route-switch-app.apps.okd-us-east-1-p.softekh.com` | 100    | `us-east-1` |
| `app.softekh.com` | `my-app-route-switch-app.apps.okd-us-east-2-p.softekh.com` | 0      | `us-east-2` |
---------------------------------------------------------------------------------------------------------

### DNS Records for Active-Standby Setup

| Field            | Record 1 (Primary - Active Region)                              | Record 2 (Standby Region)                                  |
|------------------|------------------------------------------------------------------|-------------------------------------------------------------|
| **Record Name**  | `app.softekh.com`                                               | `app.softekh.com`                                           |
| **Record Type**  | `CNAME`                                                         | `CNAME`                                                     |
| **Value**        | `my-app-route-switch-app.apps.okd-us-east-1-p.softekh.com`     | `my-app-route-switch-app.apps.okd-us-east-2-p.softekh.com` |
| **Routing Policy**| `Weighted`                                                     | `Weighted`                                                  |
| **Weight**       | `100` (active traffic)                                          | `0` (no traffic until switch)                               |
| **Set ID**       | `useast1`                                                       | `useast2`                                                   |
| **TTL**          | `60` (or less for quick propagation)                            | `60`                                                        |
---------------------------------------------------------------------------------------------------------------------------------------------
