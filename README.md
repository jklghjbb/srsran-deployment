# Scripts to run srsRAN split8 with ZMQ in an OpenStack cloud
## Tested Environments
- Python 3.9
- ansible 2.15

## Create a single node VM k8s cluster
ansible-playbook create_srsran_resources.yaml

## Deploy srsRan with zmq and UE
ansible-playbook deploy_srs.yaml -i inventory
## Deploy srsran dev env [TODO]

`
dev_env: true
`

create a separate dev VM that will run for dev/debug purpose, and no CU container/component in gNodeB in the running cluster