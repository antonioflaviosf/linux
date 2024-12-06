# SOLUÇÃO DE PROBLEMAS



## Problema: `FAIL TO LOAD SELINUX POLICY. FREEZING`

Este erro ocorre ao tentar inicializar um sistema operacional Oracle Linux 7.9 e indica que o sistema falhou ao carregar a política do SELinux (Security-Enhanced Linux). Como resultado, o sistema congela durante o processo de boot.

---

### **Causa**

A causa principal é uma falha na leitura ou carregamento das políticas do SELinux. Isso pode ocorrer devido a:
- Arquivos de política do SELinux corrompidos.
- Problemas de configuração do SELinux.
- Modificações ou atualizações incompletas do sistema.

---

### **Solução**

#### 1. **Adicionar o Parâmetro `selinux=0` no Boot**
1. Na primeira tela de boot do GRUB, pressione `CTRL+E` para editar a entrada do boot.
2. Localize a linha que começa com `linux16`.
3. Adicione o parâmetro abaixo no final da linha:
   ```bash
   selinux=0
   ```
4. Pressione `CTRL+X` ou `F10` para inicializar o sistema com o SELinux desativado temporariamente.

---

#### 2. **Solução Permanente**
Após inicializar o sistema, é recomendado resolver o problema de forma definitiva:

##### **a) Verificar e Reinstalar as Políticas do SELinux**
```bash
sudo yum reinstall selinux-policy selinux-policy-targeted
```

##### **b) Configurar o SELinux para Permissivo ou Desativar**
- Para desativar o SELinux permanentemente, edite o arquivo de configuração:
  ```bash
  sudo nano /etc/selinux/config
  ```
  Altere a linha `SELINUX=enforcing` para:
  ```bash
  SELINUX=disabled
  ```

- Para definir como permissivo:
  ```bash
  SELINUX=permissive
  ```

##### **c) Regenerar o Contexto de Segurança (Opcional)**
Se preferir reativar o SELinux:
```bash
sudo touch /.autorelabel
sudo reboot
```
Isso força uma relabeling dos arquivos no próximo boot.

---

### **Considerações**
Desativar o SELinux deve ser a última opção, já que ele é uma camada importante de segurança. Prefira corrigir a causa do erro, reinstalando as políticas e ajustando permissões.

--- 

> **Nota:** Caso o sistema ainda não inicialize corretamente, analise os logs do sistema e a configuração do SELinux para identificar problemas adicionais.