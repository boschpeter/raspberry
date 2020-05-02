ansible playbooks
# ansible raspi-condig

Automation is key to reduce the manual effort associated with provisioning, scaling, cloning and managing database workloads in the #cloud. Discover the processes and techniques required to automate the #database lifecycle on the Autonomous Shared deployment model.



# How to build your own Raspberry Pi Kubernetes Cluster

I'll show off the process of doing this on the Raspberry Pi platform, 
I'll show you how to set up your very own Kubernetes cluster. But not just any boring old cluster, 
I'll show off the process of doing this on the Raspberry Pi platform, 
which is a cheaper and more energy-efficient foundation for your clustering needs.

https://youtu.be/yLzO88h40uw DrupalCon Seattle 2019: Everything I know about Kubernetes I learned from raspberry pi


## wiki learnlinux.tx

[learn linux tv](https://wiki.learnlinux.tv/index.php/How_to_build_your_own_Raspberry_Pi_Kubernetes_Cluster)

After setting up the hosts and hostname files you could have installed ClusterSSH or 
Ansible on the Pi's so you could run the same commands on the pi's at once, 
saving tonnes of doubling and tripling up commands ;) Pods created do not have access to the Internet.  

Kubernetes is a very powerful platform to scale your applications, and the Raspberry Pi is a low-cost computer with excellent power efficiencty you can use to run tasks without breaking the bank. If you put the two together, you can have a low-cost and scalable platform for Kubernetes on the Raspberry Pi. In this video, we take a look at how to create a Pi-powered Kubernetes cluster. 

## youtube
[raspberrypi4](https://www.youtube.com/watch?v=B2wAJ5FLOYw&feature=youtu.be)


## create rol upd
````ansible-galaxy init --init-path roles/ upd````



# k8s-head playbook

|ansible playbook head|
|----------------------------|
|`ansible-playbook site01-sd-card-config.yml -i hosts -k -K`|
|`ssh-keygen -R 192.168.2.20`|
|`ssh-keygen -R 192.168.2.30`|
|`ssh-copy-id -i ~/.ssh/id_rsa.pub pi@192.168.2.30`  pwd=raspberry|
|`ansible-playbook site02-raspi-config.yml -i hosts -k -K`|
|`site03-install-docker-kubectl.yml`|

````
sudo kubeadm join 192.168.2.20:6443 --token cqjiyd.ztesuhchn7fsncyb \
    --discovery-token-ca-cert-hash sha256:9d9730535dc43de3593442085bf219d514a5c1421b10241142060a1acf206bf3
````

## kubeadm token create`

https://discuss.kubernetes.io/t/not-able-to-join-node-to-master/7123/6

pi@k8s-head:~ $  ````kubeadm token create````

````
W0419 18:27:07.792331   13127 configset.go:202] WARNING: kubeadm cannot validate component configs for API groups [kubelet.config.k8s.io kubeproxy.config.k8s.io]
1rwf1n.g3mv6gf37muaa6v4
````

## sudo kubeadm join 192.168.2.20:6443 --token 1rwf1n.g3mv6gf37muaa6v4    --discovery-token-ca-cert-hash sha256:9d9730535dc43de3593442085bf219d514a5c1421b10241142060a1acf206bf3

````
sudo kubeadm join 192.168.2.20:6443 --token 1rwf1n.g3mv6gf37muaa6v4 \
    --discovery-token-ca-cert-hash sha256:9d9730535dc43de3593442085bf219d514a5c1421b10241142060a1acf206bf3

````

````
pi@k8s-head:~ $ kubectl -o wide get nodes
NAME            STATUS     ROLES    AGE    VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE                         KERNEL-VERSION     CONTAINER-RUNTIME
hp              Ready      <none>   100s   v1.18.1   192.168.2.5    <none>        Ubuntu 19.10                     5.3.0-46-generic   docker://19.3.8
k8s-head        Ready      master   16d    v1.18.0   192.168.2.30   <none>        Raspbian GNU/Linux 10 (buster)   4.19.97-v7l+       docker://19.3.8
k8s-worker-01   NotReady   <none>   16d    v1.18.0   192.168.2.21   <none>        Raspbian GNU/Linux 10 (buster)   4.19.97-v7l+       docker://19.3.8
k8s-worker-02   Ready      <none>   16d    v1.18.2   192.168.2.11   <none>        Raspbian GNU/Linux 10 (buster)   4.19.97-v7l+       docker://19.3.8
k8s-worker-03   Ready      <none>   16d    v1.18.2   192.168.2.33   <none>        Raspbian GNU/Linux 10 (buster)   4.19.97-v7l+       docker://19.3.8
````
