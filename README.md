# Linux Jumpstart
my personal script that I could run every time I install a new Linux server

# first time setup steps
## update
`sudo apt-get update && sudo apt-get upgrade -y && sudo apt update && sudo apt upgrade -y && sudo apt-get autoremove -y && sudo apt-get autoclean`
`sudo apt-get install build-essential -y`

## install RDP for windows remote desktop setup
`sudo apt install xrdp -y`
`sudo systemctl start xrdp`
`sudo systemctl enable xrdp`
`sudo ufw allow from any to any port 3389 proto tcp`

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

## cron
```bash
sudo crontab /home/shuai/linux_jumpstart/root.cron
crontab /home/shuai/linux_jumpstart/my.cron
```

## Docker stuff
```bash
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo $VERSION_CODENAME) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo usermod -aG docker $USER
newgrp docker
docker run hello-world
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
sudo sed -r -i.orig 's/^#?DNSStubListener=.*/DNSStubListener=no/' /etc/systemd/resolved.conf
sudo rm -f /etc/resolv.conf
sudo ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf
sudo systemctl restart systemd-resolved
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

### build image
docker build -t apache2-hello .

## ssh.shuainium.com
### ssh is up, with the help of chatgpt
### example access
```
Host ssh.shuainium.com
    User shuai
    ProxyCommand "C:\Program Files (x86)\cloudflared\cloudflared.exe" access ssh --hostname %h
```