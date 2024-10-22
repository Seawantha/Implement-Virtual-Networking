# Implement-Virtual-Networking
# To create the architecture shown in the image using Azure Cloud Shell, you need to perform the following tasks:

        +------------------+  Task3  +-----------------+
       | Task 1             | <---   | Task 2           |
       | CoreServicesVnet   |        | ManufacturingVnet|
       | (10.20.0.0/16)     |        | (10.30.0.0/16)   |
       | +----------------+ |        | +-------------+  |
       | | SharedServices | |        | | SensorSubnet1| |
       | | Subnet         | |        | | (10.30.20.0) | |
       | | (10.20.10.0/24)| |        | +-------------+  |
       | +----------------+ |        | +-------------+  |
       | +----------------+ |        | | SensorSubnet2| |
       | | DatabaseSubnet  | |       | | (10.30.21.0) | |
       | | (10.20.20.0/24)| |        | +-------------+  
       +------------------+          +-----------------+  
            ^                Task 4           ^
            |                                 |
           contoso.com                    private.contoso.com        

<!-- Task 1: Create the CoreServicesVnet
Create two subnets within CoreServicesVnet: -->
<!-- SharedServicesSubnet with an address range of 10.20.10.0/24.
DatabaseSubnet with an address range of 10.20.20.0/24.
Commands for Task 1: -->


az network vnet create \
    --name CoreServicesVnet \
    --resource-group az104-rg4 \
    --address-prefix 10.20.0.0/16 \
    --location <Your_Azure_Region>

az network vnet subnet create \
    --name SharedServicesSubnet \
    --vnet-name CoreServicesVnet \
    --resource-group az104-rg4 \
    --address-prefix 10.20.10.0/24

az network vnet subnet create \
    --name DatabaseSubnet \
    --vnet-name CoreServicesVnet \
    --resource-group az104-rg4 \
    --address-prefix 10.20.20.0/24
<!-- #Task 2: Create the ManufacturingVnet -->
<!-- Create another VNet named ManufacturingVnet with an address space of 10.30.0.0/16.
Create two subnets within ManufacturingVnet:
SensorSubnet1 with an address range of 10.30.20.0/24.
SensorSubnet2 with an address range of 10.30.21.0/24.
Commands for Task 2: -->

az network vnet create \
    --name ManufacturingVnet \
    --resource-group az104-rg4 \
    --address-prefix 10.30.0.0/16 \
    --location <Your_Azure_Region>

az network vnet subnet create \
    --name SensorSubnet1 \
    --vnet-name ManufacturingVnet \
    --resource-group az104-rg4 \
    --address-prefix 10.30.20.0/24

az network vnet subnet create \
    --name SensorSubnet2 \
    --vnet-name ManufacturingVnet \
    --resource-group az104-rg4 \
    --address-prefix 10.30.21.0/24
<!-- Task 3: Establish VNet Peering
Create VNet peering between CoreServicesVnet and ManufacturingVnet for network connectivity.
Commands for Task 3: -->


<!-- Peering from CoreServicesVnet to ManufacturingVnet -->
az network vnet peering create \
    --name CoreToManufacturingPeering \
    --resource-group az104-rg4 \
    --vnet-name CoreServicesVnet \
    --remote-vnet ManufacturingVnet \
    --allow-vnet-access

<!-- Peering from ManufacturingVnet to CoreServicesVnet -->
az network vnet peering create \
    --name ManufacturingToCorePeering \
    --resource-group az104-rg4 \
    --vnet-name ManufacturingVnet \
    --remote-vnet CoreServicesVnet \
    --allow-vnet-access
<!-- Task 4: Configure DNS for the VNets
Optionally, configure custom DNS servers if needed for the two VNets.
Commands for Task 4 (optional): -->

az network vnet update \
    --name CoreServicesVnet \
    --resource-group az104-rg4 \
    --dns-servers <DNS_IP_Address>

az network vnet update \
    --name ManufacturingVnet \
    --resource-group az104-rg4 \
    --dns-servers <DNS_IP_Address>
<!-- Make sure to replace <Your_Azure_Region> with the region where you want to create the resources, and <DNS_IP_Address> with any custom DNS settings you may need. -->

<!-- This will set up the VNets, subnets, and peering as shown in your architecture diagram. -->






