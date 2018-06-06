# Ansible playbook for installing postgres and pgadmin on FreeBSD

This is a very simple playbook for installing postgresql96 and pgadmin into a freebsd server. 

There is no clustering or backups or tuning - both are installed Out of the box and not by any stretch of the imagination suitable for production.

This is based on the instructions garnered from this useful web page: https://www.howtoforge.com/tutorial/how-to-install-postgresql-and-pgadmin-on-freebsd-11/

I run this in an iocage jail in a jenkins server:

```
RELEASE=11.1.0
export JLN="postgres_test_${RELEASE}"
sudo iocage create -n ${JLN} dhcp=on allow_sysvipc=1 bpf=yes vnet=on -r$RELEASE
sudo iocage start ${JLN}
sudo iocage exec ${JLN} 'ASSUME_ALWAYS_YES=yes pkg install -y ansible git wget curl'
sudo iocage exec ${JLN} 'git clone https://github.com/coetzeer/freebsd_postgres_ansible.git'
sudo iocage exec ${JLN} 'cd /freebsd_postgres_ansible && ansible-playbook -i localhost play.yml'

```
