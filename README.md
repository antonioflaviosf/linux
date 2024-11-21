# linux

---
### Instalar o docker no ubuntu

```bash=
sudo apt update; sudo apt upgrade -y; sudo apt install docker.io
sudo curl -SL https://github.com/docker/compose/releases/download/v2.30.3/docker-compose-linux-x86_64 -o /usr/bin/docker-compose
sudo chmod +x /usr/bin/docker-compose
```



---
### Conectando interface console no linux
primeiramente verificar a interface conectada em /dev/ttyXX, no exemplo abaixo foi usado a interface "/dev/ttyUSB0"
```bash=
sudo lsusb
```

Instalar picocom
```bash=
sudo zypper install picocom 
```

conectar a interface via cli: 
```bash=
sudo picocom -b115200 /dev/ttyUSB0
```
