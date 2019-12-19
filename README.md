# massim-docker

How to watch a running game:

When everything is up and running you can watch the game in your browser at this url: http://cluster-ip:31235

How can I communicate with the server from outside the cluster:

When everything is up and running you can comunicate with the server from outside the cluster at this url: http://cluster-ip:31234

You can get the cluster-ip by using the command 'minikube ip' while your cluster is running.

# capnzero

Use weave to allow Multicast.
To use weave enter

kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"

after starting a minikube cluster
