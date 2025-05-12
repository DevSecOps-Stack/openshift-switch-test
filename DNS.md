| CNAME             | Value (Target)                                             | Weight | Region      |
| ----------------- | ---------------------------------------------------------- | ------ | ----------- |
| `app.softekh.com` | `my-app-route-switch-app.apps.okd-us-east-1-p.softekh.com` | 100    | `us-east-1` |
| `app.softekh.com` | `my-app-route-switch-app.apps.okd-us-east-2-p.softekh.com` | 0      | `us-east-2` |
---------------------------------------------------------------------------------------------------------

✅ Record 1 (Primary or active region

Field	Value

Record name	app.softekh.com

Record type	CNAME

Value	my-app-route-switch-app.apps.okd-us-east-1-p.softekh.com

Routing Policy	Weighted

Weight	100 (active traffic)

Set ID	useast1

TTL	60 (or less for quick propagation)

✅ Record 2 (Standby region)

Field	Value

Record name	app.softekh.com

Record type	CNAME

Value	my-app-route-switch-app.apps.okd-us-east-2-p.softekh.com

Routing Policy	Weighted

Weight	0 (no traffic until switch)

Set ID	useast2

TTL	60
