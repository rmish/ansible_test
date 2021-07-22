# Ansible playbook for several netdata nodes after a nginx proxy
Playbook sets two group of hosts:
 * Frontend: CentOS 8.x with nginx as proxy for all nodes in backend (including table of contents of backend nodes).
 * Backend: Ubuntu 21.04 with netdata service.
 
Playbook requires collection ansible.posix from ansible-galaxy (for changing selinux setings)

 Playbook has two global variables: web port for front and netdata port for all backend nodes. No restriction for group size. 
 Each frontend host can show information from all backend hosts and has "table of contents" for all monitored hosts.
 
 Using ssh connection for all nodes with root user. Tested with ssh keys authentication, 
 if using password-based auth needs cheking sshd config for allowing root to connect with that method.
 
 Inventory for test run is in **hosts** file, all nodes should contain ip addresses.
 
 Right now playbook contains hack for working with static addresses. It requires adding all backend hosts to file **/etc/hosts**,
 running **systemd-resolved** service and adding **resolver** settings to nginx conf. Can be converted to normal deployment when all hosts are registered in dns.
 Tested only with IPv4 adresses. For correct DNS shouldn`t be any differences, for current "static" IPv6 connectivity require modification.
 
 Playbook doesn`t check OS family, it assumes CentOS 8.x for frontend and Ubuntu 21.04 for backend.
 For backend there is no settings,  that strictly require ubuntu 21.04 (if netdata config 
 is staying in /etc/netdata). 
 All frontend settings implemented for CentOS. On other distribution nginx config
 could be in different directorу or in different file. Other settings should work, but not tested. 
