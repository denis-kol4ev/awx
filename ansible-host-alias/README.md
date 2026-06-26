Опустить алиасы
ansible-playbook host-alias-down.yml --user den --ask-pass --extra-vars "\
host_list=pc-lab-ora01,pc-lab-ora02 \
primary_vip=10.10.10.235 \
standby_vip=10.10.10.236 \
"

Поднять алиасы
ansible-playbook host-alias-up.yml --user den --ask-pass --extra-vars "\
host_list=pc-lab-ora01,pc-lab-ora02 \
primary_vip=10.10.10.235 \
standby_vip=10.10.10.236 \
"

Вручную вернуть алиасы из бэкапа
cd /etc/NetworkManager/system-connections
sudo cp ens160.nmconnection.back ens160.nmconnection
sudo nmcli connection reload
sudo nmcli connection up ens160