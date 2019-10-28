
Installing kubectl,kubeadm and kubelet:

----------------------------------------

Prerequsite check

update-alternatives --set iptables /usr/sbin/iptables-legacy

cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF

# Set SELinux in permissive mode (effectively disabling it)
setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

Run the below command

swapoff -a

yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes

systemctl enable --now kubelet

Bootstrapping Kubernetes with kubeadm:
--------------------------------------
kubeadm init --apiserver-advertise-address=172.31.17.208 --pod-network-cidr=10.244.0.0/16 

Similar O/p:kubeadm join 172.31.17.208:6443 --token 21dktk.41al9g2lgs6ctsx4 \
    --discovery-token-ca-cert-hash sha256:9b57809dd029664f5b040058b485a3af53dcf7438a78623c25ca26e8111ea1e3

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

Installing CNI(container n/w interface) plugins:
----------------	popular plugins flannel,calico,weavenet
sysctl net.bridge.bridge-nf-call-iptables=1

kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/2140ac876ef134e0ed5af15c65e414cf26827915/Documentation/kube-flannel.yml

Check CoreDNS pod is Running 
kubectl get pods --all-namespaces

Installing Helm:
----------------
curl -LO https://git.io/get_helm.sh
chmod 700 get_helm.sh
./get_helm.sh

./helm init


OUTPUT:
Script started on Sun 27 Oct 2019 03:44:37 PM UTC
]0;root@praghu1c:/home/cloud_user [?1034h[root@praghu1c cloud_user]# update-alternatives --set iptables /usr/sbin/iptables-legacy
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# 
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# cat <<EOF > /etc/yum.repos.d/kubernetes.repo
> [kubernetes]
> name=Kubernetes
> baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
> enabled=1
> gpgcheck=1
> repo_gpgcheck=1
> gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
> EOF
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# setenforce 0
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.advancedhosters.com
 * epel: iad.mirror.rackspace.com
 * extras: mirrors.advancedhosters.com
 * nux-dextop: li.nux.ro
 * updates: mirrors.advancedhosters.com

kubernetes/signature                                                                                                                             |  454 B  00:00:00     
Retrieving key from https://packages.cloud.google.com/yum/doc/yum-key.gpg
Importing GPG key 0xA7317B0F:
 Userid     : "Google Cloud Packages Automatic Signing Key <gc-team@google.com>"
 Fingerprint: d0bc 747f d8ca f711 7500 d6fa 3746 c208 a731 7b0f
 From       : https://packages.cloud.google.com/yum/doc/yum-key.gpg
Retrieving key from https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg

kubernetes/signature                                                                                                                             | 1.4 kB  00:00:00 !!! 

kubernetes/primary                                                                                                                               |  58 kB  00:00:00     

                                                                      421/421
Resolving Dependencies
--> Running transaction check
---> Package kubeadm.x86_64 0:1.16.2-0 will be installed
--> Processing Dependency: kubernetes-cni >= 0.7.5 for package: kubeadm-1.16.2-0.x86_64
--> Processing Dependency: cri-tools >= 1.13.0 for package: kubeadm-1.16.2-0.x86_64
---> Package kubectl.x86_64 0:1.16.2-0 will be installed
---> Package kubelet.x86_64 0:1.16.2-0 will be installed
--> Processing Dependency: socat for package: kubelet-1.16.2-0.x86_64
--> Processing Dependency: ebtables for package: kubelet-1.16.2-0.x86_64
--> Processing Dependency: conntrack for package: kubelet-1.16.2-0.x86_64
--> Running transaction check
---> Package conntrack-tools.x86_64 0:1.4.4-5.el7_7.2 will be installed
--> Processing Dependency: libnetfilter_cttimeout.so.1(LIBNETFILTER_CTTIMEOUT_1.1)(64bit) for package: conntrack-tools-1.4.4-5.el7_7.2.x86_64
--> Processing Dependency: libnetfilter_cttimeout.so.1(LIBNETFILTER_CTTIMEOUT_1.0)(64bit) for package: conntrack-tools-1.4.4-5.el7_7.2.x86_64
--> Processing Dependency: libnetfilter_cthelper.so.0(LIBNETFILTER_CTHELPER_1.0)(64bit) for package: conntrack-tools-1.4.4-5.el7_7.2.x86_64
--> Processing Dependency: libnetfilter_queue.so.1()(64bit) for package: conntrack-tools-1.4.4-5.el7_7.2.x86_64
--> Processing Dependency: libnetfilter_cttimeout.so.1()(64bit) for package: conntrack-tools-1.4.4-5.el7_7.2.x86_64
--> Processing Dependency: libnetfilter_cthelper.so.0()(64bit) for package: conntrack-tools-1.4.4-5.el7_7.2.x86_64
---> Package cri-tools.x86_64 0:1.13.0-0 will be installed
---> Package ebtables.x86_64 0:2.0.10-16.el7 will be installed
---> Package kubernetes-cni.x86_64 0:0.7.5-0 will be installed
---> Package socat.x86_64 0:1.7.3.2-2.el7 will be installed
--> Running transaction check
---> Package libnetfilter_cthelper.x86_64 0:1.0.0-10.el7_7.1 will be installed
---> Package libnetfilter_cttimeout.x86_64 0:1.0.0-6.el7_7.1 will be installed
---> Package libnetfilter_queue.x86_64 0:1.0.2-2.el7_2 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

========================================================================================================================================================================
 Package                                         Arch                            Version                                      Repository                           Size
========================================================================================================================================================================
Installing:
 kubeadm                                         x86_64                          1.16.2-0                                     kubernetes                          9.5 M
 kubectl                                         x86_64                          1.16.2-0                                     kubernetes                           10 M
 kubelet                                         x86_64                          1.16.2-0                                     kubernetes                           22 M
Installing for dependencies:
 conntrack-tools                                 x86_64                          1.4.4-5.el7_7.2                              updates                             187 k
 cri-tools                                       x86_64                          1.13.0-0                                     kubernetes                          5.1 M
 ebtables                                        x86_64                          2.0.10-16.el7                                base                                123 k
 kubernetes-cni                                  x86_64                          0.7.5-0                                      kubernetes                           10 M
 libnetfilter_cthelper                           x86_64                          1.0.0-10.el7_7.1                             updates                              18 k
 libnetfilter_cttimeout                          x86_64                          1.0.0-6.el7_7.1                              updates                              18 k
 libnetfilter_queue                              x86_64                          1.0.2-2.el7_2                                base                                 23 k
 socat                                           x86_64                          1.7.3.2-2.el7                                base                                290 k

Transaction Summary
========================================================================================================================================================================
Install  3 Packages (+8 Dependent packages)

Total download size: 57 M
Installed size: 262 M
Downloading packages:

(1/11): ebtables-2.0.10-16.el7.x86_64.rpm                                                                                                        | 123 kB  00:00:00     

(2/11): conntrack-tools-1.4.4-5.el7_7.2.x86_64.rpm                                                                                               | 187 kB  00:00:00     

(4/11): bd3de06f520c4a8c0017b653e2673cd6cd1b1386213b600f018fb67d93ffd 0% [                                                            ]  0.0 B/s | 309 kB  --:--:-- ETA 
warning: /var/cache/yum/x86_64/7/kubernetes/packages/14bfe6e75a9efc8eca3f638eb22c7e2ce759c67f95b43b16fae4ebabde1549f3-cri-tools-1.13.0-0.x86_64.rpm: Header V4 RSA/SHA512 Signature, key ID 3e1ba8d5: NOKEY
Public key for 14bfe6e75a9efc8eca3f638eb22c7e2ce759c67f95b43b16fae4ebabde1549f3-cri-tools-1.13.0-0.x86_64.rpm is not installed

(3/11): 14bfe6e75a9efc8eca3f638eb22c7e2ce759c67f95b43b16fae4ebabde1549f3-cri-tools-1.13.0-0.x86_64.rpm                                           | 5.1 MB  00:00:00     

(4/11): bd3de06f520c4a8c0017b653e2673cd6cd1b1386213b600f018fb67d93ffd60b-kubeadm-1.16.2-0.x86_64.rpm                                             | 9.5 MB  00:00:00     

(6/11): 0939db1dc940fa6800429c7ebef9d51fd9d46ff540589817cdb1927b8fae7 39% [=======================                                    ]  24 MB/s |  23 MB  00:00:01 ETA 

(5/11): 26d3e29e517cb0fd27fca12c02bd75ffa306bc5ce78c587d83a0242ba20588f0-kubectl-1.16.2-0.x86_64.rpm                                             |  10 MB  00:00:00     

(6/11): libnetfilter_cthelper-1.0.0-10.el7_7.1.x86_64.rpm                                                                                        |  18 kB  00:00:00     

(7/11): socat-1.7.3.2-2.el7.x86_64.rpm                                                                                                           | 290 kB  00:00:00     

(8/11): libnetfilter_cttimeout-1.0.0-6.el7_7.1.x86_64.rpm                                                                                        |  18 kB  00:00:00     

(9/11): libnetfilter_queue-1.0.2-2.el7_2.x86_64.rpm                                                                                              |  23 kB  00:00:00     

(10/11): 0939db1dc940fa6800429c7ebef9d51fd9d46ff540589817cdb1927b8fae 75% [============================================-              ]  26 MB/s |  44 MB  00:00:00 ETA 

(10/11): 548a0dcd865c16a50980420ddfa5fbccb8b59621179798e6dc905c9bf8af3b34-kubernetes-cni-0.7.5-0.x86_64.rpm                                      |  10 MB  00:00:00     

(11/11): 0939db1dc940fa6800429c7ebef9d51fd9d46ff540589817cdb1927b8fae7aaa-kubelet-1.16.2-0.x86_64.rpm                                            |  22 MB  00:00:01     
------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                    31 MB/s |  57 MB  00:00:01     
Retrieving key from https://packages.cloud.google.com/yum/doc/yum-key.gpg
Importing GPG key 0xA7317B0F:
 Userid     : "Google Cloud Packages Automatic Signing Key <gc-team@google.com>"
 Fingerprint: d0bc 747f d8ca f711 7500 d6fa 3746 c208 a731 7b0f
 From       : https://packages.cloud.google.com/yum/doc/yum-key.gpg
Retrieving key from https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
Importing GPG key 0x3E1BA8D5:
 Userid     : "Google Cloud Packages RPM Signing Key <gc-team@google.com>"
 Fingerprint: 3749 e1ba 95a8 6ce0 5454 6ed2 f09c 394c 3e1b a8d5
 From       : https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction

  Installing : ebtables-2.0.10-16.el7.x86_64 [                                                                                                                  ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [##                                                                                                                ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [####                                                                                                              ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [#########                                                                                                         ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [##############                                                                                                    ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [##################                                                                                                ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [#####################                                                                                             ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [##########################                                                                                        ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [#############################                                                                                     ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [#################################                                                                                 ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [####################################                                                                              ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [######################################                                                                            ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [##########################################                                                                        ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [#############################################                                                                     ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [#################################################                                                                 ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [###################################################                                                               ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [#####################################################                                                             ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [##########################################################                                                        ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [#############################################################                                                     ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [################################################################                                                  ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [###################################################################                                               ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [#####################################################################                                             ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [#######################################################################                                           ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [############################################################################################                      ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [#############################################################################################                     ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [###############################################################################################                   ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [####################################################################################################              ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [#####################################################################################################             ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [###########################################################################################################       ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [#############################################################################################################     ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64 [################################################################################################################  ]  1/11
  Installing : ebtables-2.0.10-16.el7.x86_64                                                                                                                       1/11 

  Installing : libnetfilter_cttimeout-1.0.0-6.el7_7.1.x86_64 [                                                                                                  ]  2/11
  Installing : libnetfilter_cttimeout-1.0.0-6.el7_7.1.x86_64 [################################################                                                  ]  2/11
  Installing : libnetfilter_cttimeout-1.0.0-6.el7_7.1.x86_64 [############################################################################################      ]  2/11
  Installing : libnetfilter_cttimeout-1.0.0-6.el7_7.1.x86_64 [################################################################################################# ]  2/11
  Installing : libnetfilter_cttimeout-1.0.0-6.el7_7.1.x86_64                                                                                                       2/11 

  Installing : socat-1.7.3.2-2.el7.x86_64 [                                                                                                                     ]  3/11
  Installing : socat-1.7.3.2-2.el7.x86_64 [#####                                                                                                                ]  3/11
  Installing : socat-1.7.3.2-2.el7.x86_64 [########                                                                                                             ]  3/11
  Installing : socat-1.7.3.2-2.el7.x86_64 [###############                                                                                                      ]  3/11
  Installing : socat-1.7.3.2-2.el7.x86_64 [################                                                                                                     ]  3/11
  Installing : socat-1.7.3.2-2.el7.x86_64 [#######################                                                                                              ]  3/11
  Installing : socat-1.7.3.2-2.el7.x86_64 [##############################                                                                                       ]  3/11
                                                                                   3/11 

  Installing : cri-tools-1.13.0-0.x86_64 [                                                                                                                      ]  4/11
  Installing : cri-tools-1.13.0-0.x86_64 [#                                                                                                                     ]  4/11
 -1.13.0-0.x86_64 [#########                                                                                                             ]  4/11
  Installing : cri-tools-1.13.0-0.x86_64 [##########                                                                                                            ]  4/11
                                                                  4/11 

  Installing : libnetfilter_queue-1.0.2-2.el7_2.x86_64 [                                                                                                        ]  5/11
  Installing : libnetfilter_queue-1.0.2-2.el7_2.x86_64 [##############################################################                                          ]  5/11
  Installing : libnetfilter_queue-1.0.2-2.el7_2.x86_64 [######################################################################################################  ]  5/11
  Installing : libnetfilter_queue-1.0.2-2.el7_2.x86_64                                                                                                             5/11 

  Installing : kubectl-1.16.2-0.x86_64 [                                                                                                                        ]  6/11
  Installing : kubectl-1.16.2-0.x86_64 [#                                                                                                                       ]  6/11
  Installing : kubectl-1.16.2-0.x86_64 [##                                                                                                                      ]  6/11
  Installing : kubectl-1.16.2-0.x86_64 [###                                                                                                                     ]  6/11
                                                                   6/11 

  Installing : libnetfilter_cthelper-1.0.0-10.el7_7.1.x86_64 [                                                                                                  ]  7/11
  Installing : libnetfilter_cthelper-1.0.0-10.el7_7.1.x86_64 [##########################################                                                        ]  7/11
  Installing : libnetfilter_cthelper-1.0.0-10.el7_7.1.x86_64 [###########################################                                                       ]  7/11
  Installing : libnetfilter_cthelper-1.0.0-10.el7_7.1.x86_64 [############################################################################################      ]  7/11
  Installing : libnetfilter_cthelper-1.0.0-10.el7_7.1.x86_64 [################################################################################################# ]  7/11
  Installing : libnetfilter_cthelper-1.0.0-10.el7_7.1.x86_64                                                                                                       7/11 

  Installing : conntrack-tools-1.4.4-5.el7_7.2.x86_64 [                                                                                                         ]  8/11
  Installing : conntrack-tools-1.4.4-5.el7_7.2.x86_64 [##                                                                                                       ]  8/11
  Installing : conntrack-tools-1.4.4-5.el7_7.2.x86_64 [####                                                                                                     ]  8/11
  Installing : conntrack-tools-1.4.4-5.el7_7.2.x86_64 [#####                                                                                                    ]  8/11
  I
  Installing : kubernetes-cni-0.7.5-0.x86_64 [                                                                                                                  ]  9/11
  Installing : kubernetes-cni-0.7.5-0.x86_64 [#                                                                                                                 ]  9/11
  Installing : kubernetes-cni-0.7.5-0.x86_64 [##                                                                                                                ]  9/11
  Installing : kubernetes-cni-0.7.5-0.x86_64 [###                                                                                                               ]  9/11
  Installing : kubernetes-cni-0.7.5-0.x86_64 [####                                                                                                              ]  9/11
  Installing : kubernetes-cni-0.7.5-0.x86_64 [#####                                                                                                             ]  9/11
  Installing : kubernetes-cni-0.7.5-0.x86_64 [######                                                                                                            ]  9/11
                                                                                                       9/11 

  Installing : kubelet-1.16.2-0.x86_64 [                                                                                                                        ] 10/11
  Installing : kubelet-1.16.2-0.x86_64 [#                                                                                                                       ] 10/11
  Installing : kubelet-1.16.2-0.x86_64 [##                                                                                                                      ] 10/11
  Installing : kubelet-1.16.2-0.x86_64 [###                                                                                                                     ] 10/11
  Installing : kubelet-1.16.2-0.x86_64 [####                                                                                                                    ] 10/11
  Installing : kubelet-1.16.2-0.x86_64 [######                                                                                                                  ] 10/11
                                                                                                                  10/11 

  Installing : kubeadm-1.16.2-0.x86_64 [                                                                                                                        ] 11/11
  Installing : kubeadm-1.16.2-0.x86_64 [#                                                                                                                       ] 11/11
  Installing : kubeadm-1.16.2-0.x86_64 [##                                                                                                                      ] 11/11
  Installing : kubeadm-1.16.2-0.x86_64 [###                                                                                                                     ] 11/11
  Installing : kubeadm-1.16.2-0.x86_64 [####                                                                                                                    ] 11/11
  Installing : kubeadm-1.16.2-0.x86_64 [######                                                                                                                  ] 11/11
  Installing : kubeadm-1.16.2-0.x86_64 [#######                                                                                                                 ] 11/11
  Installing : kubeadm-1.16.2-0.x86_64 [########                                                                                                                ] 11/11
  Installing : kubeadm-1.16.2-0.x86_64 [#########                                                                                                               ] 11/11
                                                                                                         11/11 

  Verifying  : libnetfilter_cthelper-1.0.0-10.el7_7.1.x86_64                                                                                                       1/11 

  Verifying  : kubectl-1.16.2-0.x86_64                                                                                                                             2/11 

  Verifying  : conntrack-tools-1.4.4-5.el7_7.2.x86_64                                                                                                              3/11 

  Verifying  : libnetfilter_queue-1.0.2-2.el7_2.x86_64                                                                                                             4/11 

  Verifying  : cri-tools-1.13.0-0.x86_64                                                                                                                           5/11 

  Verifying  : kubelet-1.16.2-0.x86_64                                                                                                                             6/11 

  Verifying  : kubeadm-1.16.2-0.x86_64                                                                                                                             7/11 

  Verifying  : kubernetes-cni-0.7.5-0.x86_64                                                                                                                       8/11 

  Verifying  : socat-1.7.3.2-2.el7.x86_64                                                                                                                          9/11 

  Verifying  : libnetfilter_cttimeout-1.0.0-6.el7_7.1.x86_64                                                                                                      10/11 

  Verifying  : ebtables-2.0.10-16.el7.x86_64                                                                                                                      11/11 

Installed:
  kubeadm.x86_64 0:1.16.2-0                              kubectl.x86_64 0:1.16.2-0                              kubelet.x86_64 0:1.16.2-0                             

Dependency Installed:
  conntrack-tools.x86_64 0:1.4.4-5.el7_7.2           cri-tools.x86_64 0:1.13.0-0                              ebtables.x86_64 0:2.0.10-16.el7                         
  kubernetes-cni.x86_64 0:0.7.5-0                    libnetfilter_cthelper.x86_64 0:1.0.0-10.el7_7.1          libnetfilter_cttimeout.x86_64 0:1.0.0-6.el7_7.1         
  libnetfilter_queue.x86_64 0:1.0.2-2.el7_2          socat.x86_64 0:1.7.3.2-2.el7                            

Complete!
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# systemctl enable --now kubelet
Created symlink from /etc/systemd/system/multi-user.target.wants/kubelet.service to /usr/lib/systemd/system/kubelet.service.
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# ifconfig
docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        inet6 fe80::42:caff:fec3:6e  prefixlen 64  scopeid 0x20<link>
        ether 02:42:ca:c3:00:6e  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 3  bytes 266 (266.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

ens5: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 9001
        inet 172.31.17.208  netmask 255.255.240.0  broadcast 172.31.31.255
        inet6 fe80::c4:d7ff:fe33:81f  prefixlen 64  scopeid 0x20<link>
        ether 02:c4:d7:33:08:1f  txqueuelen 1000  (Ethernet)
        RX packets 144784  bytes 190283553 (181.4 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 29349  bytes 4601980 (4.3 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 74  bytes 19488 (19.0 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 74  bytes 19488 (19.0 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# kubeadm init --apiserver-advertise-address172.31.17.208 --pod-network-cidr=10.244.0.0/16 
Error: unknown flag: --apiserver-advertise-address172.31.17.208
Usage:
  kubeadm init [flags]
  kubeadm init [command]

Available Commands:
  phase       Use this command to invoke single phase of the init workflow

Flags:
      --apiserver-advertise-address string   The IP address the API Server will advertise it's listening on. If not set the default network interface will be used.
      --apiserver-bind-port int32            Port for the API Server to bind to. (default 6443)
      --apiserver-cert-extra-sans strings    Optional extra Subject Alternative Names (SANs) to use for the API Server serving certificate. Can be both IP addresses and DNS names.
      --cert-dir string                      The path where to save and store the certificates. (default "/etc/kubernetes/pki")
      --certificate-key string               Key used to encrypt the control-plane certificates in the kubeadm-certs Secret.
      --config string                        Path to a kubeadm configuration file.
      --control-plane-endpoint string        Specify a stable IP address or DNS name for the control plane.
      --cri-socket string                    Path to the CRI socket to connect. If empty kubeadm will try to auto-detect this value; use this option only if you have more than one CRI installed or if you have non-standard CRI socket.
      --dry-run                              Don't apply any changes; just output what would be done.
  -k, --experimental-kustomize string        The path where kustomize patches for static pod manifests are stored.
      --feature-gates string                 A set of key=value pairs that describe feature gates for various features. Options are:
                                             IPv6DualStack=true|false (ALPHA - default=false)
  -h, --help                                 help for init
      --ignore-preflight-errors strings      A list of checks whose errors will be shown as warnings. Example: 'IsPrivilegedUser,Swap'. Value 'all' ignores errors from all checks.
      --image-repository string              Choose a container registry to pull control plane images from (default "k8s.gcr.io")
      --kubernetes-version string            Choose a specific Kubernetes version for the control plane. (default "stable-1")
      --node-name string                     Specify the node name.
      --pod-network-cidr string              Specify range of IP addresses for the pod network. If set, the control plane will automatically allocate CIDRs for every node.
      --service-cidr string                  Use alternative range of IP address for service VIPs. (default "10.96.0.0/12")
      --service-dns-domain string            Use alternative domain for services, e.g. "myorg.internal". (default "cluster.local")
      --skip-certificate-key-print           Don't print the key used to encrypt the control-plane certificates.
      --skip-phases strings                  List of phases to be skipped
      --skip-token-print                     Skip printing of the default bootstrap token generated by 'kubeadm init'.
      --token string                         The token to use for establishing bidirectional trust between nodes and control-plane nodes. The format is [a-z0-9]{6}\.[a-z0-9]{16} - e.g. abcdef.0123456789abcdef
      --token-ttl duration                   The duration before the token is automatically deleted (e.g. 1s, 2m, 3h). If set to '0', the token will never expire (default 24h0m0s)
      --upload-certs                         Upload control-plane certificates to the kubeadm-certs Secret.

Global Flags:
      --add-dir-header           If true, adds the file directory to the header
      --log-file string          If non-empty, use this log file
      --log-file-max-size uint   Defines the maximum size a log file can grow to. Unit is megabytes. If the value is 0, the maximum file size is unlimited. (default 1800)
      --rootfs string            [EXPERIMENTAL] The path to the 'real' host root filesystem.
      --skip-headers             If true, avoid header prefixes in the log messages
      --skip-log-headers         If true, avoid headers when opening log files
  -v, --v Level                  number for the log level verbosity

Use "kubeadm init [command] --help" for more information about a command.

unknown flag: --apiserver-advertise-address172.31.17.208
To see the stack trace of this error execute with --v=5 or higher
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# kubeadm init --apiserver-advertise-address172.31.17.208 --pod-network-cidr=10.244.0.0/16 [C[1@=
[init] Using Kubernetes version: v1.16.2
[preflight] Running pre-flight checks
	[WARNING Service-Docker]: docker service is not enabled, please run 'systemctl enable docker.service'
	[WARNING IsDockerSystemdCheck]: detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd". Please follow the guide at https://kubernetes.io/docs/setup/cri/
	[WARNING SystemVerification]: this Docker version is not on the list of validated versions: 19.03.4. Latest validated version: 18.09
error execution phase preflight: [preflight] Some fatal errors occurred:
	[ERROR Swap]: running with swap on is not supported. Please disable swap
[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`
To see the stack trace of this error execute with --v=5 or higher
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# systemctl enable dockey[Kr
Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# docker run -d hellow-world
Unable to find image 'hellow-world:latest' locally
docker: Error response from daemon: pull access denied for hellow-world, repository does not exist or may require 'docker login': denied: requested access to the resource is denied.
See 'docker run --help'.
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# docker run -d hellow-world[1P-world
dbe0b4ab3581f1e2d137b16ade153c28a241f6bbf0684790d1dc05927445b8e1
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# docker run -d hello-worldw-world[3Psystemctl enable dockerkubeadm init --apiserver-advertise-address=172.31.17.208 --pod-network-cidr=10.244.0.0/16 
[init] Using Kubernetes version: v1.16.2
[preflight] Running pre-flight checks
	[WARNING IsDockerSystemdCheck]: detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd". Please follow the guide at https://kubernetes.io/docs/setup/cri/
	[WARNING SystemVerification]: this Docker version is not on the list of validated versions: 19.03.4. Latest validated version: 18.09
error execution phase preflight: [preflight] Some fatal errors occurred:
	[ERROR Swap]: running with swap on is not supported. Please disable swap
[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`
To see the stack trace of this error execute with --v=5 or higher
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# swapoff -a
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# swapoff -akubeadm init --apiserver-advertise-address=172.31.17.208 --pod-network-cidr=10.244.0.0/16 
[init] Using Kubernetes version: v1.16.2
[preflight] Running pre-flight checks
	[WARNING IsDockerSystemdCheck]: detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd". Please follow the guide at https://kubernetes.io/docs/setup/cri/
	[WARNING SystemVerification]: this Docker version is not on the list of validated versions: 19.03.4. Latest validated version: 18.09
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Activating the kubelet service
[certs] Using certificateDir folder "/etc/kubernetes/pki"
[certs] Generating "ca" certificate and key
[certs] Generating "apiserver" certificate and key
[certs] apiserver serving cert is signed for DNS names [praghu1c.mylabserver.com kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 172.31.17.208]
[certs] Generating "apiserver-kubelet-client" certificate and key
[certs] Generating "front-proxy-ca" certificate and key
[certs] Generating "front-proxy-client" certificate and key
[certs] Generating "etcd/ca" certificate and key
[certs] Generating "etcd/server" certificate and key
[certs] etcd/server serving cert is signed for DNS names [praghu1c.mylabserver.com localhost] and IPs [172.31.17.208 127.0.0.1 ::1]
[certs] Generating "etcd/peer" certificate and key
[certs] etcd/peer serving cert is signed for DNS names [praghu1c.mylabserver.com localhost] and IPs [172.31.17.208 127.0.0.1 ::1]
[certs] Generating "etcd/healthcheck-client" certificate and key
[certs] Generating "apiserver-etcd-client" certificate and key
[certs] Generating "sa" key and public key
[kubeconfig] Using kubeconfig folder "/etc/kubernetes"
[kubeconfig] Writing "admin.conf" kubeconfig file
[kubeconfig] Writing "kubelet.conf" kubeconfig file
[kubeconfig] Writing "controller-manager.conf" kubeconfig file
[kubeconfig] Writing "scheduler.conf" kubeconfig file
[control-plane] Using manifest folder "/etc/kubernetes/manifests"
[control-plane] Creating static Pod manifest for "kube-apiserver"
[control-plane] Creating static Pod manifest for "kube-controller-manager"
[control-plane] Creating static Pod manifest for "kube-scheduler"
[etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
[apiclient] All control plane components are healthy after 20.004407 seconds
[upload-config] Storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config-1.16" in namespace kube-system with the configuration for the kubelets in the cluster
[upload-certs] Skipping phase. Please see --upload-certs
[mark-control-plane] Marking the node praghu1c.mylabserver.com as control-plane by adding the label "node-role.kubernetes.io/master=''"
[mark-control-plane] Marking the node praghu1c.mylabserver.com as control-plane by adding the taints [node-role.kubernetes.io/master:NoSchedule]
[bootstrap-token] Using token: 21dktk.41al9g2lgs6ctsx4
[bootstrap-token] Configuring bootstrap tokens, cluster-info ConfigMap, RBAC Roles
[bootstrap-token] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstrap-token] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstrap-token] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[bootstrap-token] Creating the "cluster-info" ConfigMap in the "kube-public" namespace
[addons] Applied essential addon: CoreDNS
[addons] Applied essential addon: kube-proxy

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 172.31.17.208:6443 --token 21dktk.41al9g2lgs6ctsx4 \
    --discovery-token-ca-cert-hash sha256:9b57809dd029664f5b040058b485a3af53dcf7438a78623c25ca26e8111ea1e3 
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# kubectl get[K[K[K[K[K[K[K[K[K[K[K        kubectl get m[Knodes
The connection to the server localhost:8080 was refused - did you specify the right host or port?
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# mkdir -p $HOME/.kube
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo: Account or password is expired, reset your password and try again
Changing password for root.
(current) UNIX password: 

sudo: unable to change expired password: Authentication token manipulation error
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# 
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[1P[1P[1P[1P[1P
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
cp: overwrite ‘/root/.kube/config’? 
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# 
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# chown $(id -u):$(id -g) $HOME/.kube/config
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# kubectl get nodes
NAME                       STATUS     ROLES    AGE     VERSION
praghu1c.mylabserver.com   NotReady   master   7m48s   v1.16.2
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# kubectl get nodes
NAME                       STATUS     ROLES    AGE    VERSION
praghu1c.mylabserver.com   NotReady   master   10m    v1.16.2
praghu3c.mylabserver.com   NotReady   <none>   104s   v1.16.2
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# sysctl net.bridge.bridge-nf-call-iptables=1
net.bridge.bridge-nf-call-iptables = 1
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/2140ac876ef134e0ed5af15c65e414cf26827915/Documentation/kube-flannel.yml
podsecuritypolicy.policy/psp.flannel.unprivileged created
clusterrole.rbac.authorization.k8s.io/flannel created
clusterrolebinding.rbac.authorization.k8s.io/flannel created
serviceaccount/flannel created
configmap/kube-flannel-cfg created
daemonset.apps/kube-flannel-ds-amd64 created
daemonset.apps/kube-flannel-ds-arm64 created
daemonset.apps/kube-flannel-ds-arm created
daemonset.apps/kube-flannel-ds-ppc64le created
daemonset.apps/kube-flannel-ds-s390x created
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# kubectl get nodes
NAME                       STATUS     ROLES    AGE     VERSION
praghu1c.mylabserver.com   Ready      master   11m     v1.16.2
praghu3c.mylabserver.com   NotReady   <none>   2m54s   v1.16.2
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# kubectl get nodes
NAME                       STATUS   ROLES    AGE    VERSION
praghu1c.mylabserver.com   Ready    master   11m    v1.16.2
praghu3c.mylabserver.com   Ready    <none>   3m5s   v1.16.2
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# curl -LO https://git.io/get_helm.sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed

  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0

100  7034  100  7034    0     0  16551      0 --:--:-- --:--:-- --:--:-- 16551
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# chmod 700 get_helm.sh
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# ./get_helm.sh
Downloading https://get.helm.sh/helm-v2.15.1-linux-amd64.tar.gz
Preparing to install helm and tiller into /usr/local/bin
helm installed into /usr/local/bin/helm
tiller installed into /usr/local/bin/tiller
which: no helm in (/usr/local/rvm/gems/ruby-2.4.1/bin:/usr/local/rvm/gems/ruby-2.4.1@global/bin:/usr/local/rvm/rubies/ruby-2.4.1/bin:/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/rvm/bin)
helm not found. Is /usr/local/bin on your $PATH?
Failed to install helm
	For support, go to https://github.com/helm/helm.
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# ls
[0m[38;5;27mDesktop[0m  [38;5;27mDocuments[0m  [38;5;34mget_helm.sh[0m  kubernetes  output
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# pwd
/home/cloud_user
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# .sh[K[K[K pwd[1Pls./get_helm.sh
Helm v2.15.1 is already latest
which: no helm in (/usr/local/rvm/gems/ruby-2.4.1/bin:/usr/local/rvm/gems/ruby-2.4.1@global/bin:/usr/local/rvm/rubies/ruby-2.4.1/bin:/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/rvm/bin)
helm not found. Is /usr/local/bin on your $PATH?
Failed to install helm
	For support, go to https://github.com/helm/helm.
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# helm -version
bash: helm: command not found
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# install [K[K[K[K[K[K[K[K  pwd
/home/cloud_user
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# export pa[K[K[K PAR[KTH=home[K[K[K[K/home/cloud_user/[K:$PATH 
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# export PATH=/home/cloud_user:$PATH 
[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[Cpwd[Khelm -version[K[K[K[K[K[K[K-version
bash: helm: command not found
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# helm --versionexport PATH=/home/cloud_user:$PATH 
[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[Cpwd[Khelm -version./get_helm.sh
Helm v2.15.1 is already latest
which: no helm in (/home/cloud_user:/usr/local/rvm/gems/ruby-2.4.1/bin:/usr/local/rvm/gems/ruby-2.4.1@global/bin:/usr/local/rvm/rubies/ruby-2.4.1/bin:/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/rvm/bin)
helm not found. Is /usr/local/bin on your $PATH?
Failed to install helm
	For support, go to https://github.com/helm/helm.
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# curl -LO https://git.io/get_helm.sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed

  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0

100  7034  100  7034    0     0  45965      0 --:--:-- --:--:-- --:--:-- 45965
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# chmod 700 get_helm.sh
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# ./get_helm.sh
Helm v2.15.1 is already latest
which: no helm in (/home/cloud_user:/usr/local/rvm/gems/ruby-2.4.1/bin:/usr/local/rvm/gems/ruby-2.4.1@global/bin:/usr/local/rvm/rubies/ruby-2.4.1/bin:/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/rvm/bin)
helm not found. Is /usr/local/bin on your $PATH?
Failed to install helm
	For support, go to https://github.com/helm/helm.
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# ls
[0m[38;5;27mDesktop[0m  [38;5;27mDocuments[0m  [38;5;34mget_helm.sh[0m  kubernetes  output
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# less get_helm.sh 
[?1049h[?1h=
#!/usr/bin/env bash

# Copyright The Helm Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# The install script is based off of the MIT-licensed script from glide,
# the package manager for Go: https://github.com/Masterminds/glide.sh/blob/master/get

PROJECT_NAME="helm"
TILLER_NAME="tiller"

: ${USE_SUDO:="true"}
: ${HELM_INSTALL_DIR:="/usr/local/bin"}

# initArch discovers the architecture for this system.
initArch() {
  ARCH=$(uname -m)
  case $ARCH in
    armv5*) ARCH="armv5";;
    armv6*) ARCH="armv6";;
    armv7*) ARCH="arm";;
    aarch64) ARCH="arm64";;
[7mget_helm.sh[27m[K
[K    x86) ARCH="386";;
:[K
[K    x86_64) ARCH="amd64";;
:[K
[K    i686) ARCH="386";;
:[K
[K    i386) ARCH="386";;
:[K
[K  esac
:[K
[K}
:[K
[K
:[K
[K# initOS discovers the operating system for this system.
:[K
[KinitOS() {
:[K
[K  OS=$(echo `uname`|tr '[:upper:]' '[:lower:]')
:[K
[K
:[K
[K  case "$OS" in
:[K
[K    # Minimalist GNU for Windows
:[K
[K    mingw*) OS='windows';;
:[K
[K  esac
:[K
[K}
:[K
[K
:[K
[K# runs the given command as root (detects if we are root already)
:[K
[KrunAsRoot() {
:[K
[K  local CMD="$*"
:[K
[K
:[K
[K  if [ $EUID -ne 0 -a $USE_SUDO = "true" ]; then
:[K
[K    CMD="sudo $CMD"
:[K
[K  fi
:[K
[K
:[K
[K  $CMD
:[K
[K}
:[K
[K
:[K
[K# verifySupported checks that the os/arch combination is supported for
:[K
[K# binary builds.
:[K
[KverifySupported() {
:[K
[K  local supported="darwin-386\ndarwin-amd64\nlinux-386\nlinux-amd64\nlinux-arm\nlinux-arm64\nlinux-ppc64le\nwindows-386\nwindows-amd64"
:[K
[K  if ! echo "${supported}" | grep -q "${OS}-${ARCH}"; then
:[K
[K    echo "No prebuilt binary for ${OS}-${ARCH}."
:[K
[K    echo "To build from source, go to https://github.com/helm/helm"
:[K
[K    exit 1
:[K
[K  fi
:[K
[K
:[K
[K  if ! type "curl" > /dev/null && ! type "wget" > /dev/null; then
:[K
[K    echo "Either curl or wget is required"
:[K
[K    exit 1
:[K
[K  fi
:[K
[K}
:[K
[K
:[K
[K# checkDesiredVersion checks if the desired version is available.
:[K
[KcheckDesiredVersion() {
:[K
[K  if [ "x$DESIRED_VERSION" == "x" ]; then
:[K
[K    # Get tag from release URL
:[K
[K    local latest_release_url="https://github.com/helm/helm/releases/latest"
:[K
[K    if type "curl" > /dev/null; then
:[K
[K      TAG=$(curl -Ls -o /dev/null -w %{url_effective} $latest_release_url | grep -oE "[^/]+$" )
:[K
[K    elif type "wget" > /dev/null; then
:[K
[K      TAG=$(wget $latest_release_url --server-response -O /dev/null 2>&1 | awk '/^  Location: /{DEST=$2} END{ print DEST}' | grep -oE "[^/]+$")
:[K
[K    fi
:[K
[K  else
:[K
[K    TAG=$DESIRED_VERSION
:[K
[K  fi
:[K
[K}
:[K
[K
:[K
[K[?1l>[?1049l]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# wget -[Khttps://github.com/helm/helm/releases
--2019-10-27 16:23:37--  https://github.com/helm/helm/releases
Resolving github.com (github.com)... 192.30.253.113
Connecting to github.com (github.com)|192.30.253.113|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: unspecified [text/html]
Saving to: ‘releases’


    [<=>                                                                                                                            ] 0           --.-K/s              
    [ <=>                                                                                                                           ] 401,407     --.-K/s   in 0.01s   

2019-10-27 16:23:38 (37.1 MB/s) - ‘releases’ saved [401407]

]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# ls
[0m[38;5;27mDesktop[0m  [38;5;27mDocuments[0m  [38;5;34mget_helm.sh[0m  kubernetes  output  releases
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# wget https://get.helm.sh/helm-v2.15.1-linux-386.tar.gz
--2019-10-27 16:24:54--  https://get.helm.sh/helm-v2.15.1-linux-386.tar.gz
Resolving get.helm.sh (get.helm.sh)... 152.195.19.95, 2606:2800:11f:b40:171d:1a2f:2077:f6b
Connecting to get.helm.sh (get.helm.sh)|152.195.19.95|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 22918756 (22M) [application/x-tar]
Saving to: ‘helm-v2.15.1-linux-386.tar.gz’


 0% [                                                                                                                               ] 0           --.-K/s              
 1% [>                                                                                                                              ] 253,536      963KB/s             
 6% [======>                                                                                                                        ] 1,416,800   2.63MB/s             
18% [======================>                                                                                                        ] 4,202,496   5.05MB/s             
36% [=============================================>                                                                                 ] 8,396,800   7.93MB/s             
69% [=======================================================================================>                                       ] 15,966,208  12.4MB/s             
91% [===================================================================================================================>           ] 20,979,712  13.8MB/s             
100%[==============================================================================================================================>] 22,918,756  15.0MB/s   in 1.5s   

2019-10-27 16:24:56 (15.0 MB/s) - ‘helm-v2.15.1-linux-386.tar.gz’ saved [22918756/22918756]

]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# ls
[0m[38;5;27mDesktop[0m  [38;5;27mDocuments[0m  [38;5;34mget_helm.sh[0m  [38;5;9mhelm-v2.15.1-linux-386.tar.gz[0m  kubernetes  output  releases
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# tar -he[K[Kx[Kzxvf hel,[Km-v2.15.1-linux-386.tar.gz 
linux-386/
linux-386/tiller
linux-386/LICENSE
linux-386/helm
linux-386/README.md
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# ls
[0m[38;5;27mDesktop[0m  [38;5;27mDocuments[0m  [38;5;34mget_helm.sh[0m  [38;5;9mhelm-v2.15.1-linux-386.tar.gz[0m  kubernetes  [38;5;27mlinux-386[0m  output  releases
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# ls
[0m[38;5;27mDesktop[0m  [38;5;27mDocuments[0m  [38;5;34mget_helm.sh[0m  [38;5;9mhelm-v2.15.1-linux-386.tar.gz[0m  kubernetes  [38;5;27mlinux-386[0m  output  releases
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# mkdir helm
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# helm mv[K[K[K[K[K[K[K  mv helm 
helm/                          helm-v2.15.1-linux-386.tar.gz  
[root@praghu1c cloud_user]# mv helm-v2.15.1-linux-386.tar.gz  [Khelm
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# ls
[0m[38;5;27mDesktop[0m  [38;5;27mDocuments[0m  [38;5;34mget_helm.sh[0m  [38;5;27mhelm[0m  kubernetes  [38;5;27mlinux-386[0m  output  releases
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# cd helm
]0;root@praghu1c:/home/cloud_user/helm [root@praghu1c helm]# tar[K[K[K cd helmls[Kmv helm-v2.15.1-linux-386.tar.gz helm
[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[C[Ckdir helm[Kls[Ktar -zxvf helm-v2.15.1-linux-386.tar.gz 
linux-386/
linux-386/tiller
linux-386/LICENSE
linux-386/helm
linux-386/README.md
]0;root@praghu1c:/home/cloud_user/helm [root@praghu1c helm]# ls
[0m[38;5;9mhelm-v2.15.1-linux-386.tar.gz[0m  [38;5;27mlinux-386[0m
]0;root@praghu1c:/home/cloud_user/helm [root@praghu1c helm]# rm helm-v2.15.1-linux-386.tar.gz 
rm: remove regular file ‘helm-v2.15.1-linux-386.tar.gz’? y
]0;root@praghu1c:/home/cloud_user/helm [root@praghu1c helm]# cd ..
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# helm --version
bash: helm: command not found
]0;root@praghu1c:/home/cloud_user [root@praghu1c cloud_user]# cd helm
]0;root@praghu1c:/home/cloud_user/helm [root@praghu1c helm]# cd linux-386/
]0;root@praghu1c:/home/cloud_user/helm/linux-386 [root@praghu1c linux-386]# ls
[0m[38;5;34mhelm[0m  LICENSE  README.md  [38;5;34mtiller[0m
]0;root@praghu1c:/home/cloud_user/helm/linux-386 [root@praghu1c linux-386]# helm --version
bash: helm: command not found
]0;root@praghu1c:/home/cloud_user/helm/linux-386 [root@praghu1c linux-386]# ./helm
The Kubernetes package manager

To begin working with Helm, run the 'helm init' command:

	$ helm init
