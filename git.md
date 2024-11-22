# GIT


## Instalar git

Ubuntu:
```bash=
sudo apt update; sudo apt upgrade -y; sudo apt install git
```

Suse: 
```bash=
sudo zypper  update; sudo zypper upgrade -y; sudo zypper install git
```

## Comandos Basicos

Configuração basico de nome e email:
```bash=
git config --global user.name "Seu Nome"
git config --global user.email "seuemail@example.com"
```

Verificar configs autais: 
```bash
git config --list
```

## Trabalhando com Repositorios
1. Clonar repositorio remoto: 
```bash
git clone <url_do_repo>
```
2. Inicializar um novo repositorio: 
```bash
git init
```

## Controle de versao
1. Verificar o status do repositório:

```bash
git status
```

2. Adicionar arquivos para staging (preparar para commit):

- Um arquivo específico:
```bash
git add <arquivo>
```
- Todos os arquivos:
```bash
git add .
```

3. Fazer commit:
```bash
git commit -m "Mensagem do commit"
```

## Trabalhando com branches
1. Criar uma nova branch:
```bash
git branch <nome-da-branch>
```

2. Trocar de branch:
```bash
git checkout <nome-da-branch>
```

3. Criar e trocar para uma nova branch ao mesmo tempo:
```bash
git checkout -b <nome-da-branch>
```

4. Listar branches:
```bash
git branch
```

5. Mesclar uma branch:
```bash
git merge <nome-da-branch>
```

