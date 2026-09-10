# Histórias
## master e main
Quando você cria um repositório, o Git precisa nomear a primeira branch. Esse nome sempre foi master.

Em 2020, a Software Freedom Conservancy (organização sem fins lucrativos que abriga projetos de software livre, e que hospeda o próprio Git) pediu a mudança do termo, reconhecendo que o nome inicial master ofende algumas pessoas.

LKKKKKKKKKKK

# organização de itens
## .git

**`description`**  
Praticamente inútil hoje. Serve só para o `gitweb`, uma interface web antiga do Git. Contém um texto placeholder que ninguém lê. Pode esquecer que existe.

**`hooks/`**  
Scripts que o Git executa automaticamente em momentos específicos, por exemplo antes de um commit ou depois de um merge. Agora está cheio de exemplos desativados, todos com extensão `.sample`. É a base de coisas como rodar teste automático antes de deixar você commitar. Assunto avançado, mas bom saber que existe.

**`info/`**  
Configurações auxiliares. O mais relevante ali é o arquivo `exclude`, que funciona como um `.gitignore` particular seu, não compartilhado com quem clonar o repositório.

**`config`**  
Aquele arquivo de nível `--local` que eu te mostrei na tabela dos três níveis. É este arquivo. Se você rodar `git config user.name "Outro"` aqui dentro, ele escreve aqui e sobrescreve o global só neste repositório.

**`objects/`**  
Aqui mora o repositório de verdade. É o banco de dados de conteúdo do Git, onde ficam guardados todo arquivo, toda pasta e todo commit que já existiram na história. Está vazio agora, e é literalmente por isso que a mensagem disse "empty repository".

O Git guarda tudo ali indexado por um hash SHA-1 (Secure Hash Algorithm 1, uma função que transforma qualquer conteúdo numa impressão digital de 40 caracteres hexadecimais). Você vai ver esses hashes o tempo todo, tipo `a3f7c9e`. São endereços de objetos dentro dessa pasta.

**`refs/`**  
Se `objects/` é o banco de dados, `refs/` é o índice de nomes. Nomes legíveis por humanos apontando para hashes. Toda branch e toda tag é apenas um arquivo aqui contendo um hash de 40 caracteres, mais nada. Uma branch no Git não é uma cópia do projeto nem uma pasta, é um ponteiro. Um arquivo de texto com um hash dentro. É por isso que criar branch no Git é instantâneo e não custa espaço, diferente de outros sistemas de versionamento.

**`HEAD`**  
O item mais importante e o menor de todos. Ele responde a uma pergunta: onde eu estou agora? É o que o Git consulta para saber em qual branch você está, e é o que muda quando você faz `git checkout` ou `git switch`.

Repare que `HEAD` está em maiúsculas. Isso não é decoração. No Git, referências em maiúsculas na raiz da `.git` são especiais, como `HEAD`, `ORIG_HEAD`, `FETCH_HEAD`. É uma convenção que distingue ponteiros do sistema de nomes que você criou.
## ciclo

Existem 4 tipos de objetos:
blob = tipo de arquivo, objeto; basicamente o que tem escrito dentro do txt.
tree = representa uma pasta, o conteúdo dela é uma lista de linhas, cada linha com: modo, tipo, hash e nome.
commit = 
tag = 

working tree ──add──> objects/ (blob gravado)
                 └──> index    (lista: caminho → hash)

index ──commit──> objects/ (tree + commit gravados)
              └──> refs/heads/main atualizado

Após o `git add`, o Git adiciona `blob <tamanho>\0<conteúdo>` ao objeto de tal arquivo novo no add e após isso, calcula o SHA-1 dessa string inteira (somente para criar o nome da pasta e arquivo e servir de indentificador) e comprime a string(blob) com zlib. O resultado comprimido é gravado em `.git/objects`, usando o hash como nome: 2 primeiros caracteres viram pasta, 38 restantes viram o arquivo.
Depois o `.git/index` é criado (ou atualizado, se já existir) com uma linha por arquivo: modo, hash do blob, stage number, caminho, mais metadados do arquivo na work-tree.
O index é binário, então se lê com `git ls-files --stage`.

Ex: git ls-files --stage
100644 96f73678a01fa027c9cf686657979a6690b831df 0	README.md
este comando serve para ler arquivos index e de forma detalhada.

No `git commit`, o Git leu o index e gravou uma tree em `.git/objects`, contendo modo, hash do blob e nome do arquivo. Depois gravou o objeto commit, que aponta pra essa tree e guarda autor, data, mensagem e o commit pai.

Sobre recuperação, se fizer algo errado, pode usar o `git restore (arquivo)`, ele faz com que o index procure esse arquivo, identifique o hash do objeto do arquivo, vai até .git/objects/ e procura pelo diretório com o objeto, descomprime ele, descarta o cabeçalho (`blob x\0`) e escreve o conteúdo na work-tree.
Por padrão o `restore` puxa do index. Pra puxar do último commit é `git restore --source=HEAD <arquivo>`.

# github

`git@github.com:luizgoneagain/git-lab.git` endereço no formato SSH. Usuário `git`, servidor `github.com`, e depois dos dois-pontos o caminho do repositório [[hardware-predator-phn16-72]]