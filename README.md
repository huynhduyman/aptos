Run Fullnode Aptos in Unbuntu OS

Minimum Configuration If running node for dev and test purposes:
CPU: 2 cores Memory: 4GiB RAM

Run node for the product with the following minimum configuration:

CPU: Intel Xeon Skylake or later, 4 cores.

Memory: 8GiB RAM

Step-by-step installation instructions Step 1 Install Screen and Docker
sudo apt install screen

screen -S homepage

sudo apt update

sudo apt install ca-certificates curl gnupg lsb-release wget -y

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release) -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io -y

Step 2 Install Docker Compose

mkdir -p ~/.docker/cli-plugins/

curl -SL https://github.com/docker/compose/releases/download/v2.2.3/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose

chmod +x ~/.docker/cli-plugins/docker-compose

sudo chown $USER /var/run/docker.sock

STEP 3 Create Aptos folder to Download Config Files

mkdir $HOME/aptos

cd $HOME/aptos

wget https://raw.githubusercontent.com/aptos-labs/aptos-core/main/docker/compose/public_full_node/docker-compose.yaml

wget https://raw.githubusercontent.com/aptos-labs/aptos-core/main/docker/compose/public_full_node/public_full_node.yaml

wget https://devnet.aptoslabs.com/genesis.blob

wget https://devnet.aptoslabs.com/waypoint.txt

STEP 4 Run Node

docker compose up -d

Well done , you are in.

** Useful Codes**

Check sync status

curl 127.0.0.1:9101/metrics 2> /dev/null | grep aptos_state_sync_version | grep type

View log

docker logs -f aptos-fullnode-1 --tail 5000
