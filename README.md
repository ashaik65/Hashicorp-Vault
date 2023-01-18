### Deploy Hashicorp vault on kubernetes ###

Steps to deploy :- As prequsist we need Kubernetes cluster ready for this 

Adding Helm repo :- 

$ helm repo add hashicorp https://helm.releases.hashicorp.com
"hashicorp" has been added to your repositories

But for best practice we need to change thing in values.yaml and install
First search repo using command 

$ helm search repo vault --versions

NAME           	CHART VERSION	APP VERSION	DESCRIPTION                               
hashicorp/vault	0.23.0       	1.12.1     	Official HashiCorp Vault Chart            
hashicorp/vault	0.22.1       	1.12.0     	Official HashiCorp Vault Chart            
hashicorp/vault	0.22.0       	1.11.3     	Official HashiCorp Vault Chart            
hashicorp/vault	0.21.0       	1.11.2     	Official HashiCorp Vault Chart            
hashicorp/vault	0.20.1       	1.10.3     	Official HashiCorp Vault Chart            
hashicorp/vault	0.20.0       	1.10.3     	Official HashiCorp Vault Chart            
hashicorp/vault	0.19.0       	1.9.2      	Official HashiCorp Vault Chart            
hashicorp/vault	0.18.0       	1.9.0      	Official HashiCorp Vault Chart            
hashicorp/vault	0.17.1       	1.8.4      	Official HashiCorp Vault Chart            
hashicorp/vault	0.17.0       	1.8.4      	Official HashiCorp Vault Chart            
hashicorp/vault	0.16.1       	1.8.3      	Official HashiCorp Vault Chart            
hashicorp/vault	0.16.0       	1.8.2      	Official HashiCorp Vault Chart            
hashicorp/vault	0.15.0       	1.8.1      	Official HashiCorp Vault Chart            
hashicorp/vault	0.14.0       	1.8.0      	Official HashiCorp Vault Chart            
hashicorp/vault	0.13.0       	1.7.3      	Official HashiCorp Vault Chart            
hashicorp/vault	0.12.0       	1.7.2      	Official HashiCorp Vault Chart            
hashicorp/vault	0.11.0       	1.7.0      	Official HashiCorp Vault Chart            
hashicorp/vault	0.10.0       	1.7.0      	Official HashiCorp Vault Chart            
hashicorp/vault	0.9.1        	1.6.2      	Official HashiCorp Vault Chart            
hashicorp/vault	0.9.0        	1.6.1      	Official HashiCorp Vault Chart            
hashicorp/vault	0.8.0        	1.5.4      	Official HashiCorp Vault Chart            
hashicorp/vault	0.7.0        	1.5.2      	Official HashiCorp Vault Chart            
hashicorp/vault	0.6.0        	1.4.2      	Official HashiCorp Vault Chart            
hashicorp/vault	0.5.0        	           	Install and configure Vault on Kubernetes.
hashicorp/vault	0.4.0        	           	Install and configure Vault on Kubernetes.

you can see lots of version are there but we need to go with latest version for vault but before that we will take values.yaml for this use command

$ helm show values hashicorp/vault > values.yaml

Then now we need to install this chart

$ helm install vault hashicorp/vault -f values.yaml

NAME: vault
LAST DEPLOYED: Wed Jan 18 11:04:19 2023
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
Thank you for installing HashiCorp Vault!

Now that you have deployed Vault, you should look over the docs on using
Vault with Kubernetes available here:

https://www.vaultproject.io/docs/


Your release is named vault. To learn more about the release, try:

  $ helm status vault
  $ helm get manifest vault


now once you check resources using kubectl found that vault pod is stuck in 0/1 Running because we need to exec into the pod and unseal the keys

``` yaml

anis.shaikh@C02F60RVML7H hashicorp-vault % kubectl get all
NAME                                        READY   STATUS         RESTARTS        AGE
pod/vault-0                                 0/1     Running             0          35m
pod/vault-agent-injector-77fd4cb69f-npbl9   1/1     ContainerCreating   0          35m

NAME                               TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)             AGE
service/kubernetes                 ClusterIP   10.96.0.1        <none>        443/TCP             11d
service/vault                      ClusterIP   10.100.90.213    <none>        8200/TCP,8201/TCP   35m
service/vault-agent-injector-svc   ClusterIP   10.107.152.178   <none>        443/TCP             35m
service/vault-internal             ClusterIP   None             <none>        8200/TCP,8201/TCP   35m
service/vault-ui                   ClusterIP   10.107.61.141    <none>        8200/TCP            35m

NAME                                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/vault-agent-injector   1/1     1            1           35m

NAME                                              DESIRED   CURRENT   READY   AGE
replicaset.apps/vault-agent-injector-77fd4cb69f   1         1         1       35m

NAME                     READY   AGE
statefulset.apps/vault   0/1     35m





