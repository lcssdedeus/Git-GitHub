**O Git é projetado como um sistema distribuído e seguro por várias razões que envolvem tanto a forma como ele gerencia os dados quanto a maneira como ele organiza o histórico do repositório.**

1. Distribuição:

      Em um sistema distribuído, cada usuário possui uma cópia completa do repositório, incluindo seu histórico completo de commits, árvores de diretórios e Blobs. Isso significa que, ao contrário de sistemas centralizados, onde todas as informações ficam armazenadas em um único servidor, o Git permite que qualquer máquina participante tenha acesso total ao histórico de versão de todo o repositório. Se um repositório central falhar, as cópias em outras máquinas continuam a existir, tornando o sistema mais resiliente.

2. Controle de integridade com hashes criptográficos (SHA-1):

      Git usa SHA-1 (Secure Hash Algorithm) para gerar identificadores únicos para todos os objetos dentro do repositório (Blobs, Trees, Commits, etc.). Cada vez que o Git registra uma alteração, ele cria um hash para essa alteração, garantindo que qualquer modificação posterior que altere o conteúdo de um arquivo ou do histórico do repositório seja detectada imediatamente. Se alguém tentar alterar um commit ou um arquivo de forma maliciosa, o hash não corresponderá mais, e o Git detectará essa alteração. Isso garante integridade e segurança no versionamento.

3. Imutabilidade dos objetos:

      Uma característica essencial do Git é a imutabilidade dos objetos. Uma vez que um objeto (como um commit ou um Blob) é criado, ele não pode ser alterado. Se você tentar modificar um commit ou um arquivo diretamente, o hash do objeto muda, criando um novo objeto. Isso significa que o histórico de versões de um repositório nunca pode ser reescrito ou corrompido sem deixar evidências, o que ajuda a garantir a segurança e confiabilidade dos dados ao longo do tempo.

4. Distribuição e redundância:

      Git utiliza a redundância dos dados, já que todas as cópias do repositório são independentes e completas. Isso significa que, mesmo se um servidor central falhar, as outras cópias do repositório, armazenadas nas máquinas locais de diferentes desenvolvedores, garantem que nenhuma informação seja perdida. Isso torna o Git altamente resiliente a falhas de infraestrutura.

5. Autenticação e permissões:

      Embora o Git não imponha controles de acesso diretamente, a integração com plataformas como GitHub, GitLab ou Bitbucket oferece mecanismos robustos de autenticação (usando chaves SSH ou HTTPS) e controle de acesso. Isso garante que apenas usuários autorizados possam modificar o repositório, tornando o sistema ainda mais seguro contra acessos não autorizados.

**Em resumo, o Git é seguro por sua estrutura distribuída, a utilização de hashes criptográficos para verificar a integridade dos dados, a imutabilidade dos objetos e a redundância proporcionada pelas cópias locais de todos os repositórios. Isso torna o Git um sistema resistente a falhas e alterações maliciosas, garantindo a confiabilidade e a integridade do código em um contexto colaborativo.**