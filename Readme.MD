# kubectl create -f https://download.elastic.co/downloads/eck/2.10.0/crds.yaml

# kubectl apply -f https://download.elastic.co/downloads/eck/2.10.0/operator.yaml

# PASSWORD=$(kubectl get secret quickstart-es-elastic-user -o go-template='{{.data.elastic | base64decode}}')


# kubectl port-forward service/quickstart-es-http 9200

# curl -u "elastic:$PASSWORD" -k "https://localhost:9200"

# kubectl port-forward service/quickstart-kb-http 5601


# kubectl delete -f https://download.elastic.co/downloads/eck/2.10.0/operator.yaml

# kubectl delete -f https://download.elastic.co/downloads/eck/2.10.0/crds.yaml

# kubectl delete elasticsearch quickstart

# kubectl delete kibana quickstart


