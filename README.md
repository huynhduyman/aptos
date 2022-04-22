# Run Fullnode Aptos in Unbuntu OS

Minimum Configuration If running node for dev and test purposes:
CPU: 2 cores Memory: 4GiB RAM

Run node for the product with the following minimum configuration:

CPU: Intel Xeon Skylake or later, 4 cores.

Memory: 8GiB RAM

Step-by-step installation instructions Step 1 Install Screen and Docker

Feel like a linux guru and install everything yourself. I installed on Ubuntu 20.04.4 LTS.

Installation may take 10-30 minutes

Install docker if it is not installed

`sudo apt update`

`sudo apt install ca-certificates`

`curl gnupg lsb-release wget jq sed -y`

`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg`

`echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`

`sudo apt update`


`sudo apt install docker-ce docker-ce-cli containerd.io -y`

Checking

`sudo chown $USER /var/run/docker.sock`

`docker version`

## If there is no error in the terminal, then everything is OK
Client: Docker Engine - Community...
### Install docker compose if not installed
`mkdir -p ~/.docker/cli-plugins/`

`curl -SL https://github.com/docker/compose/releases/download/v2.2.3/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose`

`chmod +x ~/.docker/cli-plugins/docker-compose`

`sudo chown $USER /var/run/docker.sock`

### Checking

`docker compose version`

## If there is no error in the terminal, then everything is OK
Docker Compose version v2.2.3
Create a folder aptos in which we will download config files

`mkdir $HOME/aptos`

`cd $HOME/aptos`

`wget https://raw.githubusercontent.com/aptos-labs/aptos-core/main/docker/compose/public_full_node/docker-compose.yaml`

`wget https://raw.githubusercontent.com/aptos-labs/aptos-core/main/docker/compose/public_full_node/public_full_node.yaml`

`wget https://devnet.aptoslabs.com/genesis.blob`

`wget https://devnet.aptoslabs.com/waypoint.txt`

Create an identity folder where we will store a unique node ID

`mkdir $HOME/aptos/identity`

Generating a unique ID for a node

`docker run --rm --name aptos_tools -d -i aptoslab/tools:devnet`
`docker exec -it aptos_tools aptos-operational-tool generate-key --encoding hex --key-type x25519 --key-file $HOME/private-key.txt`

`docker exec -it aptos_tools cat $HOME/private-key.txt > $HOME/aptos/identity/private-key.txt`

`docker exec -it aptos_tools aptos-operational-tool extract-peer-from-file --encoding hex --key-file $HOME/private-key.txt --output-file $HOME/peer-info.yaml > $HOME/aptos/identity/id.json`

`PEER_ID=$(cat $HOME/aptos/identity/id.json | jq -r '.Result | keys[]')`

`PRIVATE_KEY=$(cat $HOME/aptos/identity/private-key.txt)`

`docker stop aptos_tools`

Set a unique node ID
`cd $HOME/aptos`

`sed -i '/ discovery_method: "onchain"$/a\
      identity:\
          type: "from_config"\
          key: "'$PRIVATE_KEY'"\
          peer_id: "'$PEER_ID'"' public_full_node.yaml`

Data with node ID and private key will be stored on your HDD

View private key

`cat $HOME/aptos/identity/private-key.txt`

View data with public identifiers

`cat $HOME/aptos/identity/id.json`

Launching the node
If the node is not running then

`docker compose up -d`

If the node is already running then

`docker compose restart`

Checking the sync status
`curl 127.0.0.1:9101/metrics 2> /dev/null | grep aptos_state_sync_version | grep type`

Should look something like this

website url

If you wish, you can see the logs

`docker logs -f aptos-fullnode-1 --tail 5000`

Good luck

# Guide you to update to the new chain: Update Fullnode With New Releases

# change to root user
sudo su

# stop docker
cd $HOME/aptos
docker compose down

# delete old file
rm -r /var/lib/docker/volumes/aptos_db/_data/db
rm genesis.blob
rm waypoint.txt

# update file
docker compose pull
wget https://devnet.aptoslabs.com/genesis.blob
wget https://devnet.aptoslabs.com/waypoint.txt

docker compose up -d


restart node : `docker compose restart`
sync check :`curl 127.0.0.1:9101/metrics 2> /dev/null | grep aptos_state_sync_version | grep type`

check log :`docker logs -f aptos-fullnode-1 --tail 5000`

![This is an image](https://scontent.fsgn2-4.fna.fbcdn.net/v/t1.15752-9/275563754_5008618015889184_2033242216791790752_n.png?_nc_cat=109&ccb=1-5&_nc_sid=ae9488&_nc_ohc=fvZBWJAI7-YAX997Dgy&_nc_ht=scontent.fsgn2-4.fna&oh=03_AVIzHxdok7qdkjxJTWkR1foGz0fmKdBX5Nes_T98QXU1Dg&oe=626A4EFD)


