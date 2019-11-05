With two playbook , it will prepare bastion host tftpboot for provisioning vms, haproxy as loadbalancer , dnsmasq for dhcp and dns requirement and fixed ips , httpd server for holding installation files , create vms on RHV for installing OCP 4.x as baremetal on RHV. 

DO THESE before start 


subscription-manager register

subscription-manager list --available

subscription-manager attach --pool=XXXXXXXX

subscription-manager repos --enable rhel-7-server-ansible-2.8-rpms

yum install ansible screen bind-utils -y 

ansible-galaxy install bertvv.dnsmasq

ansible-galaxy install bertvv.hosts

subscription-manager repos --enable=rhel-7-server-rhv-4.3-manager-rpms

yum install python-ovirt-engine-sdk4

mkdir files; cd files; wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/openshift-install-linux-4.2.0.tar.gz ;wget https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/4.2/latest/rhcos-4.2.0-x86_64-installer-initramfs.img ; gunzip openshift-install-linux-4.2.0.tar.gz ; tar xvf openshift-install-linux-4.2.0.tar  ;  wget  https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/4.2/latest/rhcos-4.2.0-x86_64-metal-bios.raw.gz ; wget https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/4.2/latest/rhcos-4.2.0-x86_64-installer-kernel



PREPARE install-config.yaml CHECK myvars.yaml

ansible-playbook create_openshift_vms.yaml -e ovirt_url=xxxx -e ovirt_username=xxxxx -e ovirt_password=''

ansible-playbook ocpbaremetal.yaml



MYVARS

cluster_name: asocp42test   --> clustername should be same with install install-config.yaml

domain_name: lp.int

start_addr: 192.168.191.3   --> dhcp start address

end_addr: 192.168.191.253   --> dhcp end address

haproxyip: 192.168.191.178  --> haproxy dnsmasq hosting bastion

httpd_ip: "{{ haproxyip }}" --> also httpd serving files for pxe 

webport: 9000   --> port number of httpd server

installer_kernel: rhcos-4.2.0-x86_64-installer-kernel

initramfs_path: rhcos-4.2.0-x86_64-installer-initramfs.img

coreos_inst_install_dev: vda    --> check your RHV server which disk

coreos_inst_image_file: rhcos-4.2.0-x86_64-metal-bios.raw.gz

coreos_inst_ignition_dir: install_dir --> install dir at httpd ,you will see also in pxe conf /var/lib/tftpboot/pxelinux.cfg/ with macs

otherinstall_dir: install_dir  --> install dir at httpd for metal and initramfs simply give same name with above


ovirt_url: "https://rhev-man.lp.int/ovirt-engine/api"

ovirt_username: ""   --> dont forget RHV  username

ovirt_password: ""   --> dont forget RHV pass

ocp_master_vm_count: 3  --> number of master hosts 

master_ips: 192.168.191.220,192.168.191.221,192.168.191.222  --> master ip addresess care with ocp_master_vm_count

vm_master_name_prefix: asocp42master   --> rhv vm host name prefix

vm_worker_name_prefix: asocp42worker   --> for worker name prefix 

ocp_master_vm_tag: asocp42master    

ocp_worker_vm_count: 2    --> number of worker hosts

worker_ips: 192.168.191.223,192.168.191.224  --> worker ip addresess care with ocp_worker_vm_count

bootstrap_ip: 192.168.191.219  --> bootstrap ip 

ocp_master_vm_dns_domain: "{{ domain_name }}"   -

cluster: RedHat   --> RHV cluster name 

ocp_master_vm_memory: "16GiB"

cpus: 4

nic_profile_name: vlan_301  --> do not forget , network profile 

vm_disk_size: 90GiB

disk_interface: virtio

vm_storage_domain: data_disk3 --> check your RHV for storage domain

vm_tag: asocp42test ---> you can filter your hosts at RHV

install_config_file: install-config.yaml
