# Linux Jumpstart
my personal script that I could run every time I install a new Linux server (mainly raspberry pi)

## update
`sudo apt-get update && sudo apt-get upgrade && sudo apt update && sudo apt upgrade`

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
# after installing docker
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
`sudo docker compose up -d`

### kill and remove
`sudo docker kill octoprint && sudo docker rm octoprint`


## pi-hole stuff

### to disable ubuntu dns server taking port 53:
```bash
sudo sed -r -i.orig 's/#?DNSStubListener=yes/DNSStubListener=no/g' /etc/systemd/resolved.conf
sudo sh -c 'rm /etc/resolv.conf && ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf'
systemctl restart systemd-resolved
```

### boot
`sudo docker compose up -d`

### kill and remove
`sudo docker kill pihole && sudo docker rm pihole`

### change password
`sudo docker exec -it pihole pihole -a -p`

### schedule update
`corntab ./docker-pi-hole.cron`
chcek cron/log after a few weeks


## apache2 stuff
use sudo to put stuff in /html and enjoy !

### boot
`sudo docker compose up -d`

### kill and remove
`sudo docker kill apache2 && sudo docker rm apache2`

### get a shell
`sudo docker exec -it apache2 /bin/bash`

### reboot
`sudo docker exec -it apache2 service apache2 restart`
