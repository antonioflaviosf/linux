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
```

extende o lv **/dev/ubuntu-vg/ubuntu-lv** em 100% do espaço disponível do VG
```bash
lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
```

extende o lv com valor acima aplicado. (*obs.: somente para ext{2,3,4}*)
bash```
resize2fs /dev/ubuntu-vg/ubuntu-lv              
```

OBS.: para **sistemas de arquivos XFS**

verifica a integridade do sistema de arquivos.
```bash= 
xfs_check   <lv_name>
```

redimensiona o sistema de arquivos. *(obs.: qdo reduzido por exemplo)*
```bash= 
xfs_repair -f <lv_name>
```

para aumentar uma qtd específica, por exemplo: 200G
```bash
lvextend -L +200G <nome_do_lv>
```

verificar se realmente o lv foi expandido
```bash
lvdisplay
```

para redimensionar o sistema de arquivos xfs
```bash
xfs_growfs <nome_do_lv>
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
