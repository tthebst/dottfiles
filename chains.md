# Chains

## ETH 1
```
cat > $HOME/scratch/eth1.service << EOF 
[Unit]
Description     = geth execution client service
Wants           = network-online.target
After           = network-online.target 

[Service]
User            = $(whoami)
ExecStart       = /usr/bin/geth --http --goerli --metrics --pprof --datadir /home/tim/nfs/ethereum
Restart         = on-failure
RestartSec      = 3
KillSignal      = SIGINT
TimeoutStopSec  = 300

[Install]
WantedBy    = multi-user.target
EOF
sudo mv $HOME/eth1.service /etc/systemd/system/eth1.service
sudo chmod 644 /etc/systemd/system/eth1.service
```


bitcoin

```
# It is not recommended to modify this file in-place, because it will
# be overwritten during package upgrades. If you want to add further
# options or overwrite existing ones then use
# $ systemctl edit bitcoind.service
# See "man systemd.service" for details.

# Note that almost all daemon options could be specified in
# /etc/bitcoin/bitcoin.conf, but keep in mind those explicitly
# specified as arguments in ExecStart= will override those in the
# config file.

[Unit]
Description=Bitcoin daemon
Documentation=https://github.com/bitcoin/bitcoin/blob/master/doc/init.md
Wants           = network-online.target
After           = network-online.target

[Service]
User            = $(whoami)
ExecStart=/usr/bin/bitcoind -daemonwait \
                            -pid=/run/bitcoind/bitcoind.pid \
                            -conf=/etc/bitcoin/bitcoin.conf \
                            -datadir=/home/tim/nfs/bitcoind
Restart         = on-failure
RestartSec      = 3
KillSignal      = SIGINT
TimeoutStopSec  = 300


[Install]
WantedBy=multi-user.target
```

Move to systemd

```
sudo mv bitcoin.service /etc/systemd/system/bitcoin.service
sudo chmod 644 /etc/systemd/system/bitcoin.service
sudo systemctl start bitcoin
```
