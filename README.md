**1. OKD Installer & Client**

curl -LO https://github.com/openshift/okd/releases/download/4.14.0-0.okd-2024-04-14-080220/openshift-install-linux-4.14.0-0.okd-2024-04-14-080220.tar.gz

tar -xvf openshift-install-linux-*.tar.gz

sudo mv openshift-install /usr/local/bin/

curl -LO https://github.com/openshift/okd/releases/download/4.14.0-0.okd-2024-04-14-080220/openshift-client-linux-4.14.0-0.okd-2024-04-14-080220.tar.gz

tar -xvf openshift-client-linux-*.tar.gz

sudo mv oc kubectl /usr/local/bin/

**2. Set up AWS CLI:**

aws configure

**3. Create Install Config**

openshift-install create install-config --dir=okd-sno

**You will be prompted for:**

Platform: AWS

Region: your preferred region (e.g., us-east-1)

Base domain: e.g., example.com

Cluster name: e.g., okd

Pull secret: get from https://cloud.redhat.com/openshift/install/pull-secret

SSH key: your public SSH key

Then, edit the generated install-config.yaml to make it single-node:

**4. Deploy the Cluster**

openshift-install create cluster --dir=okd-sno --log-level=info

**5. Configure DNS (If not using Route53)**

If using external DNS (e.g., Cloudflare), manually create:

api.okd.example.com → points to the API LB

*.apps.okd.example.com → points to the apps LB

Get the IPs from the AWS console or via CLI.
