# Gensyn Node Run Guide By sszenox

A simple VPS guide by **sszenox** for setting up and managing a Gensyn RL Swarm node.

This README is made for users who want a clear step-by-step command guide for installing dependencies, running the node, managing screen sessions, exposing the frontend, and backing up node files.

## Important Note

Gensyn testnet tools and versions may change over time. Always check the official Gensyn resources before running commands on a production VPS.

Official links:

- Gensyn Docs: https://docs.gensyn.ai/
- Gensyn Dashboard: https://dashboard.gensyn.ai/
- Gensyn Explorer: https://gensyn-testnet.explorer.alchemy.com/

## Node Management

### Enter Gensyn Screen

```bash
screen -r gensyn
```

### Check Screen Sessions

```bash
screen -ls
```

### Reattach To A Screen Session

```bash
screen -r
```

### Delete A Screen Session

Replace `65432.gensyn` with your actual screen session name.

```bash
screen -S 65432.gensyn -X quit
```

### Remove Existing Swarm Directory

Use this only if you want a fresh setup.

```bash
rm -rf rl-swarm
```

## Step 1: System Preparation

### Install sudo

```bash
sudo apt update && sudo apt install -y sudo
```

### Install Essential Packages

```bash
sudo apt update && sudo apt install -y \
  python3 python3-venv python3-pip curl wget screen git lsof \
  nano unzip iproute2 build-essential gcc g++
```

## Step 2: Install CUDA

Use this step only if your VPS/GPU setup needs CUDA.

```bash
[ -f cuda.sh ] && rm cuda.sh
curl -o cuda.sh https://raw.githubusercontent.com/zunxbt/gensyn-testnet/main/cuda.sh
chmod +x cuda.sh
. ./cuda.sh
```

## Step 3: Install Node.js, Yarn, And Python Tools

```bash
sudo apt update && sudo apt install -y python3 python3-venv python3-pip curl wget screen git lsof

curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -

sudo apt update && sudo apt install -y nodejs

curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -

echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

sudo apt update && sudo apt install -y yarn
```

## Step 4: Verify Installations

```bash
node -v
npm -v
yarn -v
python3 --version
```

## Step 5: Open A Screen Session

```bash
screen -S gensyn
```

## Step 6: Clone RL Swarm Repository

```bash
git clone https://github.com/gensyn-ai/rl-swarm.git && cd rl-swarm
```

## Step 7: Set Up Python Virtual Environment

```bash
python3 -m venv .venv
source .venv/bin/activate
```

## Step 8: Set Up Frontend

```bash
cd modal-login

yarn install
yarn upgrade
yarn add next@latest
yarn add viem@latest

cd ..
```

## Step 9: Pull Latest Code And Checkout Version

```bash
git reset --hard
git pull origin main
git checkout tags/v0.4.3
```

## Step 10: Edit Configuration File

```bash
nano hivemind_exp/configs/mac/grpo-qwen-2.5-0.5b-deepseek-r1.yaml
```

To save after editing:

```text
CTRL + X
CTRL + Y
Enter
```

## Step 11: Add Your Node File

If you have a `swarm.pem` file, upload it from your local PC to your VPS.

Make sure the file is placed in the correct project directory before starting the node.

## Step 12: Start RL Swarm

```bash
RL_SWARM_UNSLOTH=False ./run_rl_swarm.sh
```

## Step 13: Expose Frontend With LocalTunnel

### Install LocalTunnel

```bash
sudo npm install -g localtunnel
```

### Start Tunnel On Port 3000

```bash
lt --port 3000
```

Open the generated LocalTunnel URL in your browser and complete the required login or setup flow.

## Step 14: Backup Your Node

```bash
[ -f backup.sh ] && rm backup.sh
curl -sSL -O https://raw.githubusercontent.com/AbhiEBA/gensyn1/main/backup.sh
chmod +x backup.sh
./backup.sh
```

## Useful Links

- Gensyn Docs: https://docs.gensyn.ai/
- Gensyn Dashboard: https://dashboard.gensyn.ai/
- Gensyn Explorer: https://gensyn-testnet.explorer.alchemy.com/
- Gensyn Track Bot: https://t.me/gensyntrackbot
- Gensyn Rewards Bot: https://t.me/gensyn_rewards_me_bot

## Created By

Created by **sszenox** for users who want a simple Gensyn node setup guide.
