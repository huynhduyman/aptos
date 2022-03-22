# aptos
Run Fullnode Aptos
1. Cấu hình tối thiểu
Nếu chạy node node cho mục đích dev và test:

CPU: 2 cores Memory: 4GiB RAM

Chạy node cho product cấu hình tối thiểu dưới đây:

CPU: Intel Xeon Skylake hoặc mới hơn, 4 nhân.

Memory: 8GiB RAM

2. Hướng dẫn cài đặt từng bước
Bước 1 Cài Screen và Docker

sudo apt install screen

screen -S homepage

sudo apt update

sudo apt install ca-certificates curl gnupg lsb-release wget -y

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io -y

Bước 2 Cài Docker Compose

mkdir -p ~/.docker/cli-plugins/

curl -SL https://github.com/docker/compose/releases/download/v2.2.3/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose

chmod +x ~/.docker/cli-plugins/docker-compose

sudo chown $USER /var/run/docker.sock

BƯỚC 3 Tạo folder Aptos để Download Config Files

mkdir $HOME/aptos

cd $HOME/aptos

wget https://raw.githubusercontent.com/aptos-labs/aptos-core/main/docker/compose/public_full_node/docker-compose.yaml

wget https://raw.githubusercontent.com/aptos-labs/aptos-core/main/docker/compose/public_full_node/public_full_node.yaml

wget https://devnet.aptoslabs.com/genesis.blob

wget https://devnet.aptoslabs.com/waypoint.txt

BƯỚC 4 Chạy Node

docker compose up -d

Well done , you are in.

** Code hữu ích**

Kiểm tra trạng thái sync

curl 127.0.0.1:9101/metrics 2> /dev/null | grep aptos_state_sync_version | grep type

Xem log

docker logs -f aptos-fullnode-1 --tail 5000
