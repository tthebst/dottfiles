# Nas Server

Current IP: 192.168.1.124:5000

## Mount on ubuntu

https://kb.synology.com/en-uk/DSM/tutorial/How_to_access_files_on_Synology_NAS_within_the_local_network_NFS


```
sudo apt update
sudo apt install nfs-common
```

Manual mount
```
sudo mount 192.168.1.124:/volume1/blockchain /home/tim/nfs
```

Add this line to `/etc/fstab` to mount on reboot

```
192.168.1.124:/volume1/blockchain               /home/tim/nfs      nfs defaults 0 0
```

```
cat > $HOME/eth1.service << EOF 
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
