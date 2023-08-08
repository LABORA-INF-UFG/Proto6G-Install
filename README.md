# Proto6G-Install
This repository describes steps for installing the elements that make up Proto6G (UE-non3GPP + N3IWF + Free5gc). The installation process involves 3 different machines 1st responsible for running 5GC, 2nd responsible for running N3IWF and 3rd responsible for running UE-non3GPP

#### 5GC environment
* SO: Ubuntu 20.04 (LTS) x64
* Uname -r: >= 5.4.0-122-generic
* Memory: 4 GB
* Disk: 80 GB

#### N3IWF environment
* SO: Ubuntu 20.04 (LTS) x64
* Memory: 2 GB
* Disk: 20 GB

#### UE-non3GPP environment
* SO: Ubuntu 20.04 (LTS) x64
* Memory: 2 GB
* Disk: 20 GB

## Steps installation
The installation steps for each of the 3 components are described below.

#### 1º 5GC (Free5gc)
Install python + git + ansible:
```
sudo apt update && apt -y install python && sudo apt -y install git && sudo apt -y install ansible
```

Clone this repository:
```
git clone https://github.com/LABORA-INF-UFG/Proto6G-Install.git
```

Install GOLang 1.14.4:
```
cd Proto6G-Install &&  ansible-playbook -K install-golang.yml
source ~/.bashrc
```

Run ```ifconfig``` and get the name of **internet network interface**, that provides to the host with access to the internet, like as illustrated in the figure below:
<p align="center">
    <img src="images/if_config.png"/> 
</p>
This interface that will be used by the UPF to provide internet access to the UE's registered in the 5GC.

Run the following Ansible playbook (password for sudo is required):
```
cd 5GCore-easy-install && ansible-playbook -K 5gc-install.yml -e  "internet_network_interface=<< internet network interface name>>"
```

Wait a few seconds for Ansible to finish configuring the 5GC. The result of the execution should look something like the one shown in the figure below.
<p align="center">
    <img src="images/5gc_ansible_result.png"/> 
</p>

#### Initializing the 5GC functions
After installation we must initialize the 5GC. The entire installation was performed following the standard for applications written in GOLang. Go to `` /root/go/src/free5gc `` and run the following command to init 5GC functions ``./run.sh ``. After execution, the expected result is something similar to what is shown in the following figure.
<p align="center">
    <img src="images/5gc_start.png"/> 
</p>

#### Initializing the 5GC API Server
After initializing the microservices that control the data and control planes, it is also necessary to initialize the API that controls access to the 5GC database in MongoDB. This service will be used later for the registration of UEs in the 5GC.  Open another terminal and access the  `` /root/go/src/free5gc/webconsole `` directory. Then type the following command  `` go run server.go ``. . After execution, the expected result is something similar to what is shown in the following figure.
<p align="center">
    <img src="images/5gc_api_start.png"/> 
</p>