# ELK-Stack-Project
Project 1 - ELK Stack Project
# install_elk.yml
- name: Configure Elk VM with Docker
 hosts: elkserver
 remote_user: sysadmin
 become: true
 tasks:
   # Use apt module
   - name: Install docker.io
     apt:
       update_cache: yes
       name: docker.io
       state: present
# Use apt module
   - name: Install pip3
     apt:
       force_apt_get: yes
       name: python3-pip
       state: present
# Use pip module
   - name: Install Docker python module
     pip:
       name: docker
       state: present
 # Use command module
   - name: Increase virtual memory
     command: sysctl -w vm.max_map_count=262144
# Use sysctl module
   - name: Use more memory
     sysctl:
       name: vm.max_map_count
       value: "262144"
       state: present
       reload: yes
 # Use docker_container module
   - name: download and launch a docker elk container
     docker_container:
       name: elk
       image: sebp/elk:761
       state: started
       restart_policy: always
       published_ports:
         - 5601:5601
         - 9200:9200
         - 5044:5044

Description of the Topology
These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the  file may be used to install only certain pieces of it, such as Filebeat.
Load balancing ensures that the application will be highly avaible, in addition to restricting acsess to the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
| jumpbox   | gateway    | 10.1.0.4 | linux |   
| RedWeb1   | webserver  | 10.1.0.5 | linux |   
| RedWeb2   | webserver  | 10.1.0.6 | linux |   
| ELKserver | monituring | 10.1.0.4 | linux |   
Only the ELK server machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
Access Policies
Only the jump box provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
Standard DS1
Machines within the network can only be accessed by jump box provisioner.
Which machine did you allow to access your ELK VM?
My IP Address: 40.122.228.33
A summary of the access policies in place can be found in the table below.
| jumpbox   | yes | 40.122.228.33 | linux | 
| RedWeb1   | no  | 10.1.0.4      | linux |   
| RedWeb2   | no  | 10.1.0.4      | linux |   
| ELKserver | no  | 10.1.0.4      | linux |   
Elk Configuration
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because..Ansible automation helps considerably with the representation of Infrastructure as Code (IAC)..Free: Ansible is an open-source tool.
in 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc.
Install docker.io
Install pip3
Install Docker python module
Increase virtual memory
Download and launch a docker
Target Machines & Beats
This ELK server is configured to monitor the following machines: -Private IPs of RedWeb1 and RedWeb2
| RedWeb1 | 10.1.0.5 |   
| RedWeb2 | 10.1.0.6 |   

We have installed the following Beats on these machines:
Microbeats

We have installed the following Beats on these machines:
Filebeat - collects data about the file system
Metricbeat - collects machine metrics, such as uptime
Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:
SH into the control node and follow the steps below:
$ cd /etc/ansible
$ mkdir files
# Clone Repository + IaC Files
$ git clone https://github.com/TenkiYamada/Project-1-ELK-Stack-Project.git
# Move Playbooks and hosts file Into `/etc/ansible`
$ cp /Project-1-ELK-Stack-Project/ReadMe/Playbooks/*


Update the hosts file to include webserver and elk
Edit hosts file to update and to make Ansible run the playbook on a specific machine, and specify which machine to install the ELK server on versus which to install Filebeat.
Copy of the hosts file is also here: hosts
$ cd /etc/ansible
$ cat > hosts <<EOF
[webservers]
10.0.0.7
10.0.0.8
[elk]
10.1.0.4
EOF
Run the playbook, and navigate to Kibana (http://[Host IP]/app/kibana#/home) to check that the installation worked as expected.
cd /etc/ansible
 $ ansible-playbook install_elk.yml elk
 $ ansible-playbook install_filebeat.yml webservers
 $ ansible-playbook install_metricbeat.yml webservers
Check that the ELK server is running: http://[Host IP]/app/kibana#/home

## Fill in this as your project documentation
