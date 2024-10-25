# Implementing Role Based Access Control (RBAC) Using Azure Portal, Azure Cloud Bash Shell and Azure Cloud Powershell

In this article, we’ll explore the implementation of Role-Based Access Control (RBAC) using Azure Cloud Shell, a powerful tool for managing Azure resources directly from your browser. Azure RBAC is essential for ensuring that users and applications have the appropriate level of access to resources, promoting security and compliance within your organization. By leveraging Azure Cloud Shell, we can streamline the process of configuring RBAC settings, making it easier to assign roles, create custom permissions, and maintain governance across our Azure environment. Join me as we dive into practical steps that will empower you to manage access efficiently and securely using Azure Cloud Shell.

## Role-Based Access Control Architectural Diagram

 ![SOC](https://github.com/Virus192/Azure-Cloud-Role-Based-Access-Control/blob/main/Images/RBAC/photo_5827795537615768718_w.jpg)

## KEY OBJECTIVES
- Create a Senior Admins group with the user account ‘John Capenter’ as its member using (Azure portal).
- Create a Junior Admins group with the user account ‘Abel Hook’ as its member using (Azure Portal).
- Create the Service Desk group with the user ‘Abdriane Joseph’ as its member (Azure CLI).
- Assign the Virtual Machine Contributor role to the Service Desk group.
- Assign the Virtual Machine Administrator Login role to the Senior and Junior Admin groups.

## STEP 1: create and configure the Senior Administrators Group with Joseph Price as the member

First, Navigate to Azure portal, Search and select Microsoft Entra ID

 ![SOC](https://github.com/Virus192/Azure-Cloud-Role-Based-Access-Control/blob/main/photo_5825543737802081741_w.jpg)

 **Next,** Create New User (***John Capenter***)and Ensure that the Auto-generate password is selected, select the Show password checkbox to identify the automatically generated password. You would need to provide this password, along with the user name to ***Johnie***.

  ![SOC](https://github.com/Virus192/Azure-Cloud-Role-Based-Access-Control/blob/main/Images/RBAC/photo_5825543737802081746_w.jpg)

  **Next,** create a senior administrator group, and add Joseph Price to the group.

- Switch to the Groups blade, and create new group
- Assign the Security Group type, and set Membership type to asigned.
- Select one owner and add Joseph Price as the owner of this group.

  ![SOC](https://github.com/Virus192/Azure-Cloud-Role-Based-Access-Control/blob/main/Images/RBAC/photo_5825543737802081749_y.jpg)

**Now, We have uccessfully used the Azure Portal to create a user and a group, and assigned the user to the group.**

## STEP 2: Create a Junior Administrators group containing the user account of Abel Hook as its member. 

- Create a new user account for **Abel Hook**
- Then create the Junior Administrator Group, and add **Abel** to this group, as an owner.
  
  ![SOC]( https://github.com/Virus192/Azure-Cloud-Role-Based-Access-Control/blob/main/Images/RBAC/photo_5825543737802081754_w%20(1).jpg)

**Now We Have a Junior Administrator Group and Senior Administrator Group**
  
 ![SOC](https://github.com/Virus192/Azure-Cloud-Role-Based-Access-Control/blob/main/Images/RBAC/photo_5825543737802081755_w.jpg)

 ## STEP 3: Create a Service Desk group containing the user account of Abdriane Joseph as its member using Azure Cloud (***Bash Shell***)

 Open In the Bash session within the Cloud Shell pane, run the following to identify the name of your Microsoft Entra tenant:

      ***DOMAINNAME=$(az ad signed-in-user show --query 'userPrincipalName' | cut -d '@' -f 2 | sed 's/\"//')***
 
Then, in the Bash session within the Cloud Shell pane, run the following to create a user, **Abdriane Johnson**. 

       ***az ad user create --display-name "Dylan Williams" --password "Pa55w.rd1234" --user-principal-name Dylan@$DOMAINNAME***

![SOC](https://github.com/Virus192/Azure-Cloud-Role-Based-Access-Control/blob/main/Images/RBAC/photo_5825543737802081766_w.jpg)

Now, In the Bash session within the Cloud Shell pane, run the following to list Microsoft Entra ID user accounts (the list should include user accounts of John, Abel, and Abdriane)

    ***az ad user list --output table***

![SOC](https://github.com/Virus192/Azure-Cloud-Role-Based-Access-Control/blob/main/photo_5825543737802081778_w.jpg)

**Next, we’ll create a Service Desk Group, and list Abdriane as it’s member.**

In the same Bash session within the Cloud Shell pane, run the following to create a new security group named Service Desk

    ***az ad group create --display-name "Service Desk" --mail-nickname "ServiceDesk"***
       
In the Bash session within the Cloud Shell pane, run the following to list the Microsoft Entra ID groups (the list should include Service Desk, Senior Admins, and Junior Admins groups):

    ***az ad group list -o table***

we can confirm that we’ve created a Service Desk group, in addition to our previous groups.

In the Bash session within the Cloud Shell pane, run the following to obtain a reference to the user account of Abdriane Joseph:

    ***USER=$(az ad user list --filter "displayname eq 'Abdriane Joseph'")***
## Next: 1. Obtain a Reference to the User Account

First, we will retrieve the user account details for Abdriane Joseph. Run the following command in the Bash session within the Cloud Shell pane:

*** USER=$(az ad user list --filter "displayname eq 'Abdriane Joseph'")***

## 2. Extract the Object ID of the User
- We will extract the objectId property from the user account details we retrieved earlier. Execute the following command:

*** OBJECTID=$(echo $USER | jq '.[].id' | tr -d '"')***

## 3. Add the User to the Service Desk Group

- Now that we have the objectId, we can add Abdriane to the **"Service Desk"** group. Run the following command:


***  az ad group member add --group "Service Desk" --member-id $OBJECTID***

## run the following to list members of the Service Desk group and verify that it includes the user account of Abdriane:


*** az ad group member list --group "Service Desk"***


![SOC](https://github.com/Virus192/Azure-Cloud-Role-Based-Access-Control/blob/main/Images/RBAC/photo_5825543737802081780_w.jpg)


## STEP 4: Assign the Virtual Machine Contributor role to the Service Desk group.

- First, I quickly created a resource group ***AuroraRG***, to ***centralus*** region, using powershell

![SOC](https://github.com/Virus192/Azure-Cloud-Role-Based-Access-Control/blob/main/Images/RBAC/photo_5827795537615768689_y.jpg)

## Next, Assign the Service Desk Virtual Machine Contributor permissions.
- Open Azure Portal and navigate to the Resource Group we just created, and click the IAM Blade.
- Add new role assignment
- Search for the Virtual Machine Contributor role


![SOC](https://github.com/Virus192/Azure-Cloud-Role-Based-Access-Control/blob/main/Images/RBAC/photo_5827795537615768693_w.jpg)


- Select, and configure new role assinment that automatically assigns the role to members in the Service Desk group.

![SOC](https://github.com/Virus192/Azure-Cloud-Role-Based-Access-Control/blob/main/Images/RBAC/photo_5827795537615768696_w.jpg)


## Next: Check the RBAC permissions

- On the ***Aurora-RG***
- Go to Access control (IAM) blade,
- On the Check access tab, in the Search by name or email address text box, type ***Abdriane Joseph***.

![SOC](https://github.com/Virus192/Azure-Cloud-Role-Based-Access-Control/blob/main/Images/RBAC/photo_5827795537615768697_w%20(1).jpg)

- We can confirm that ***Abdriane Joseph*** has been assigned to the Virtual Machine contributor role.


