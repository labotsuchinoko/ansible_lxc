
Ansible LXC

#####What is.
Maltipul host automated build lxc enviroment.

	------------------------------   ------------------------------   ------------------------------
	| vm01 | vm02 | vm03 | vm04  |   | vm01 | vm02 | vm03 | vm04  |    | vm01 | vm02 | vm03 | vm04  |
	------------------------------   ------------------------------   ------------------------------
	|dhcp(10.10.10.0/24)masqarade|   |dhcp(10.10.10.0/24)masqarade|   |dhcp(10.10.10.0/24)masqarade|
	------------------------------   ------------------------------   ------------------------------
	|     bridge(lxcbr0)         |   |     bridge(lxcbr0)         |   |     bridge(lxcbr0)         |
	------------------------------   ------------------------------   ------------------------------
	|         host1              |   |         host2              |   |         host3              |
	------------------------------   ------------------------------   ------------------------------


#####test environment
CentOS-6.6

#####prepare

	sudo yum install ansible
	git clone https://github.com/tsuchinokooyabun/ansible_lxc.git
	cd ansible-lxc
	vi hosts

#####make lxc images
	ansible-playbook -i hosts playbook.yml [-k]

#####make guest

	ansible-playbook -i hosts createguest.yml --extra-vars '{"guesthostname":"vm01"}' [-k]
	ansible-playbook -i hosts createguest.yml --extra-vars '{"guesthostname":"vm02"}' [-k]
	ansible-playbook -i hosts createguest.yml --extra-vars '{"guesthostname":"vm03"}' [-k]

#####boot guest
	ansible all -i hosts -m shell -a "lxc-start -n vm01 -d" [-k]
	ansible all -i hosts -m shell -a "lxc-start -n vm02 -d" [-k]
	ansible all -i hosts -m shell -a "lxc-start -n vm03 -d" [-k]
