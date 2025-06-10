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
nano secure-vm-lab-eastus.json

![Screenshot from 2025-06-10 12-40-36](https://github.com/user-attachments/assets/a0ad187d-4f06-4a34-b88f-6be5857aaf77)

4. 🗝️ Prepare your variables
   # Use your actual public IP
export MY_IP=$(curl -s ifconfig.me)

# Use your public SSH key (adjust if needed)
export MY_KEY=$(cat ~/.ssh/id_rsa.pub)

4. 🚀 Deploy to your Resource Group
az deployment group create \
  --resource-group SecLab-RG \
  --template-file secure-vm-lab-eastus.json \
  --parameters adminPublicKey="$MY_KEY" allowedIP="$MY_IP"

5. 🔐 Connect to your VM
   az network public-ip show \
  --resource-group SecLab-RG \
  --name SecLab-PublicIP \
  --query ipAddress \
  --output tsv

_______________________________________________________________________________________________

Ik liep tegen een error aan mijn json template was correct opgesteld alleen kreeg ik een interne error bij het verwerken van de API-respons, wat resulteert in deze RuntimeError:
The content for this response was already consumed.

![Screenshot from 2025-06-10 19-47-20](https://github.com/user-attachments/assets/55fbb5a2-812a-48b4-9a55-2bd76ca1bccb)


Dit is een bekend probleem in bepaalde versies van de Azure CLI, vooral bij Python 3.12-gebaseerde builds. 
Het heeft niets met jouw template of parameters te maken — de fout zit in de foutafhandeling van de CLI zelf.

ik heb 3 opties; python downgraden, vanuit een docker container werken, of vanuit een venv virtual omgeving.

Ik koos voor de virtuele omgeving en parameters json file toevoegen aan de project. 

vervolgens heb ik beide json files geupload via de cloudshell en ben toen vanuit de shell gaan werken, dus niet lokaal. 

![Screenshot from 2025-06-10 19-49-11](https://github.com/user-attachments/assets/1a95a05d-d551-4b82-b980-53dcdd4a836e)


Recources Deployen in cloudshell:

az deployment group create   --resource-group myResourceGroupEast   --template-file secure-vm-lab-eastus.json   --parameters @parameters.json

![Screenshot from 2025-06-10 20-14-30](https://github.com/user-attachments/assets/083a4c3d-f4ec-49ce-a1ff-2cea523dd328)


![Screenshot from 2025-06-10 20-11-56](https://github.com/user-attachments/assets/f3a3d31d-f2fa-45f8-aa90-1289c15705d3)


connecten met de SecLAb Virtual machine:

ssh azureuser@ your-public-ip
![Screenshot from 2025-06-10 20-10-43](https://github.com/user-attachments/assets/0ef3014c-44b2-4d2b-b77d-55a8e48abb4d)


We are in !

![Screenshot from 2025-06-10 20-18-08](https://github.com/user-attachments/assets/d64e2452-ad2d-445c-8ff2-67b9f131059d)

______________________________________________________________________________________________________________________________

🔥 Simulation Plan: Simulate a Malware Download & Execution

This technique mimics an attacker downloading and running a suspicious script — one of the most common attack vectors.

______________________________________________________________________________________________________________________________

