# Comandos

* git = comandos git
* git init = só cria a pasta .git, só isso
* git add = adiciona ao index
* git commit -m = commitar com mensagem
	* --amend = refaz o último commit, Serve pra corrigir a mensagem ou incluir algo que você esqueceu de dar `add`. Não edita o commit antigo, cria um commit novo com o hash diferente e o `main` passa a apontar pra ele. O antigo fica órfão. Cuidado: se o commit já foi pro GitHub, isso reescreve histórico e dá dor de cabeça.
* git config = arquivo de config dentro
* git status = lê a branch, commits e repository
* git branch = mostra as branches existentes e mostra a atual
* git switch = troca de uma branch para outra. Muda o `HEAD` e reescreve a work-tree e o index pro conteúdo daquela branch
	* -c = create, cria a branch e já troca para ela no mesmo comando
* git merge = junta o histórico de outra branch na branch atual, 
* git cat-file (flag) (arquivo) = inspecionar .git/objects
	* -t = type, fala o tipo
	* -p = pretty-print, mostra o conetúdo descomprimido.
* git ls-files = listar arquivos que estão no index
	* --stage = olunas extras: modo, hash e stage number. Sem ela sai só o caminho
	* --modified = modificados na work-tree
	* --deleted = apagados na work-tree
* git restore (arquivo) = o git lê o index, vê o hash pertencente ao arquivo, descomprime e escreve na work-tree
	* --staged = tirar do index
* git reset = move o ponteiro da branch pra outro commit
	* --soft =move só o ponteiro. Index e work-tree intactos, as mudanças ficam prontas pra commitar de novo
	* --mixed =padrão. Move o ponteiro e limpa o index. As mudanças continuam na work-tree, mas sem add
	* --hard = move o ponteiro, limpa o index e sobrescreve a work-tree. Trabalho não commitado é perdido de vez
* git log = Ler o histórico e enxergar a corrente de commits pelo campo parent.
* git diff = (work-tree contra index) sem flag, que eu editei e ainda não dei add.
	* --staged = (index contra HEAD) está preparado pro próximo commit.
	* HEAD (work-tree contra último commit) udo que mudou desde o último commit.
* git remote add = registra um endereço remoto / conecta o rep que ta no gh
	* origin = apelido dado a ele. É só convenção, poderia ser qualquer nome, mas todo mundo usa `origin` pro principal
	* -v = em ela, `git remote` mostra só o apelido `origin`. Com ela, mostra também as URLs.
<br>
* --system = todos usuários da máquina (arquivo: /etc/gitconfig , fica junto da instalação do git, no arch)
* --global = só o usuário x, em todos repositórios dele. (arquivo: ~/.gitconfig ou ~/.config/git/config , o meu é .gitconfig , home)
* --local = só aquele repositório específico, é o padrão quando não configura nada. (arquivo: .git/config)


# Exemplos:

git config --global user.name "nome"
git config --global user.email "x@gmail.com" <br>
(modificou config do repositório do usuário "nome" email "x@gmail.com")

git config --global init.defaultBranch main <br>
(seção init, chave defaultBranch, valor main. só afeta rep novos)
