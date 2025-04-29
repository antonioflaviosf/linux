# Linux

## utilitários
Ubuntu:
```bash=
sudo apt update; sudo apt upgrade -y; apt install -y terminator silversearcher-ag ;\
sudo apt install --no-install-recommends -y apt-utils wget curl vim && \
sudo apt install --no-install-recommends -y net-tools dnsutils
```

---
**Criação de `banners` para linux** [click_aqui](https://manytools.org/hacker-tools/ascii-banner/)
---

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
w    #<- salva alterações e sai do fdisk
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

## Usando o `find`
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
*OBS.: para parametro de tempo pode seru usado `-mtime` para dias ex.: -mtime -10 , busca por arquivos recentes dentro dos ultimos 10 dias, porém*

### Diferença entre + e \; no find:

- **Com `+`:** Agrupa vários arquivos e os passa de uma só vez ao comando, o que é mais eficiente porque reduz o número de chamadas ao comando externo.
- **Com `\;`:** Executa o comando para cada arquivo individualmente, o que pode ser mais lento para uma grande quantidade de arquivos.

**Exemplo:**
Com `+`:
```bash
find . -type f -exec rm -f {} +
```
O comando acima pode ser traduzido para algo como:
```bash
rm -f arquivo1 arquivo2 arquivo3 ...
```

Com `\;`:
```bash
find . -type f -exec rm -f {} \;
```
O comando acima será equivalente a executar:
```bash
rm -f arquivo1
rm -f arquivo2
rm -f arquivo3
```

**Vantagens do uso de `+`:**
1. **Melhor desempenho**: O comando é executado menos vezes, pois trabalha em lotes.
2. **Limitação de chamadas ao comando**: Para muitos arquivos, o uso de \; pode ser lento e consumir mais recursos.

Por isso, é geralmente preferível usar `+` sempre que o comando permitir

# Comando `history`

1. Para listar linhas específicas do historico bash usando o `sed`
```bash
history | sed -n '900,950p'
```

2. listando usando o `awk`
```bash
history | awk 'NR>=900 && NR<=950'
```

*obs.: o comando sed para filtrar linhas pode ser usado em qualquer saida, por exemplo, no uso também com o comando `cat`*
```bash
cat <arquivo> | sed -n '10,20p'
```

# Editor de Texto `vi`
| **[referência](https://vimhelp.org/)**

Procurar e substituir
```bash
:%s/texto_a_procurar/texto_novo/g
```

Deletar várias linhas que começam com "202501" em um arquivo.
```bash
:g/^202501/d
```
- `g` é o comando global que aplica a ação em todas as linhas que correspondem o padrão.
- `^202501`é o padrão que procura linhas que começam com "202501".
- `d` é o comando para deletar as linhas correspondentes.

### Comentar multiplas linhas: 

1. selecionar primeira linha e ser comentada e entrar no modo " Visual" com as Teclas de Atalho `CTRL+V`
depois disso selecionar as linhas desejadas com auxílio das setas. 
2. pressionar `I + #`depois `esc` duas vezes para que as linhas selecionadas sejam comentadas.


# Utilitários DNS `dig`

Consulta básica a um dominio 
```bash
dig <nome_dominio>
```

exibir tipo específicos de registros: 
- MX
```bash
dig <nome_dominio> MX
```

- NS
```bash
dig <nome_dominio> NS
```

- TXT (registros de texto, uteis para SPF, verificaçao de dominios,etc.)
```bash
dig <nome_dominio> TXT
```

consultar versão do servidor DNS
```bash
dig @8.8.8.8 version.bind TXT
```

Traz uma resposta curta: 
```bash 
dig +short @8.8.8.8 <nome_domain>
```

Resposta com nome de dominio e tipo de RR + IP:
```bash 
dig +noall +answer @8.8.8.8 <nome_dominio>
```

Pesquisa reversa
```bash
dig -x 8.8.8.8
```

Pesquisa sem recursividade 
```bash
dig @1.1.1.1 +norecurse <nome_dominio>
```
