---
title: Anotações GIT
date: 2019-08-01 16:10:00 Z
---

# Anotações do curso GIT e GITHUB

Os repositórios do tipo BARE são usados para CENTRALIZAR o trabalho de diversos outros repositórios que enviam objetos , ele propriamente não possui Working Dir e não permite trabalhar diretamente nele .

## Etapas do fluxo de trabalho

O working directory são os arquivos que visualizamos ao navegar em nosso diretório usando o navegador de arquivos do nosso sistema operacional.
1 - Modificamos os arquivos no nosso diretório local (working directory)

A medida que vamos trabalhando vamos adicionando essas modificações para a área de staging e por fim podemos enviar para o repositório do git. 
2 - Colocamos essas modificações em uma área de staging
3 - Movemos toda a área de staging na forma de commit no repositório

É possível fazer também o fluxo contrário e alterar os arquivos do disco com o conteúdo do repositório, chamando esse processo de checkout.

Usamos o comando git status para analisar as diferenças entre o que está no staging com nosso repository e o working directory.

## Mudanças de estados entre arquivos

Exemplo:
    
    working dir   |  staging   | repository
                  |            |
    Cenoura       |  Cenoura   | Cenoura
    Batata        |  Batata    | Batata
    Beterraba     |  Beterraba | Beterraba
    Rabanete      |            |

Executando git add
    
    working dir   |  staging   | repository
                  |            |
    Cenoura       |  Cenoura   | Cenoura
    Batata        |  Batata    | Batata
    Beterraba     |  Beterraba | Beterraba
    Rabanete     >>> Rabanete  |        

Executando git commit
    
    working dir   |  staging    |  repository
                  |             |
    Cenoura       |  Cenoura    |  Cenoura
    Batata        |  Batata     |  Batata
    Beterraba     |  Beterraba  |  Beterraba
    Rabanete      |  Rabanete  >>> Rabanete        

## Ações dos comandos no fluxo básico
![Screenshot_20191201_224637.png](/uploads/Screenshot_20191201_224637.png)

## Comandos utilizados

##### Listar as configurações existentes
git config --list

##### Configurar nome e e-mail
git config --global user.name MartinsMessias
git config --global user.email messiasads2017@gmail.com

##### Criar chave SSH
ssh-keygen -t rsa -b 4096 -C "messiasads2017@gmail.com"
ssh-add ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub

##### Cria o repositório no diretório atual
git init

##### Cria um repositório bare
git init --bare

##### Sincronizando com o GitHub
git remote add origin https://github.com/MartinsMessias/outro_repo.git
git push -u origin master

##### Clonando um repositório remoto
git clone git@github.com:treinaweb-git/primeiro-repo.git

##### Adiciona arquivos ao staging
git add

##### Cria um commit diretamente com todos os arquivos modificados
git commit -m "First Commit"
git commit –a  # Manda direto pro repo local

##### Adiciona alterações no commit anterior (use com cuidado)
git commit –amend

##### Envia as alterações para o repositório remoto, onde origin é o remote e master o branch
git push origin master

##### Baixa commits do repositório remoto
git pull origin

##### Atualiza as referências com um repositório remoto
git fetch origin

_____________________________________________________________________________
##### Path do gitignore global
$HOME/.config/git/ignore

##### gitignore local
.gitignore

### Patterns avançados

\# - A cerquilha marca um comentário em nosso arquivo
\- Ignora partes do nome de um pattern, agindo como um coringa
! - Permite que um determinado pattern seja excluído. Geralmente vem acompanhado de outra regra mais genérica
/ - Tem função diferente dependendo da sua localização. No início da linha, ignora um pattern somente naquele diretório, mas não em seus subdiretórios. No final ignora toda a árvore do diretório
\ - A barra invertida atua como um caractere de escape
\** - Dois asteriscos em seguida são usados para ignorar um pattern específico dentro de uma árvore de diretórios

Para ficar mais claro, veja alguns exemplos:

##### Ignora todos os arquivos html do repositório
*.html

##### Exceto do arquivo index.html
!index.html

##### Ignora o README no diretório atual, mas permite subdir/README
/README

##### Ignora todos os arquivos no diretório build/
build/

##### Ignora os arquivos .txt do diretório doc/, mas permite subdiretórios (doc/server/arch.txt)
doc/*.txt

##### Ignora todos arquivos .pdf na árvore doc/
doc/**/*.pdf

_____________________________________________________________________________

### Branch

Permitem ramificar o repositório.

As branches são simplesmente ponteiros para um commit (snapshot) em nosso repositório. 
Logo a criação de muitas branches não influência no tamanho do repositório tampouco no seu desempenho.
Um commit não pertence a uma branch, pois o mesmo é só uma referência a uma hierarquia de snapshots, que podem ser combinados de volta com a branch de origem (merge). 

##### Criar uma nova branch
git nova-branch

##### Alterar entre as branches
git checkout master

##### Voltar para a branch anterior
git checkout -

##### Remover uma branch local
git branch -d branch

##### Remover a branch remota
git push --delete origin branch

_____________________________________________________________________________

### Times de dev

Num projeto de software, alguns dos papéis que podemos apontar são:

Developer: é o responsável pelo desenvolvimento do software. Seu papel é enviar commits constantes no código, a fim de adicionar funcionalidades de forma incremental, mesmo que em partes ainda incompletas.

Tester: a pessoa responsável por manter a qualidade do projeto, avaliando e testando o código gerado. Pode ser uma pessoa dedicada ou mesmo a equipe de desenvolvimento, caso a mesma trabalhe com alguma metodologia orientada a testes (TDD, BDD).

Bug Hunter: a partir do momento que um bug é reportado, é preciso encontrar a raiz do problema. A pessoa com esse papel precisa entender o porquê determinado trecho de código foi alterado e separar qual o commit introduziu a mudança. Para isolar esse problema, é importante que o histórico de commits estejam organizados e com commits atômicos.

Package Builder: Depois que uma funcionalidade é desenvolvida, podemos ter passos extras antes de enviar um código para produção, como minificação de css, inclusão de dependências externas, entre outros. É o papel do Package Builder preparar esses arquivos que serão enviados para produção.

Deployer: Após gerado um package de alterações, é preciso que esse seja para o ambiente de produção. O papel do deployer é preparar os ambientes que vão receber esse novo código. Muitas vezes essa função pode ser automatizada, juntamente com o Package Builder.

Integrador: Algumas vezes precisamos restringir o acesso das branches permanentes para garantir sua integridade. O Integrador tem como papel fitrar e avaliar os commits enviados pelos developers antes de fazer o merge com os branches permanentes, executando verificações das funcionalidades.

Nota: Embora sejam vários papéis, os mesmos podem ser executados por uma mesma pessoa, cada hora agindo de acordo com um papel.

_____________________________________________________________________________

### Tipos de Branches
Depois de conhecer os papéis da equipe, temos que conhecer um pouco sobre os tipos de branches que estamos manipulando. Podemos dividir essas branches em algumas categorias.

Branches Pública e Local: Quando criamos uma branch em nosso repositório local, podemos ou não enviá-la para um repositório remoto. Caso essa branch seja enviada para outro repositório ela passa a ser uma branch pública. Por ser uma branch onde outros usuários podem basear seus trabalhos, temos que tomar cuidado ao modificar o histórico dos commits.

Temporárias ou Permanentes: Na questão de duração de uma branch, ela pode ter sido criada para uma funcionalidade específica para depois ser combinada ou essa branch pode existir durante todo o ciclo de vida do projeto, dependendo da estratégia adotada. Um exemplo de uma branch permanente comum é a branch master e cada funcionalidade pode ser criada numa branch temporária, como uma feature/*.

Outro ponto importante é a forma como combinamos essas branches. Temos duas formas para isso, usando um workflow baseado em merge ou baseado em rebase, como veremos a seguir.
_____________________________________________________________________________

### Merge
No git, a ação de combinar dois branches se chama merge. Ela pode ser efetuada a partir do comando git merge <branch>:


##### Entra na branch develop
git checkout develop

##### Combina a branch feature/a com develop
git merge feature/a

Dependendo dos commit das duas branches, na hora de efetuar o merge podemos ter duas situações: 
    um merge do tipo fast forward ou outro com um commit de merge (3-way merge).

##### Fast Forward
O merge por fast forward simplesmente move a referência da branch que estamos trabalhando com o HEAD da branch a ser combinada.
Isso gera um histórico de commits linear.

##### 3-Way Merge
Mas em alguns casos não será possível combinar as branches com fast forward, principalmente se a branch corrente tiver novos commits que não fazem parte da branch a ser combinada. Nessas situações o git utiliza um commit extra que junta essas duas branches distintas. Esse tipo de merge é conhecido também como 3-way merge pois ele utiliza 3 commits para gerar o merge, o HEAD das duas branches e o commit de merge que junta essas duas branches.

### Tipos de fluxos
Existem basicamente dois tipos de fluxos que podemos utilizar, o fluxo de merge e o fluxo de rebase.

O fluxo de merge é o que utiliza o 3-Way Merge, ou seja, toda vez que um branch é margeado ele adiciona um terceiro commit referente a união de 2 branches.
Já o fluxo de rebase apenas um commit é criado sem manter o histórico da existência de uma outra branch, o que também é conhecido como histórico linear.

