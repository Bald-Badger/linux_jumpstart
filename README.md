# Linux Jumpstart
my personal script that I could run every time I install a new Linux server (mainly raspberry pi)

## update
`sudo apt-get update && sudo apt-get upgrade -y && sudo apt update && sudo apt upgrade -y && sudo apt-get autoremove -y && sudo apt-get autoclean`
`sudo apt-get install build-essential -y`

## update time & time zone
```bash
sudo date -s "$(wget -qSO- --max-redirect=0 google.com 2>&1 | grep Date: | cut -d' ' -f5-8)Z"
timedatectl list-timezones | grep "America"
sudo timedatectl set-timezone America/Phoenix
```

## fan contorl

```bash
curl https://download.argon40.com/argon1.sh | bash
argonone-config
argonone-uninstall
```

## thefuck
```bash
sudo apt install python3-dev python3-pip python3-setuptools
pip3 install thefuck --user
code ~/.bashrc
```
```bash
eval $(thefuck --alias)
eval $(thefuck --alias FUCK)
```

## Docker stuff
```bash
sudo snap install docker
sudo groupadd docker
sudo usermod -aG docker $USER
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
sudo systemctl restart docker
```

## 3d printer stuff
```bash
sudo usermod -aG dialout shuai
sudo apt-get install arduino
```
port should be /dev/ttyUSB0

### boot
don't forget to boot 3dprinter before booting docker
`docker compose up -d`

### kill and remove
`docker kill octoprint && docker rm octoprint`

### update
`docker pull octoprint/octoprint`

## pi-hole stuff

### to disable ubuntu dns server taking port 53:
```bash
sudo sed -r -i.orig 's/#?DNSStubListener=yes/DNSStubListener=no/g' /etc/systemd/resolved.conf
sudo sh -c 'rm /etc/resolv.conf && ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf'
systemctl restart systemd-resolved
```
allow outbound UDP for IPv4/IPv6 port 53.
`sudo ufw allow out 53/udp`

### boot
`docker compose up -d`

### kill and remove
`docker kill pihole && docker rm pihole`

### change password
`docker exec -it pihole pihole setpassword`

### schedule update
check shuai.cron

### upgrade
```
docker pull pihole/pihole
docker rm -f pihole
docker-compose up -d
```

## apache2 stuff

### boot
`docker compose up -d`

### kill and remove
`docker kill apache2 && docker rm apache2`

### get a shell
`docker exec -it apache2 /bin/bash`

### reboot
`docker exec -it apache2 service apache2 restart`


# Django stuff

##
`sudo apt install python3-django`


# Samba
`sudo service smbd start`
`sudo service smbd restart`
add smb user: sudo smbpasswd -a <USERNAME>