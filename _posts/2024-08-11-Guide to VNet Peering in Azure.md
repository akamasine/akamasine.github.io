# A Comprehensive Guide to VNet Peering in Azure

Cloud infrastructures, with time, have started to get a little more complex; hence, the ability to interconnect and manage your virtual networks has become very important. VNet Peering in Azure is among those features that enable you to communicate naturally between virtual networks (VNets), giving you a simple and scalable tool to make a robust network architecture.

What Is VNet Peering?

![diagram-3449061352.png](A%20Comprehensive%20Guide%20to%20VNet%20Peering%20in%20Azure%200b704e2cee304d059c5aed1a2de2bd7b/diagram-3449061352.png)

VNet peering allows you to connect two or more VNets inside Azure so that resources in those VNets can communicate with each other as if they were on the same network. It is a private, low-latency, and high-bandwidth connection that fits nicely in different scenarios, like multi-region deployments, resource segregation, and complex application architectures.

Here a terraform code snippt you can use to deploy 2 vnet in same region and create a vnet peer between them.

```jsx
#Vnet 1 creation
resource "azurerm_virtual_network" "peervnet" {
  name                = "peervnet"
  location            = azurerm_resource_group.peerrg.location
  resource_group_name = azurerm_resource_group.peerrg.name
  address_space       = ["172.31.0.0/16"]
}
resource "azurerm_subnet" "vpeersub1" {
  name                 = "peersub1"
  address_prefixes     = ["172.31.20.0/24"]
  virtual_network_name = azurerm_virtual_network.peervnet.name
  resource_group_name  = azurerm_resource_group.peerrg.name
}

#Vnet 2 creation
resource "azurerm_virtual_network" "peervnet2" {
  name                = "peervnet2"
  location            = var.location
  resource_group_name = azurerm_resource_group.peerrg.name
  address_space       = ["10.10.0.0/16"]
}
resource "azurerm_subnet" "peersub1" {
  name                 = "peersub1"
  address_prefixes     = ["10.10.20.0/24"]
  virtual_network_name = azurerm_virtual_network.peervnet2.name
  resource_group_name  = azurerm_resource_group.peerrg.name
}

#VNET Peering
resource "azurerm_virtual_network_peering" "vnetpeer" {
  name                      = "peer1to2"
  resource_group_name       = azurerm_resource_group.peerrg.name
  virtual_network_name      = azurerm_virtual_network.peervnet.name
  remote_virtual_network_id = azurerm_virtual_network.peervnet2.id
}
resource "azurerm_virtual_network_peering" "vnetpeer1" {
  name                      = "peer2to1"
  resource_group_name       = azurerm_resource_group.peerrg.name
  virtual_network_name      = azurerm_virtual_network.peervnet2.name
  remote_virtual_network_id = azurerm_virtual_network.peervnet.id
}

```

Further you can deploy VM in both Vnet to check the connection between them.

**Key Benefits of VNet Peering**

VNet Peering enables direct, low-latency, and high-bandwidth connectivity between VNets. As such, it is best suited for scenarios where performance is important, including database replication, application tiering, and distributed systems.

**Simplified Network Architecture:**

There is no need for the extra VPN gateway and complex routing setup, so the overall network architecture remains simplified. This will now make communication between the peered VNets easier, improve network topology simplicity, and reduce management overhead.

**Cost Efficiency:**

VNet Peering reduces data transfer costs because you do not have to use a VPN gateway for transferring data between VNets located in the same region. But you need to monitor and manage traffic in such a way that it does not create inadvertent extra costs—this mainly happens with global peering.

**Enhanced security:**

Traffic between peered VNets stays inside Azure's private backbone network. Security is improved, as it avoids exposure to the public internet. In addition, network security groups (NSGs) can be used to control traffic between the VNets and fine-grained management for security purposes.

### **Global VNet Peering**

One of the most powerful features of VNet Peering is its support for Global VNet Peering. This enables you to connect VNets from different Azure regions and allows global scale applications and disaster recovery solutions.

**Key Considerations for Global VNet Peering**

Cross-Region Traffic Costs:

Unlike same-region VNet Peering, Global VNet Peering comes with additional data transfer costs. Traffic emanating between peered VNets at cross-regions is charged based on the volume of data transfer.

**Latency**:

VNet Peering within the same region has very low latency, but cross-region connectivity might introduce some higher latencies due to the physical distance between regions. Think about the impact on your applications before you enable Global VNet Peering.

**Availability**:

While Global VNet Peering is available in most Azure regions, it's always important to check regional support, particularly if you have a global deployment strategy.

**VNet Peering Limitations**

VNet Peering is a very flexible feature, but it does have some limitations to consider when designing your network architecture:

**Non-Transitive Nature:**

VNet Peering is non-transitive; that is, if VNet A is peered with VNet B, and VNet B is peered with VNet C, then VNet A is not peered with VNet C. So, all the necessary VNets will have to be directly peered if communication is required.

**Overlapping Address Spaces:**

VNets whose IP address range overlaps cannot be peered. For this reason, the assignment of IP addresses should be given a critical eye, especially if the scope of VNet creation is from several VNets in a large organization or even from multi-teams within an organization.

**Gateway Transit:**

VNet Peering supports the transit of gateways. That is to say, the VPN gateway of one VNet can be used by another, but it has specific configurations and limitations. For instance, there should be only one peering between the connections that can make use of the gateway for traffic forwarding.

**Security Considerations:**

VNet Peering is private communication, but it is in order to remind that NSG should be configured properly to control the traffic between peered VNets. Wrong rules defined in NSG can make peering unintentionally expose VNet to undesired traffic.

**No Support for Multi-Region Failover with Global Peering:**

Although Global VNet Peering establishes connectivity between VNets across regions, it does not provide region failover. To enable a failover scenario, there is a requirement to make additional configurations and use other resources like Traffic Manager and having redundant infrastructures.

### Conclucion

VNet Peering is a core capability in Azure which ensures an efficient, cost-effective network experience. With a solid understanding of its benefits, limitations, and best practices, a scalable and strong cloud network architecture can be designed to fulfill organizational goals. Be it connecting VNets within a region or across the globe, VNet Peering has tuned the level of flexibility and performance for modern cloud environments.