**Elastic Cloud on Kubernetes!**
-------

First install the Requirements for Deploying the Cluster

# CRD & Operators

```sh

kubectl create -f https://download.elastic.co/downloads/eck/2.10.0/crds.yaml

kubectl apply -f https://download.elastic.co/downloads/eck/2.10.0/operator.yaml
```

# Than apply the file for Elastic
```sh

kubectl apply -f elastic.yml
```

# To check the status
```sh


kubectl get elasticsearch
```

It will create a random password and store it

```sh

PASSWORD=$(kubectl get secret quickstart-es-elastic-user -o go-template='{{.data.elastic | base64decode}}')

kubectl port-forward service/quickstart-es-http 9200

curl -u "elastic:$PASSWORD" -k "https://localhost:9200"

```

Than similary Deploy Kibana 

```sh

kubectl apply -f kibana.yml

kubectl port-forward service/quickstart-kb-http 5601


```

# To check the status
```sh
kubectl get kibana
```



To Delete the Deployment 

```sh
kubectl delete elasticsearch quickstart

kubectl delete kibana quickstart

```



kubectl delete -f https://download.elastic.co/downloads/eck/2.10.0/operator.yaml

kubectl delete -f https://download.elastic.co/downloads/eck/2.10.0/crds.yaml



SECRET
-------

# to create a secret first convert the password into base64
```sh
echo -n "NewElastic" | base64
```

# than create a secret file(check the file in this repo) manually and paste the above echo output in it and apply it 

--------------OR----------

# use below command to directly create a password and store it in the cluster

```sh
kubectl create secret generic quickstart-es-elastic-user --from-literal=elastic="Elastic" --dry-run=client -o yaml | kubectl apply -f -
```

# To save create a secret file and than manually apply it 
```sh
kubectl create secret generic quickstart-es-elastic-user --from-literal=elastic="Elastic" --dry-run=client -o yaml > secret.yml 
```


--------------OR----------
# TO create a password ->> create a secret file --> apply that file 
```sh
kubectl create secret generic quickstart-es-elastic-user --from-literal=elastic="Elastic" --dry-run=client -o yaml > secret.yml | kubectl apply -f secret.yml
```

* NOTE if you just write this without "| kubectl apply -f secret.yml" it will create a secret.yml but wont store it in the 
* cluster to story you have to manualy apply the file


