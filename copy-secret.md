kubectl get secret tls-star-kaeptn-dev --namespace=default -o yaml | sed 's/namespace: default/namespace: traefik/g' | kubectl create -f - 
