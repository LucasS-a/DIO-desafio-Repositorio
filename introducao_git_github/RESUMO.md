# Introdução de Git/Github. 

O curso teve uma ótima abordagem, primeiro nos ensinando como o git trabalha para identificar alterações. Ele se utiliza do código de encriptação SHA1, 
pois qualquer alteração reflete em uma modificação do código gerado.

> echo "teste" | openssl sha1
>
> SHA1(stdin)= 9dc628289966d144c1a5fa20dd60b1ca1b9de6ed

Pequena alteração:

> echo "teste2" | openssl sha1
>
> SHA1(stdin)= ff0846a3fb101ac42bbb7577afa1d3b1bc4e266d

Mas lógico que ele não se utilizario desse recurso de uma forma tão genérica, e por isso que ele salva os arquivos no formato de um objeto chamdo *blob*.
O Git constrói um *cabeçalho* que começa com o tipo de objeto, neste caso, um *blob*. Depois, ele adiciona um espaço seguido do tamanho do conteúdo e
finalmente um byte nulo (\0). O Git concatena o cabeçalho e o conteúdo original e então calcula o checksum SHA-1 do novo conteúdo:

> content="teste"
> 
> header="blob 6\0"
> 
> store="$header$content"
> 
> echo -e $store | openssl sha1
>
> SHA1(stdin)= 6f16acbbc80d444145a09d897a93591cd806d3f8

Note que retornou a mesma chave hash.

> echo 'teste' | git hash-object -w --stdin
>
> 6f16acbbc80d444145a09d897a93591cd806d3f8

Mas como não existe projeto de um arquivo só ("graças a Deus"), e sim vários arquivos e diretórios. Para ter um registro de todos eles o git se utiliza 
de um objeto, conhecido como *tree*.

![objeto tree](./imagens/data-model-2.png)

Um único objeto tree contém uma ou mais entradas, cada uma contendo uma referência SHA-1 para um blob ou subtree com seu modo, tipo e nome de arquivo 
associados. Por exemplo, a tree mais recente em um projeto deverá se parecer com algo assim:

> git cat-file -p main^{tree}
> 
> 100644 blob 547f7b1b49e6cb439287bacd16cd09a61e810e63    README.md
> 
> 040000 tree 7feca6cc94f43773e8c4099d6afa9385f685fa6a    introducao_git_github

Agora o objeto mais importante do git, pois é ele que possibilita o git ser um software de controle de verção, pois ele irá apontar para o tree atual,
a tree anterior sem auteração, autor, uma menssagem e um timestamp.

![objeto commit](./imagens/object-commit.png)

Se dermos uma especionada em commit por meio de seu hash, teremos essa saída ("eu consegui a chave hash por meio do comando 'git log'."):

> git cat-file -p 31df60a38bc8ff0c78db8cc1bd0176dcdb7684e9
> 
> tree 01f0d6b2651325edaa370ec03560f2bfb99fac26
> 
> parent e210ba0bf814645c3e99a75117b9268524777162
> 
> author Lucas Souza de Aruajo <lucass.a.6991@gmail.com> 1738883695 -0300
> 
> committer Lucas Souza de Aruajo <lucass.a.6991@gmail.com> 1738883695 -0300

E essa estrutura que faz com que o git seje tão seguro, pois qualquer alateração por menor que seja, alterará o hash do *blob*, que por sua vez altera 
o hash da *tree* e que também alterará o hash do seu *commit*. 



