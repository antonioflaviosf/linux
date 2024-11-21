# linux

---
## Instalar o docker no ubuntu

```bash=
sudo apt update; sudo apt upgrade -y; sudo apt install docker.io
sudo curl -SL https://github.com/docker/compose/releases/download/v2.30.3/docker-compose-linux-x86_64 -o /usr/bin/docker-compose
sudo chmod +x /usr/bin/docker-compose
```



---
## LVM 

Para identificar o disco executar o comando:
```bash
fdisk -l
```

Para criar partição do tipo lvm (confirmar perguntas padrão)
```bash=
fdisk /dev/sdX
n    #<- cria nova partição 
p    #<- particao primaria
t    #<- altera tipo de partição, selecionar '8e' (lvm)
x    #<- salva alterações e sai do fdisk
```

Criar PV (physical volume)
```bash
pvcreate /dev/sdX1
```

extendo o VG (Volume Group) 
```bash=
vgdisplay 
vgextend ubuntu-vg /dev/sdX1
```

extendendo o Volume Lógico (LV) com todo espaço livre do VG
```bash=
lvdisplay                                           # <- conferir o caminho do lv
lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
resize2fs /dev/ubuntu-vg/ubuntu-lv                  #<- extende o valor acima aplicado
```

OBS.: para **sistemas de arquivos XFS**

```bash= 
xfs_check   <lv_name>                   # verifica a integridade do sistema de arquivos.
xfs_repair -f <lv_name>                 # redimensiona o sistema de arquivos
```



---
## Conectando interface console no linux
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
