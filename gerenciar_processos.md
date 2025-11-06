# Gerenciar Processos



## Identificar processo que está usando uma porta específica

As vezes notamos uma comunicação do sistema e se faz necessário identificar qual processo está usando aquela porta específica. Para esse cenário, idenitificamos uma porta de origem (58694) via tcpdump que estava constantemente em comunicação e foi necessário identificar o processo: 

```bash
tcpdump -nni any <src> port 58694
```

1. lsof - mostra o PID e o nome do processo.
```bash 
sudo lsof -iTCP:58694 -n -P 
``` 

