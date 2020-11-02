
# Deploying to Kubernetes with Cloudstate
Reference:  https://cloudstate.io/docs/develop/gke.html

## Deploying Cloudstate operator
```bash
kubectl create namespace cloudstate
kubectl apply -n cloudstate -f https://raw.githubusercontent.com/cloudstateio/cloudstate/v0.5.1/operator/cloudstate.yaml
```

## Cassandra Set up
If using an event sourced entity, install Cassandra. This can be done from the Google Marketplace, by visiting the Cassandra Cluster, selecting configure, selecting your GCloud project, and then installing it in the Kubernetes cluster you just created.

The defaults should be good enough, in our examples we called the app instance name cassandra.

## Deploying the sample shopping cart application

Create Kube namespace for application:
```bash 
kubectl create namespace shoppingcart
```

### Installing StatefulStore
For cassandra store, execute:
```bash
kubectl apply -f ./shopping-store-cassandra.yaml -n shoppingcart
```
For InMemory store, execute:
```bash
kubectl apply -f ./shopping-store-inmemory.yaml -n shoppingcart
```
Note: If executing "kubectl config set-context --current --namespace=shoppingcart" firstly, the "-n shoppingcart" in above command can be removed. 

### Installing StatefulService
Execute the following command for cassandra or inmemory store: 
```bash
kubectl apply -f ./shopping-cart-cassandra.yaml -n shoppingcart

kubectl apply -f ./shopping-cart-inmemory.yaml -n shoppingcart
```
### Installing LoadBalance service
```bash
kubectl apply -f ./shopping-cart-service.yaml -n shoppingcart
```
The can expose public IP for test. For example, 35.224.197.137:1981

Note: there is an option to use an in memory store if you just want to test it out, of course, as soon as your pods shut down (o$

# Test
BloomPRC is a good gprc client to test. 
