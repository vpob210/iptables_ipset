# iptables_ipset

Наброски настроек iptables + ipset что бы не забыть

скрипт-крон для подстраховки:
перед началом сохранить деф настройки iptables-save > iptables.rules потмо в крон добавить
*/10 * * * * root /sbin/iptables-restore /root/iptables.rules


на Centos / RHEL
dnf install iptables-services
systemctl start iptables
systemctl enable iptables

dnf install ipset-service
systemctl enable ipse
systemctl start ipset

systemctl disable firewalld.service !!! именно дисейбл не стопать.

vi /etc/sysconfig/iptables-config  - IPTABLES_SAVE_ON_STOP="yes" , IPTABLES_SAVE_ON_RESTART="yes"
vi /etc/sysconfig/ipset-config     - IPSET_SAVE_ON_STOP="yes"

systemctl restart iptables.service
systemctl restart ipset.service

ipset restore < /root/ipset.final
iptables-restore /root/iptables.final

service ipset save
service iptables save

vi /etc/crontab
systemctl restart crond

на Debian / Ubuntu
apt-get install iptables-persistent
sudo apt-get install ipset-persistent

настройки как выше только нужно дизейблить ufw, для сохранения настроек
iptables-save > /etc/iptables.rules.v4
ipset save > /etc/iptables/ipsets (если не работает sudo sh -c 'ipset save > /etc/iptables/ipsets' или sudo ipset save | sudo tee /etc/iptables/ipsets )
