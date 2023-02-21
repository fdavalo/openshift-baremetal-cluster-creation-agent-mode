# Creation of an Openshift BareMetal cluster with Advanced Cluster Management and Agent Mode (assisted installer)

## Check you have available hosts in Infrastructure env **bm4**

* Those hosts can be added with PXE or by inserting a discovery ISO

    cf https://github.com/fdavalo/mce-agent-provisioning-pxe for PXE provisioning of bare metal hosts

    cf https://github.com/fdavalo/mce-agent-provision-vms for provisioning with terraform and a discovery ISO (not for bm4 in this example)
    
[![Available Hosts](https://github.com/fdavalo/openshift-baremetal-cluster-creation-agent-mode/blob/main/create-cluster-bm4-available-hosts.png?raw=true)](create-cluster-bm4-available-hosts.png)

## Create your cluster with Advanced Cluster Management User Interface

* LogIn with hpe_redhat user/login on bm1 cluster

    cf https://console-openshift-console.apps.bm1.redhat.hpecic.net/multicloud/infrastructure/clusters/managed

* Click on Create button

[![Create Cluster](https://github.com/fdavalo/openshift-baremetal-cluster-creation-agent-mode/blob/main/create-cluster-bm4-1.png?raw=true)](create-cluster-bm4-1.png)

* Choose Host Inventory (Agent Mode)

[![Host Inventory](https://github.com/fdavalo/openshift-baremetal-cluster-creation-agent-mode/blob/main/create-cluster-bm4-2.png?raw=true)](create-cluster-bm4-2.png)

* Choose Standalone (not hypershift)

[![Standalone](https://github.com/fdavalo/openshift-baremetal-cluster-creation-agent-mode/blob/main/create-cluster-bm4-3.png?raw=true)](create-cluster-bm4-3.png)

* Choose existing hosts

[![Use existing Hosts](https://github.com/fdavalo/openshift-baremetal-cluster-creation-agent-mode/blob/main/create-cluster-bm4-4.png?raw=true)](create-cluster-bm4-4.png)

* Fill form inputs : 

  ** Infrastructure Credentials (pull secret) = hostinventory
  
  ** Cluster Name = bm4
  
  ** Cluster Set = bm_dev_clusters  (for automatic day 2 configuration/customization with argoCD/application sets)
  
  ** Base Domaine = redhat.hpecic.net 
  
  ** Openshift Version <= 4.11
  
[![Detail page](https://github.com/fdavalo/openshift-baremetal-cluster-creation-agent-mode/blob/main/create-cluster-bm4-details.png?raw=true)](create-cluster-bm4-details.png)

* Choose your hosts

[![Host page](https://github.com/fdavalo/openshift-baremetal-cluster-creation-agent-mode/blob/main/create-cluster-bm4-hosts.png?raw=true)](create-cluster-bm4-hosts.png)

* Hosts are bound to your cluster

[![Binding hosts](https://github.com/fdavalo/openshift-baremetal-cluster-creation-agent-mode/blob/main/create-cluster-bm4-hosts-binding.png?raw=true)](create-cluster-bm4-hosts-binding.png)

* Fill form inputs : 

  ** Machine subnet = 10.6.82.0/24
  
  ** API VIP = 10.6.82.111
  
  ** Ingress VIP = 10.6.82.112 (wildcard provisioning)
  
[![Network page](https://github.com/fdavalo/openshift-baremetal-cluster-creation-agent-mode/blob/main/create-cluster-bm4-network.png?raw=true)](create-cluster-bm4-network.png)

* Before going to review page, checks are done 

[![Checking ready for cluster creation](https://github.com/fdavalo/openshift-baremetal-cluster-creation-agent-mode/blob/main/create-cluster-bm4-network-notready.png?raw=true)](create-cluster-bm4-network-notready.png)

* Review the information provided, you can still customize on **edit yaml** tab or fetch the yaml for automatic creation

[![Review page](https://github.com/fdavalo/openshift-baremetal-cluster-creation-agent-mode/blob/main/create-cluster-bm4-review.png?raw=true)](create-cluster-bm4-review.png)

* Creation started, you can follow the installation (see cluster events popup)

[![Start Creation](https://github.com/fdavalo/openshift-baremetal-cluster-creation-agent-mode/blob/main/create-cluster-bm4-start.png?raw=true)](create-cluster-bm4-start.png)

* Installation is started on bare metal hosts 

  ** power on was done by metal3d pod
  
  ** host triggers PXE boot and retrieves ISO boot from metal3d pod
  
  ** CoreOS iso is written on boot disk then first reboot
  
  ** first ignition boot (update coreos image then reboot)
  
  ** second boot starts pods and join the bootstrap nodes
  
  ** operator pod images are pulled/updated
  
[![Installation started](https://github.com/fdavalo/openshift-baremetal-cluster-creation-agent-mode/blob/main/create-cluster-bm4-started.png?raw=true)](create-cluster-bm4-started.png)

* Bootstrap node is changing to a normal control plane after the two first control plane are taking over the Etcd, Api, ...

[![Bootstrap node changing to control plane node](https://github.com/fdavalo/openshift-baremetal-cluster-creation-agent-mode/blob/main/create-cluster-bm4-end.png?raw=true)](create-cluster-bm4-end.png)

* Cluster is created and ready to ride

[![Cluster created](https://github.com/fdavalo/openshift-baremetal-cluster-creation-agent-mode/blob/main/create-cluster-bm4-final.png?raw=true)](create-cluster-bm4-final.png)
