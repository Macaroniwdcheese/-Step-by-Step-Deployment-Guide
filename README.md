# -Step-by-Step-Deployment-Guide

Building hands-on cybersecurity lab in Azure  a step-by-step command guide so you can run everything from your Linux terminal using the Azure CLI:

🧰 Prerequisites (One-Time Setup)

Make sure you have:

![Screenshot from 2025-06-10 12-03-55](https://github.com/user-attachments/assets/30063914-c0f8-46f6-a2ff-fda16c76562a)

![Screenshot from 2025-06-10 11-56-45](https://github.com/user-attachments/assets/324a6ff3-330f-457b-9254-d59dde9693af)

verify if azure cli is installed:
command: ''az version''

![Screenshot from 2025-06-10 12-07-05](https://github.com/user-attachments/assets/fe369a16-4498-407c-b902-5b779e450cff)

1. 📁 Create a working folder
mkdir azure-sec-lab && cd azure-sec-lab

2. ✍️ Save the ARM template
nano secure-vm-lab.json

![Screenshot from 2025-06-10 12-40-36](https://github.com/user-attachments/assets/a0ad187d-4f06-4a34-b88f-6be5857aaf77)

4. 🗝️ Prepare your variables
   # Use your actual public IP
export MY_IP=$(curl -s ifconfig.me)

# Use your public SSH key (adjust if needed)
export MY_KEY=$(cat ~/.ssh/id_rsa.pub)

4. 🚀 Deploy to your Resource Group
az deployment group create \
  --resource-group SecLab-RG \
  --template-file secure-vm-lab.json \
  --parameters adminPublicKey="$MY_KEY" allowedIP="$MY_IP"

5. 🔐 Connect to your VM
   az network public-ip show \
  --resource-group SecLab-RG \
  --name SecLab-PublicIP \
  --query ipAddress \
  --output tsv

then:
ssh azureuser@ your-public-ip

