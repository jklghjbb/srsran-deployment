# Scripts to run srsRAN split8 with ZMQ in an OpenStack cloud
## Tested Environments
- Python 3.9
- ansible 2.15

## Create a single node VM k8s cluster
ansible-playbook create_srsran_resources.yaml

## Deploy srsRan split8 with zmq and UE
ansible-playbook deploy_srs.yaml -i inventory
