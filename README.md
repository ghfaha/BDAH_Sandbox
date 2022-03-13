
System Prepration:
First setup 5 instances(Ubuntu) with your required CPU and Ram: # Workers should have the dedicated more RAM \
2- Master for HA \
3- Workers 

---->> Prepare Cluster Instances:
1. kubectl:
  ### Linux ### 
  curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl \
  chmod +x ./kubectl \
  sudo mv ./kubectl /usr/local/bin/kubectl \
  kubectl version --client 
  
2. rke
  ### Linux ###
  curl -s https://api.github.com/repos/rancher/rke/releases/latest | grep download_url | grep amd64 | cut -d '"' -f 4 | wget -qi - \
  chmod +x rke_linux-amd64 \
  sudo mv rke_linux-amd64 /usr/local/bin/rke \
  rke --version \
3. helm 
  ### Helm 3 ### 
  curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 \
  chmod 700 get_helm.sh \
  ./get_helm.sh \

Update your Linux System: 
### CentOS ### 
sudo yum -y update \
sudo reboot \

### Ubuntu / Debian ### 
sudo apt-get update \
sudo apt-get upgrade \
sudo reboot \
---> now you can config each instances for kuberntes cluster installation using Ansible Playbooks in this repository or Manually. \
---> After configuration of all instances,  you are ready to install Kubernetes. First, Generate RKE cluster configuration file as below: 
#the rke sample file is also located in repository, you can use of it to get idea. \
#Do not copy this file. In my configuration, the master nodes only have etcd and controlplane roles. But they can be used to schedule pods by adding worker role. \
     rke config --name cluster.yml 

---> To deploy Kubernetes Cluster with RKE, run this command. ( rke up ) This command assumes the cluster.yml file is in the same directory as where you are running the command. If using a different filename, specify it like below. \
$ rke up --config ./rancher_cluster.yml 
Using SSH private key with a passphrase – eval ssh-agent -s && ssh-add 

Ensure the setup doesn’t show any failure in its output. 

---> Accessing your Kubernetes cluster:
As part of the Kubernetes creation process, a kubeconfig file has been created and written at kube_config_cluster.yml. 

->Set KUBECONFIG variable to the file generated. \
   export KUBECONFIG=./kube_config_cluster.yml \
->Check list of nodes in the cluster. \
   kubectl get nodes 

 
