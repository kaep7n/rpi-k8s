kubectl create secret tls tls-star-kaeptn-dev --cert=kaeptn.dev_ssl_certificate.cer --key=kaeptn.dev_private_key.key -n kubernetes-dashboard

kubectl get secret tls-star-kaeptn-dev --namespace=default -o yaml | sed 's/namespace: default/namespace: monitoring/g' | kubectl create -f - 