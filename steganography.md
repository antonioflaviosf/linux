# Steghide 


## instalação: 
```bash 
sudo apt install -y steghide
```

1. Ocultar dados em um arquivo (Embed)
ex.: file.txt (arquivo secreto), foto.jpg (arquivo portador) 

```bash
steghide embed -cf foto.jpg -ef file.txt 
```

se preferir gerar um novo arquivo ao inves de sobrescrever o arquivo original (foto.jpg) é necessário usar  o parâmetro `-sf` :
```bash
steghide embed -cf foto.jpg -ef file.txt  -sf foto_nova.jpg
``` 


2. Extrair dados 
```
stehide extract -sf foto_nova.jpg
```


3. Obter informações 

```bash
steghide info foto_nova.jpg
```