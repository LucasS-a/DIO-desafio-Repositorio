# Introdução de Git/Github. 

O curso teve uma ótima abordagem, primeiro nos ensinando como o git trabalha para identificar alterações. Ele se utiliza do código de encriptação SHA1, 
pois qualquer alteração reflete em uma modificação do código gerado.

> echo "teste" | openssl sha1
>
SHA1(stdin)= 9dc628289966d144c1a5fa20dd60b1ca1b9de6ed

Pequena alteração:

>echo "teste2" | openssl sha1
>
>SHA1(stdin)= ff0846a3fb101ac42bbb7577afa1d3b1bc4e266d

Mas lógico que ele não se utilizario desse recurso de uma forma tão genérica, e por isso que ele salva os arquivos no formato de um objeto chamdo *blob*.
O Git constrói um *cabeçalho* que começa com o tipo de objeto, neste caso, um *blob*. Depois, ele adiciona um espaço seguido do tamanho do conteúdo e
finalmente um byte nulo (\0). O Git concatena o cabeçalho e o conteúdo original e então calcula o checksum SHA-1 do novo conteúdo:

> content="teste"
> header="blob 6\0"
> store="$header$content"
> echo -e $store | openssl sha1
> SHA1(stdin)= 6f16acbbc80d444145a09d897a93591cd806d3f8

Note que retornou a mesma chave hash.

> echo 'teste' | git hash-object -w --stdin
>
>6f16acbbc80d444145a09d897a93591cd806d3f8

Mas como não existe projeto de um arquivo só ("graças a Deus"), e sim vários arquivos e diretórios. e para ter um registro de todos eles ele se utiliza 
de uma objeto,
conhecido como *tree*.

Um único objeto tree contém uma ou mais entradas, cada uma contendo uma referência SHA-1 para um blob ou subtree com seu modo, tipo e nome de arquivo 
associados. Por exemplo, a tree mais recente em um projeto deverá se parecer com algo assim: