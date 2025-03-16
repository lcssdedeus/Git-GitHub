# Objetos internos do Git

Explicando de forma detalhada e técnica os três tipos fundamentais de objetos que o Git utiliza para versionamento de código: Blobs, Trees, e Commits. Esses objetos formam a espinha dorsal do funcionamento do Git, e entender cada um deles de forma detalhada pode ser fundamental para uma compreensão avançada, especialmente para quem já está caminhando para um estágio ou posição júnior.

## 1. Blobs (Binary Large Objects)
O que é? O Blob é o objeto básico que contém o conteúdo real de um arquivo no Git. O Git armazena os arquivos do seu repositório como blobs. Quando você faz um commit, o Git não armazena o conteúdo dos arquivos de forma tradicional, como em um sistema de arquivos comum. Ele cria uma representação comprimida do conteúdo de cada arquivo e armazena esse conteúdo como um Blob.

Como funciona? O Git gera um hash SHA-1 para o conteúdo de cada arquivo. Esse hash serve como um identificador único para o Blob. O Git então armazena esse conteúdo em sua base de dados interna. O importante é que blobs não contêm informações sobre o nome do arquivo ou sobre o diretório onde o arquivo está. Eles apenas armazenam os dados brutos do arquivo.

- Exemplo de fluxo:
1. Suponha que você tem um arquivo de texto chamado `exemplo.txt` com o conteúdo "Olá, Git!".
2. Quando você faz um commit, o Git cria um Blob para o conteúdo de `exemplo.txt` e gera um hash SHA-1 único para esse conteúdo, como `20ab8e92c82....`
3. Esse Blob é armazenado no repositório e, no futuro, quando precisar recuperar o arquivo, o Git usará o hash para acessar esse conteúdo.
- Importância: Blobs são essenciais porque são os responsáveis por armazenar o conteúdo de seus arquivos de forma eficiente e comprimida. Como Git é um sistema distribuído, os Blobs tornam o versionamento de grandes quantidades de código ou arquivos pesados muito mais eficaz, sem a necessidade de armazenar cópias inteiras dos arquivos sempre que há uma mudança.

## 2. Trees (Árvores)
O que é? As Trees são objetos que representam a estrutura de diretórios do repositório. Cada Tree contém referências para outros objetos, que podem ser Blobs (arquivos) ou outras Trees (subdiretórios). A Tree é um objeto que organiza e referencia os Blobs e outras Trees de forma hierárquica.

- Como funciona? Quando você faz um commit, o Git cria uma Tree que contém os hashes dos arquivos (Blobs) e dos diretórios (ou subdiretórios). Cada item na Tree é associado a um tipo de objeto (se é um Blob ou uma Tree) e ao nome do arquivo ou diretório.

- Exemplo de fluxo:
1. Suponha que você tenha a seguinte estrutura de diretórios:
```
pasta/
└── exemplo.txt
```
2. O Git cria uma Tree para a pasta `pasta`, e dentro dessa Tree, ele faz referência ao Blob do arquivo `exemplo.txt`.
3. Essa Tree será armazenada no Git como uma forma de estruturar os objetos de forma hierárquica. Quando você realiza um commit, a estrutura dos diretórios é organizada por Trees.
- Importância: As Trees são cruciais para manter a organização do seu repositório. Elas permitem que o Git gerencie a estrutura do projeto de forma eficiente e rápida, facilitando operações como `git status`, `git diff`, e `git checkout`, onde o Git precisa saber a estrutura completa de diretórios para verificar as mudanças.

## 3. Commits (Commits)
O que é? O Commit é o objeto que representa um ponto específico na história do repositório. Ele é a "fotografia" do estado atual do repositório em determinado momento, incluindo a referência para uma Tree (a estrutura do diretório) e metadados como o autor, data, mensagem do commit, entre outros.
- Como funciona? Quando você faz um commit, o Git cria um objeto de commit que contém:
  - A referência para a Tree que representa o estado dos arquivos.
  - O hash do commit anterior (se houver).
  - O nome do autor, data e a mensagem do commit.

Esse commit serve como um "marco" no repositório, permitindo que você navegue pelo histórico de mudanças. Cada commit tem um identificador único, um hash SHA-1, que torna possível identificar e acessar esse commit específico.
- Exemplo de fluxo:
1. Após a criação de uma Tree (que representa a estrutura de diretórios e arquivos), você faz um commit.
2. O commit inclui o hash da Tree, o hash do commit anterior (se houver), e outras informações como autor e mensagem.
3. O commit é armazenado no Git e torna-se um ponto imutável no histórico do repositório.
- Importância: O Commit é o principal "bloco de construção" do histórico no Git. Ele permite o versionamento, o rastreamento de alterações e a colaboração entre múltiplos desenvolvedores. Sem ele, seria impossível reverter mudanças ou entender a evolução do projeto ao longo do tempo.

## Resumo da Estrutura Interna do Git
1. Blobs: Armazenam o conteúdo real dos arquivos (sem estrutura de diretórios).
2. Trees: Organizam e referenciam Blobs e outras Trees, representando a estrutura de diretórios.
3. Commits: Representam snapshots do repositório, associando uma Tree ao estado do projeto, além de registrar metadados.

## Como esses objetos interagem no Git
- Fluxo de trabalho típico:
  - Você faz alterações em arquivos (Blobs).
  - Essas alterações são registradas em uma Tree.
  - Quando você faz um commit, o Git cria um objeto Commit que faz referência à Tree mais recente, permitindo que o histórico de mudanças seja mantido.
- Armazenamento eficiente: Git utiliza essas estruturas para manter a eficiência do repositório, armazenando apenas os Blobs que mudaram e utilizando hashes para garantir a integridade e a consistência dos dados.

## Conclusão
Embora esse nível de detalhamento não seja necessário para um desenvolvedor júnior no início da carreira, entender os conceitos por trás dos objetos internos do Git pode ser extremamente útil para otimizar o uso da ferramenta e entender como ela realmente funciona por baixo dos panos. A longo prazo, esse conhecimento é valioso para a resolução de conflitos, recuperação de versões, e até para personalizações de fluxos de trabalho em projetos mais complexos.