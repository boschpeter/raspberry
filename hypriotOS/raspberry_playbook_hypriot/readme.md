


![cloudlett case](https://www.amazon.com/CLOUDLET-CASE-Raspberry-Single-Computers/dp/B07D5MJ7PQ)
[learn linux How_to_build_your_own_Raspberry_Pi_Kubernetes_Cluster](https://wiki.learnlinux.tv/index.php/How_to_build_your_own_Raspberry_Pi_Kubernetes_Cluster)
# How to build your own Raspberry Pi Kubernetes Cluster using raspberry PI platform 


## quick reference 

flash sd card and re mount sd card  for boot and rootfs 
|playbook|reference| |
|--|--------|--------------|
|1|`lsblk`|show mount point sdcard writer boot and rootfs|
|1|`df -h`|show disk free  / mount point sdcard writer|
|1|`free -m  `|show cpu free |
|1|`cd /home/boscp08/Kubernetes/ansible/raspberry_playbook_k8s`|cd into|
|1|`ansible-playbook site01-sd.yml -i hosts -k -K`| playbook|


put sd cart into raspberry and boot


|kubernetes|wlan macadres|eth0 mac adres|wlan0 IP binding |Eth0 IP binding|
|-------|----------|-------------|------------ |------------|
|K8s-master-lltv|dc:a6:32:51:f4:50|dc:a6:32:51:f4:4f|192.168.2.20|192.168.2.30|
|K8s-worker-01|dc:a6:32:69:91:ff|dc:a6:32:69:91:fe|192.168.2.21|192.168.2.31|
|K8s-worker-02|dc:a6:32:18:aa:20|dc:a6:32:18:aa:1f|192.168.2.22|192.168.2.32|
|K8s-worker-03|dc:a6:32:43:df:ea|dc:a6:32:43:df:e9|192.168.2.23|192.168.2.33|

![eth0](https://github.com/boschpeter/Kubernetes/blob/master/pictures/47E1B69B-4216-446A-898C-74A0495D0E51.jpeg)

cleanuo known hosts
|playbook|reference| |
|--|--------|--------------|
|2|`ssh-keygen -R 192.168.2.20`|/home/boscp08/.ssh/known_hosts updated.|
|2|`ssh-keygen -R 192.168.2.21`|/home/boscp08/.ssh/known_hosts updated.|
|2|`ssh-keygen -R 192.168.2.22`|/home/boscp08/.ssh/known_hosts updated.|
|2|`ssh-keygen -R 192.168.2.23`|/home/boscp08/.ssh/known_hosts updated.|
|2|`ssh-keygen -R 192.168.2.30`|/home/boscp08/.ssh/known_hosts updated.|
|2|`ssh-keygen -R 192.168.2.31`|/home/boscp08/.ssh/known_hosts updated.|
|2|`ssh-keygen -R 192.168.2.32`|/home/boscp08/.ssh/known_hosts updated.|
|2|`ssh-keygen -R 192.168.2.33`|/home/boscp08/.ssh/known_hosts updated.|

expose new knownhost so ansible can automatically login
|2|`ssh-copy-id -i ~/.ssh/id_rsa.pub pi@192.168.2.30`|/home/boscp08/.ssh/known_hosts updated.|

````
ECDSA key fingerprint is SHA256:UM5mQXwpU1XeiSqyCO7PqMynzcVzTG1PqTzo2Crlt/U.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
pi@192.168.2.30's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'pi@192.168.2.30'"
and check to make sure that only the key(s) you wanted were added.

````

# ssh-copy-id -i ~/.ssh/id_rsa.pub pi@192.168.2.x

````
boscp08@boscp08-dingo:~/.../raspberry_playbook_k8s$ ssh-copy-id -i ~/.ssh/id_rsa.pub pi@192.168.2.30
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/boscp08/.ssh/id_rsa.pub"
The authenticity of host '192.168.2.30 (192.168.2.30)' can't be established.
ECDSA key fingerprint is SHA256:UM5mQXwpU1XeiSqyCO7PqMynzcVzTG1PqTzo2Crlt/U.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
pi@192.168.2.30's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'pi@192.168.2.30'"
and check to make sure that only the key(s) you wanted were added.

boscp08@boscp08-dingo:~/.../raspberry_playbook_k8s$ ssh  pi@192.168.2.30
Linux k8s-head 4.19.97-v7l+ #1294 SMP Thu Jan 30 13:21:14 GMT 2020 armv7l

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.

SSH is enabled and the default password for the 'pi' user has not been changed.
This is a security risk - please login as the 'pi' user and type 'passwd' to set a new password.


pi@k8s-head:~ $ free
              total        used        free      shared  buff/cache   available
Mem:        3999756       93592     3625992       24992      280172     3757748
Swap:        102396           0      102396


Wi-Fi is currently blocked by rfkill.
Use raspi-config to set the country before use.


````
swap is on and Wi-Fi is blocked next step = Raspiconfig 

|playbook|reference| |
|--|--------|-----------|
|2|`ssh-keygen -R 192.168.2.30`| cleanup |
|2|`ssh-keygen -R 192.168.2.20`| cleanup
|2|`ssh-copy-id -i ~/.ssh/id_rsa.pub pi@192.168.2.30` | #pwd=raspberry head|
|2|`cd /home/boscp08/Kubernetes/ansible/raspberry_playbook_k8s`|cd into|
|2|`ansible-playbook site02-raspi-config.yml -i hosts -k -K`|playbook|
|2|`free`| show swap  should be 0|
|2|`ifconfig`| shows eth0 and wlan0|


````
pi@k8s-head:~ $ ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.2.30  netmask 255.255.255.0  broadcast 192.168.2.255
        inet6 fe80::6fd9:9108:3f66:3e28  prefixlen 64  scopeid 0x20<link>
        inet6 2a02:a449:3b40:1:433f:4552:2cb:4a27  prefixlen 64  scopeid 0x0<global>
        ether dc:a6:32:18:aa:1f  txqueuelen 1000  (Ethernet)
        RX packets 1147  bytes 295185 (288.2 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 258  bytes 31505 (30.7 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.2.20  netmask 255.255.255.0  broadcast 192.168.2.255
        inet6 fe80::67c0:6cd8:45cb:5c65  prefixlen 64  scopeid 0x20<link>
        inet6 2a02:a449:3b40:1:6852:a2bf:d520:4eb7  prefixlen 64  scopeid 0x0<global>
        ether dc:a6:32:18:aa:20  txqueuelen 1000  (Ethernet)
        RX packets 376  bytes 70955 (69.2 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 50  bytes 7009 (6.8 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

pi@k8s-head:~ $ free
              total        used        free      shared  buff/cache   available
Mem:        3946976       94376     3762316        8636       90284     3729800
Swap:             0           0           0
pi@k8s-head:~ $ uptime
 16:28:51 up 6 min,  2 users,  load average: 0.00, 0.00, 0.00

````


now you are good to go and install the neccery software on cluster (head and workers)  

install docker, kubeadm kubelet and kubectl
|playbook|reference| |
|--|--------|----------------|
|3|`cd /home/boscp08/Kubernetes/ansible/raspberry_playbook_k8s`|cd into |
|3|`ansible-playbook site03-install-docker-kubectl.yml -i hosts -k -K`| playbook|
|3|`docker --version`| show version|
|3|`docker run hello-world`|check if docker is working|
|3|`groups`| pi groups should show docker. |


|cluster init  |reference on master only | |
|--|--------|----------------|
|master|`sudo kubeadm init --pod-network-cidr=192.168.0.0/16`|initializaion |
|worker| `sudo kubeadm join 192.168.2.30:6443 --token 19knyv.82g80mbknewctvuk  --discovery-token-ca-cert-hash sha256:9d6b8272b1a3ca96eb6f435f78e941df4db38436c0ee5a995e031aa00e2f7acf `|join command on worker|
|master|`mkdir -p $HOME/.kube`|
|master|`sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config`|
|master|`sudo chown $(id -u):$(id -g) $HOME/.kube/config` |
|master|`kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml`|network|
|master|`kubectl get nodes` |k8-head   Ready    master   10m   v1.18.0|
|master|`kubectl get namespaces` |k8-head   Ready    master   10m   v1.18.0|




## 20200402

![stay the fuck at home](https://github.com/boschpeter/Kubernetes/blob/master/pictures/stay_calm_move_fast.png)

[How to build your own Raspberry Pi Kubernetes Cluster](https://www.youtube.com/watch?v=B2wAJ5FLOYw&feature=youtu.be)

## etcher process 2minutes 2020-02-13-raspbian-buster-lite.zip -> 32gb SD card
![select etcher image](https://github.com/boschpeter/Kubernetes/blob/master/pictures/balena_etcher_select_image_zip.png)

![30procent](https://github.com/boschpeter/Kubernetes/blob/master/pictures/balena_etcher_30procent_flash.png)

![finish](https://github.com/boschpeter/Kubernetes/blob/master/pictures/balena_etcher_flash_complete2minutes.png)


## sd-card configuration after flash before initial-run on the PI4
remount after flash shows `boot` and `rootfs` mount points
|reference|
|--------|
|`lsblk`|
|`df -h`|
|`cd /home/boscp08/Kubernetes/ansible/raspberry_playbook_k8s`|
|`ansible-playbook site-sd.yml -i hosts -k -K`|


### ansible-galaxy init --init-path roles/ k8s-sd-config

````
boscp08@boscp08-dingo:~/.../raspberry_playbook_k8s$ cat site-sd.yml 
---
# This is a sitewide playbook run with ansible-playbook site-sd.yml -i hosts -k -K

  - name: k8s-sd-config
    hosts: localhost 
    vars_prompt:
     - name: K8S_HOSTNAME
       prompt: "What is your K8S_HOSTNAME  k8s-worker-01  ?"
       private: no
    remote_user: boscp08
    become: True
    gather_facts: True
    pre_tasks:
      - shell: echo 'I":" k8s-sd-config'
    roles:
        - k8s-sd-config
    post_tasks:
      - shell: echo 'I":" k8s-sd-config and put the sd card into your pi and boot..'

````

````
boscp08@boscp08-dingo:~/.../raspberry_playbook_k8s$ ansible-playbook site-sd.yml -i hosts -k -K
SSH password: 
BECOME password[defaults to SSH password]: 
What is your K8S_HOSTNAME  k8s-worker-01  ?: k8s-master   

PLAY [k8s-sd-config] *****************************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************
ok: [127.0.0.1]

TASK [shell] *************************************************************************************************************************************************
changed: [127.0.0.1]

TASK [k8s-sd-config : Ansible delete file /media/boscp08/rootfs/etc/hostname] ********************************************************************************
changed: [127.0.0.1]

TASK [k8s-sd-config : Ansible delete file /media/boscp08/rootfs/etc/hosts] ***********************************************************************************
changed: [127.0.0.1]

TASK [k8s-sd-config : Ansible touch  /media/boscp08/boot/ssh   enable ssh] ***********************************************************************************
changed: [127.0.0.1]

TASK [k8s-sd-config : create file /media/boscp08/rootfs/etc/hostsname] ***************************************************************************************
changed: [127.0.0.1]

TASK [k8s-sd-config : create file /media/boscp08/rootfs/etc/hosts] *******************************************************************************************
changed: [127.0.0.1]

TASK [k8s-sd-config : create file /media/boscp08/rootfs/etc/wpa_supplicant/wpa_supplicant.conf] **************************************************************
changed: [127.0.0.1]

TASK [shell] *************************************************************************************************************************************************
changed: [127.0.0.1]

PLAY RECAP ***************************************************************************************************************************************************
127.0.0.1                  : ok=9    changed=8    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

boscp08@boscp08-dingo:~/.../raspberry_playbook_k8s$ pwd
/home/boscp08/Kubernetes/ansible/raspberry_playbook_k8s

boscp08@boscp08-dingo:~/.../raspberry_playbook_k8s$ cat /media/boscp08/rootfs/etc/hosts
127.0.0.1      localhost
::1	    	localhost ip6-localhost ip6-loopback
ff02::1		ip6-allnodes
ff02::2		ip6-allrouters
127.0.1.1      k8s-master 

````

## put SD card into PI and start with ETH0 connected



|reference|
|--------|
|`ssh-keygen -R 192.168.2.30`|
|`ssh-keygen -R 192.168.2.20`|
|`ssh-copy-id -i ~/.ssh/id_rsa.pub pi@192.168.2.30`  #pwd=raspberry head|
|`ansible-playbook site-raspi-config.yml -i hosts -k -K`|

````
---
# This is a sitewide playbook run with ansible-playbook site-raspi-config.yml -i hosts -k -K
# filename: site.yml
# ansible -i hosts raspifarm_workers -m ping
# ansible -i hosts rapifarm_workers -m setup | grep user
# ansible-galaxy init --init-path roles/ upd
# Role upd was created successfully
 # This playbook deploys the whole raspberry PI kubernetes in this site.


###   Configuration raspberry aka raspi-config without screen that is
#    ssh-keygen -R 192.168.2.30   
#    ssh-copy-id -i ~/.ssh/id_rsa.pub pi@192.168.2.30  #pwd=raspberry head
#    Number of key(s) added: 1
#    Now try logging into the machine, with:   "ssh 'pi@192.168.2.30'"
#    and check to make sure that only the key(s) you wanted were added.
#    ansible-playbook site-raspi-config.yml -i hosts -k -K
    
  - name: k8s-raspi-config
    hosts: raspifarm_eth0_master
    handlers:
      - name: wait-for-reboot
        wait_for_connection:
           delay: "5"
           timeout: "300"
    #vars_prompt:
    # - name: NEWPASSWORD
    #   prompt: "What is your pi NEWPASSWORD Nw..  ?"
    #   private: no
    # - name: PASSPHRASE
    #   prompt: "What is your WIFI PASSPHRASE F0..  ?"
    #   private: no      
    remote_user: pi
    become: true
    pre_tasks:
     - shell: echo 'I":" k8s-raspi-config'
    roles:
         - k8s-raspi-config
    post_tasks:
       - shell: echo 'I":" k8s-raspi-config ..'


````


````
boscp08@boscp08-dingo:~/.../raspberry_playbook_k8s$ ssh-keygen -R 192.168.2.30
# Host 192.168.2.30 found: line 45
/home/boscp08/.ssh/known_hosts updated.
Original contents retained as /home/boscp08/.ssh/known_hosts.old

````

````
boscp08@boscp08-dingo:~/.../raspberry_playbook_k8s$ ssh-copy-id -i ~/.ssh/id_rsa.pub pi@192.168.2.30
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/boscp08/.ssh/id_rsa.pub"
The authenticity of host '192.168.2.30 (192.168.2.30)' can't be established.
ECDSA key fingerprint is SHA256:NTGqdOeUo2kre2sZSNKvk2hx1uBHDVJlXcCkHKz6gR8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
pi@192.168.2.30's password: raspberry

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'pi@192.168.2.30'"
and check to make sure that only the key(s) you wanted were added.
````

````
boscp08@boscp08-dingo:~/.../raspberry_playbook_k8s$ ansible-playbook site-raspi-config.yml -i hosts -k -K
SSH password: 
BECOME password[defaults to SSH password]: 
[WARNING]: While constructing a mapping from /home/boscp08/Kubernetes/ansible/raspberry_playbook_k8s/roles/k8s-raspi-config/vars/main.yml, line 4, column 1,
found a duplicate dict key (UPDATE). Using last defined value only.

PLAY [k8s-raspi-config] **************************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************
[WARNING]: Platform linux on host 192.168.2.30 is using the discovered Python interpreter at /usr/bin/python, but future installation of another Python
interpreter could change this. See https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.
ok: [192.168.2.30]

TASK [shell] *************************************************************************************************************************************************
changed: [192.168.2.30]

TASK [k8s-raspi-config : Update raspi-config itself] *********************************************************************************************************
ok: [192.168.2.30]

TASK [k8s-raspi-config : Get Raspberry Pi type] **************************************************************************************************************
ok: [192.168.2.30]

TASK [k8s-raspi-config : Show pi version] ********************************************************************************************************************
ok: [192.168.2.30] => {
    "msg": "Pi version: 0"
}

TASK [k8s-raspi-config : Change user password] ***************************************************************************************************************
changed: [192.168.2.30]

TASK [k8s-raspi-config : Get hostname] ***********************************************************************************************************************
ok: [192.168.2.30]

TASK [k8s-raspi-config : Print current hostname] *************************************************************************************************************
ok: [192.168.2.30] => {
    "msg": "Current hostname: k8-head"
}

TASK [k8s-raspi-config : Set WiFi credentials] ***************************************************************************************************************
changed: [192.168.2.30]

TASK [k8s-raspi-config : Get network names status] ***********************************************************************************************************
ok: [192.168.2.30]

TASK [k8s-raspi-config : Print current network names status] *************************************************************************************************
ok: [192.168.2.30] => {
    "msg": "Current network names status: 1"
}

TASK [k8s-raspi-config : Enable network names] ***************************************************************************************************************
skipping: [192.168.2.30]

TASK [k8s-raspi-config : Disable network names] **************************************************************************************************************
skipping: [192.168.2.30]

TASK [k8s-raspi-config : Get boot CLI] ***********************************************************************************************************************
ok: [192.168.2.30]

TASK [k8s-raspi-config : Print boot CLI] *********************************************************************************************************************
ok: [192.168.2.30] => {
    "msg": "Boot CLI is: 0"
}

TASK [k8s-raspi-config : Get boot autologin] *****************************************************************************************************************
ok: [192.168.2.30]

TASK [k8s-raspi-config : Print boot autologin status] ********************************************************************************************************
ok: [192.168.2.30] => {
    "msg": "Boot autologin is: 0"
}

TASK [k8s-raspi-config : Set Boot behaviour] *****************************************************************************************************************
changed: [192.168.2.30]

TASK [k8s-raspi-config : Get boot wait for network status] ***************************************************************************************************
ok: [192.168.2.30]

TASK [k8s-raspi-config : Print boot wait for network status] *************************************************************************************************
ok: [192.168.2.30] => {
    "msg": "Boot wait is: 0"
}

TASK [k8s-raspi-config : Get splash status] ******************************************************************************************************************
ok: [192.168.2.30]

TASK [k8s-raspi-config : Print boot splash status] ***********************************************************************************************************
ok: [192.168.2.30] => {
    "msg": "Boot splash status is: 1"
}

TASK [k8s-raspi-config : Change locale] **********************************************************************************************************************
changed: [192.168.2.30]

TASK [k8s-raspi-config : Change timezone] ********************************************************************************************************************
changed: [192.168.2.30]

TASK [k8s-raspi-config : Change keyboard layout] *************************************************************************************************************
changed: [192.168.2.30]

TASK [k8s-raspi-config : Get WiFi country] *******************************************************************************************************************
ok: [192.168.2.30]

TASK [k8s-raspi-config : Print current WiFi country] *********************************************************************************************************
ok: [192.168.2.30] => {
    "msg": "Wifi country is: OK"
}

TASK [k8s-raspi-config : Check if SSH is enabled or not] *****************************************************************************************************
ok: [192.168.2.30]

TASK [k8s-raspi-config : Print SSH status] *******************************************************************************************************************
ok: [192.168.2.30] => {
    "msg": "SSH status is: 0"
}

TASK [k8s-raspi-config : Enable SSH] *************************************************************************************************************************
skipping: [192.168.2.30]

TASK [k8s-raspi-config : Disable SSH] ************************************************************************************************************************
skipping: [192.168.2.30]

TASK [k8s-raspi-config : Check if FS is expandable] **********************************************************************************************************
ok: [192.168.2.30]

TASK [k8s-raspi-config : Print if FS is expandable or not] ***************************************************************************************************
ok: [192.168.2.30] => {
    "msg": "Filesystem is expandable! [0]"
}

TASK [k8s-raspi-config : Expand Filesystem] ******************************************************************************************************************
changed: [192.168.2.30]

TASK [k8s-raspi-config : Get Overscan status] ****************************************************************************************************************
ok: [192.168.2.30]

TASK [k8s-raspi-config : Print Overscan] *********************************************************************************************************************
ok: [192.168.2.30] => {
    "msg": "Boot overscan is: 0"
}

TASK [k8s-raspi-config : Enable Overscan] ********************************************************************************************************************
skipping: [192.168.2.30]

TASK [k8s-raspi-config : Get current GPU memory split (1/4)] *************************************************************************************************
ok: [192.168.2.30]

TASK [k8s-raspi-config : Get current GPU memory split 256 (2/4)] *********************************************************************************************
ok: [192.168.2.30]

TASK [k8s-raspi-config : Get current GPU memory split 512 (3/4)] *********************************************************************************************
ok: [192.168.2.30]

TASK [k8s-raspi-config : Get current GPU memory split 1024 (4/4)] ********************************************************************************************
ok: [192.168.2.30]

TASK [k8s-raspi-config : Print current GPU memory split] *****************************************************************************************************
ok: [192.168.2.30] => {
    "msg": "Current GPU memory split: 128 - 0 - 0 - 0"
}

TASK [k8s-raspi-config : Set GPU memory split] ***************************************************************************************************************
changed: [192.168.2.30]

TASK [k8s-raspi-config : Set audio out] **********************************************************************************************************************
changed: [192.168.2.30]

TASK [k8s-raspi-config : Get HDMI group] *********************************************************************************************************************
ok: [192.168.2.30]

TASK [k8s-raspi-config : Get HDMI mode] **********************************************************************************************************************
ok: [192.168.2.30]

TASK [k8s-raspi-config : Print HDMI group & mode] ************************************************************************************************************
ok: [192.168.2.30] => {
    "msg": "HDMI group and mode is: 0 - 0"
}

TASK [k8s-raspi-config : Get pixel doubling] *****************************************************************************************************************
ok: [192.168.2.30]

TASK [k8s-raspi-config : Print pixel doubling status] ********************************************************************************************************
ok: [192.168.2.30] => {
    "msg": "Pixel Doubling is: 1"
}

TASK [k8s-raspi-config : Enable pixel doubling] **************************************************************************************************************
skipping: [192.168.2.30]

TASK [k8s-raspi-config : Disable pixel doubling] *************************************************************************************************************
skipping: [192.168.2.30]

TASK [k8s-raspi-config : Edit /boot/cmndline.txt and add] ****************************************************************************************************
changed: [192.168.2.30]

TASK [k8s-raspi-config : Disable SWAP since kubernetes can't work with swap enabled (1/2) check with free -m] ************************************************
changed: [192.168.2.30]

TASK [k8s-raspi-config : Update and upgrade apt packages] ****************************************************************************************************
[WARNING]: The value True (type bool) in a string field was converted to u'True' (type string). If this does not look like what you expect, quote the entire
value to ensure it does not change.
changed: [192.168.2.30]

TASK [k8s-raspi-config : Reboot] *****************************************************************************************************************************
changed: [192.168.2.30]

TASK [k8s-raspi-config : Reboot] *****************************************************************************************************************************
changed: [192.168.2.30]

RUNNING HANDLER [wait-for-reboot] ****************************************************************************************************************************
ok: [192.168.2.30]

TASK [shell] *************************************************************************************************************************************************
changed: [192.168.2.30]

PLAY RECAP ***************************************************************************************************************************************************
192.168.2.30               : ok=51   changed=16   unreachable=0    failed=0    skipped=7    rescued=0    ignored=0   

````


````
pi@k8-head:~ $ uptime
 16:35:48 up 2 min,  2 users,  load average: 0.03, 0.03, 0.00
pi@k8-head:~ $ cat /etc/hosts
127.0.0.1      localhost
::1	    	localhost ip6-localhost ip6-loopback
ff02::1		ip6-allnodes
ff02::2		ip6-allrouters
127.0.1.1      k8-head 
pi@k8-head:~ $ cat /etc/wpa_supplicant/wpa_supplicant.conf 
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=NL

network={
	ssid="ThomsomFB0530"
	psk="SSIDPASSPRHASE"
}
pi@k8-head:~ $ ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.2.30  netmask 255.255.255.0  broadcast 192.168.2.255
        inet6 fe80::6cd8:c40c:613d:86c4  prefixlen 64  scopeid 0x20<link>
        inet6 2a02:a449:3b40:1:5d31:9021:ac47:9522  prefixlen 64  scopeid 0x0<global>
        ether dc:a6:32:51:f4:4f  txqueuelen 1000  (Ethernet)
        RX packets 509  bytes 210767 (205.8 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 316  bytes 39200 (38.2 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.2.20  netmask 255.255.255.0  broadcast 192.168.2.255
        inet6 2a02:a449:3b40:1:ee43:2dff:bfdd:c784  prefixlen 64  scopeid 0x0<global>
        inet6 fe80::8d6f:1d5e:5bda:e3e1  prefixlen 64  scopeid 0x20<link>
        ether dc:a6:32:51:f4:50  txqueuelen 1000  (Ethernet)
        RX packets 51  bytes 6452 (6.3 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 31  bytes 4679 (4.5 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

pi@k8-head:~ $ free
              total        used        free      shared  buff/cache   available
Mem:        1933216       62400     1780084        8636       90732     1785224
Swap:             0           0           0



````
    

![host alive](https://github.com/boschpeter/Kubernetes/blob/master/pictures/host_alive.png)




## install docker and kubernetes


|reference|
|--------|
|`cd /home/boscp08/Kubernetes/ansible/raspberry_playbook_k8s`|
|`ansible-playbook site03-install-docker-kubectl.yml -i hosts -k -K`|
|docker --version|
|docker run hello-world|
|groups|

````
boscp08@boscp08-dingo:~/.../raspberry_playbook_k8s$ ansible-playbook site03-install-docker-kubectl.yml -i hosts -k -K
SSH password: 
BECOME password[defaults to SSH password]: 
[WARNING]:  * Failed to parse /home/boscp08/Kubernetes/ansible/raspberry_playbook_k8s/hosts with yaml plugin: Syntax Error while loading YAML.   did not find
expected <document start>  The error appears to be in '/home/boscp08/Kubernetes/ansible/raspberry_playbook_k8s/hosts': line 23, column 1, but may be
elsewhere in the file depending on the exact syntax problem.  The offending line appears to be:  [localhost] 127.0.0.1 ansible_user=boscp08 ^ here
[WARNING]:  * Failed to parse /home/boscp08/Kubernetes/ansible/raspberry_playbook_k8s/hosts with ini plugin:
/home/boscp08/Kubernetes/ansible/raspberry_playbook_k8s/hosts:55: Section [cluster:vars] not valid for undefined group: cluster
[WARNING]: Unable to parse /home/boscp08/Kubernetes/ansible/raspberry_playbook_k8s/hosts as an inventory source
[WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'

PLAY [k8s-install-docker-kubectl] ****************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************
[WARNING]: Platform linux on host 192.168.2.30 is using the discovered Python interpreter at /usr/bin/python, but future installation of another Python
interpreter could change this. See https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.
ok: [192.168.2.30]

TASK [shell] *************************************************************************************************************************************************
changed: [192.168.2.30]

TASK [k8s-install-docker-kubectl : install docker] ***********************************************************************************************************
[WARNING]: Consider using the get_url or uri module rather than running 'curl'.  If you need to use command because get_url or uri is insufficient you can
add 'warn: false' to this command task or set 'command_warnings=False' in ansible.cfg to get rid of this message.
changed: [192.168.2.30]

TASK [k8s-install-docker-kubectl : usermod -aG docker pi] ****************************************************************************************************
changed: [192.168.2.30]

TASK [k8s-install-docker-kubectl : create file /etc/docker/daemon.json] **************************************************************************************
ok: [192.168.2.30]

TASK [k8s-install-docker-kubectl : Edit /etc/sysctl.conf enable port forwarding] *****************************************************************************
ok: [192.168.2.30]

TASK [k8s-install-docker-kubectl : create file /etc/apt/sources.list.d/kubernetes.list] **********************************************************************
changed: [192.168.2.30]

TASK [k8s-install-docker-kubectl : curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -] ***************************************
changed: [192.168.2.30]

TASK [k8s-install-docker-kubectl : Update apt-get repo and cache] ********************************************************************************************
ok: [192.168.2.30]

TASK [k8s-install-docker-kubectl : Update apt-get repo and cache] ********************************************************************************************
ok: [192.168.2.30]

TASK [k8s-install-docker-kubectl : Install Kubernetes binaries] **********************************************************************************************
changed: [192.168.2.30]

TASK [k8s-install-docker-kubectl : Reboot] *******************************************************************************************************************
changed: [192.168.2.30]

RUNNING HANDLER [wait-for-reboot] ****************************************************************************************************************************
ok: [192.168.2.30]

TASK [shell] *************************************************************************************************************************************************
changed: [192.168.2.30]

PLAY RECAP ***************************************************************************************************************************************************
192.168.2.30               : ok=14   changed=8    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   


````


````
pi@k8-head:~ $ docker --version
Docker version 19.03.8, build afacb8b
pi@k8-head:~ $ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
4ee5c797bcd7: Pull complete 
Digest: sha256:f9dfddf63636d84ef479d645ab5885156ae030f611a56f3a7ac7f2fdd86d7e4e
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (arm32v7)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

pi@k8-head:~ $ groups
pi adm dialout cdrom sudo audio video plugdev games users input netdev docker gpio i2c spi





