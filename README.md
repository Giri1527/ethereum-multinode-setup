# Multi-Node Ethereum Setup (Geth)

This repository provides step-by-step instructions to set up a **Private multi-node Ethereum blockchain** using **Geth** (Go Ethereum).

---

## 📌 Prerequisites
- Ubuntu / Linux system
- Basic command-line knowledge
- Internet connection

---

## Step 1: Install Go

```bash
wget https://golang.org/dl/go1.16.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.16.linux-amd64.tar.gz
```

Update `.profile`:
```bash
sudo gedit ~/.profile
```

Add at the end:
```bash
export GOPATH=~/go
export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin
```

Apply changes:
```bash
source ~/.profile
go env
```

---

## Step 2: Build Geth
```bash
go install -v ./cmd/geth
```

---

## Step 3: Create Ethereum Accounts
```bash
geth --datadir ~/ethereum/node1 account new
```
Repeat for `node2`, `node3`, etc.

---

## Step 4: Create Genesis Block
```bash
# Use Puppeth or an existing genesis.json file
```

Initialize each node:
```bash
geth --datadir ~/ethereum/node1 init genesis.json
geth --datadir ~/ethereum/node2 init genesis.json
```

---

## Step 5: Create and Run Bootnode
```bash
bootnode -genkey boot.key
bootnode -nodekey boot.key
```

Example enode:
```
enode://6d20dc312a4c9262a7251a68dc539db0557123309c6aae8b34bad9b1ee0afc1bf36d3be604ebec067ad7f962ec745fc2749bfcba868a01ce616542f22bafea58@127.0.0.1:0?discport=30301
```

---

## Step 6: Start Ethereum Nodes
```bash
geth --allow-insecure-unlock --datadir ~/ethereum/node1/ --syncmode 'full' \
--port 30311 --http --http.addr 'localhost' --http.port 8501 \
--http.api 'personal,eth,net,web3,txpool,miner' \
--bootnodes 'enode://<bootnode-enode-url>' \
--networkid 1515 --miner.gasprice '1' --mine console
```
---

