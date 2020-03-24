# Comandos azure CLI
## Login
```az login```
## Listar las imagenes disponibles para una máquina virtual
```az vm image list```
## Crear una máquina virtual
```az vm create -n vmname -g resourcegroupname --image imageUrnAlias --authentication -type password --admin.username username --location westus```
## Correr una serie de comandos dentro de una máquina virtual
```az vm extension set --resource-group resourcegroupname --vm-name vmname --name scriptname --publisher Microsoft.Azure.Extensions --settings ./script.json(route)```

## Mostrar el listado de subscripciones
```az account list --output table```
## Escojer una subscripcion 
```az account set --subscription "Nombre Subscripcion"```

# Pasos a seguir para alta disponibilidad en aplicaciones
## 1) Crear grupo de recursos
```az group create --name groupname --location eastus```
## 2) Crear una ip publica
```az network public-ip create --g groupname --name myPublicIP```
## 3) Crear un balanceador de cargas
```az network lb create -g groupname -n loadbalancername --frontend-ip-name myFrontEndPool --backend-pool-name myBackEndPool --public-ip-address myPublicIP```
## 4) Configurar un seguimiento de desempeño para el balanceador de cargas
```az network lb probe create -g groupname --lb-name loadbalancername --name myHealthProbe --protocol tcp --port 80```
## 5) Establecer un regla de red en el balanceador de cargas
```az network lb rule create -g groupname --lb-name loadbalancername -n myLoadBalancerRule --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool --backend-pool myBackEndPool  --probe-name myHealthProbe```
## 6) Crear un red virtual
```az network vnet create -g groupname -n myVnet --subnet-name mySubnet```
## 7) Crear un grupo de seguridad en red (NSG)
```az network nsg create  -g groupname -n myNSG```
## 8) Establecer un regla de entrada en el grupo de seguridad de red
```az network nsg rule create -g groupname --nsg-name myNSG --name myNSGRule --priority 1001 --protocol tcp --destination-port-range 80```
## 9) Crear interfaces de red para cada maquina virtual
```for i in `seq 1 3`; do ```
```az network nic create -g groupname --name myNic$i --vnet-name myVnet --subnet mySubnet --network-security-group myNSG --lb-name loadbalancername --lb-address-pools myBackEndPool```
```done```
## 10) Crear conjunto de disponibilidad
```az vm availability-set create -g platziBalancer --name myAvailabilitySet```
## 11) Crear un archivo de configuracion tipo (cloud-init.txt)
## 12) Crear maquinas virtuales con configuración inicial
```for i in `seq 1 3`; do ```
```az vm create -g groupname --name myVM$i --availability-set myAvailabilitySet --nics myNic$i --image UbuntuLTS --admin-username azureuser --generate-ssh-key --custom-data ./cloud-init.txt --no-wait```
```done```
## 13) Obtener la ip del balanceador de cargas
```az network public-ip show -g PlatziBalancer --name myPublicIP --query [ipAddress] --output tsv```
## 14) Probar la aplicacion en la direccion IP