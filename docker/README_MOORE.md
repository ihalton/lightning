# Start Moore test network

- buid start Moore regtest network on host system:
```
$ sudo apt-get install build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils git cmake libboost-all-dev
$ sudo apt-get install software-properties-common
$ sudo add-apt-repository ppa:bitcoin/bitcoin
$ sudo apt-get update
$ sudo apt-get install libdb4.8-dev libdb4.8++-dev
$ git clone https://github.com/ihalton/moore --recursive
$ cd moore
$ ./autogen.sh
$ ./configure 
$ make -j2
$ src/moored -regtest -daemon
```
- start moore-lightning containers:
```
docker run -dt --name=moore1 gilyav/moored
docker run -dt --name=moore2 gilyav/moored
```

# Log into node moore1
```
$ docker exec -it moore1 bash
```

# Log into node moore2
```
$ docker exec -it moore2 bash
```

`moore1#` - prompt for moore1 node instance
`moore2#` - prompt for moore2 node instance

# Generate blocks in the network

more than 500 blocks need to be generated to make balance available

```
moore1# moore-cli -regtest generate 100
moore2# moore-cli -regtest generate 100
moore1# moore-cli -regtest generate 100
moore2# moore-cli -regtest generate 100
moore1# moore-cli -regtest generate 100
moore2# moore-cli -regtest generate 100
moore1# moore-cli -regtest generate 100
```

# Check blockchain info and wallet balance
moore1# moore-cli -regtest getblockchaininfo
moore1# moore-cli -regtest getwalletinfo
moore2# moore-cli -regtest getblockchaininfo
moore2# moore-cli -regtest getwalletinfo

# Create channel wallets
moore1# lightning-cli newaddr
moore2# lightning-cli newaddr

# Send coins to channels
moore1# moore-cli -regtest sendtoaddress <address> 10
moore2# moore-cli -regtest sendtoaddress <address> 10

# Check node IP addresses with commands
$ docker inspect moore1
$ docker inspect moore2

# Check node IDs addresses with commands
moore1# lightning-cli getinfo
moore2# lightning-cli getinfo

# Make connection from node1 to mode2
moore1# lightning-cli connect <node2_id> <node2_ip>

# Stop Moore test network

- stop and remove docker containers:
```
docker rm -f moore1
docker rm -f moore2
```
- stop Moore on the host system
```
src/moore-cli -regtest stop
```
