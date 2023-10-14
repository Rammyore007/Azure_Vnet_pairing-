# Azure_Vnet_pairing-
# Implementing Vnet Pairing Using Azure CLI
## Step 1: Login to Azure: 
Open your terminal and run below command to login

az login

A pop-up browser window will open asking for Azure login credentials

## Step 2: create a reource group 
az group create --CloudTrainer --location westus

## Step 3: Create West VNet with SubNet

az network vnet create --resource-group CloudTrainer --name WestVNet --address-prefix 10.1.0.0/16 --subnet-name WestSubnet1 --subnet-prefix 10.1.0.0/24 --location westus

## Step 4: Verify West Network created 

az network vnet list --output table

## Step 5: Verify West Network and Subnet created

az network vnet subnet list --resource-group CloudTrainer --vnet-name WestVNet --output table

## Step 6: create the second Subnet

az network vnet create --resource-group CloudTrainer --name EastVNet --address-prefix 10.2.0.0/16 --subnet-name EastSubnet1 --subnet-prefix 10.2.0.0/24 -location eastus

## Step 7: add additional SubNets to an existing VNet

az network vnet subnet create --resource-group CloudTrainer --vnet-name EastVNet --name EastSubnet2 --address-prefix 10.2.1.0/24

## Step 8: verify the East NetWork (EastVNet) created

az network vnet list --output table

## Step 9: Verify East Network Subnets were created

az network vnet subnet list --resource-group CloudTrainer --vnet-name EastVNet --output table

## Step 10: Create Peering between West and East VNets

az network vnet show --resource-group CloudTrainer --name EastVNet --query id --out tsv

## Step 11: command to peer WestVNet to EastVNet

az network vnet peering create --name WestToEast --resource-group CloudTrainer --vnet-name WestVNet --remote-vnet "/subscriptions/c2b7716c-4dde-49d4-b5a8-7886e03f7013/resourceGroups/CloudTrainer/providers/Microsoft.Network/virtualNetworks/EastVNet" --allow-vnet-access

## Step 12: Verify State of peering

az network vnet peering list --resource-group CloudTrainer --vnet-name WestVNet --output table

## Step 13: Create pairing between East and West VNets

az network vnet show --resource-group CloudTrainer --name WestVNet --query id --out tsv

## Step 14: command to peer EastVNet to WestVNet

az network vnet peering create --name EasttoWestPeering --resource-group CloudTrainer --vnet-name EastVNet --remote-vnet "/subscriptions/c2b7716c-4dde-49d4-b5a8-7886e03f7013/resourceGroups/CloudTrainer/providers/Microsoft.Network/virtualNetworks/WestVNet" --allow-vnet-access

## Step 15: Verify State of peering

az network vnet peering list --resource-group CloudTrainer --vnet-name EastVNet --output table

