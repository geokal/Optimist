# Multi-access Edge Computing Platform
# 1. MEC Architecture 

![](https://www.mdpi.com/electronics/electronics-09-01392/article_deploy/html/images/electronics-09-01392-g001.png)


**MEC Orchestrator (MEO)**: is mainly responsible for coordinating resources across various VIMs and deciding where to deploy MEAs. Through the API provided by MEM, MEO can make requests for the life cycle of the MEAs (initiation, deletion, update, etc.). Since MEO has global view of resources across multiple VIMs, MEM can also make a request to obtain VIM access from MEO so that the life cycle of the MEAs can be performed on the specific infrastructure. Such a design helps MEO to avoid bottleneck when managing the life cycle of the MEAs accross multiple VIMs.


**MEC Platform Manager (MEPM)**: is in charge of MEA instances’ life cycle, which includes automated provisioning, monitoring, configuration, healing, and scaling. They are managed by several modules as follows. The automated provisioning functionality is in charge of receiving and dispatching users’ requests for MEC applications. MEM provides an API for users to send their requests on MEC application’s resources (CPU, memory, network, etc.). Then, the module translates those requests into a VIM’s understandable format. Finally, MEM converts requests into VIM’s function calls to actually instantiate MEA instances.


# 2. ETSI NFV Compliant MANO Orchestrators and possible candidates for ETSI MEC

## 2.1 TACKER 


Tacker is an OpenStack project implementing a generic VNFM and NFVO of the ETSI NFV specification. At the input it consumes Tosca-based templates, converts them to Heat templates which are then used to spin up VMs on OpenStack

**<center>Architecture:</center>**
![](https://i.imgur.com/ulGSmou.png)

 
:::success

https://docs.openstack.org/tacker/latest/user/architecture.html 
https://wiki.openstack.org/wiki/Tacker

:::


![](https://i.imgur.com/Rm0wZEn.png)



| Components  | |
| -------- | -------- |
| tacker-client | provides CLI and communication with Tacker via REST API |
| server | provides REST API and calls conductor via RPC. |
|conductor | implements all logics to operate VNF and call required drivers providing interface to NFV infrastructures | 
| infra-driver | responsible for exact actions to operate OpenStack or Kubernetes resources |
| vim-driver | responsible for registration of VIM. |
|mgmt-driver | responsible for exact actions to configure VNFs. |
| monitor-driver | responsible for exact actions to monitor VNFs |
| policy-driver | responsible for policy based VNF operations. |

![](https://i.imgur.com/S3XQIyT.png)


Installation Guide: https://docs.openstack.org/tacker/latest/user/index.html 
User Guide for VNF’s: 	https://docs.openstack.org/tacker/latest/user/index.html 
Source Code: 		https://opendev.org/openstack/tacker.git 
                    https://github1s.com/openstack/tacker(with VSCode Integration) 
                    
                    
                    
**Other Tacker features**

* VNF monitoring - through monitoring driver its possible to do VNF monitoring from VNFM using various methods ranging from a single ICMP/HTTP ping to Alarm-based monitoring using OpenStack’s Telemetry framework
* Enhanced Placement Awareness - VNFD Tosca template extensions that allow the definition of required performance features like NUMA topology mapping, SR-IOV and CPU pinning.
* Mistral workflows - ability to drive Tacker workflows through Mistral


## 2.2  Opensourse MANO


OSM provides an integrated building block that combines NFVO and  Generic VNFM in one component name Resource Orchestrator

**<center>Architecture:</center>**
![](https://i.imgur.com/DXrWrKI.png)

![](https://i.imgur.com/FueHLhO.png)



:::success

https://osm.etsi.org/docs/user-guide/02-osm-architecture-and-functions.html 
https://osm-download.etsi.org/ftp/Documentation/201902-osm-scope-white-paper/#!00-introduction.md 
https://osm.etsi.org/docs/developer-guide/02-developer-how-to.html 

:::

* Installation Guide: 	https://osm.etsi.org/docs/user-guide/03-installing-osm.html 
* User Guide for VNF’s: 	https://osm.etsi.org/docs/vnf-onboarding-guidelines/ 
* Source Code: 		https://osm-download.etsi.org/repository/osm/debian/ReleaseNINE/dists/stable/ 

## 2.3  Openbaton

![](https://i.imgur.com/p9bJZUC.png)

![](https://i.imgur.com/DW3Cp2T.png)




## 2.4  ONAP 

### TODO


## 2.5 MANO Comparison

| Features | Tacker | ETSI OSM | OpenBaton | ONAP |
| -------- | -------- | -------- | -- | -- |
|Apache 2.0 license |y |y |y |
|Release | Wallaby | 9.0 | 6.0 |
|Prog Lang | python| python  | java- go/python (ext) |
|Mature | | y  | |
|ETSI NVFMANO Compliant | y | y |
|multi-node | | y | |
|NFVO | y| y | y | y | y |
|Generic VNFM | y | y | y | y |
|Specific VNFM support | y | y |y |
|k8s support | y | y | y |
|OpenStack | y | y | y | y |
|Multiple OpenStack | y | y | y |
|Other VIM | y | y | y |
|TOSCA | y | | |
|Yang | | | | y |
|Dashboard | y | y | y | y
|Service Orchestrator | | LCM  | |


# 3. Virtual Infastructure Managers

|ViM Categories based on deployment type |      |
| ---------------------------------------- | --- |
| Kubernetes  | for CNF |
| Openstack    | for VNF |
| Openstack + Kubernetes | CNF+VNF |
| Fog05 | |
| OpenVIM | |
| OpenNebula | |

## 3.1 Openstack

**Deployment & Lifecycle Methods**

![](https://i.imgur.com/KbD0ppn.png)


**Openstack Modules**
![](https://i.imgur.com/Vbk1C3c.png)


## Vanilla Openstack Production Requirements

---
### <center>Architecture</center>

![](https://i.imgur.com/F7qwI5E.png)

**Controller**
The controller node runs the Identity service, Image service, Placement service, management portions of Compute, management portion of Networking, various Networking agents, and the Dashboard. It also includes supporting services such as an SQL database, message queue, and NTP.


**Compute**
The compute node runs the hypervisor portion of Compute that operates instances. By default, Compute uses the KVM hypervisor. The compute node also runs a Networking service agent that connects instances to virtual networks and provides firewalling services to instances via security groups.You can deploy more than one compute node. 

**Block Storage**
The optional Block Storage node contains the disks that the Block Storage and Shared File System services provision for instances.

For simplicity, service traffic between compute nodes and this node uses the management network. Production environments should implement a separate storage network to increase performance and security.

**Object Storage**
The optional Object Storage node contain the disks that the Object Storage service uses for storing accounts, containers, and objects.

---
 
:::danger
Problem: But we need many of them !!! WHY ??
:::
# 4. Openstack on the EDGE (scenarios)

---
# Openstack distributed Deployment Scenarios
## Distributed Control Plane Scenario 


This design describes an architecture in which an independent control plane is placed within each edge site. A deployment of this type will benefit from greater autonomy in the event of a network partition between the edge site and the main datacenter. This comes at the cost of greater effort in maintaining a large quantity of independent control planes.

:::success
This scenario applies also to StarlingX Distr Cloud implementation
```
https://docs.starlingx.io/deploy_install_guides/r4_release/distributed_cloud/index.html
```
:::

![](https://wiki.openstack.org/w/images/9/95/Edge_Ref_Mod_Distributed.jpeg)

### Distributed Scenario I

![](https://wiki.openstack.org/w/images/c/c6/Edge_reference_architectures_Distributed_003.png)

While we defined multiple layers of edge sites in a hierarchical structure we don't require all of them present in order to apply the minimal reference architecture. In a distributed control plane scenario we consider a deployment without the 'small edge' nodes present to be a valid architecture option as well as it is depicted below.


![](https://wiki.openstack.org/w/images/2/26/Edge_reference_architectures_Distributed_004.png)

---

## Centralized Control Plane Scenario
This design describes an architecture in which edge and far edge cloudlets are managed from a control plane in a Main datacenter. A deployment of this type will enjoy simplified management of compute resources across regions. This comes at the cost of losing the ability to manage instances in an edge or far edge cloudlets during a network partition between the Regional data center and Edge.


![](https://wiki.openstack.org/w/images/b/b0/Edge_Ref_Mod_Centralized.jpeg)




### Centralized Case I
---

![](https://wiki.openstack.org/w/images/9/9f/Edge_reference_architectures_Central_001.png)



While we defined multiple layers of edge sites in a hierarchical structure we don't require all of them present in order to apply the minimal reference architecture. In a centralized control plane scenario we consider a deployment without the 'large/medium edge' nodes present to be a valid architecture option as well as it is illustrated below.

In this case the centralized DC has all the required control plane services which doesn't require the large/medium edge site present if the use case doesn't demand it in order to have the expected functionality available on the small edge site.

---
### Centralized Case II

![](https://wiki.openstack.org/w/images/4/4f/Edge_reference_architectures_Central_002.png)


:::danger
Problem: If we need HA Openstack infastacture and take advantage of container features ??
:::

# Distributed Openstack Deployments on K8s with OSH

The goal of OpenStack-Helm is to provide a collection of Helm charts that simply, resiliently, and flexibly deploy OpenStack and related services on Kubernetes.

* The OpenStack-Helm project provides a framework to enable the deployment, maintenance, and upgrading of loosely coupled OpenStack services and their dependencies individually or as part of complex environments.

* OpenStack-Helm is essentially a marriage of Kubernetes, Helm, and OpenStack, and seeks to create Helm charts for each OpenStack service. These Helm charts provide complete life cycle management for these OpenStack services.
 
* Users of OpenStack-Helm either deploy all or individual OpenStack components along with their required dependencies. It heavily borrows concepts from Stackanetes and complex Helm application deployments. Ideally, at the end of the day, this project is meant to be a collaborative project that brings OpenStack applications into a cloud-native model.


[Deploy Openstack using Openstack Helm Infra scripts](https://github.com/danpawlik/openstack-helm-deployment)

[Multinode Installation - Openstack Helm](https://docs.openstack.org/openstack-helm/latest/install/multinode.html)



# Deploying both ViM's- Provisioning of Kubernetes w/o Openstack 

**Manually**

- KubeADM
- Kurl
- kubeSpray
- Bootkube
- Kubernetes Operations (kops)
- Canonical JuJu ( charmed K8s with openstack)
- Openstack Magnum on top of Openstack 




:::success
Kops,kubeadm can deploy the oposite as well--> k8s on Openstack
*Example with kubeadmin*
```
https://kubernetes.io/blog/2020/02/07/deploying-external-openstack-cloud-provider-with-kubeadm/
```
*Example with kops*
```
https://kops.sigs.k8s.io/getting_started/openstack/
```
:::

:::danger
Problem: MANO's does not instantiate K8s & VIM clusters. You have to have it before hand. This means that you have to have your ViM's (Kubernetes or Openstack) ready
:::


## Automating the Provisioning of Kubernetes (Clusters as a Service)

- Airship ( KubeADM with ClusterAPI)
- Gardener (ClusterAPI)
- Rancher ( need research)
- MAAS ( Metal as a Service) + Juju charmed K8s

:::success
Airship and Rancher can deploy Openstack Helm and K8s on demand
:::


## 3.2 StarlingX [Openstack/K8s]


### Integrating OpenStack, Kubernetes and Ceph(storage backend)

StarlingX is a fully integrated, open source platform that is supported by the Open Infrastructure Foundation. The project integrates together open source projects such as Ceph, Kubernetes, the Linux kernel, OpenStack


### <center>Architecture</center>

**Controller Node**

![](https://i.imgur.com/SS8d5iU.png)
---

**Worker Node**

![](https://i.imgur.com/feYbuh4.png)
---

---

**HA Controller Nodes**
![](https://i.imgur.com/IfYAmo6.png)

---

**Networking Layer**
![](https://i.imgur.com/Zj3Tg5Q.png)


---

![](https://i.imgur.com/x3tvCaG.png)



### EDGE Sub-clouds

A distributed cloud system consists of a central cloud, and one or more subclouds connected to the System Controller , which is a central cloud region, over L3 networks as it is shown on the diagram below

![](https://i.imgur.com/Si3QmW5.png)

***Central cloud***
The central cloud provides a region for managing the physical platform of the central cloud. The System Controller component is responsible for managing and orchestrating over the subclouds.

***System Controller***
In the Horizon GUI, System Controller is the name of the access mode, or region, used to manage the subclouds:

* You can use it to add subclouds, synchronize select configuration data across all subclouds and monitor subcloud operations and alarms. Software updates for the subclouds are also centrally managed and applied.

* DNS, NTP, and other configuration settings are centrally managed at the System Controller and pushed to the subclouds in parallel to maintain synchronization across the distributed cloud infrastructure.

***Edge Subclouds***
The subclouds are StarlingX Kubernetes edge systems/clusters used to host containerized applications: 

* Alarms raised at the subclouds are sent to the System Controller for central reporting. Any type of StarlingX Kubernetes configuration, (including single-server, dual-redundant servers, or standard cluster with or without storage nodes), can be used for a subcloud. 
 
* Remote nodes can survive isolation from the control plane and continue to operate and re-synchronize upon reconnection. All control functions can exist at all sites. 

* Remote sites can be zero-touch enrolled and replicated across thousands of sites with fully automated deployment of known-good configurations.

**STARLINGX DEPLOYMENT MODELS**

StarlingX supports the scalable deployment models from small to large:

- All-in-one Simplex: the Simplex configuration runs all edge cloud functions (controller, compute, and storage) on one physical server. This configuration is intended for very small Edge sites that do not require high availability.
- All-in-one Duplex: the Duplex configuration runs all edge cloud functions (controller, compute, and storage) on one physical server. There is also a second physical server in the system for active / standby based high availability for all platform and application services.
- Standard with Controller Storage: this configuration allows for 2x controller physical servers to provide storage for the edge cloud. High availability services run across the controller nodes in either active/active or active/standby mode. The configuration also allows between one and 99 compute physical servers to run application workloads. This configuration works best for edge clouds with smaller storage needs.
- Standard with Dedicated Storage: this configuration has dedicated storage servers in addition to the controller and compute physical servers. This configuration is best for edge clouds requiring larger amounts of storage capacity.



### Requirements
#### StarlingX R4.0 virtual All-in-one Duplex

Hardware requirements:

* Processor: x86_64 only supported architecture with BIOS enabled hardware virtualization extensions 
* Cores: 8
* Memory: 32GB RAM
* Hard Disk: 500GB HDD
* Network: One network adapter with active Internet connection
* A workstation computer with Ubuntu 16.04 LTS 64-bit

#### StarlingX R4.0 bare metal Standard with Controller Storage

Release 4.0 is Kubernetes 1.18 certified distribution 

| Minimum Requirements | Controller Node | Worker Node |
| -------- | -------- | -------- |
| Number of servers  | 2 | 2-10 |
| Minimum processor class | Dual-CPU Intel® Xeon® E5 26xx family (SandyBridge) 8 cores/socket | |
| Minimum memory | 64 GB | 32 GB |
| Primary disk | 500 GB SSD or NVMe (see Configure NVMe Drive as Primary Disk) | 120 GB (Minimum 10k RPM) |
| Additional disks | 1 or more 500 GB (min. 10K RPM) for Ceph OSD | Recommended, but not required: 1 or more SSDs or NVMe drives for Ceph journals (min. 1024 MiB per OSD journal) | For OpenStack, recommend 1 or more 500 GB (min. 10K RPM) for VM local ephemeral storage |
| Minimum network ports | Mgmt/Cluster: 1x10GE, OAM: 1x1GE | Mgmt/Cluster: 1x10GE, Data: 1 or more x 10GE|

:::danger
Problem: StarlingX does not deploy itself automaticaly to support additional sub-clusters?
:::

:::success
We can accomplish that with Airship or Gardener
:::
# Important Features for MEC

## Lifecycle Management Module
## Workflow Module 
- Argo Workflows
- Mistral Openstack Module

## Live Migration Management (LMM) and State Management (SM)
- For MEA

## Service Function Chaining
:::success
Also used by OSMANO
:::

**Service Function Chaining** is a mechanism for overriding the basic destination based forwarding that is typical of IP networks. It is conceptually related to Policy Based Routing in physical networks but it is typically thought of as a Software Defined Networking technology. It is often used in conjunction with security functions although it may be used for a broader range of features. Fundamentally SFC is the ability to cause network packet flows to route through a network via a path other than the one that would be chosen by routing table lookups on the packet’s destination IP address. It is most commonly used in conjunction with Network Function Virtualization when recreating in a virtual environment a series of network functions that would have traditionally been implemented as a collection of physical network devices connected in series by cables.

A very simple example of a service chain would be one that forces all traffic from point A to point B to go through a firewall even though the firewall is not literally between point A and B from a routing table perspective.

A more complex example is an ordered series of functions, each implemented in multiple VMs, such that traffic must flow through one VM at each hop in the chain but the network uses a hashing algorithm to distribute different flows across multiple VMs at each hop.

* Source: https://opendev.org/openstack/networking-sfc
* Documentation: https://docs.openstack.org/networking-sfc/latest
* Overview: https://launchpad.net/networking-sfc




# Appendix

## Openstack components


| Compute  |     |
| -------- | -------- |
| NOVA | Compute Service |
| ZUN  | Containers Service |


| Hardware Lifecycle |     |
| -------- | -------- |
| IRONIC | Bare Metal Provisioning Service |
| CYBORG | Lifecycle management of accelerators |

| Storage | |
| -------- | -------- |
| SWIFT | Object store |
| CINDER | Block Storage |
| MANILA   | Shared filesystems |


| Networking | |
| -------- | -------- |
| NEUTRON | Networking |
| OCTAVIA | Load balancer |
| DESIGNATE | DNS service |

| Shared Services | |
| -------- | -------- |
| KEYSTONE | Identity service | 
| PLACEMENT | Placement service |
| GLANCE | Image service |
| BARBICAN | Key management |

| Orchestration | |
| -------- | -------- |
| HEAT | Orchestration |
| SENLIN | Clustering service |
| MISTRAL | Workflow service |
| ZAQAR | Messaging Service |
| BLAZAR | Resource reservation service |
| AODH | Alarming Service |

| Workload Provisioning | |
| -------- | -------- |
| MAGNUM | Container Orchestration Engine Provisioning |
| SAHARA | Big Data Processing Framework Provisioning |
| TROVE | Database as a Service |

| Application Lifecycle | |
| -------- | -------- |
| MASAKARI | Instances High Availability Service |
| MURANO | Application Catalog |
| SOLUM | Software Development Lifecycle Automation |
| FREEZER | Backup, Restore, and Disaster Recovery |

| API Proxies | |
| -------- | -------- |
| EC2API | EC2 API proxy |

| Web Frontend | |
| -------- | -------- |
| HORIZON | Dashboard |



## StarlingX components

![](https://i.imgur.com/8i04rIU.png)

## Opensource MANO components

```
     __________                                                              ________
    |          |                                                            |        |
    | light-ui |OSM_SERVER               _______                            |keystone|
    | :80      |----------------------> |       |-------------------------> |:5000   |
    |__________|                        | nbi   |                           |________|
                     OSMNBI_STORAGE_PATH| :9999 |OSMNBI_DATABASE_HOST        _______ 
    .............. <--------------------|_______|-------------------------> |       |
    . volume:    .                                                          |       |
    . osm_osm_   .                                                          | mongo |
    . packages   .   OSMLCM_STORAGE_PATH _______ OSMLCM_DATABASE_HOST       | :27017|
    .............. <--------------------|       |-------------------------> |_______|
                                        | lcm   |
    **************       OSMLCM_VCA_HOST|       |OSMLCM_RO_HOST
    * lxd: juju  * <--------------------|_______|--------------|
    * controller *                                             |
    **************                       _______               |             _______
                                        |       | <-------------            |       |
                                        | ro    |                           | mysql |  
                                        | :9090 |RO_DB_HOST                 | :3306 |
                                        |_______|-------------------------> |_______|

     _______     _______                 _______                             _________
    |       |   |       |               |       |                           |         |
    | mon   |   | pm    |               | kafka |KAFKA_ZOOKEEPER_CONNECT    |zookeeper|  
    | :8662 |   |       |               | :9092 |-------------------------> | :2181   |
    |_______|   |_______|               |_______|                           |_________|
```

| COMPONENTS | DESCRIPTION |
| -------- | ------------- |
| kafka | Provides a Kafka bus used for OSM communication. This module relies on zookeeper. |
| zookeeper | Used by kafka. |
|nbi | North Bound Interface of OSM. Restful server that follows ETSI SOL005 interface. Relies on mongo database and kafka bus. For authentication it can optionally uses keystone. |
| keystone | Used for NBI authentication and RBAC. It stores the users, projects and role permissions. It relies on mysql. |
| lcm | Provides the Live Cycle Management. It uses ro for resource orchestration and juju for configuration. It relies also on mongo. |
| ro | Makes the Resource Orchestration, or VIM deployment. Relies on mysql. |
| light-ui | Web user interface. It communicates with nbi. |
| mon | Performs OSM monitoring. Relies on mongo and mysql. |
| mongo | Common non relational database for OSM modules. |
| mysql | Relational database server used for ro, keystone, mon and pol. |
| pol | Policy Manager for OSM. |
| prometheus | for monitoring. |

### Resource Orchestrator module - RO 

The RO component is capable of deploying networking services over OpenStack, VMware, and OpenVIM.

<p>NFVO engine itself is embedded in RO module and essentially is split between nfvo.py, nfvo_db.py and vim_thread.py. Regarding the first two, changes were made to process the new VNFFG element of a Network Service Descriptor (NSD), provided via YAML file, and store its information in internal database tables, also mapping to existing descriptors such as VNFDs.</p>
<p>An HTTP server runs automatically and interacts with the Resource Orchestrator Engine to provide/request data. 
Regarding vim_thread.py, changes were made so that, when a Network Scenario Instance is created, it is able to process existing NSDs that include a VNFFGD. 
<p>This module’s role is to call the VIM Plugin component and to support VNFFGD, special calls have to be made to the VIM Plugin. Essentially, the upper part of Resource Orchestrator Engine will insert tasks in the VIM thread, so that it can call VIM connector’s normalized interface for the creation of networks, functions, service function chains (the eventual transformation of a VNFFG), etc</p>


- The API Service & Utilities endpoint provides the interface into the RO (for the LCM to consume) and provides a number of utilities for internal to RO consumption.
- The Resource Orchestration Engine manages and coordinates resource allocations across multiple geo-distributed VIMs and multiple SDN controllers.
- The VIM and SDN Plugins connect the Resource Orchestration Engine with the specific interface provided by the VIMs and SDN controllers.


Directory Organization
The code organized into the following high level directories:

* RO/ main RO server engine
* RO/osm_ro/ contains the RO server code files
* RO/test/ contains scripts and code for testing
* RO/database_utils/ contains scripts for database creation, dumping and migration
* RO/scripts/ general scripts, as installation, execution, reporting
* RO/scenarios/ examples and templates of network scenario descriptors
* RO/vnfs/ examples and templates of VNF descriptors
* RO-client/ contains the own RO client CLI
* RO-SDN-*/ contains the SDN plugins
* RO-VIM-*/ contains the VIM plugins

### RO Architecture

![](https://osm.etsi.org/docs/developer-guide/_images/400px-OpenmanoArchitecture.png)

**RO Server modules**
The RO module contains the following modules:

* openmanod.py is the main program. It reads the configuration file (openmanod.cfg) and execute httpserver and wait for the end
* httpserver.py is a thread that implements the northbound API interface, uses python bottle module. Calls main engine methods to perform the tasks
* nfvo.py is the main engine, implementing all the methods for the creation, deletion and management of vnfs, scenarios and instances. Operations against a VIM are asynchornous. ACTIONs to be done are stored at database before returning ok to the client
* nfvo_db.py is the module in charge of database operations. Uses base db_base.py. Database is managed with MySQLdb python library
* openmano_schemas.py is a dictionary schemas used to validate API request and response content using jsonschema library
* vim_thread.py There is a thread per VIM and credentials. It performs basic tasks of creating/deleting VM, networks, flavors, etc. In addition it refreshes the VM and network status. It calls vimconn.py methods
* vimconn.py is the base class for the VIM plugin. It contains the definition of the methods to be implemented. The inherited - console_proxy_thread.py is a thread that implements a TCP/IP proxy for the console access to ## a VIM.

**RO Client modules**
Other modules not part of the server are:

roclient.py is a CLI client

**ACTIONS and TASKS**

ACTIONS: A group of tasks performed against a a concrete instance-scenarios (NS record). The creation and deletion of the instance-scenario itself is an action. As it is asynchonous, NBI returns an “Action_id” that can be used to check the status. For each action, nfvo.py generates individual tasks for the related VIMs. Tasks are both stored at database and sent to the related vim_thread.py. A task has a concrete relation with a VIM, e.g. create/delete a VM, a network, look for a flavor network

### Lifecycle Module - LCM

The most important role of any MANO framework is the life-cycle management of every NS.From instantiation to termination, the MANO executes all tasks related to placement calculations, healing, reconfiguration, etc . In OSM, these actions are performed by the RO module. 

In particular, regarding the run-time reconfiguration of the VNFs the RO module in collaboration with the Lightweight Management uses Juju charms to manage the complete life cycle of the VNF, including software installation, configuration, clustering, and scaling.

There are two types of charms; proxy and machine:
* Proxy charms are operating remotely from a VNF (running on a VM or physical device) using SSH.
* Machine charms are written to inside the VNF and handles the complete life cycle of the VNF from installation to removal



![](https://i.imgur.com/6VudQmW.png)
* A VNF package is instantiated via the LCM.
* The LCM requests a virtual machine from the RO.
* The RO instantiates a VM with your VNF image.
* The LCM instructs N2VC, using the VCA, to deploy a VNF proxy charm, and tells it how to access your VM (hostname, user name, and password).

https://osm.etsi.org/wikipub/index.php/Creating_your_VNF_Charm
### Northbound Interface - NBI module

The Northbound interface is based on REST and it allows performing actions over the following entities:

* Tenant: Intended to create groups of resources. In this version no security mechanisms are implemented.
* Datacenters: Represents the VIM information stored by openmano.
* VIMs: used to perform an action over a datacenter (specific pool of resources)
* VNFs: SW-based network function, composed of one or several VM that can be deployed on an NFV datacenter.
* Scenarios: topologies of VNFs and their interconnections.
* Instances: Each one of the Scenarios deployed in a datacenter.

**Swagger Schema for NBI** 
```
https://forge.etsi.org/swagger/ui/?url=https%3A%2F%2Fosm.etsi.org%2Fgitweb%2F%3Fp%3Dosm%2FSOL005.git%3Ba%3Dblob_plain%3Bf%3Dosm-openapi.yaml%3Bhb%3DHEAD
```
### OSM Client

The RO code contains a python CLI (openmano) that allows friendly command execution. This CLI client can run on a separate machine where openmano server is running. openmano config indicates where the ## server is (by default localhost).
```
https://osm.etsi.org/wikipub/index.php/OSM_client
```

### VNF Configuration and Abstraction (VCA)

VCA module performs the initial VNF configuration using Juju Charms. Considering this purpose, the VCA module can be considered as a generic VNFM with a limited feature set.

- Juju Controller/Client --> interact with Proxy Charms
- LXD Containers (Host the proxy Charms)
- Proxy Charms


### Opensource MANO Container List

![](https://i.imgur.com/k7BlQt6.png)



## Tacker components

| Components  | |
| -------- | -------- |
| tacker-client | provides CLI and communication with Tacker via REST API |
| server | provides REST API and calls conductor via RPC. |
|conductor | implements all logics to operate VNF and call required drivers providing interface to NFV infrastructures | 
| infra-driver | responsible for exact actions to operate OpenStack or Kubernetes resources |
| vim-driver | responsible for registration of VIM. |
|mgmt-driver | responsible for exact actions to configure VNFs. |
| monitor-driver | responsible for exact actions to monitor VNFs |
| policy-driver | responsible for policy based VNF operations. |

## Airship components

**Airship**
Airship is a collection of loosely coupled and interoperable open source tools that declaratively automate cloud provisioning.

Airship is a robust delivery mechanism for organizations who want to embrace containers as the new unit of infrastructure delivery at scale. Starting from raw bare metal infrastructure, Airship manages the full lifecycle of data center infrastructure to deliver a production-grade Kubernetes cluster with Helm deployed artifacts, including OpenStack-Helm. Airship allows operators to manage their infrastructure deployments and lifecycle through the declarative YAML documents that describe an Airship environment.

![](https://i.imgur.com/D8kDclS.png)

| COMPONENTS | DESCRIPTION            |
| -------- | ------------------------------------- |
| Airship Shipyard | provides single front door for Airship, and is the minimal surface area that an Airship operator will typically need to work with. It exposes two primary APIs (deploy_site and update_site), along with the ability to interrogate workflow status, view logs, and stop workflows. Shipyard persists all site definition documents to Deckhand, and supports the ability for different deployment engineer roles to push different sets of documents prior to a deployment or upgrade. Shipyard executes graph-based workflows via Apache Airflow to orchestrate the other Airship components.    |
| Airship Promenade | provides declarative, YAML-driven deployment of a highly-available, self-hosted Kubernetes cluster.    |
| Airship Pegleg | provides gitops-oriented YAML document management: document aggregation, validation, and (as appropriate) encryption.    |
| Airship Drydock | provides pluggable, YAML-driven deployment of bare metal hosts. Canonical MaaS is the initial plugin implementation, and a temporary Airship MaaS fork hosts a small number of changes that allow MaaS to operate in a Kubernetes context.    |
| Airship Deckhand | provides YAML document management, storage, and validation. Deckhand supports Barbican-backed secret encryption, and data de-duplication features such as layering and substitution.    |
| Airship Armada | provides declarative, YAML-driven deployment of complex Helm-based application on top of Kubernetes. It orchestrates Helm Tiller with functionality to sequence or parallelize deployment of related Helm charts, delete or updating Kubernetes resources via pre/post-upgrade hooks, and wait (and time out) on labeled Kubernetes resources to become active. By default, Armada executes Helm Tests for all charts that support them.    |
| Airship Divingbell | provides day 1 and day 2 configuration management for bare metal hosts. It currently supports setting/changing sysctl tunables, host level limits, mounts, NIC tunables, and user management. It creates a framework for package management and file permission enforcement in the future.    |
| Airship Treasuremap | provides a realistic collection of reference deployment YAML documents, which can be copied and modified per operator use cases and compute/network configuration. These YAMLs also integrate specific builds of the different Airship components together, and merges to Treasuremap are protected via a nightly CICD job (Seaworthy) to ensure that Treasuremap always represents a solid, operating integration of Airship. |
|Airship Berth | is a minimalist VM runner for Kubernetes, and provides a means to deploy virtual machines via a Helm chart. The VMs share the lifecycle of their corresponding Kubernetes pods.    |
|          | Airship leverages the Calico software-defined networking provider out of box, allowing operators to define advanced network policy for their deployments.    |
|      | Airship leverages Keystone-based authentication across components, as well as oslo_policy for role-based access definition.

## Helm
You can think Helm as the apt-get / yum of Kubernetes, it is a package manager for Kubernetes. 

Helm “Charts” is the terminology that Helm uses for a package of configured Kubernetes resources.

If you deploy applications to Kubernetes, Helm makes it incredibly easy to:

1. version deployments
2. package deployments
3. make a release of it
4. deploy, delete, upgrade
5. rollback those deployments as “charts”.




## TOSCA (OASIS)

An OASIS standard, TOSCA (Topology and Orchestration Specification for Cloud Applications) aims to standardize how to describe software application and everything that is needed to run that application in cloud environments. 

TOSCA is designed to facilitate the ‘portability’ and ‘lifecycle management’ of cloud services. TOSCA supports many cloud orchestration tools such as OpenStack Heat, Cloudify





# Additional Tooling & Projects

**Networking** 
* https://networkservicemesh.io/
* https://submariner.io/

**Provisioning**


**Orchastrators**
* Cloudify

**Security**
* https://github.com/IBM/portieris (Container Image Signature Validation) - to be featured in starlingx 

**Coding Style and Rules**

* https://12factor.net/



## Canonical Specific Tools

**Proxy charms and VNF development**
* https://github.com/5GinFIRE/ffmpeg_transcoder_vnf
* https://osm.etsi.org/wikipub/index.php/Creating_your_VNF_Charm


**MAAS Provisioning**
* https://medium.com/@madushan1000/how-to-run-kubernetes-on-bare-metal-with-maas-juju-d5ba8e981710
* https://technology.amis.nl/platform/kubernetes/charmed-kubernetes-on-kvm-using-maas-and-juju/



# References

1. https://wiki.openstack.org/wiki/Edge_Computing_Group/Edge_Reference_Architectures#Distributed_Control_Plane_Scenario
2. https://object-storage-ca-ymq-1.vexxhost.net/swift/v1/6e4619c416ff4bd19e1c087f27a43eea/www-assets-prod/presentation-media/OpenStack-Summit-Edge-Computing-Operations2.pdf
3. https://www.airshipit.org/collateral/Airship_2.0_White_Paper.pdf
4. https://docs.airshipit.org/
5. https://www.dropbox.com/s/ihczi2f5odccn6f/SynchFramework-DC-StarlingX.pptx?dl=0
6. https://opnfv-edgecloud.readthedocs.io/en/latest/development/requirements/requirements.html
7. https://res.mdpi.com/d_attachment/jsan/jsan-09-00004/article_deploy/jsan-09-00004-v3.pdf
8. https://docs.openstack.org/networking-sfc/latest/
9. https://docs.openstack.org/tacker/latest/user/architecture.html
10. https://wiki.openstack.org/wiki/Tacker
11. https://opnfv-airship.readthedocs.io/en/latest/release/installation/installation.instruction.html
12. https://indico.cern.ch/event/915714/attachments/2100077/3530496/Webinar__Managing_OpenStack_with_Kubernetes.pdf
13. https://object-storage-ca-ymq-1.vexxhost.net/swift/v1/6e4619c416ff4bd19e1c087f27a43eea/www-assets-prod/summits/27/presentations/24092/slides/CERN-OpenStack-Cloud-control-plane-from-VMs-to-K8s2.pdf
14. https://www.researchgate.net/publication/332630638_Benchmarking_Open-Source_NFV_MANO_Systems_OSM_and_ONAP
15. https://thenewstack.io/de-ossify-the-network-with-function-virtualization/
16. https://thenewstack.io/opensource-nfv-part-4-opensource-mano/
17. https://sdn.ieee.org/home/sitemap/22-newsletter/july-2016/59-opensource-mano


