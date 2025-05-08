
**1. Configure DNS (If not using Route53)

If using external DNS (e.g., Cloudflare), manually create:

api.okd.example.com → points to the API LB

*.apps.okd.example.com → points to the apps LB

Get the IPs from the AWS console or via CLI.

**2. OKD Installer & Client**

wget https://github.com/okd-project/okd/releases/download/4.15.0-0.okd-2024-03-10-010116/openshift-install-linux-4.15.0-0.okd-2024-03-10-010116.tar.gz

tar xvf openshift-install-linux-arm64-4.15.0-0.okd-2024-03-10-010116.tar.gz

sudo mv openshift-install /usr/bin/

wget https://github.com/okd-project/okd/releases/download/4.15.0-0.okd-2024-03-10-010116/openshift-client-linux-4.15.0-0.okd-2024-03-10-010116.tar.gz

tar xvf openshift-client-linux-arm64-4.15.0-0.okd-2024-03-10-010116.tar.gz

sudo mv oc kubectl /usr/bin/

**3. Set up AWS CLI:**

aws configure

**4. Create Install Config**

openshift-install create install-config --dir=okd-sno

You will be prompted for

Platform: AWS

Region: your preferred region (e.g., us-east-1)

Base domain: e.g., example.com

Cluster name: e.g., okd

Pull secret: get from https://cloud.redhat.com/openshift/install/pull-secret

SSH key: your public SSH key

ssh-keygen -t ed25519 -N '' -f ~/.ssh/id_rsa

Then, edit the generated install-config.yaml to make it single-node:

**5. Deploy the Cluster**

./openshift-install create cluster --log-level=info

**6. Optional: Clean Up

openshift-install destroy cluster --dir=okd-sno

