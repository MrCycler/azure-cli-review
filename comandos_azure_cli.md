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

