## Taiko Node Kurulumu
# Git kurulumu
En son sürümü çalıştırdığınızdan emin olmak lazım. Bunu yapmak için, komut satırı kabuğunuza gidin ve her şeyin güncel olduğundan emin olmak için aşağıdaki komutu çalıştırın: 
```
sudo apt-get update.
```

Git'i yüklemek için aşağıdaki komutu çalıştırın: 

``` sudo apt-get install git-all ```

Komut çıktısı tamamlandıktan sonra, yüklemenin doğru bir şekilde yapıldığını doğrulamak için şunu yazarak kontrol edebilirsiniz: 

```Git version ```


#Docker Kurulumu

```apt install docker-compose ```

```sudo apt-get update && sudo apt install jq && sudo apt install apt-transport-https ca-certificates curl software-properties-common -y && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin && sudo apt-get install docker-compose-plugin```

#Taiko Proje Kurulumu

Repoyu klonlayalım.

```git clone https://github.com/taikoxyz/simple-taiko-node.git```
```cd simple-taiko-node```

Dosyada düzenlemeler yapılmalı. 

```cp .env.sample .env```
```nano .env```

Açıldıktan sonra yön tuşları ile en alta geliyoruz.
ENABLE_PROPOSER kısmını true yapıyoruz
L1_PROPOSER_PRIVATE_KEY= kısmına metamasktan private key alıyoruz
L2_SUGGESTED_FEE_RECIPIENT=bu kısımda Metamask 0x Cüzdan Adresi olacak
sonra CTRL + X + Y ile çıkıyoruz.

Metamasktan 3 noktaya tıklayınca özel anahtarı dışarı aktar.

Node çalıştırmak

```docker compose up```

Nodu'u arka planda çalıştırmak: 

```docker compose up -d```


RPA Bilgileri: 

1/ RPC:

-l1
Ethereum A1
31338
https://l1rpc.a1.taiko.xyz
ETH
-l2
Taiko A1
167003
https://l2rpc.a1.taiko.xyz
ETH

Faucet: 
https://l1faucet.a1.taiko.xyz
https://l2faucet.a1.taiko.xyz

Bridge: https://bridge.a1.taiko.xyz

Diğer kommutlar: 

Node Durdurmak

```docker compose down```

Node Silmek

```docker compose down -v```
```rm -f .env```

Node Update

```docker compose pull```

Logları incelemek

docker compose logs -f

View the prover image's logs

docker logs simple-taiko-node-taiko_client_prover_relayer-1 -f

View the L2 execution engine logs
docker compose logs -f l2_execution_engine

View the live data streams of your running containers
This shows the CPU/MEM USAGE % and consumption of your machine's resources (add prefix "docker stats -a" to display all containers)

docker stats

Diğer Bilgiler 

A Grafana(opens in a new tab) dashboard with a Prometheus(opens in a new tab) datasource is also included to display the L2 execution engine's real time status. You can visit it at http://localhost:3000/d/L2ExecutionEngine/l2-execution-engine-overview?orgId=1&refresh=10s(opens in a new tab).

Troubleshooting
Node warning logs
You can ignore any WARN logs.

Node error logs
error: L1_ID
The block that you want to prove has already been verified, you can ignore this.

error: L1_ALREADY_PROVEN
This block has been proven by someone else, but its not verified yet, you can ignore it.

Fatal: Failed to register the Ethereum service: database contains incompatible genesis
Try to remove the node with docker compose down -v and then try again.

Unhandled trie error: missing trie node
You can ignore this error, it doesn't affect you and goes away after a while.

Block batch iterator callback error; error="failed to fetch L2 parent block: not found
You can ignore this error.

Error starting ...: listen tcp4 0.0.0.0:{port} bind: address already in use
The port is already in use by another service. You can either shut down the other program or change the port in the .env file.

error parsing HTTP 403 response body: invalid character '<' looking for beginning of value
Your IP address is being geo-blocked due to sanctions lists. If you're affected, try changing hosting locations or utilize a VPN to change your IP address.

ERROR: The Compose file './docker-compose.yml' is invalid because: Unsupported config option for some_serivce 'pull_policy'
Your docker installation is out of date. You need to update your docker compose installation: https://docs.docker.com/compose/install/(opens in a new tab).

daemon docker is not running
Cannot connect to the Docker daemon
Need to start the Docker before running the commands.

database contains incompatible genesis
If you ran an alpha-1 testnet node make sure to first run a docker compose down -v to remove the old volumes.

