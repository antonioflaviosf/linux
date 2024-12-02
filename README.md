# Linux

## utilitários
Ubuntu:
```bash=
sudo apt update; sudo apt upgrade -y; apt install -y terminator silversearcher-ag ;\
sudo apt install --no-install-recommends -y apt-utils wget curl vim && \
sudo apt install --no-install-recommends -y net-tools dnsutils
```

---
## Usando o *for* no linux, exemplo: 
1. definindo uma variavel
```bash=
lista="8.8.8.8 1.1.1.1 1.1.1.2 8.8.4.4"
```

2. executando o *for*. No exemplo abaixo será executado 1 ping para todos os IPs definidos em *lista*.
```bash
for i in $lista; do ping -c 1 $i; done
```


## Instalar o docker e docker-compose no ubuntu

1. Instalar
```bash=
sudo apt update; sudo apt upgrade -y; sudo apt install docker.io
sudo curl -SL https://github.com/docker/compose/releases/download/v2.30.3/docker-compose-linux-x86_64 -o /usr/bin/docker-compose
sudo chmod +x /usr/bin/docker-compose
```

2. Adicionar usuario ao grupo docker
```bash
sudo usermod -aG docker <seu_user>
```

## lvm

Para identificar o disco executar o comando:
```bash
fdisk -l
```

Para criar partição do tipo lvm (confirmar perguntas padrão):
```bash=
fdisk /dev/sdX
n    #<- cria nova partição 
p    #<- particao primaria
t    #<- altera tipo de partição, selecionar '8e' (lvm)
x    #<- salva alterações e sai do fdisk
```

Criar PV (physical volume):
```bash
pvcreate /dev/sdX1
```

extendo o VG (Volume Group):
```bash=
vgdisplay 
vgextend ubuntu-vg /dev/sdX1
```

conferir o caminho do lv:
```bash=
lvdisplay                                          
```

extende o lv **/dev/ubuntu-vg/ubuntu-lv** em 100% do espaço disponível do VG:
```bash
lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
```

extende o lv com valor acima aplicado. *(obs.: somente para ext{2,3,4})*:
```bash
resize2fs /dev/ubuntu-vg/ubuntu-lv              
```

OBS.: para **sistemas de arquivos XFS**

verifica a integridade do sistema de arquivos:
```bash= 
xfs_check   <lv_name>
```

redimensiona o sistema de arquivos. *(obs.: qdo reduzido por exemplo)*:
```bash= 
xfs_repair -f <lv_name>
```

para aumentar uma qtd específica, por exemplo: 200G:
```bash
lvextend -L +200G <nome_do_lv>
```

verificar se realmente o lv foi expandido:
```bash
lvdisplay
```

para redimensionar o sistema de arquivos xfs:
```bash
xfs_growfs <nome_do_lv>
```


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


## Journal
1. verificar tamanho atual dos logs jornal

```bash
sudo journalcctl --disk-usage
```

2. Limpar logs antigos com base em tempo. O comando abaixo remove logs com mais de 7 dias. Poderá ser ajustado para 1month 30min, etc.
```bash
sudo journalctl --vacuum-time=7d
```

3. Limpar logs com baso no tamanho 
```bash
sudo journalctl --vacuum-size=500M
```

4. Limpar todos os logs 
```bash
sudo journalctl --vacuum-size=0
```

### Para configurar limite permanente 
Você pode configurar o tamanho máximo permitido para os logs do journal editando o arquivo de configuração:

1. Abra o arquivo de configuração:
```bash
sudo vi /etc/systemd/journald.conf
```

2. Encontre (ou adicione) a linha abaixo. Substitua *500M* pelo tamanho desejado para os logs, depois salve e saia (":wq")
```ini
SystemMaxUse=500M
```

3. Reinicie o serviço **systemd-journald** para aplicar as alterações:
```bash
sudo systemctl restart systemd-journald
```

### Apagar logs manualmente (opcional)
Se você quiser apagar os arquivos diretamente:

1. Localize os logs:
```bash
sudo du -sh /var/log/journal
```

2. Apague manualmente (não recomendado se você não configurou limites):
```bash
sudo rm -rf /var/log/journal/*
```

Depois, reinicie o serviço **systemd-journald**:
```bash
sudo systemctl restart systemd-journald
```

## Usando find  
1. Listando arquivos criados ou modificados nas ultimas 10 hroas
```bash
find /caminho/desejado -type f -mmin -600
``` 
**Explicação dos parâmetros:**
- /caminho/desejado: O diretório onde você deseja buscar os arquivos. Substitua por . para buscar no diretório atual.
- type f: Limita a busca a arquivos (excluindo diretórios).
- mmin -600: Busca arquivos modificados nos últimos 600 minutos (10 horas).

2. Para remover tudo o que foi trazido na saida do caminho acima: 
```bash
find /caminho/desejado -type f -mmin -600 -exec rm -f {} +
``` 
