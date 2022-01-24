## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](Images/diagram_filename.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yml and config file may be used to install only certain pieces of it, such as Filebeat.

  

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly # functional and available, in addition to restricting traffic to the network.

: What aspect of security do load balancers protect?

  Load balancers add resiliency by rerouting live traffic from one server to another if a server falls prey to a DDos attack or otherwise become unavailable.

 What is the advantage of a jump box? 

  A Jump Box Provisioner is also important as it prevents Azure VMs from being exposed via a public IP Address. This allows us to do monitoring and logging on a single box. We can also restrict the IP addresses able to communicate with the Jump Box as we've done here.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network and system systemlogs.

What does Filebeat watch for?
  Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.

What does Metricbeat record?
  Metricbeats takes the metrics and statistics that it collects and ship them to the output that you specify, such as Elastic or Logstash.

The configuration details of each machine may be found below.


| Name     | Function             | IP Address                    | Operating System |
|----------|----------------------|-------------------------------|------------------|
| Jump Box | Gateway              | 10.0.0.1 / 20.85.232.66       | Linux            |
| Web-1    | UbuntuServer         | 10.1.0.5 / 40.114.124.38      | Linux            |
| Web-2    | UbuntuServer         | 10.1.0.6 / 40.114.124.38      | Linux            |
| DVWA-VM3 | UbuntuServer         | 10.1.0.7 / 40.114.124.38      | Linux            |
| ELKserver| UbuntuServer         | 10.2.0.4 / 20.84.136.248      | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
     Workstation MY Public IP through TCP 5601.

Machines within the network can only be accessed by Workstation and Jump-Box-Provisioner through SSH Jump-Box.
  Which machine did you allow to access your ELK VM?
    Jump-Box_provisioner IP: 10.1.0.4 via SSH port 22

   What was its IP address?
    Workstation MY Public IP via port 5601

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses                      |
|----------|---------------------|-------------------------------------------|
| Jump Box | Yes                 | 20.85.232.66 (Workstation IP o SSH 22)    |
| Web-1    | No                  | 10.1.0.4 on SSH 22                        |
| Web-2    | No                  | 10.4.0.4 on SSH 22                        |
| DVWA-VM3 | No                  | 10.4.0.4 on SSH 22                        |
| ELKserve | No                  | WorkStation MY Public IP using TCP 5601   |
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
  What is the main advantage of automating configuration with Ansible?
    There are multiple advantages, Ansible lets you quickly and easily deploy multi-tier application through a YAML playbook.

    You don't need to write custom code to automate your systems.

    Ansible will also figure out how to get your systems to the state you want them to be in.


The playbook implements the following tasks:
- In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._

  Specify a different group of machine:

- name: Config elk VM with Docker
    hosts: elk
    become: true
    tasks:

  Install Docker.io

- name: Install docker.io
    apt:
      update_cache: yes
      force_apt_get: yes
      name: docker.io
      state: present

 Install Python-pip
  - name: Install python3-pip
    apt:
      force_apt_get: yes
      name: python3-pip
      state: present

    #Use pip module (It will default to pip3)
  - name: Install Docker module
    pip:
      name: docker
      state: present
      `docker`, which is the Docker Python pip module.
Increase Virtual Memory
 - name: Use more memory
   sysctl:
     name: vm.max_map_count
     value: '262144'
     state: present
     reload: yes
Download and Launch ELK Docker Container (image sebp/elk)
 - name: Download and launch a docker elk container
   docker_container:
     name: elk
     image: sebp/elk:761
     state: started
     restart_policy: always
Published ports 5044, 5601 and 9200 were made available
     published_ports:
       -  5601:5601
       -  9200:9200
       -  5044:5044    
    
The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

ELKserver
  ![Elkserver](https://user-images.githubusercontent.com/85652618/150703715-437f0562-ae97-4cba-82fd-800af9c26c83.png)


 Jump-Box-Provisioner
 ![Provisioner](https://user-images.githubusercontent.com/85652618/150703734-ba2874f9-52de-4fc2-85c8-ddc3a6d3fdfd.png)
 
 Web-1
 ![WEB-1](https://user-images.githubusercontent.com/85652618/150703768-a852487a-d583-4a17-b9c4-d576cd4a01bd.png)

 Web-2
 ![WEB-2](https://user-images.githubusercontent.com/85652618/150703785-a8d3055c-de5f-4a86-98de-9b817314716d.png)

  DVWA-VM3
  ![VM3](https://user-images.githubusercontent.com/85652618/150703816-9fe6dc25-16a7-4c58-8fdb-021b86c52572.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- List the IP addresses of the machines you are monitoring
    Web-1: 10.1.0.5
    Web-2: 10.1.0.6
    DVWA-VM3: 10.1.0.7

We have installed the following Beats on these machines:

  Filebeat

    ![filebeat success](https://user-images.githubusercontent.com/85652618/150703890-23768563-8ecb-490d-8098-2660798ea368.png)


  Metricbeat

    ![metricbeat success](https://user-images.githubusercontent.com/85652618/150703903-b463cf69-349b-4b5b-9b1b-79f1b12f2353.png)
  

These Beats allow us to collect the following information from each machine:

Filebeat will be used to collect log files from very specific files such as Apache, Microsoft Azure tools and webservers, MySQL databases.

  ![Filebeat_System Syslog dashboard](https://user-images.githubusercontent.com/85652618/150703981-02c58eab-bdd9-4392-b810-5d665fea4ec7.png)

Metricbeat will be used to monitor VM stats, per CPU core stats, per filesystem stats, memory stats and network stats.

  ![Metricbeat Docker Overview ECS dashboard](https://user-images.githubusercontent.com/85652618/150703995-e496df8d-5c9f-423e-9d39-57762355c694.png)

  ![Metricbeat Docker Web-1_metrics](https://user-images.githubusercontent.com/85652618/150704122-2f2003c9-8ad6-43f4-a322-179367a3ee21.png)

  ![Metricbeat Docker Web-2_metrics](https://user-images.githubusercontent.com/85652618/150704035-087e2d1b-1824-4219-8d5d-3a50f8074300.png)

  ![Metricbeat Docker DVWA-VM3_metrics](https://user-images.githubusercontent.com/85652618/150704047-c5a7e474-aef9-470f-a7e6-44521d05f5a6.png)

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

  Verify the Public IP address to see if it has change. www.whatismyip.com

  If change then update the Security Rules that uses the My Public IPv4

SSH into the control node and follow the steps below:
- Copy the yml file to ansible folder.
- Update the config file to include remote users and ports
- Run the playbook, and navigate to Kibana ((YOUR IP ADDRESS):5601) to check that the installation worked as expected.

For ELK VM Configuration:
Copy the ELK Installation and VM Configuration
Run the playbook using this command : ansible-playbook /etc/ansible/install-elk.yml

For Filebeat
Download Filebeat playbook usng this command:

  curl -L -O https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/filebeat-config.yml
Copy the Filebeat Config file to /etc/ansible
Update the filebeat-config.yml file to include the ELK private IP 10.2.0.4 as below from root@9ddf6fe7eb3f:~# nano /etc/ansible/filebeat-config.yml

output.elasticsearch:
  Boolean flag to enable or disable the output module.
  enabled: true
  
   Array of hosts to connect to.
   Scheme and port can be left out and will be set to the default (http and 9200)
   In case you specify and additional path, the scheme is required: http://localhost:9200/path
   IPv6 addresses should always be defined as: https://[2001:db8::1]:9200
  hosts: ["10.2.0.4:9200"]
  username: "elastic"
  password: "changeme" # TODO: Change this to the password you set

 Starting with Beats version 6.0.0, the dashboards are loaded via the Kibana API.
 This requires a Kibana endpoint configuration.
setup.kibana:
  host: "10.2.0.4:5601" 
 TODO: Change this to the IP address of your ELK server
Run the playbook using this command ansible-playbook filebeat-playbook.yml and navigate to Kibana > Logs : Add log data > System logs (DEB) > 5:Module Status > Check Incoming data on Kibana to check that the installation worked as expected.

For Metricbeat

Download Metricbeat playbook using this command:

curl -L -O https://gist.githubusercontent.com/slape/58541585cc1886d2e26cd8be557ce04c/raw/0ce2c7e744c54513616966affb5e9d96f5e12f73/metricbeat > /etc/ansible/files/metricbeat-config.yml Copy the Metricbeat Config file to /etc/ansible
Update the metricbeat-config.yml file to include the ELK private IP 10.2.0.4 as below from root@9ddf6fe7eb3f:~# nano /etc/ansible/metricbeat-config.yml

# ============================== Kibana =====================================

 Starting with Beats version 6.0.0, the dashboards are loaded via the Kibana API.
 This requires a Kibana endpoint configuration.
setup.kibana:
  host: "10.2.0.4:5601"
  
# -------------------------- Elasticsearch output ------------------------------
output.elasticsearch:
   TODO: Change the hosts IP address to the IP address of your ELK server
   TODO: Change password from `changem` to the password you created
  hosts: ["10.2.0.4:9200"]
  username: "elastic"
  password: "changeme"

Run the playbook using this command ansible-playbook metricbeat-playbook.yml and navigate to Kibana > Logs : Add Metric data > Docker Metrics (DEB) > 5:Module Status > Check data_on Kibana to check that the installation worked as expected.
  Metricbeat Module Kibana - Metricbeat Docker Overview ECS Dashboard
  Metricbeat Module Kibana - Metricbeat Docker Web-1 metrics
  Metricbeat Module Kibana - Metricbeat Docker Web-2 metrics
  Metricbeat Module Kibana - Metricbeat Docker DVWA-VM3 metrics

Install Filebeat onto VM's
Login to Kibana > Logs : Add log data > System logs > DEB > Getting started
Copy: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb
(Download the Filebeat to the VM)

Install Metricbeat onto VM's
Login to Kibana > Add Metric Data > Docker Metrics > DEB > Getting Started
Copy: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.6.1-amd64.deb
(Download the Metricbeat to the VM)
Answer the following questions to fill in the blanks:

Which file is the playbook?

For Ansible create My First Playbook
For Filebeat create Filebeat Playbook
For Metricbeat create Metricbeat Playbook - Where do you copy it?
/etc/ansible/
Which file do you update to make Ansible run the playbook on a specific machine?

/etc/ansible/hosts file (IP of the Virtual Machines).
How do I specify which machine to install the ELK server on versus which to install Filebeat on?

I have specified two separate groups in the etc/ansible/hosts file. One of the group will be webservers which has the IPs of the 3 VMs that I will install Filebeat to. The other group is named ELKserver which will have the IP of the VM I will install ELK to.
