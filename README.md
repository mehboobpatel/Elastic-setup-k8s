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


# PERSISTENT VOLUME 

* Bydefault if you use the documentation than it gives 1gb of PV(without mentioning the PVC in the elastic.yml or without creating a PV manually in the default storage class)

* lets say you want to expand the PV u can refer the documentation and add the following code 
https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-volume-claim-templates.html

* But this will throw a error if the AllowVolumeExpansion property is set to false u can check it via 

```sh

kubectl get sc
```

* You can create a PV Manually(20gb) check the pvelastic.yml (make sure PV contains the name 
* starting with pvc otherwise it wont bound with pvc that will be created after u apply elastic.yml)
```sh
          resources:
            requests:
              memory: "2Gi"
              cpu: "0.5"
```


* Otherwise you can directly follow the documentation of Persitent volume on ECK 
* which will create a pv and pvc on runtime 

# You can add this code if you get any error related to resources inside container definition 
```sh
          resources:
            requests:
              memory: "2Gi"
              cpu: "0.5"
```




