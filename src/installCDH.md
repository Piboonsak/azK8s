#Prepare CDH Node

##Step_1

`sudo -i`

`passwd`

##Step_2

`nano /etc/ssh/sshd_config`

```
###PermitRootLogin yes
###..
###ChallengeResponseAuthentication yes
###..
###PasswordAuthentication yes
```

## Do every node
`service ssh restart`

`ssh-keygen`

`service ssh restart`

##Step3
###At name node

`ssh-copy-id root@dn-node1`

`ssh-copy-id root@dn-node2`

`ssh-copy-id root@dn-node3`

```
ssh 'root@dn-node1'
ssh 'root@dn-node2'
ssh 'root@dn-node3'
```

###At data node1
```
ssh-copy-id root@nm-node
ssh-copy-id root@dn-node2
ssh-copy-id root@dn-node3
```
```
ssh 'root@nm-node'
ssh 'root@dn-node2'
ssh 'root@dn-node3'
```

###At data node2
```
ssh-copy-id root@nm-node
ssh-copy-id root@dn-node1
ssh-copy-id root@dn-node3
```
```
ssh 'root@nm-node'
ssh 'root@dn-node1'
ssh 'root@dn-node3'
```
###At data node3
```
ssh-copy-id root@nm-node
ssh-copy-id root@dn-node1
ssh-copy-id root@dn-node2
```
```
ssh 'root@nm-node'
ssh 'root@dn-node1'
ssh 'root@dn-node2'
```

## Step4

`sysctl -w vm.swappiness=0`

# Install CDH 5

`wget http://archive.cloudera.com/cm5/installer/latest/cloudera-manager-installer.bin`

`chmod u+x cloudera-manager-installer.bin` 

`sudo ./cloudera-manager-installer.bin`

# Install CDH6
`wget https://archive.cloudera.com/cm6/6.1.1/cloudera-manager-installer.bin`

`chmod -R 755 cloudera-manager-installer.bin`

