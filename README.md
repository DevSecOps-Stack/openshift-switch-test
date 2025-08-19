## 🧭 Step 1: Configure DNS (If Not Using Route 53)

create the hosted zone softekh.com and update the nameservers in the domain register

---

## ⚙️ Step 2: Install OKD Installer & Client

### 📥 Download the OKD Installer

#### Linux

```bash
wget https://github.com/okd-project/okd/releases/download/4.15.0-0.okd-2024-03-10-010116/openshift-install-linux-4.15.0-0.okd-2024-03-10-010116.tar.gz
tar xvf openshift-install-linux-arm64-4.15.0-0.okd-2024-03-10-010116.tar.gz
```

#### macOS

```bash
wget https://github.com/okd-project/okd/releases/download/4.15.0-0.okd-2024-03-10-010116/openshift-install-mac-4.15.0-0.okd-2024-03-10-010116.tar.gz
```

### 📥 Download the OKD Client

#### Linux

```bash
wget https://github.com/okd-project/okd/releases/download/4.15.0-0.okd-2024-03-10-010116/openshift-client-linux-4.15.0-0.okd-2024-03-10-010116.tar.gz
tar xvf openshift-client-linux-arm64-4.15.0-0.okd-2024-03-10-010116.tar.gz
sudo mv oc kubectl /usr/bin/
```

#### macOS

```bash
wget https://github.com/okd-project/okd/releases/download/4.15.0-0.okd-2024-03-10-010116/openshift-client-mac-4.15.0-0.okd-2024-03-10-010116.tar.gz
```
---

## 🔐 Step 3: Set Up AWS CLI

```bash
aws configure
```

---

## 🛠️ Step 4: Create Install Config

```bash
openshift-install create install-config --dir=okd-sno
```

You will be prompted to enter:

* **Platform**: AWS
* **Region**: e.g., `us-east-1`
* **Base Domain**: e.g., `example.com`
* **Cluster Name**: e.g., `okd`
* **Pull Secret**: [Get from Red Hat](https://cloud.redhat.com/openshift/install/pull-secret)
* **SSH Key**: Your public SSH key

Generate SSH key if needed:

```bash
ssh-keygen -t ed25519 -N '' -f ~/.ssh/id_rsa
```

✏️ After creation, edit `install-config.yaml` to convert the cluster to a Single Node (SNO) setup.

---

## 🚀 Step 5: Deploy the Cluster

```bash
./openshift-install create cluster --log-level=info
```

After the cluster is ready, get the login credentials:

```bash
export KUBECONFIG=okd-sno/auth/kubeconfig
oc whoami
```

### Deploy the Application

Deploy the switch test application:

```bash
# Create the namespace
oc create namespace switch-app

# Deploy the application
oc apply -f config-files.yaml

# Verify deployment
oc get pods -n switch-app
oc get svc -n switch-app  
oc get route -n switch-app

# Update the route host (replace with your actual domain)
oc patch route switch-app-route -n switch-app -p '{"spec":{"host":"app.softekh.com"}}'
```

---

## 🛑 Step 6: Stop Application

To stop the application without destroying the cluster:

### Stop Traffic Routing

Set DNS weights to 0 to stop traffic routing to the application:

```bash
# Update Route 53 weights to 0 for both regions (if using Route 53)
# This stops traffic from reaching the application
aws route53 change-resource-record-sets --hosted-zone-id YOUR_ZONE_ID --change-batch '{
  "Changes": [{
    "Action": "UPSERT",
    "ResourceRecordSet": {
      "Name": "app.softekh.com",
      "Type": "CNAME",
      "SetIdentifier": "useast1",
      "Weight": 0,
      "ResourceRecords": [{"Value": "your-route-url-1"}]
    }
  }, {
    "Action": "UPSERT", 
    "ResourceRecordSet": {
      "Name": "app.softekh.com",
      "Type": "CNAME",
      "SetIdentifier": "useast2", 
      "Weight": 0,
      "ResourceRecords": [{"Value": "your-route-url-2"}]
    }
  }]
}'
```

### Scale Down Application

Scale down the application to 0 replicas:

```bash
# Scale down the nginx deployment
oc scale deployment nginx --replicas=0 -n switch-app

# Verify the pods are stopped
oc get pods -n switch-app
```

### Remove Application Resources (Optional)

To completely remove the application resources:

```bash
# Delete the application resources
oc delete -f config-files.yaml

# Or delete individual resources
oc delete deployment nginx -n switch-app
oc delete service switch-app-service -n switch-app
oc delete route switch-app-route -n switch-app

# Delete the namespace (optional)
oc delete namespace switch-app
```

---

## 🧹 Step 7: Optional - Clean Up Cluster

To destroy the deployed cluster:

```bash
openshift-install destroy cluster --dir=okd-sno
```
