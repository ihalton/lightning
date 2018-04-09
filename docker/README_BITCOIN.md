# Start bitcoin test network

- start bitcoin regtest on host system:
```
bitcoind -regtest -daemon
```
- start bitcoin-lightning containers:
```
docker run -dt --name=bitcoin1 gilyav/bitcoind
docker run -dt --name=bitcoin2 gilyav/bitcoind
```
# Stop bitcoin test network

- stop and remove docker containers:
```
docker rm -f bitcoin1
docker rm -f bitcoin2
```
- stop bitcoin regtest on host system
```
bitcoin-cli -regtest stop
```
