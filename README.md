# Multi-Node Ethereum Setup (Geth)

This repository provides step-by-step instructions to set up a **Private multi-node Ethereum blockchain** using **Geth** (Go Ethereum).

---

## 📌 Prerequisites

Before starting, make sure you have:

- **Ubuntu / Linux system** (recommended)
- **Basic command-line knowledge**
- **Internet connection** to download Go and Ethereum dependencies

---

##  Step 1: Install Go 

Download and install Go:

wget https://golang.org/dl/go1.16.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.16.linux-amd64.tar.gz


Update your .profile to set the Go environment variables:
   sudo gedit ~/.profile
Add at the end:
   export GOPATH=~/go
   export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin
Apply changes:
    source ~/.profile
    go env
Output:


## Step 2: Build Geth
   go install -v ./cmd/geth

## Step 3: Create Ethereum Accounts

   geth --datadir ~/ethereum/node1 account new
 
 Repeat for multiple nodes (node2, node3, etc.) if needed.

##  Step 4: Create Genesis Block
     Use Puppeth or an existing genesis.json file.
Make sure to include your accounts' public keys.

Initialize each node with the genesis file:

geth --datadir ~/ethereum/node1 init genesis.json
geth --datadir ~/ethereum/node2 init genesis.json

## Step 5: Create and Run Bootnode

bootnode -genkey boot.key
bootnode -nodekey boot.key

Example enode:
enode://6d20dc312a4c9262a7251a68dc539db0557123309c6aae8b34bad9b1ee0afc1bf36d3be604ebec067ad7f962ec745fc2749bfcba868a01ce616542f22bafea58@127.0.0.1:0?discport=30301

## Step 6: Start Ethereum Nodes

geth --allow-insecure-unlock --datadir ~/ethereum/node1/ --syncmode 'full' \
--port 30311 --http --http.addr 'localhost' --http.port 8501 \
--http.api 'personal,eth,net,web3,txpool,miner' \
--bootnodes 'enode://<bootnode-enode-url>' \
--networkid 1515 --miner.gasprice '1' --mine console
