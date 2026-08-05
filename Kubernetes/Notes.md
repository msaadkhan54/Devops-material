**CLUSTER DOWNLOADING:**



first install **DOCKER AND GIT on EC2.**



Then go in: **cd /etc/containerd**



Here delete: **sudo rm config.toml**



Then do: **sudo containerd config default | sudo tee /etc/containerd/config.toml**



Then do: **nano config.toml**



In it find '**SystemdCgroup: false'**



**and make it true.**

Then do: **sudo systemctl restart containerd**



Then do: **sudo systemctl daemon-reload**



Then : **sudo swapoff -a**

Then : **swapon --show**



Then run commands from documentation link: **https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/**

then run : '**sudo kubeadm init' or 'sudo kubeadm init --pod-network-cidr=192.168.0.0/16'** on only master node



Then run the following giving commands on terminal and then add token given on master node to the workerNode

Then on Master Node: **sudo curl https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/calico.yaml -O**



&#x20; **''       ''      :  kubectl apply -f calico.yaml**



After this makes sure that both nodes should have been given **INBOUND RULE ON PORT 179**  for **CALICO**.



Then give installing command of **METRIC SERVER**(on master node): **kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml**



# **KUBERNETES**



To roll back a Kubernetes deployment to its previous state, run this command "**kubectl rollout undo deployment/<deployment-name>"**

To get status of roll out **"kubectl rollout status deployment/<deployment-name>"**

To get history of roll out **"kubectl rollout history deployment/<deployment-name>"**



For HPA: "**kubectl autoscale deployment/<deployment-name> --cpu=50% --min=2 --max=4"**











# **HELM:**

check what helm created "**kubectl get all"**



check all of the yaml files "**helm get manifest <release-name>"**



Instead of editing Deployment manually **"helm upgrade <release-name> <repo-name>/<chart-name> --set replicaCount=3"**

Check replica created "**kubectl get deploy"**

Check all the revision "**helm history <realese-name>"**

Rollback to previous revision of release "**helm rollback <release-name> <revison-num>"**

Everything created by that release disappears **"helm uninstall my-nginx"**

* **Each one contains the rendered manifests and metadata for that revision. This is how Helm can roll back without asking Git or regenerating old YAML. ->**

Helm stores release metadata as Kubernetes Secrets **"kubectl get secrets"**

In order to run chart(make release): **"helm install <choose any release name> <CHART\_PATH>"**

This is one of the most-used Helm commands.It shows the exact Kubernetes YAML that Helm will send to the cluster **"helm template <RELEASE\_NAME> <CHART\_PATH>"**



RBAC:


To get tokens of service account of pod:'kubectl exec -it <pod-name> -- ls /var/run/secrets/kubernetes.io/serviceaccount'

Role: (in this we set permissions in a namespace)
----------------------------------------------------
apiVersion: rbac.authorization.k8s.io/v1
kind: Role

metadata:
  name: pod-reader
  namespace: default

rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get","list","watch"]
---------------------------------------------------
There can be several permissions such as:
 get
 list
 watch
 create
 update
 patch
 delete

RoleBinding: (in this we assign the role to specific serviceaccount and namespace and give refrence of role that is needed to be attached)

Too see permission of our admin account: kubectl auth can-i delete pods

Too see permission of PODS:

too test the Role attach to service Account: kubectl auth can-i list pods --as=system:serviceaccount:default:<service-name>

to test the ClusterRole attach to service account: kubectl auth can-i list pods --all-namespaces --as=system:serviceaccount:default:<service-name>

``````````````````````````````````
But what if you want someone to:

-View Pods in every namespace
-ead Nodes
-Manage PersistentVolumes
-Create Namespaces

A Role cannot do that.

That's why Kubernetes provides:
-ClusterRole
-ClusterRoleBinding


SERVICE ACCOUNT CAN BE IN ANY NAMESPACE OR ATTACH TO ANY DEPLOYMENT BUT THE ROLE ATTACH TO THAT SERVICE ACCOUNT BELONGS TO PARTICULAR NAMESPACE SO ROLES ONLY WORK THEIR.

SO FOR EACH NAMESPACE WE NEED TO CREATE SEPERATE ROLES

hERE COMES CLUSTER ROLES

