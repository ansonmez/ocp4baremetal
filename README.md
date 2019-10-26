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
cd files; wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/openshift-install-linux-4.2.0.tar.gz ;wget https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/4.2/latest/rhcos-4.2.0-x86_64-installer-initramfs.img ; gunzip openshift-install-linux-4.2.0.tar.gz ; tar xvf openshift-install-linux-4.2.0.tar  ;  wget  https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/4.2/latest/rhcos-4.2.0-x86_64-metal-bios.raw.gz ; wget https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/4.2/latest/rhcos-4.2.0-x86_64-installer-kernel
