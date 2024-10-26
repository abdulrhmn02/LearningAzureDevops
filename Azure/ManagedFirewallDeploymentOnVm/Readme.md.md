


# Firewall-Managed Access to Azure VM via Bastion

This guide details the steps to create an Azure infrastructure that includes a resource group, a virtual network (VNet), a virtual machine (VM), and Bastion for secure access. Additionally, it covers how to install Nginx and access a custom HTML page.

## Prerequisites

- Azure account with access to create resources
- Azure CLI or Azure Portal access

## Steps

### 1. Create a Resource Group

- Navigate to the Azure Portal.
- Create a new Resource Group (RG) and select a region.

### 2. Create a Virtual Network (VNet)

- In the Azure Portal, create a new VNet within the Resource Group.
- Choose the desired region.

### 3. Enable Bastion

- In the VNet settings, enable Bastion.
- Create a new public IP address for Bastion.
- Enable firewall settings.

### 4. Create a Virtual Machine (VM)

- Go to the "Create a VM" option.
- In the "Basics" tab:
  - Select the Resource Group.
  - Choose the VM name and region.
  - Select "SSH public key" and generate a new key.
  
- In the "Networking" tab:
  - Select the default subnet.
  - Enable the option to delete the NIC when the VM is deleted.
  - Leave other settings as default.

### 5. Connect to the VM using Bastion

- Navigate to the VM in the Azure Portal.
- Click on the "Connect" option.
- Select "Bastion" from the sidebar.
- For authentication, choose "SSH Private Key from Local File".
- Enter the username you set and select the generated `.pem` file.
- Click "Connect". A new tab will open, connecting you to your VM.

### 6. Install Nginx

- Update the package list:

  ```bash
  sudo apt update
  ```

- Install Nginx:

  ```bash
  sudo apt install nginx -y
  ```

### 7. Create and Edit `index.html`

- Create the `index.html` file:

  ```bash
  vim index.html
  ```

- paste the html code u want to be displayed 
- Press `i` to enter insert mode, paste your HTML code, then press `Esc`.
- Save and exit with:

  ```bash
  :wq!
  ```

### 8. Restart Nginx

- Restart the Nginx service:

  ```bash
  sudo systemctl restart nginx
  ```

### 9. Verify Nginx Installation

- Check if Nginx is running:

  ```bash
  curl localhost:80
  ```

### 10. Configure Bastion DNAT Rules

- Go to the Azure Portal and search for Bastion.
- Open the Bastion resource created earlier.
- Navigate to DNAT rules and create a new rule:
  - Name the rule and set priority to 100.
  
- Add the following details:
  - **Source**: Your IP or a specific range.
  - **Destination**: Public IP address of the firewall.
  - **Protocol**: TCP.
  - **Destination Ports**: 4000.
  - **Translated IP Address**: VM's private IP address.
  - **Translated Port**: 80.

### 11. Access the HTML Page

- In a web browser, navigate to `http://<Public-IP>:4000` to see your HTML page.

## Conclusion

You have successfully set up an Azure environment with Bastion and Nginx, allowing secure access to your VM and serving a web page.

