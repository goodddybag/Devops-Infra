# Infrastructure Deployment with Terraform and Ansible

This project demonstrate IaC by automating the provisioning of an Azure infrastructure using **Terraform**, followed by **Ansible** configuration management. The setup includes a virtual machine with SSH key authentication, a network security group, and an Ansible playbook deployment.

## **Project Overview**
- **Provisioning**: Terraform provisions Azure resources.
- **Configuration Management**: Ansible playbook is copied to the VM and executed.
- **Automation Tools Used**:
  - Terraform
  - Ansible
  - Azure Cloud
  - SSH Key Authentication

## **Infrastructure Components**
1. **Resource Group**: `devops-stage-4-rg`
2. **Virtual Network & Subnet**:
   - VNet: `10.0.0.0/16`
   - Subnet: `10.0.1.0/24`
3. **Network Security Group (NSG)**:
   - Allows **SSH (22), HTTP (80), HTTPS (443)**
4. **Public IP Address**: Assigned to the VM
5. **Linux Virtual Machine**: Ubuntu 22.04 LTS
6. **Ansible Playbook Execution**: Provisioned using Terraform

## **Prerequisites**
- [Terraform](https://developer.hashicorp.com/terraform/downloads)
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)
- SSH Key Pair

## **Setup Instructions**

### **1. Generate SSH Key Pair**
Run the following command to generate an SSH key:
```sh
ssh-keygen -t rsa -b 4096 -f "C:/Users/USER/.ssh/id_rsa_new"
```
This creates:
- **Public Key:** `id_rsa_new.pub`
- **Private Key:** `id_rsa_new`

### **2. Clone the Repository and login to your Azure account**
```sh
git clone (https://github.com/goodddybag/Devops-Infra.git)
cd Infra-code
az login --tenant tenantID
```

```sh
terraform init
```

### **4. Validate the Terraform Configuration**
```sh
terraform validate
```

### **5. Plan the Deployment**
```sh
terraform plan
```

### **6. Deploy the Infrastructure**
```sh
terraform apply -auto-approve
```

### **7. Ansible Provisioning**
Terraform will automatically:
- Copy the Ansible playbook to the VM
- Execute the playbook remotely

If needed, you can manually run:
```sh
ansible-playbook -i inventory playbook.yml
```

## **Terraform Configuration Highlights**
```hcl
resource "azurerm_linux_virtual_machine" "vm" {
  name                = "devops-vm"
  resource_group_name = azurerm_resource_group.rg.name
  location            = azurerm_resource_group.rg.location
  size                = "Standard_B2s"
  admin_username      = "adminuser"

  admin_ssh_key {
    username   = "adminuser"
    public_key = file("C:/Users/Maryk/Downloads/task4-piv.pub")
  }
}
```

## **Outputs**
After deployment, get the public IP:
```sh
echo $(terraform output public_ip)
```

## **Destroy the Infrastructure**
To remove all resources:
```sh
terraform destroy -auto-approve
```

## **Troubleshooting**
### **Invalid File Path for SSH Key**
If you see an error related to `file()` function, ensure:
- The SSH key exists at the specified location
- Use escaped paths in Windows: `C:/Users/USER/.ssh/id_rsa_new.pub`

### **Accessing the Services
The endpoints are accessible via:
https://goodybag.name.ng/api/users
<br>
https://goodybag.name.ng/api/auth
<br>
Traefik dashboard: http://52.151.35.11:8080/dashboard

## **Conclusion**
This project demonstrates an automated approach to provisioning and configuring cloud infrastructure using **Terraform** and **Ansible**. The infrastructure is secure, scalable, and repeatable for production deployments.
