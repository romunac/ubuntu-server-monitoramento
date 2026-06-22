# Comandos Utilizados

## Atualização
sudo apt update
sudo apt upgrade -y

## SSH
sudo apt install openssh-server -y
sudo systemctl enable ssh
sudo systemctl start ssh

## Firewall UFW
sudo ufw allow 22/tcp
sudo ufw allow 443/tcp
sudo ufw enable
sudo ufw status

## Fail2Ban
sudo apt install fail2ban -y
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
sudo systemctl status fail2ban
