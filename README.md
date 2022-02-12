# Create an Azure Virtual WAN playground to test configurations and customer scenarios

This repo contains ARM template that can be used to deploy a _network playground_ composed by:

* an Azure Virtual WAN with 2 hubs and 1 secure hub.
* a simulated on-premise architecture

All network are peered with a single network where a Bastion host is deployed to simplify direct access to all the machine in the lab.

All spokes are able to talk to any other spoke.





# Deploy to Azure
You can use the following buttons to deploy the demo to your Azure subscription:

| | playground parts| &nbsp; |
|---|---|---|
| 1 | deploys virtual WAN playground | [![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fnicolgit%2Fazure-virtual-wan-playground%2Fmain%2Fcloud-deploy.json)
| 2 | connect vNets to hubs | [![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fnicolgit%2Fazure-virtual-wan-playground%2Fmain%2Fvnet-connections-deploy.json)
|3| deploys France branch | |
|4| deploys Germany branch | |
|5| deplolys Australia branch  | |


# Architecture

This diagram shows the overall architecture:


![lab architecture](images/lab-architecture.png)

The ARM template cloud-deploy deploys:

* an [Azure Virtual WAN](https://docs.microsoft.com/en-us/azure/virtual-wan/virtual-wan-about): a networking service that brings many networking, security, and routing functionalities together to provide a single operational interface. These functionalities include branch connectivity (via connectivity automation from Virtual WAN Partner devices such as SD-WAN or VPN CPE), Site-to-site VPN connectivity, remote user VPN (Point-to-site) connectivity, private (ExpressRoute) connectivity, intra-cloud connectivity (transitive connectivity for virtual networks), VPN ExpressRoute inter-connectivity, routing, Azure Firewall, and encryption for private connectivity. 
* 3 virtual Hubs. A virtual Hub is a Microsoft-managed virtual network core of the network in a region, that contains various service endpoints to enable connectivity.
  * `we-hub`: a secure hub located in west europe with an Azure Premium Firewall and a VPN gateway
    * [Azure Firewall](https://docs.microsoft.com/en-us/azure/firewall/overview) is a cloud-native and intelligent network firewall security service that provides the best of breed threat protection for your cloud workloads running in Azure. It's a fully stateful, firewall as a service with built-in high availability and unrestricted cloud scalability. It provides both east-west and north-south traffic inspection.
    * [VPN Gateway](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpngateways) a specific type of virtual network gateway that is used to send encrypted traffic between an Azure virtual network and an on-premises location over the public Internet.
  *  `ne-hub`: a virtual hub located in north europe
  *  `australia-hub`: a virtual hub located in Central Australia
* 5 Azure Virtual Networks:
  * `spoke-01` with 1 subnet used to connect spoke-01-vm machine with we-hub
  * `spoke-02` with 1 subnet used to connect spoke-02-vm machine with we-hub
  * `spoke-03` with 1 subnet used to connect spoke-03-vm machine with ne-hub
  * `spoke-04` with 1 subnet used to connect spoke-04-vm machine with australia-hub
  * `bastion-network` with 2 subent used as common Bastion Gateway to allows over-HTTPS access to all the VMs
    * [Azure Bastion](https://docs.microsoft.com/en-us/azure/bastion/bastion-overview) is a service you deploy that lets you connect to a virtual machine using your browser and the Azure portal. The Azure Bastion service is a fully platform-managed PaaS service that you provision inside your virtual network. It provides secure and seamless RDP/SSH connectivity to your virtual machines directly from the Azure portal over TLS. When you connect via Azure Bastion, your virtual machines do not need a public IP address, agent, or special client software.








--




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


understand this configuration: https://blog.cloudtrooper.net/2020/11/17/virtual-wan-secure-hubs-in-multiple-region

