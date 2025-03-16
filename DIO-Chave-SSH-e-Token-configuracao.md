# Guia Completo: Chave SSH para GitHub/GitLab

## O que é a Chave SSH?

A **chave SSH (Secure Shell Key)** é um método de autenticação segura usada para acessar servidores e repositórios remotos sem a necessidade de inserir senha a cada conexão. Ela utiliza criptografia assimétrica e funciona com um **par de chaves**:

1. **Chave privada (`id_rsa`)** → Fica armazenada localmente e nunca deve ser compartilhada.
2. **Chave pública (`id_rsa.pub`)** → Deve ser adicionada ao servidor remoto (GitHub, GitLab, etc.).

Quando um usuário tenta se conectar, o servidor verifica se a chave privada local corresponde à chave pública cadastrada. Se houver correspondência, a conexão é autorizada sem necessidade de senha.

---

## Para que Serve a Chave SSH?

A chave SSH é amplamente usada para:

- **Autenticação segura** em servidores remotos.
- **Acesso a repositórios Git** sem necessidade de digitar credenciais.
- **Automação de deploys** e outras tarefas de CI/CD.
- **Conexão remota com servidores** sem expor senhas.

Agora, vamos configurar uma chave SSH nos sistemas **Windows** e **Linux**.

---

## Configuração da Chave SSH no Windows

### Passo 1: Instalar o OpenSSH (se necessário)

No Windows 10/11, o OpenSSH já vem instalado. Para verificar:

```powershell
Get-Service -Name ssh-agent
```

Se não estiver instalado, ative o OpenSSH seguindo estes passos:

1. Vá até **Configurações > Aplicativos > Recursos opcionais**.
2. Busque por **OpenSSH Client** e **OpenSSH Server**.
3. Instale ambos, se necessário.

### Passo 2: Gerar a Chave SSH

Abra o **Prompt de Comando (cmd)** ou o **PowerShell** e execute:

```powershell
ssh-keygen -t rsa -b 4096 -C "seu-email@example.com"
```

- **`-t rsa`**: Usa o algoritmo RSA.
- **`-b 4096`**: Define um tamanho de 4096 bits para maior segurança.
- **`-C`**: Adiciona um comentário (normalmente seu e-mail).

Pressione **Enter** para aceitar o local padrão (`C:\Users\SeuUsuário\.ssh\id_rsa`).
Se desejar, defina uma senha para proteger a chave privada.

### Passo 3: Iniciar o Agente SSH e Adicionar a Chave

Habilite o serviço SSH-Agent para gerenciar suas chaves:

```powershell
Start-Service ssh-agent
ssh-add $HOME\.ssh\id_rsa
```

### Passo 4: Adicionar a Chave ao GitHub/GitLab

Copie a chave pública:

```powershell
Get-Content $HOME\.ssh\id_rsa.pub
```

No **GitHub**:

1. Vá para [GitHub SSH Keys](https://github.com/settings/keys).
2. Clique em **New SSH Key**.
3. Cole a chave no campo e salve.

No **GitLab**:

1. Vá para **Preferences > SSH Keys**.
2. Cole a chave e salve.

### Passo 5: Testar a Conexão

```powershell
ssh -T git@github.com
```

Se tudo estiver correto, a resposta será:

```powershell
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## Configuração da Chave SSH no Linux

### Passo 1: Verificar a Existência de uma Chave SSH

Abra o terminal e execute:

```bash
ls -al ~/.ssh
```

Se já houver arquivos como `id_rsa` e `id_rsa.pub`, você já tem uma chave SSH configurada.

Caso contrário, siga para o próximo passo.

### Passo 2: Gerar a Chave SSH

```bash
ssh-keygen -t rsa -b 4096 -C "seu-email@example.com"
```

Pressione **Enter** para manter o local padrão (`~/.ssh/id_rsa`).
Se desejar, defina uma senha para proteger a chave privada.

### Passo 3: Adicionar a Chave ao SSH-Agent

Inicie o agente SSH e adicione a chave:

```bash
eval $(ssh-agent -s)
ssh-add ~/.ssh/id_rsa
```

### Passo 4: Adicionar a Chave ao GitHub/GitLab

Copie a chave pública:

```bash
cat ~/.ssh/id_rsa.pub
```

No **GitHub**:

1. Vá para [GitHub SSH Keys](https://github.com/settings/keys).
2. Clique em **New SSH Key**.
3. Cole a chave e salve.

No **GitLab**:

1. Vá para **Preferences > SSH Keys**.
2. Cole a chave e salve.

### Passo 5: Testar a Conexão

```bash
ssh -T git@github.com
```

Se tudo estiver correto, a resposta será:

```bash
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## Conclusão

Agora sua chave SSH está configurada e pronta para ser usada tanto no **Windows** quanto no **Linux**. Com isso, você poderá autenticar em repositórios Git sem precisar digitar sua senha constantemente, garantindo mais segurança e praticidade no seu fluxo de trabalho.

Se tiver dúvidas, consulte a [documentação oficial do GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh).

# Guia Completo: Criação e Uso de Tokens de Acesso no GitHub/GitLab

## O que é um Token de Acesso?

Um **Token de Acesso** é uma credencial gerada para autenticar o usuário em serviços como **GitHub** e **GitLab** sem a necessidade de inserir nome de usuário e senha a cada operação. Ele substitui o uso de senhas em operações HTTPS e permite um controle granular de permissões.

### Por que Usar um Token?

- **Mais segurança**: Evita o uso de senhas em repositórios remotos.
- **Facilidade de automação**: Ideal para scripts, CI/CD e integrações.
- **Controle de permissões**: Define acesso específico para leitura, escrita, repositórios privados, etc.

---

## Como Criar um Token no GitHub

### Passo 1: Acessar a Configuração de Tokens

1. Acesse [GitHub Personal Access Tokens](https://github.com/settings/tokens).
2. Clique em **Generate new token**.

### Passo 2: Escolher Escopo e Permissões

- **Nota (Note):** Dê um nome descritivo ao token (ex: “Token para GitHub CLI”).
- **Expiration (Expiração):** Selecione uma duração ou escolha “No expiration” (não recomendado).
- **Scopes (Permissões):** Selecione permissões como:
  - `repo` → Acesso total a repositórios públicos e privados.
  - `workflow` → Para CI/CD com GitHub Actions.
  - `write:packages` → Se precisar gerenciar pacotes.

### Passo 3: Gerar e Copiar o Token

1. Clique em **Generate Token**.
2. **Copie o token** e armazene-o em um local seguro.

> **Atenção**: Após sair da página, você **não poderá ver o token novamente**. Se perder, terá que gerar outro.

### Passo 4: Usar o Token no Git

Se o Git pedir usuário e senha ao fazer um `git push`, use o **token** como senha:

```bash
git clone https://github.com/usuario/repo.git
Username: seu-usuario
Password: seu-token-gerado
```

Para evitar inserir o token toda vez, configure no Git:

```bash
git config --global credential.helper store
```

Depois da primeira autenticação, o Git armazenará o token localmente.

---

## Como Criar um Token no GitLab

### Passo 1: Acessar a Configuração de Tokens

1. Acesse [GitLab Access Tokens](https://gitlab.com/-/profile/personal_access_tokens).
2. Em **Name**, escolha um nome descritivo.
3. Defina uma **Data de Expiração** (opcional, mas recomendado).
4. Selecione as **permissões** necessárias:
   - `api` → Acesso total à API do GitLab.
   - `read_repository` → Somente leitura dos repositórios.
   - `write_repository` → Permite `push` de código.

### Passo 2: Criar e Copiar o Token

1. Clique em **Create Personal Access Token**.
2. **Copie e salve** o token gerado.

> **Atenção**: Assim como no GitHub, se perder o token, terá que gerar um novo.

### Passo 3: Usar o Token no Git

Se precisar clonar um repositório via HTTPS, use o token como senha:

```bash
git clone https://gitlab.com/usuario/repo.git
Username: seu-usuario
Password: seu-token-gerado
```

Para evitar inserir o token repetidamente:

```bash
git config --global credential.helper store
```

---

## Conclusão

Agora você sabe como criar e usar tokens de acesso para autenticação segura no **GitHub** e **GitLab**. Isso evita o uso de senhas e melhora a segurança ao trabalhar com repositórios privados e automações.

Se precisar de mais informações, consulte a [documentação oficial do GitHub](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) e do [GitLab](https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html).
