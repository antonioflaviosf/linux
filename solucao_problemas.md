# SOLUCAO PROBLEMAS



## Oracle 7.9 s.o nao inicializa = `erro: FAIL TO LOAD SELINUX POLICY. FREEZING`.

na primeira tela de boot pressione `CTRL+e` e adicione na linha onde começa com `linux16`o parâmetro: 
```bash
selinux=0
```

