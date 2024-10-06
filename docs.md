# Howto(UvA): Deploy srsran split8 in Kubernetes  
## 1. Dependecy: Install Ansible in your local workstation
### 1.1 https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html
### 1.2 Ensure there is a VM running, and your workstation have ssh accessit using ssh key


## 2. Clone the repo 
```git clone https://github.com/jklghjbb/srsran-deployment.git```

## 3. Fill the <i>inventory</i> file in the repo
ansible_host is the VM ip address, ansible_user is the VM user name
```
[srs]
srs ansible_host=145.100.135.30 ansible_user=hongjie

[master]
srs ansible_host=145.100.135.30 ansible_user=hongjie

[workers]

[k8s_hosts]
srs ansible_host=145.100.135.30 ansible_user=hongjie
```

## 4. Deploy srs
Run:

```anisble-playbook -i inventory deploy_srs.yaml -vvv```

This playbook will 
* Deploy containerd
* Deploy Kubernetes
* Deploy open5gs core using upstream helm chart
* Deploy srsran 
    * Deploy multus and a bridge network for CU/DU midhaul
    * Deploy srsran spkl using customized container and helm chart( zmq driver + split 8)

## 5. Verify the installation
In your VM run

```kubectl get pods -n open5gs -owide```
```
NAME                                READY   STATUS    RESTARTS       AGE
open5gs-amf-5587498f5d-mzrkl        1/1     Running   0              128m
open5gs-ausf-6fcfdc89d6-dmf56       1/1     Running   0              128m
open5gs-bsf-6b6b79b4b4-br5zg        1/1     Running   0              128m
open5gs-mongodb-5b54df8774-xlcwh    1/1     Running   0              128m
open5gs-nrf-556bfcfc68-6ts6b        1/1     Running   0              128m
open5gs-nssf-759588f754-p72cm       1/1     Running   0              128m
open5gs-pcf-69c8d86644-vfw8s        1/1     Running   2 (128m ago)   128m
open5gs-populate-6d74b8f674-kzzgd   1/1     Running   0              128m
open5gs-scp-5689998dbc-mbmgk        1/1     Running   0              128m
open5gs-smf-b6b459f67-k9mxr         1/1     Running   0              128m
open5gs-udm-5f84dfcf88-m85cb        1/1     Running   0              128m
open5gs-udr-d85f6c87-gcj28          1/1     Running   2 (128m ago)   128m
open5gs-upf-598b447f86-jg42f        1/1     Running   0              128m
open5gs-webui-5ddc9fd97b-8sp27      1/1     Running   0              128m
srsran-5g-zmq-cu-bfb6fdd76-gqxjp    1/1     Running   0              20m
srsran-5g-zmq-du-57fc84564f-htdqv   2/2     Running   0              3m55s
```
Check the UE has access to the internet
```
kubectl exec -it srsran-5g-zmq-du-57fc84564f-r9mcm -c srsran-5g-zmq-du-ue -n open5gs -- ping gradiant.org -I tun_srsue
```

## 6. If you meet problem that can't be solved, please contact me


