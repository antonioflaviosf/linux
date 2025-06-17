# GIT
Comandos básicos/úteis para uso no dia-a-dia.

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
Configuração básica de nome e email:
```bash=
git config --global user.name "Seu Nome"
git config --global user.email "seuemail@example.com"
```

Verificar configs atuais: 
```bash
git config --list
```

obs.: poderá ser criado dois arquivos para assistencia as credenciais
1. arquivo ~/.gitconfig
```bash
[user]
	name = username
	email = username@mail.com
```

2. arquivo ~/.git-credentials, esse por sua vez poderá conter mais de uma credencial caso necessário:
```bash
https://<user>:<token>@<url>
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
---
6. Exemplo abaixo traz o processo de sincronizar as alterações da branch dev para a main.
✅ 1. Fazer push passando o nome da branch
Se você não quiser configurar o rastreamento (ou já está configurado), e quer apenas enviar uma branch específica (por exemplo, dev), o comando é:

```bash
git push origin dev
```
Isso significa:
origin: é o repositório remoto.

dev: é a sua branch local que será enviada para o remoto como origin/dev.

✅ 2. Fazer merge da dev para a main
Etapas:
👉 1. Vá para a branch main:
```bash
git checkout main
```

👉 2. Atualize a branch main com os dados mais recentes do repositório remoto (opcional, mas recomendado):
```bash
git pull origin main
```

👉 3. Faça o merge da dev para a main:
```bash
git merge dev
```
Isso traz as mudanças da branch dev para dentro da main.

👉 4. Envie as mudanças da main para o repositório remoto:
```bash
git push origin main
```

✅ Resumo rápido:
```=bash
# Push da branch dev para o remoto
git push origin dev

# Merge da dev para a main
git checkout main
git pull origin main
git merge dev
git push origin main
```

## Trabalhando com repositórios remotos
1. Verificar repositórios remotos configurados:
```bash
git remote -v
```

2. Enviar alterações para o repositório remoto:
```bash
git push origin <nome-da-branch>
```

3. Atualizar seu repositório local com alterações remotas:
```bash
git pull origin <nome-da-branch>
```

4. Baixar alterações sem aplicar (fetch):
```bash
git fetch
```

## Resolver problemas
1. Verificar histórico de commits:
```bash
git log
```

2. Ver diferenças entre versões:
```bash
git diff
```

3. Descartar alterações em um arquivo:
```bash
git checkout -- <arquivo>
```

4. Reverter último commit (mantendo alterações):
```bash
git reset --soft HEAD~1
```

5. Reverter último commit (desfazendo alterações):
```bash
git reset --hard HEAD~1
```
