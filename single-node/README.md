# Sig-apps RBAC demo

THIS BRANCH IS NOT STABLE AND IS LIKELY TO CHANGED BETWEEN NOW AND THE ACTUAL RELEASE.

See the Kubernetes [authorization documentation](http://kubernetes.io/docs/admin/authorization/)
once 1.3 is out for a canonical reference.

This is a demo configuration for sig-apps to show off the latest RBAC features added to 1.3. Simply run

```
vagrant up
```

and a Kubernetes cluster will be configured with RBAC enabled.

To configure your kubectl run the following commands.

```
rm ~/.kube/config

kubectl config set-cluster vagrant-single-cluster --server=https://172.17.4.99:443 --certificate-authority=${PWD}/ssl/ca.pem
kubectl config set-credentials admin --token=admin --certificate-authority=${PWD}/ssl/ca.pem
kubectl config set-credentials user1 --token=user1 --certificate-authority=${PWD}/ssl/ca.pem
kubectl config set-credentials user2 --token=user2 --certificate-authority=${PWD}/ssl/ca.pem
kubectl config set-credentials user3 --token=user3 --certificate-authority=${PWD}/ssl/ca.pem
kubectl config set-context admin --cluster=vagrant-single-cluster --user=admin
kubectl config set-context user1 --cluster=vagrant-single-cluster --user=user1
kubectl config set-context user2 --cluster=vagrant-single-cluster --user=user2
kubectl config set-context user3 --cluster=vagrant-single-cluster --user=user3

kubectl config use-context admin
```

To switch between users use.

```
kubectl config use-context user1
```

As the admin user create some rules and poke around.

```
kubectl config use-context admin
kubectl create -f rules/non-resource-urls.yml
kubectl create -f rules/deployer.yml
kubectl create -f rules/deploy.yml

kubectl config use-context user1
```
