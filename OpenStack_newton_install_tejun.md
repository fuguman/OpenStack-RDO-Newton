#Packstack�ɂ��OpenStack�C���X�g�[���K�C�h

�ŏI�X�V��: 2016/12/25


##���̕����ɂ���
���̕����͂Ƃ肠����1��ɑS�������OpenStack�����������ƍ\�z����ꍇ�̎菇�ł��B�ׂ������Ƃ͏Ȃ��Ă��܂����̂ŁA���������ׂ����菇�ɂ��Ă͎��̃y�[�W�̏��Ȃǂ��Q�l�ɂ��Ă��������B

- [Kilo](https://github.com/ytooyama/rdo-kilo)
- Newton (��ƒ�)
- [���̑�](https://github.com/ytooyama?tab=repositories)


##�O��
- Oracle Virtualbox���g�p���܂��B�i���̃o�b�N�A�b�v�AOpenStack�\�z���Disk�̒ǉ��Ȃǂ��s�����߁j
- 2Core,8GB�������[,100GB�f�B�X�N�i�f�B�X�NVDI�ŉρj�̊���p�ӂ��܂��B
- �z�X�g�I�����[�l�b�g���[�N�A�_�v�^��OpenStack�̊Ǘ��p�A�_�v�^�Ƃ��Ďg�p���܂��B
- OS���C���X�g�[����`yum update`���čŐV�̏�Ԃɂ��܂��B
- �Œ�IP�A�h���X��ݒ肵�Ă����܂��B
- �{���NIC eth1���C���^�[�l�b�g�Q�[�g�E�F�C�Ɛڑ�����Ă���NIC�ł���Ƒz�肵�܂��i�Ⴄ�ꍇ�͓ǂݑւ��Ă��������j�B

##Step 1: �C���X�g�[���܂ł̗���

###�O����

Packstack�ɂ��OpenStack�̃f�v���C���s���O�ɁA���L���Q�l�ɏ������Ă����Ă��������B

- [Packstack ������]

�ŏI�X�V��: 2016/12/25


##���̕����ɂ���
���̕����͂Ƃ肠����1��ɑS�������OpenStack�����������ƍ\�z����ꍇ�̎菇�ł��B�ׂ������Ƃ͏Ȃ��Ă��܂����̂ŁA���������ׂ����菇�ɂ��Ă͎��̃y�[�W�̏��Ȃǂ��Q�l�ɂ��Ă��������B

- [Kilo](https://github.com/ytooyama/rdo-kilo)
- Newton (���ꂾ�悧��)
- [���̑�](https://github.com/ytooyama?tab=repositories)
- [Virutalbox�ł�Mitaka�ł̐ݒ�Q�l](http://qiita.com/mfujita/items/ee2da9d1e241926fc790)


##�O��
- 
- 2Core,8GB�������[,100GB�f�B�X�N(Oracle Virtualbox,VDI�f�B�X�N,�ρj�̊���p�ӂ��܂��B
- �z�X�g�I�����[�l�b�g���[�N�A�_�v�^�̍쐬�iVirtual Box���̐ݒ�j
- �Œ�IP�A�h���X��ݒ肵�Ă����܂��B
````
# cat ifcfg-eth0
TYPE=Ethernet
BOOTPROTO=none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
NAME=eth0
UUID=xxxxxxxxxxxxxxxxxxxxxx
DEVICE=eth0
ONBOOT=yes
IPADDR=192.168.56.101
PREFIX=24
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
IPV6_PRIVACY=no
````
- �{���NIC eth1���C���^�[�l�b�g�Q�[�g�E�F�C�Ɛڑ�����Ă���NIC�ł���Ƒz�肵�܂��i�Ⴄ�ꍇ�͓ǂݑւ��Ă��������j�B

##Step 1: �C���X�g�[���܂ł̗���

###OS�̃C���X�g�[���ƃA�b�v�f�[�g
- CentOS 7.x���ŏ��C���X�g�[�����āA�A�b�v�f�[�g���s���Ă����܂��B
- [CentOS7�̃C���X�g�[����ƎQ�l�菇](http://www.kakiro-web.com/memo/centos-install.html)
- �C���X�g�[���[�̋N����ʂ��N��������A�utab�L�[�v�������Akernel�R�}���h�s�̖����Ɂhbiosdevname=0 net.ifnames=0"��ݒ肷�邱�ƁB
- �X�g���[�W�̑I���̃p�[�e�B�V�����\�����u�����̃p�[�e�B�V�����\���v��I�������A�p�[�e�B�V�����\�����s��������I�����A�u/�v�z���ɑ����̊������s�����ƁB

### NTP�ݒ���s��
- chronyd�𓱓�����B
````
# yum install chrony
# vi /etc/chrony.conf
 Use public servers from the pool.ntp.org project.
# Please consider joining the pool (http://www.pool.ntp.org/join.html).
#server 0.centos.pool.ntp.org iburst �R�����g�A�E�g
#server 1.centos.pool.ntp.org iburst�@�R�����g�A�E�g
#server 2.centos.pool.ntp.org iburst�@�R�����g�A�E�g
#server 3.centos.pool.ntp.org iburst�@�R�����g�A�E�g
server ntp.nict.jp iburst�@�@�@<-�ǉ�
server ntp1.plala.or.jp iburst <-�ǉ�
:wq
# systemctl restart chronyd.service
# systemctl enable chronyd.service
# systemctl status chronyd.service
# chronyc sources
210 Number of sources = 2
MS Name/IP address         Stratum Poll Reach LastRx Last sample
===============================================================================
^* ntp-b3.nict.go.jp             1  10   377   116   -738us[-1032us] +/- 7001us
^- ntp1.plala.or.jp              4  10   377   123  -2524us[-2818us] +/-  135ms
�̂悤�ɂȂ邱�ƁB
````

###����ݒ���s��
�W���o�͂���уG���[�o�͂��p��ŏo�͂��邽�߂Ɏ��̐ݒ���s���܂��B

````
# vi /etc/environment
LANG=en_US.utf-8
LC_ALL=en_US.utf-8
````

###�l�b�g���[�N�ݒ�̕ύX
- �t�@�C���@���@���ݒ�@���@�l�b�g���[�N �Ńz�X�g�I�����[�l�b�g���[�N�̃^�u�ō쐬
- �z�X�g�I�����[�A�_�v�^�ɂ͌Œ�IP�A�h���X��ݒ肵�܂��B
- ���O�͓K���ɁAIP:192.168.56.1�A�T�u�l�b�g�}�X�N 255.255.255.0�A DHCP�͖����Ƃ���B
- ifcfg-"NIC"��DEVICE�p�����[�^�[��ǋL���܂��B

(��)

````
....
NAME="eth1"
DEVICE="eth1" #�ǉ�
````

- IP�A�h���X�̐ݒ��K�p���܂��B���܂����f����Ȃ��ꍇ�͍ċN�����Ă��������B

````
# ifdown eth1;ifup eth1
````

###���|�W�g���[�p�b�P�[�W�̃C���X�g�[��

Kilo�ȍ~�ACentOS 7�ł�CloudSIG�v���W�F�N�g��CentOS���[�U�[�����Ƀp�b�P�[�W��p�ӂ��Ă��܂��BRDO�v���W�F�N�g���p�ӂ���p�b�P�[�W�����p�ł��܂��B

RHEL7�����CentOS 7�ȊO��RHEL7�N���[���ł́ARDO�v���W�F�N�g���p�ӂ��郊�|�W�g���[�p�b�P�[�W���C���X�g�[�����邱�Ƃ�OpenStack�̃C���X�g�[�����\�ɂȂ�܂��B

���҂̈Ⴂ�Ƃ��ẮARDO�ł̃p�b�P�[�W�̕���CloudSIG�ł������X�V�����_�ł��B

---

- Newton ���C���X�g�[������ꍇ

�ȉ��̃R�}���h��RDO���|�W�g���[�̃p�b�P�[�W�𗘗p�ł��܂�(Fedora�̓T�|�[�g����܂���)�B

CentOS 7�ł�CloudSIG�v���W�F�N�g�������e�i���X���Ă��郊�|�W�g���[�𗘗p�ł��܂��B

````
# yum install -y centos-release-openstack-newton
````
---

###�V�X�e���A�b�v�f�[�g�ƃp�b�P�[�W�̃C���X�g�[��

````
# yum update -y && yum install -y openstack-packstack
````



###Packstack�ɂ��OpenStack�̃f�v���C

���|�W�g���[�p�b�P�[�W�̃C���X�g�[���I�������A���́uPackstack�ɂ��OpenStack�̃f�v���C�v�ɐi�݂܂��B


###Packstack�ɂ��OpenStack�̃f�v���C

���L�����s���邱�Ƃ�1��̃}�V����OpenStack�R���|�[�l���g���C���X�g�[���ł��܂��B

````
# setenforce 0
# packstack --dry-run --allinone --default-password=password \
--os-manila-install=n --os-aodh-install=y --os-gnocchi-install=y \
--os-sahara-install=n --os-heat-install=y --os-trove-install=n \
--os-ironic-install=n --nagios-install=n --os-neutron-lbaas-install=y \
--neutron-fwaas=y --os-heat-cfn-install=y --os-cinder-install=y \
--os-swift-install=y --os-ceilometer-install=y --os-client-install=y \
--provision-tempest=y 

 **** Installation completed successfully ******
````

��1...RDO�R�~���j�e�B�ɂ��Fedora�̃T�|�[�g��kilo�o�[�W�����܂łł��B

--provision-demo=y�Ƃ���ƁA�f���p�̃l�b�g���[�N�⃆�[�U�[�Ȃǂ�����AOpenStack�̈�ʂ�̑�����������s�ł��܂��B�������f���p�̃l�b�g���[�N�̓N���[�Y�h�Ȃ̂ŁA�O������A�N�Z�X�s�i��ł�����\�ɂ���ɂ́ANeutron�l�b�g���[�N�̍�蒼�����K�v�j�Ȃ̂Œ��ӁB

�G���[���o���A�C���X�g�[��������Ɋ�������΁uInstallation completed successfully�v�ƕ\������܂��B

- �Q�[�g�E�F�C���ݒ肳��Ă�����ő҂��󂯂��Ă��܂��̂�answer�t�@�C����IP�A�h���X�����������܂��B
````
# sed -i -e 's/10.0.2.15/192.168.56.101/g' packstack-answers-*.txt
````
- FWaaS�Ȃǂ̃T�[�r�X��L���ɂ��܂��B
````
# vi packstack-answers-*.txt �����s���ȉ��̃p�����[�^��ύX����B
 # Specify 'y' to install OpenStack Networking's Load-Balancing-
 # as-a-Service (LBaaS). ['y', 'n']
-CONFIG_LBAAS_INSTALL=n
+CONFIG_LBAAS_INSTALL=y

 # Specify 'y' to configure OpenStack Networking's Firewall-
 # as-a-Service (FWaaS). ['y', 'n']
-CONFIG_NEUTRON_FWAAS=n
+CONFIG_NEUTRON_FWAAS=y
�@�@�@�@�F
 # Password used by Orchestration service user to authenticate against
 # the database.
-CONFIG_HEAT_DB_PW=PW_PLACEHOLDER
+CONFIG_HEAT_DB_PW=password
�@�@�@�F
 # Password to use for the Orchestration service to authenticate with
 # the Identity service.
-CONFIG_HEAT_KS_PW=PW_PLACEHOLDER
+CONFIG_HEAT_KS_PW=password
�@�@�@�F
 # Password for the Identity domain administrative user for
 # Orchestration.
-CONFIG_HEAT_DOMAIN_PASSWORD=PW_PLACEHOLDER
+CONFIG_HEAT_DOMAIN_PASSWORD=password
�@�@�@�F
 # Specify 'y' to configure the OpenStack Integration Test Suite
 # (tempest) for testing. The test suite requires OpenStack Networking
 # to be installed. ['y', 'n']
-CONFIG_PROVISION_TEMPEST=n
+CONFIG_PROVISION_TEMPEST=y
�@�@�@�F
 # CIDR network address for the floating IP subnet.
 CONFIG_PROVISION_DEMO_FLOATRANGE=172.24.4.224/28
     :
-CONFIG_TEMPEST_HOST=
+CONFIG_TEMPEST_HOST=192.168.56.101
�@�@�@�@�F
 # Password to use for the Integration Test Suite provisioning user.
+CONFIG_PROVISION_TEMPEST_USER_PW=password
         :
 CONFIG_GNOCCHI_KS_PW=password
         :
 # IP address of the server on which to install MongoDB.
-CONFIG_MONGODB_HOST=192.168.56.110
+CONFIG_MONGODB_HOST=192.168.56.101
�@�@�@�@�@�@�@�F
 # IP address of the server on which to install the Redis server.
-CONFIG_REDIS_HOST=192.168.56.110
+CONFIG_REDIS_HOST=192.168.56.101

````
- �J�[�l���p�����[�^�̕ύX
````
# vi /etc/sysctl.conf
net.ipv4.ip_forward = 1
net.ipv4.conf.all.rp_filter = 0
net.ipv4.conf.default.rp_filter = 0
net.bridge.bridge-nf-call-arptables = 1
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
:wq
# sysctl -p
````
- SELINUX�̖�����
````
# vi /etc/selinux/config
SELINUX=permissive
:wq
````

- NetworkManager����network�T�[�r�X�ւ̐؂�ւ�

Packstack�̍\�z������ɐ؂�ւ����s���܂��B

- �Z�L�����e�B�֘A�̐ݒ�ύX

```` 
# systemctl disable firewalld && systemctl stop firewalld
# systemctl disable NetworkManager
# systemctl enable network
# systemctl stop NetworkManager;systemctl start network
````
- 


##Step 2: �u���E�U�[�ŃA�N�Z�X

�C���X�g�[����ɕ\�������Dashboard��URL�Ƀu���E�U�ŃA�N�Z�X���Ă݂܂��B���[�U�[=admin�A�p�X���[�h=password�Ń��O�C���ł��܂��B

/root�f�B���N�g���[���keystonerc_admin�Ƃ���RC�t�@�C��������Ă���A���̃t�@�C���ł��m�F�ł��܂��B

![Dashboard Login](./images/login.png)


##Step 3: �l�b�g���[�N�ݒ�̕ύX

���ɊO���ƒʐM�ł���悤�ɂ��邽�߂̐ݒ���s���܂��B�O���l�b�g���[�N�Ƃ̐ڑ���񋟂���m�[�h(�l�b�g���[�N�m�[�h�A1��\�����͂��̃}�V��)�ɉ��z�l�b�g���[�N�u���b�W�C���^�[�t�F�C�X�ł���br-ex��ݒ肵�܂��B

�{��ł̓z�X�g�ɓ��NIC������Aeth1���C���^�[�l�b�g���ɂȂ����Ă���ꍇ���Ƃ��܂��Beth1�ɃQ�[�g�E�F�C���ݒ肳��Ă��邱�Ƃ��m�F���܂��B

###��public�p�Ƃ��Ďg��NIC�̐ݒ�t�@�C�����C��
Packstack�R�}���h���s��Aeth1��br-ex�ɂȂ��悤�ɐݒ�����܂�(��BOOTPROTO�͐ݒ肵�Ȃ�)

eth1����IP�A�h���X�A�T�u�l�b�g�}�X�N�A�Q�[�g�E�F�C�̐ݒ���폜���Ď��̍��ڂ������L�q���Abr-ex�̕��ɐݒ���������݂܂��

````
# vi /etc/sysconfig/network-scripts/ifcfg-eth1
DEVICE=eth1
ONBOOT=yes
HWADDR=xx:xx:xx:xx:xx:xx # Your eth1's hwaddr
TYPE=OVSPort
DEVICETYPE=ovs
OVS_BRIDGE=br-ex
````

###���u���b�W�C���^�[�t�F�C�X�̍쐬
br-ex��eth1��IP�A�h���X��ݒ肵�܂��B

````
# vi /etc/sysconfig/network-scripts/ifcfg-br-ex
DEVICE=br-ex
ONBOOT=yes
DEVICETYPE=ovs
TYPE=OVSBridge
OVSBOOTPROTO=dhcp
OVSDHCPINTERFACES=eth1 #�C���^�[�l�b�g�ɐڑ�����Ă�����̃f�o�C�X
#OVSBOOTPROTO=none
#IPADDR=10.0.2.5
#NETMASK=255.255.255.0  # netmask
#GATEWAY=10.0.2.15      # gateway
#DNS1=8.8.8.8           # nameserver
#DNS2=10.0.1.1
````

###��SELinux�̐ݒ�
SELinux���L���̏�Ԃł����삷��悤�ɒ������܂��BAll-in-One�ŃC���X�g�[�������̂ŁA���̐ݒ��ǉ����܂��B

````
# setsebool -P httpd_use_openstack on
# setsebool -P neutron_can_network on
# setsebool -P glance_api_can_network on
# setsebool -P swift_can_network on
````

###���ċN��
�����܂łł����炢������z�X�g���ċN�����܂��B

````
# reboot
````

###������m�F
Packstack�C���X�g�[���[�ɂ��C���X�g�[�����ɃG���[�o�͂�����Ȃ���Ζ��͂���܂��񂪁A�O�̂���br-ex��Nova�ANeutron�G�[�W�F���g����������Ă��������F������Ă��邱�Ƃ��m�F���܂��傤�B

�܂��͍ċN�����br-ex�����������삵�A�O�̃l�b�g���[�N�ƂȂ����Ă��邱�Ƃ��m�F���܂��B

````
# ip a s br-ex | grep inet
    inet 10.0.2.15/24 brd 10.0.1.255 scope global dynamic br-ex
    inet6 fe80::a00:27ff:fe20:757d/64 scope link
# ping 8.8.8.8 -c 3 -I br-ex | grep "packet loss"
PING 8.8.8.8 (8.8.8.8) from 10.0.2.15 br-ex: 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=63 time=26.8 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=63 time=23.4 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=63 time=21.1 ms
--- 8.8.8.8 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2001ms
````

�p�P�b�g���X���Ȃ����Ƃ��m�F���܂��B

���ɁAOpenStack Nova�R���|�[�l���g�̃X�e�[�g��OK�ł��邱�Ƃ��m�F���܂��B

````
# source /root/keystonerc_admin
(admin���[�U�[�F�؏���ǂݍ���)
(keystone_admin)]# nova service-list
+----+------------------+--------------+----------+---------+-------+--
| Id | Binary           | Host         | Zone     | Status  | State | 
+----+------------------+--------------+----------+---------+-------+--
| 3  | nova-cert        | cent7-node1  | internal | enabled | up    | 
| 4  | nova-consoleauth | cent7-node1  | internal | enabled | up    | 
| 5  | nova-scheduler   | cent7-node1  | internal | enabled | up    | 
| 6  | nova-conductor   | cent7-node1  | internal | enabled | up    | 
| 7  | nova-compute     | cent7-node1  | nova     | enabled | up    | 
+----+------------------+--------------+----------+---------+-------+--
````

�Ō�ɁANeutron�̃G�[�W�F���g��OK�ł��邱�Ƃ��m�F���܂��B

````
(keystone_admin)]# neutron agent-list -c agent_type -c host -c alive
+--------------------+--------------+-------+
| agent_type         | host         | alive |
+--------------------+--------------+-------+
| Metering agent     | cent7-node1  | :-)   |
| Open vSwitch agent | cent7-node1  | :-)   |
| L3 agent           | cent7-node1  | :-)   |
| DHCP agent         | cent7-node1  | :-)   |
| Metadata agent     | cent7-node1  | :-)   |
+--------------------+--------------+-------+
````


##���̌�̐ݒ�ɂ���

����Neutron Network���쐬���܂��B�uNeutron �l�b�g���[�N�̐ݒ�v�̎菇�ɏ]���āANeutron�l�b�g���[�N���쐬���Ă��������B

#RDO Neutron �l�b�g���[�N�̐ݒ�

�ŏI�X�V��: 2016/05/24

##���̕����ɂ���
���̕����͍\�z����OpenStack����Neutron�l�b�g���[�N���쐬����菇�ƍ쐬�����l�b�g���[�N�̊m�F���@�̈���������Ă��܂��B

##Step 1: �l�b�g���[�N�̒ǉ�
br-ex��eth1�����蓖�ĂāA���z�}�V�����n�C�p�[�o�C�U�[�O���ƒʐM�ł���悤�ɂ���ׂ̌o�H���m�ۂ���Ă��邱�Ƃ��m�F���܂��B

````
# ovs-vsctl show
afef173b-1bd9-424a-a51e-957d4b1f1239
    Manager "ptcp:6640:127.0.0.1"
        is_connected: true
    Bridge br-tun
        Controller "tcp:127.0.0.1:6633"
            is_connected: true
        fail_mode: secure
        Port patch-int
            Interface patch-int
                type: patch
                options: {peer=patch-tun}
        Port br-tun
            Interface br-tun
                type: internal
    Bridge br-ex
        Port br-ex
            Interface br-ex
                type: internal
        Port "eth1"
            Interface "eth1"
        Port "qg-252fe9f0-b9"
            Interface "qg-252fe9f0-b9"
                type: internal
    Bridge br-int
        Controller "tcp:127.0.0.1:6633"
            is_connected: true
        fail_mode: secure
        Port patch-tun
            Interface patch-tun
                type: patch
                options: {peer=patch-int}
        Port "qr-89b43179-26"
            tag: 1
            Interface "qr-89b43179-26"
                type: internal
        Port br-int
            Interface br-int
                type: internal
        Port "qvo6d9ee7a7-c6"
            tag: 1
            Interface "qvo6d9ee7a7-c6"
        Port "tapfc9417f2-d5"
            tag: 1
            Interface "tapfc9417f2-d5"
                type: internal
    ovs_version: "2.5.0"
````

OS��n�[�h�E�F�A���̐ݒ肪�I�������AOpenStack�����p����l�b�g���[�N���쐬���Ă݂܂��傤�OpenStack�ɂ�����l�b�g���[�N�̐ݒ�͈ȉ��̏��ōs���܂��

1. ���[�^�[���쐬
2. �l�b�g���[�N���쐬
3. �l�b�g���[�N�T�u�l�b�g���쐬

OpenStack�̊��\�����R�}���h�Ŏ��s����ꍇ�́A/root/keystonerc_admin�t�@�C����source�R�}���h�œǂݍ���ł�����s���Ă��������

````
# source keystonerc_admin
````

����ł͏��ɍs���Ă����܂��傤�

###�����[�^�[�̍쐬
���[�^�[�̍쐬�͎��̂悤�ɃR�}���h�����s���܂��B

````
# neutron router-create router1
````

###���l�b�g���[�N�̍쐬
�l�b�g���[�N�̍쐬�͎��̂悤�ɃR�}���h�����s���܂��B

- �e�i���g���X�g���m�F

�o�^�ς݂̃e�i���g���m�F���āA�l�b�g���[�N�쐬���Ɏw�肷��e�i���g���������܂��openstack�R�}���h�Ńe�i���g�̈ꗗ������ɂ́A"openstack project list"�����s���܂��B

````
# openstack project list
+----------------------------------+----------+
| ID                               | Name     |
+----------------------------------+----------+
| 08114e2588b14949bcf20115e7ae8a41 | admin    |
| 30f2cc33d5f8405895e5558b077a86c6 | services |
+----------------------------------+----------+
````

- �p�u���b�N�l�b�g���[�N�̍쐬


�{��ł�tenant-id��admin�̂��̂��w�肵�܂��

````
# neutron net-create public --router:external --tenant-id 08114e2588b14949bcf20115e7ae8a41
````

net-create�R�}���h�̐擪�ɂ͂܂��l�b�g���[�N�����L�q���܂��
tenant-id�́ukeystone tenant-list�v�ŏo�͂���钆����u�e�i���g�v���w�肵�܂��B
router:external�͊O���l�b�g���[�N�Ƃ��Ďw�肷�邩���Ȃ�����ݒ肵�܂��
�v���C�x�[�g�l�b�g���[�N�����ꍇ�͎w�肷��K�v�͂���܂���

- �v���C�x�[�g�l�b�g���[�N�̍쐬
�{��ł�tenant-id��service�̂��̂��w�肵�܂��

````
# neutron net-create demo-net --shared --tenant-id 30f2cc33d5f8405895e5558b077a86c6
````

�l�b�g���[�N�����L����ɂ�--shared�I�v�V������t���Ď��s���܂��

###���l�b�g���[�N�T�u�l�b�g�̓o�^
public�l�b�g���[�N�ŗ��p����T�u�l�b�g���`���܂��

````
# neutron subnet-create --name public_subnet --enable_dhcp=False \
--allocation-pool=start=10.0.2.20,end=10.0.2.50 --gateway=10.0.2.15 public 10.0.2.0/24
Created a new subnet:
�i���j
````

�����public���l�b�g���[�N�ɃT�u�l�b�g�Ȃǂ�o�^���邱�Ƃ��ł��܂����
����demo-net�i�v���C�x�[�g�j���ɓo�^���Ă݂܂��B

````
# neutron subnet-create --name demo-net_subnet --enable_dhcp=True \
--allocation-pool=start=192.168.2.100,end=192.168.2.254 --gateway=192.168.2.1 \
--dns-nameserver 8.8.8.8 demo-net 192.168.2.0/24
Created a new subnet:
�i���j
````

###���Q�[�g�E�F�C�̐ݒ�
�쐬�������[�^�[(router1)�ƃp�u���b�N�l�b�g���[�N��ڑ����邽�߁A�u�Q�[�g�E�F�C�̐ݒ�v���s���܂��

````
# neutron router-gateway-set router1 public
Set gateway for router router1
````


###���O���l�b�g���[�N�Ɠ����l�b�g���[�N�̐ڑ�
�Ō�Ƀv���C�x�[�g�l�b�g���[�N�����蓖�Ă��C���X�^���X��Floating IP�����蓖�Ă�ꂽ�Ƃ��ɊO�ɏo����悤�ɂ��邽�߂Ɂu���[�^�[�ɃC���^�[�t�F�C�X�̒ǉ��v���s���܂��

````
# neutron router-interface-add router1 subnet=demo-net_subnet
Added interface xxxx-xxxx-xxxx-xxxx-xxxx to router router1.
````

router��neutron router-list�R�}���h�Ŋm�F�A�T�u�l�b�g��neutron subnet-list�R�}���h�Ŋm�F���邱�Ƃ��ł��܂��B


##Step 2: ���z���[�^�[�����ꂽ���m�F
neutron�R�}���h�Ńl�b�g���[�N���`�������Ƃŉ��z���[�^�[(qrouter)������Ă��邱�Ƃ��m�F���܂��Bneutron-l3-agent�T�[�r�X�����s���Ă���m�[�h�Ŋm�F���܂��B

````
# ip netns
qrouter-43cad32d-03b5-4059-b3a3-2a59586392f7
qdhcp-bee1692b-07ff-4baa-a971-85c5b307bc40

# ip netns exec `ip netns|grep qrouter` ip r
default via 10.0.2.15 dev qg-252fe9f0-b9
10.0.2.0/24 dev qg-252fe9f0-b9  proto kernel  scope link  src 10.0.2.20
192.168.2.0/24 dev qr-89b43179-26  proto kernel  scope link  src 192.168.2.1

# ip netns exec `ip netns|grep qrouter` \
 ping -c 3 -I qg-252fe9f0-b9 10.0.2.20


# ip netns exec `ip netns|grep qrouter` \
 ping -c 3 -I qg-fdf68416-44 192.168.1.1

# ip netns exec `ip netns|grep qrouter` \
 ping -c 3 -I qg-252fe9f0-b9 8.8.8.8 

# ip netns exec `ip netns|grep qrouter` \
 ping -c 3 -I qr-89b43179-26 192.168.2.1
````

ping�R�}���h���ʂ�΁A�O���l�b�g���[�N�Ɛڑ������܂������Ă���Ɣ��f�ł��܂��B


##�ԊO:�C������_�Ȃ� 
- CentOS 7��Apache�ݒ�̓f�t�H���g��KeepAlive Off�Ȃ̂ŁAOn�ɂ����ق���������������Ȃ��B

````
# vi /etc/httpd/conf/httpd.conf
...
KeepAlive On
...
# systemctl restart httpd
````

- IP�ݒ��MAC�A�h���X�̋L�q��Y�ꂸ��

NetworkManager�������Ă���ꍇ��HWADDR���Ȃ��Ă����܂������܂����Anetwork�T�[�r�X�ɐ؂�ւ������ɁA�^��������NIC�f�o�C�X�����ς���ĒʐM�ł��Ȃ��Ȃ�܂��B���ׂĂ�NIC�ݒ��HWADDR��t�����čċN������ƕ����ł��܂��B


````
...
HWADDR=xx:xx:xx:xx:xx:xx
````

- Unable to establish connection to http://xxx.xxx.xxx.xxx:5000/v2.0/tokens�Ƃ����G���[����������

Newton��Unable to establish connection to http://xxx.xxx.xxx.xxx:5000/v2.0/tokens�Ƃ������G���[���o���ꍇ��Keystone������ɓ����Ă��Ȃ��̂ŁAhttpd���ċN�����Ă݂Ă��������B���̌�Akeystone token-get�Ȃǂ̃R�}���h�ŉ������Ԃ��Ă���Ζ��Ȃ��ł��B

- openstack-status�̌��ʂ��Ȃ��Ȃ��o�Ă��Ȃ�

openstack-status�R�}���h���܂܂��openstack-utils�p�b�P�[�W�́A�X�V�����݂���肨���炭�X�V����Ă��Ȃ��̂Ŏg��Ȃ��ق��������ł��B�p�b�P�[�W�ˑ��̊֌W�œ����Ă��܂����ANewton�ł͂��܂������Ȃ��悤�ł��B

- Public�Q�[�g�E�F�C��Ping���΂��Ă�ping���ʂ�Ȃ�/�C���X�^���X�ŊO���l�b�g���[�N��ping���͂��Ȃ�

iptables-save�����z���[�^�[��Ŏ��s���Ă݂Ă��������B

````
# ip netns exec `ip netns|grep qrouter` iptables-save
````

- Oracle Virtualbox�ŉ��z�}�V��������Ă��̊���OpenStack�𓮂����ꍇ�AVirtualbox���z�}�V�����ڑ����Ă���l�b�g���[�N�̃|�[�g�O���[�v�Łu�����ʃ��[�h(Promiscuous Mode)�v���u����(Accept)�v�ɂ���K�v������܂��B
1. ���z�}�V����I�����A�_�b�V���{�[�h����u�l�b�g���[�N�v���N���b�N���܂��B
1. �Ǘ��p�C���^�t�F�[�X�ɂ��Ă���u�A�_�v�^2�v��I�����A�u���x�v���N���b�N���܂��B
1. �u�v���~�X�L���X���[�h(Promiscuous Mode)�v�Łu���ׂċ��v��I�����܂��B
1.�@�uOK�v�������Đݒ���I�����܂��B


- ESXi�ŉ��z�}�V��������Ă��̊���OpenStack�𓮂����ꍇ�AESXi���z�}�V�����ڑ����Ă���l�b�g���[�N�̃|�[�g�O���[�v�Łu�����ʃ��[�h(Promiscuous Mode)�v���u����(Accept)�v�ɂ���K�v������܂��B

1. ���z�}�V�����ڑ����Ă���u�|�[�g�O���[�v�v��I�����āu�ҏW�v�{�^�����N���b�N���܂��B
1. �u�Z�L�����e�B(Security)�v�^�u���N���b�N���܂��B
1. �u�����ʃ��[�h(Promiscuous Mode)�v�̉��̃`�F�b�N�}�[�N���I���ɂ��āu����(Accept)�v�ɏ㏑�����܂��B



