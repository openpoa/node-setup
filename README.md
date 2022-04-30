# POA Node Setup Instructions

The following instructions are for docker-based Nethermind nodes.

<details>
  <summary>Install docker & docker compose</summary>

## Install docker & docker compose

```
sudo apt-get update && sudo apt-get dist-upgrade

sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io

sudo groupadd docker
sudo usermod -aG docker $(whoami)
# Refresh group
sudo su - $(whoami)

# Verify installation
docker run hello-world

sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod 666 /var/run/docker.sock
sudo chmod +x /usr/local/bin/docker-compose

# Verify installation
docker-compose --version
```

</details>


<details>
  <summary>POA Core RPC node</summary>
  
## POA Core RPC node

### Requirements
* CPU: 2
* Memory: 8
* Disk: 400G

```
git clone https://github.com/openpoa/node-setup.git

docker-compose -f docker-compose.poacore-rpc.yml up -d
```

</details>


<details>
  <summary>POA Core Validator node</summary>

## POA Core Validator node

### Requirements
* CPU: 4
* Memory: 16
* Disk: 400G

You will need 3 keys:
* Voting key
* Mining key
* Payout key 

```
git clone https://github.com/openpoa/node-setup.git

cp .env.example .env
```

Edit .env
```
ETHSTATS_ID=[validator_name] # whatever you'd like
ETHSTATS_CONTACT=[contact_email] # whatever you'd like
ETHSTATS_SECRET=[netstat_secret_key] # ask for this
KEY=[your_private_key_for_mining_address] # mining private key
SEQAPIKEY=[seq_api_key] # ask for this
```

Start
```
docker-compose -f docker-compose.poacore-validator up -d
```

Follow along with logs and when the node is fully caught up, edit the docker compose file to turn mining on
```
# Edit docker-compose.poacore-validator.yml
# Set NETHERMIND_INITCONFIG_ISMINING: "true"

# Restart node
docker-compose -f docker-compose.poacore-validator up -d
```

</details>


<details>
  <summary>POA Core Archive node</summary>

## POA Core Archive node

### Requirements
* CPU: 4
* Memory: 16
* Disk: 2TB

```
git clone https://github.com/openpoa/node-setup.git

docker-compose -f docker-compose.poacore-archive.yml up -d
```

</details>
