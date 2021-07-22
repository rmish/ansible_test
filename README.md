# Ansible playbook for several netdata nodes after a nginx proxy
Playbook sets two group of hosts:
 * Frontend: CentOS 8.x with nginx as proxy for all nodes in backend (including table of contents for backend nodes).
 * Backend: Ubuntu 21.04 with netdata service.
 
Playbook requires collection **ansible.posix** from **ansible-galaxy** (for changing selinux setings)

 Playbook has two global variables: **web_port** for nginx and **backend_port** for netdata service. No restriction for group size. 
 Each frontend host can show information from all backend hosts and has "table of contents" for all monitored hosts.
 
 URLs configured on each frontend nodes:
  * http://frontend_nodeYY:web_port/index.html - list of known backends for monitoring
  * http://frontend_nodeYY:web_port/netdata/backend_nodeXX/ - acess to netdata web console on backend_nodeXX
 
 Using ssh connection for all nodes with root user. Tested with ssh keys authentication, 
 if using password-based auth needs cheking sshd config for allowing root to connect with that method.
 
 Inventory for test run is in **hosts** file, all nodes should contain ip addresses (1 in frontend group, 2 in backend). 
 Right now playbook uses hack for working with static addresses. It requires adding all backend hosts to **/etc/hosts** file,
 running **systemd-resolved** service and adding **resolver** settings to **nginx conf**. Can be converted to normal 
 deployment when all hosts are registered in dns. Tested only with IPv4 adresses, for IPv6 connectivity
 require modification (adding aditional variable with address for each host and changing corresponding task).
 
 Playbook check OS family (distribution) only for 2 settings for frontend hosts (firewalld and selinux). 
 It assumes CentOS 8.x for frontend and Ubuntu 21.04 for backend.
 For backend there is no settings, that strictly require ubuntu 21.04 (if netdata config is staying at /etc/netdata). 
 All frontend nginx settings implemented for CentOS. On other distribution nginx config
 could be in different directorу or in different file. Other settings should work, but not tested. 
