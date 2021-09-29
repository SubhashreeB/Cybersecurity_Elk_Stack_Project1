## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Diagram/NetworkDiagram_Elkstack.drawio.png]

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the file may be used to install only certain pieces of it, such as Filebeat.

  - _Ansible Configuration file_
  - _Ansible hosts file
  - _DVWA Webserver playbook_
  -  _Elk Stack playbook_
  -  _Filebeat playbook_
  -  _Filebeat config file_
  -  _Metricbeat playbook_
  -  _Metricbeat config file_

Copy _Ansible Configuration file_ to your /etc/ansible directory.
- Change remote_user inside the ansible.cfg file.
    ![Images/ansible_file.png](:/ansible_file)
- Inside hosts file include webserver and elk server ip addresses details as below:
![Images/hosts_file.png](:/hosts_file)
Here webservers are Web1, Web2 and Web3 and elk is the Elk server.
- Generate SSH public key from the container:
~/.ssh#ssh-keygen
~/.ssh#cat id_rsa-pub
- Assign the above generated public key along with username for Web1, Web2, Web3, Elk Virtual machine in Azure graphical user interface.
	This can be done by going to Web1/Web2/Elk Server -> Reset Password -> Reset SSH Public Key.
	![Images/remote_pwd.png](:/remote_pwd)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
- _What aspect of security do load balancers protect?_
 Answer: 
 Load balancer ensures that the application will be highly available, traffic is well distributed. Addition to Load Balancer sits between internet and webservrs so direct access to servers are restricted.
 - _What is the advantage of a jump box?_
 Answer: 
 Jumob box is the only gateway for access to infratructure resuing the size of any potential attack.This ultimately sets the JumpBox as a SAW (Secure Admin Workstation). All Administrators when conducting any Administrative Task will be required to connect to the JumpBox before perfoming any task/assignment.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the jumpbox and system network.
- _What does Filebeat watch for?_
Answer:
Filebeat is a lightweight shipper for forwarding and centralizing log data. Installed as an agent on the servers, Filebeat monitors the log files or locations , collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
- _What does Metricbeat record?_
Answer:
Metricbeat is installed on the servers to periodically collect metrics from the operating system and from services running on the server. Metricbeat takes the metrics and statistics that it collects and ships them to the output specified, such as Elasticsearch or Logstash.
Metricbeat records ativity within the operating system or hardware such as processor or memory or storage utilization.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Ubuntu LTS 20.04            |
| Web1     | Webserver| 10.0.0.5   | Ubuntu LTS 20.04            |
| Web2     | Webserver| 10.0.0.6   | Ubuntu LTS 20.04            |
| Web3     | Webserver| 10.0.0.9   | Ubuntu LTS 20.04            |
| Elk-Server | Monitoring | 10.1.0.5   | Ubuntu LTS 20.04            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _My Home IP Address which is whitelisted ip address_
You can get your ipv4 personal ip by running https://whatismyipaddress.com on your browser.   
Machines within the network can only be accessed by jump box provisioner.
- Which machine did you allow to access your ELK VM? What was its IP address?
  Answer: Jump Box Provisoner can accept connections from the Internet. Access to this machine is only allowed using output from what is my ipv4 address.
  My System Ip address : output from what is my ipv4 address

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | Home IP address    |
| Web1     |   No                |   10.0.0.4         |
|  Web2    |  No                   | 10.0.0.4        |
|  Web2    |  No                   | 10.0.0.4        |
|  Web3    |  No                   | 10.0.0.4        |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _What is the main advantage of automating configuration with Ansible?_
We could deploy multiple servers easily and quickly without having to physically touch each server. 
The playbook implements the following tasks:
- _installs docker.io, pip3, and the docker module._
![Images/dockerio_install.png]
- Increase the virtual memory to 262144 and ensure it does so automatically upon restarting the machine.
- ![Images/elk_increasememory.png]
- uses sysctl module
![Images/sysctl.png]
- downloads and launches the docker container for elk server and set policy
![Images/launch_elk]

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Images/elkserver-status.png]

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _Web1 : 10.0.0.5_
- _Web2 : 10.0.0.6_
- _Web3 : 10.0.0.9_

We have installed the following Beats on these machines:
- _Filebeat_
- _Metricbeat_

These Beats allow us to collect the following information from each machine:
- _Filebeat watches for log files/locations and collect log events and send it to Elasticsearch._
![Images/Filebeat_syslog.png]
- _Metricbeat records metrics and statistical data from the operating system and from services running on the server and pass it to elasticsearch._
![Images/Metricbeat-logs.png]

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml and metricbeat-config.yml file to /etc/ansible/.
- Update the hosts file to include ip address of elk server and web servers.
- Run the playbook, and navigate to http://[elk_server_ip_address]/app/kibana to check that the installation worked as expected.
![Images/Kibana_home.png]

- _Which file is the playbook?_
- 
- _Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
