# my5GCore-install

my5G-core is a fork of the free5GC project, with some extensions to facilitate the deployment. The main go od this project is automated the process of installation throught Ansible.

**Steps**

Install python-minimal:
```
sudo apt update && apt -y install python-minimal
```

Install git:
```
sudo apt -y install git
```

Clone this repository:
```
git clone https://github.com/ciromacedo/my5GCore-install.git
```

Install Ansible:
```
sudo apt -y install ansible
```

Run the following Ansible playbook (password for sudo is required):
```
cd my5GCore-install && ansible-playbook -K my5GInstall.yml
```
