
LBTC Light Wallet Server based on ElectrumX
===============================================

Licence: MIT

Language: Python (>= 3.6)
 
 Author: Benjamin Smith

Features
========

- Efficient, lightweight reimplementation of electrum-server
- Fast synchronization of bitcoin mainnet from Genesis.  Recent
  hardware should synchronize in well under 24 hours.  The fastest
  time to height 448k (mid January 2017) reported is under 4h 30m.  On
  the same hardware JElectrum would take around 4 days and
  electrum-server probably around 1 month.
- The full Electrum protocol is implemented.  The only exception is
  the blockchain.address.get_proof RPC call, which is not used by
  Electrum GUI clients, and can only be invoked from the command line.
- Various configurable means of controlling resource consumption and
  handling denial of service attacks.  These include maximum
  connection counts, subscription limits per-connection and across all
  connections, maximum response size, per-session bandwidth limits,
  and session timeouts.
- Minimal resource usage once caught up and serving clients; tracking the
  transaction mempool appears to be the most expensive part.
- Fully asynchronous processing of new blocks, mempool updates, and
  client requests.  Busy clients should not noticeably impede other
  clients' requests and notifications, nor the processing of incoming
  blocks and mempool updates.
- Daemon failover.  More than one daemon can be specified, and
  ElectrumX will failover round-robin style if the current one fails
  for any reason.
- Peer discovery protocol removes need for IRC
- Coin abstraction makes compatible altcoin and testnet support easy.

Getting Started 
===============
All the following command work normal on Ubuntu 16.04.3 LTS

1.Install dependencies
-------------

sudo apt-get install software-properties-common
sudo add-apt-repository ppa:jonathonf/python-3.6
sudo apt-get update
sudo apt-get install python3.6 python3.6-dev
sudo wget https://bootstrap.pypa.io/get-pip.py
sudo python3.6 get-pip.py
sudo apt-get install python3-leveldb libleveldb-dev
sudo apt-get install g++
sudo pip3 install aiohttp pylru leveldb plyvel

2. Prepare work directory
-------------

To allow lbtc light wallets to connect to your server over SSL you need to create a self-signed certificate.

mkdir ~/.electrumx/mainnet.db -p
cd ~/.electrumx

openssl genrsa -des3 -passout pass:x -out server.pass.key 2048
openssl rsa -passin pass:x -in server.pass.key -out server.key
rm server.pass.key
openssl req -new -key server.key -out server.csr
openssl x509 -req -days 1825 -in server.csr -signkey server.key -out server.crt

3. Init environment
-------------
Note : You should change some config info in env.conf before run following command

source env.conf

4. Run LBTC light wallet server
-------------
nohup ./electrumx_server.py &

5. Run LBTC full node wallet
-------------
download LBTC full node on http://lbtc.io/
install a full LBTC bitcoin node first and set at least the following minimum options in bitcoin.conf:

maxconnections=100
daemon=1
txindex=1
rpcuser=random username
rpcpassword=strong password
rpcallowip=12.41.29.92

If you already have lbtc bitcoin node installed, you need to stop and restart it using the following commands:

bitcoin-cli stop
bitcoind -txindex

**Benjamin Smith**  sunshine.benjamin.smith@gmail.com

1ECSDWsm17fbCECgdb5MvR3EZMT6Sbd232


