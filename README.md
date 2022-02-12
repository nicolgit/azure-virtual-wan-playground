# Create an Azure Virtual WAN playground to test configurations and customer scenarios

This repo contains an ARM template to that can be used to deploy a playground composed by:

* an Azure Virtual WAN network aligned with with Microsoft Enterprise scale landing zone reference architecture
* a simulated on-premise architecture composed by a network, a client machine and a gateway to be used to test connectivity with the cloud

# Deploy to Azure
You can use the following buttons to deploy the demo to your Azure subscription:

| | playground parts| &nbsp; |
|---|---|---|
| 1 | deploys virtual WAN playground | [![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fnicolgit%2Fazure-virtual-wan-playground%2Fmain%2Fcloud-deploy.json)
| 2 | connect vNets to hubs | [![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fnicolgit%2Fazure-virtual-wan-playground%2Fmain%2Fvnet-connections-deploy.json)
|3| deploys France branch | |
|4| deploys Germany branch | |
|5| deplolys Australia branch  | |


I had to decouple the vnet-to-hub connections provisioning because if keep in the same deploy where virtual hub is created, I receive random NOT FOUND errors (see below). 

```json
{
    "status": "Failed",
    "error": {
        "message": "Operation f541fd00-439f-4e23-9505-a61edff6f335 not found.",
        "details": []
    }
}
```


I did not investigate more about this because the testing nature of this laboratory, makes accettable to execute different deployments in sequence.

![lab architecture](images/lab-architecture.png)


understand this configuration: https://blog.cloudtrooper.net/2020/11/17/virtual-wan-secure-hubs-in-multiple-region

