# storyprotocol-Validator-Node
The Story Protocol Validator Node is a crucial component of the Story Network ecosystem, designed to support the decentralized management of intellectual property (IP). It plays a key role in validating transactions and maintaining the integrity of the blockchain network.

 # Story Protocol

 Guide to Deploying Your Story Protocol Validator Node

 ![image](https://github.com/user-attachments/assets/da2ad101-cfb7-4622-9c10-1a0f8025e1ae)


 Join the decentralized future of intellectual property management by running your own Story Protocol validator node. Story Protocol is revolutionizing how creators register, track, and license IP on-chain, and as a node operator, you can directly contribute to this mission. Whether you’re incentivized by the technical challenge or the rewards for participating, this guide will take you through the process of installing and running your validator node.

 # Overview
 Story Protocol is an innovative blockchain platform designed to enable a new ecosystem for managing and governing intellectual property (IP). Its “Proof-of-Creativity” protocol allows for seamless provenance tracking, remixing, and licensing of creative works, including prose, music, images, and more. With $134.3 million raised in funding from top-tier investors like a16z and Polychain Capital, Story Protocol is poised to reshape the way IP is handled on a global scale. Running a node on Story Protocol not only helps decentralize the network but also provides the opportunity for participants to earn rewards through staking.

 
## Prerequisites
To get started with Story, ensure your server meets the following specifications:

Operating System: Ubuntu 22.04
CPU: 4 cores
RAM: 8 GB
Storage: 200 GB SSD
Open TCP Ports: 26666, 26667 and 30303

For hosting a Story node, opt for a VPS 2 server. I chose Contabo for its reliability and performance, ideal to meet the technical requirements of Story.
# Simplifying the Installation of the Story Node with Ansible
Even when following a detailed procedure, installing a node can often be complex and tedious, making the process difficult to maintain for users. To significantly simplify the deployment, updating, and removal of nodes, I have developed an Ansible automation script that handles all these operations. Thanks to this script, installing a Story node has never been more accessible and effortless. I am thrilled to make this script available to the community for free, so everyone can benefit from an improved user experience. If you are ready to ease your journey with Story, follow the guide and let’s discover how to achieve this together.
# System Preparation
Git and Ansible must be installed on your server. To do this, execute the following command as root. Update your system packages:


## Installation



```bash
  apt update && apt upgrade -y
``` 
# Install Git:
```bash
  apt install git -y
```
# Install Ansible:
```bash
apt remove ansible -y
apt-get install -y software-properties-common
apt-add-repository --yes --update ppa:ansible/ansible
apt install ansible 
```
# Check your Ansible version:
```bash
 ansible --version 
```
You must have Ansible version 2.15 or higher.
# Project Download
Start by cloning the project repository to access the Ansible playbook and all required files. Execute the following commands: 
```bash
git clone https://github.com/nodemasterpro/deploy-node-story.git
cd deploy-node-story  
```
# Installation of the story Node’s
Once the project files are ready, initiate the installation of the story node using this command:
```bash
  ansible-playbook install_story_nodes.yml -e moniker="your_moniker_name"

```
Replace "moniker" with the unique name you wish to assign to your node. Installation takes about 1 hour due to downloading and importing the snapshot.

![image](https://github.com/user-attachments/assets/7155608c-8a88-478d-9367-a3eb0e6a5c08)


Once completed, you can find your private key in “/root/.story/story/config/private_key.txt”. Save it, as it will be useful for the next steps.

#  Starting nodes services and consulting the Logs
Once your Geth and Consensus nodes are deployed, it’s important to monitor their activity. Use the following commands to restart and view their logs:

# Geth Node:
```bash
 systemctl start story-geth-node && journalctl -u story-geth-node -f -o cat 
```
![image](https://github.com/user-attachments/assets/b4caaf47-7479-4f59-895a-a69f5b6e5d95)

To exit the logs, type Ctrl+C.

# Consensus Node:
```bash
systemctl start story-consensus-node && journalctl -u story-consensus-node -f -o cat

```
Regularly check these logs to ensure everything is running smoothly.

![image](https://github.com/user-attachments/assets/63774831-a278-4852-8d70-550505d165ce)


To exit the logs, type Ctrl+C.
# Setting up your Metamask Story Node Wallet
To register your node as a validator, you need to fund it by obtaining tokens from a faucet using your node’s public address. To obtain this public address, import your node’s private key into a MetaMask wallet. As a reminder, the private key is present in “/root/.story/story/config/private_key.txt”.

![image](https://github.com/user-attachments/assets/0344c45d-2c8c-4095-a2eb-e70b657b4921)


Add the testnet Story network to this wallet. If you don’t have it, you can add it automatically from the chainlist website.

![image](https://github.com/user-attachments/assets/38c2f8fe-e73e-47aa-976d-15370faf0503)

# Requesting Story Testnet Funds
You need at least 0.5 IP for registering your node as a validator. Get funds for your wallet through https://story.faucetme.pro/ by logging in with your discord and joining their discord. Then, enter the public address of your node’s metamask wallet

![image](https://github.com/user-attachments/assets/7d48b960-e006-47e7-b290-6f650cbabb90)


You will get 2 IP. You can only make one request every 24 hours.

# Node Synchronization Verification
Ensure your node is fully synchronized with the Story blockchain:
```bash
 curl -s localhost:26657/status | jq '.result.sync_info' 
```
![image](https://github.com/user-attachments/assets/78eb5dae-5a9c-468e-a7ac-1c07cd778ac8)


Wait until the catching_up variable is false. Once the catching_up variable is false, your node is fully synchronized and ready for further operations.

#  Registering the Node as a Validator
To complete this step, your node must be synced with the blockchain and you must hold at least 0.5 IP (+ gas fees) in your node address for staking:
```bash
 ansible-playbook register_story_validator_node.yml -e stake_amount="500000000000000000" 
```
At the end of the script, you will obtain the public address of your node.

![image](https://github.com/user-attachments/assets/042daae7-30e8-4e78-b58d-1d90d2cbf5ed)


After successfully creating your validator, you can view it on the https://testnet.story.explorers.guru/. Simply enter the public address of your node, which was provided to you at the end of the node registration script.

![image](https://github.com/user-attachments/assets/08f9b79a-1b8a-42fe-8527-d3d49805f286)


Congratulations! Your Story validator node is operational.

#  Additional Information
# Stopping Services
To stop the story geth node and story consensus node:
```bash
 systemctl stop story-geth-node && systemctl stop story-consensus-node 
```
# Starting Services
To start the story geth node and story consensus node:
```bash
systemctl start story-geth-node && systemctl start story-consensus-node  
```
# Removing the Story Node
To remove the story node’s, run this playbook:
```bash
 ansible-playbook remove_story_nodes.yml 
```
Ensure to back up all important data before deleting the Story node, as this action may remove node data.

# By following this guide, you have successfully deployed and managed a Story validator node, contributing to the robustness of the network and potentially earning rewards. Join the Sotry community on Discord and Twitter to stay informed about the project.
