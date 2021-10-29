helm install -n elastic -f ./values.yaml elasticsearch elastic/elasticsearch
helm install kibana elastic/kibana -n elastic