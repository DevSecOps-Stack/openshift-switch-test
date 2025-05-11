Record for us-east-1 application instance:

Record name: This will be the public name for your application, for example: app (which would make it app.soktekh.com) or www (for www.soktekh.com). Let's assume app.

Record type: CNAME (if your OpenShift route gives you a hostname) or A (if you are using Alias records to point directly to an AWS Load Balancer). For OpenShift routes, CNAME is common.

Value/Route traffic to: The hostname of your Nginx application's OpenShift Route in the us-east-1 cluster (e.g., nginx-route-my-app-east-1.apps.okd-east-us-1-p.soktekh.com).

Routing Policy: Weighted

Weight: 100 (for initial state)

Record ID: A unique ID, e.g., app-soktekh-east-1

(Recommended) Associate with health check: Yes, and link to the health check for your us-east-1 Nginx endpoint.

Record for us-east-2 application instance:

Record name: app (same as above, e.g., app.soktekh.com)

Record type: CNAME (or A/Alias)

Value/Route traffic to: The hostname of your Nginx application's OpenShift Route in the us-east-2 cluster (e.g., nginx-route-my-app-east-2.apps.okd-east-us-2-s.soktekh.com).

Routing Policy: Weighted

Weight: 0 (for initial state)

Record ID: A unique ID, e.g., app-soktekh-east-2

(Recommended) Associate with health check: Yes, and link to the health check for your us-east-2 Nginx endpoint.
