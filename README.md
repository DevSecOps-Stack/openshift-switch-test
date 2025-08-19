## 🧭 Step 1: Configure DNS (If Not Using Route 53)

create the hosted zone softekh.com and update the nameservers in the domain register

---

## ⚙️ Step 2: Install OKD Installer & Client

### 📥 Download the OKD Installer

#### Linux

```bash
wget https://github.com/okd-project/okd/releases/download/4.15.0-0.okd-2024-03-10-010116/openshift-install-linux-4.15.0-0.okd-2024-03-10-010116.tar.gz
tar xvf openshift-install-linux-4.15.0-0.okd-2024-03-10-010116.tar.gz
```

#### macOS

```bash
wget https://github.com/okd-project/okd/releases/download/4.15.0-0.okd-2024-03-10-010116/openshift-install-mac-4.15.0-0.okd-2024-03-10-010116.tar.gz
```

### 📥 Download the OKD Client

#### Linux

```bash
wget https://github.com/okd-project/okd/releases/download/4.15.0-0.okd-2024-03-10-010116/openshift-client-linux-4.15.0-0.okd-2024-03-10-010116.tar.gz
tar xvf openshift-client-linux-4.15.0-0.okd-2024-03-10-010116.tar.gz
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
ssh-keygen -t ed25519 -N '' -f ~/.ssh/id_ed25519
```

✏️ After creation, edit `install-config.yaml` to convert the cluster to a Single Node (SNO) setup.

---

## 🚀 Step 5: Deploy the Cluster

```bash
openshift-install create cluster --dir=okd-sno --log-level=info
```

---

## 🧹 Step 6: Optional - Clean Up

To destroy the deployed cluster:

```bash
openshift-install destroy cluster --dir=okd-sno
```
