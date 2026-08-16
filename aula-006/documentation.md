00:09
Agora vamos para um muito simples aqui que
é o grid-gap. Ele simplesmente vai definir o

00:13
gutter. Sabe quando a gente criou lá um arquivo
de grid, coisa do tipo que a gente

00:17
botava lá uma margem de 10 pixels etc. Grid-gap
vai criar isso. Então botei aqui, olha, grid-

00:21
gap: 20px; e já definiu de 20. Uma breve
atualização do CSS Grid Layout 2020. É a

00:27
seguinte, a propriedade grid-gap que você vai ver
aí pelo curso, você viu um pouco antes

00:33
também escrito grid-gap, agora é só gap o nome.
Só isso que mudou. Se você ver até aqui,

00:40
eu esqueci de mudar o nome. Eu atualizei aqui
esse documento, mas esqueci de mudar o nome

00:44
que estava escrito grid-gap. O que acontece é
o seguinte, os browsers vão continuar dando

00:48
suporte a isso daqui, porque já tem código
antigo que já está utilizando o nome escrito

00:54
grid-gap. Eles não vão simplesmente cortar isso do nada
e aí vai quebrar tudo o que é

00:58
layout que estava usando esse nome. Mas teve
uma atualização na especificação final do CSS

01:03
Grid Layout e eles decidiram que o nome
deveria ser só gap. Então quando a gente

01:07
quiser dar um espaçamento aí, vai ser só
escrever gap, tanto os pixels ou column-gap ou

01:13
row-gap também. E aí fique atento, porque durante
o curso eu não vou mudar todas as

01:18
aulas do curso só porque isso mudou. Durante as
aulas do curso eu vou estar escrevendo grid-

01:22
gap, mas você simplesmente tire o grid e o tracinho
e deixe só o gap escrito. E eu posso

01:28
definir também o gap específico da coluna e
o gap específico das linhas. Eu posso separar,

01:34
então eu tenho column-gap e o row-gap. Vamos
ver lá como que é. Então primeiro

01:41
aqui, eu tenho aqui o grid normal, o display:
grid;. Eu tenho aqui esse grid normal que eu

01:47
criei, grid-template. E depois, tem duas classes aqui.
E agora aqui o grid-gap. Se eu

01:54
tirar o grid-gap, olha só, ele fica tudo
grudado. Só lembrando, nesse exemplo aqui, eu

02:00
tirei do meu item a margem de 5 pixels que eu
tinha. Se eu botar a margem de 5 pixels aqui no

02:07
meu item, margin: 5px;, olha só, ele tem
aqui esse gutter que está sendo criado,

02:16
ele está sendo criado pela margem do item. Só
que você não precisa usar aqui. Você pode

02:22
criar, ele vai ser até mais consistente se
você criar aqui direto. Vai ser mais simples

02:26
de você manter direto aqui no grid-gap. Mas eu posso
ter o grid-gap e ele vai somar. Ele vai

02:33
ter agora 20 pixels entre os elementos, mais aqueles
5 de cada um. Então só aumenta aqui,

02:38
olha, lá no item. Só aumentar aqui para 10,
está vendo? Olha, que ele vai somando. E

02:44
aí uma coisa interessante, o grid-gap, ele é interno,
está vendo? Então se você ver aqui as

02:50
partes externas, essa distância aqui está sendo
definida apenas pela margem daqui. Se eu

02:56
tiro a margem, você vê que eles ficam
grudadinhos, está vendo? Na ponta do grid, o

03:00
que é bom, porque aí você pode definir a área
externa mesmo do seu item. Então no grid, você

03:06
pode, sei lá, colocar um padding, 20 pixels. Está
vendo? Agora ele criou a mesma área que

03:13
divide o grid aqui. Você pode botar uma
margem para separar esse grid, não importa, entendeu?

03:20
É o que você quiser. Então o grid gap é aqui
e aí você pode colocar. Ideal sempre é usar a

03:27
unidade fixa com pixel, pode ser o em também,
1em, entendeu? 2em, coisa tipo, ou para mim

03:35
sempre vai ser pixel gap, nunca vou mudar de
pixel. Porque lembrando que pixel não é pixel,

03:41
pixel é uma unidade específica de CSS, pixel. Mas
tudo bem. Então está aqui e aí eu posso

03:48
definir também um do lado do outro, olha, 20 pixels,
20 pixels, é a mesma coisa agora, 20 e

03:53
10, olha o que ele faz. O primeiro então é
o gap aqui entre as minhas linhas, está vendo? E

04:00
o segundo é o gap entre as colunas. Então
posso botar aqui 5, está vendo? Ele vai

04:06
diminuindo, eu posso botar um valor maior, 30. E
aí esse daqui eu botei só, está vendo?

04:14
column-gap e eu botei o grid gap de 2
pixels só para ver, só para diferenciar aqui um

04:18
pouquinho, mas eu posso tirar, está vendo? Então
column, só da coluna, ele está dividindo

04:23
as colunas. E aí você tem que saber que isso
daqui é 20, entendeu? Isso aqui não é 20 para

04:29
cada lado não, 20 mais 20, 40 não, de cada item
não. Isso aqui é o gap entre a coluna em

04:35
si inteiro, entendeu? Então se você tem, geralmente
você tinha um item lá com margem

04:41
left, no Bootstrap acho que eles usam 15 pixels,
ou eles usam 1em, 1em, doido lá, mas

04:47
não importa, mais ou menos 15 pixels que eles
usam. Então no Bootstrap o grid column gap

04:52
deles é 30, porque é 15 para cada lado,
entendeu? Então é 30. No caso eu uso

04:57
geralmente 10, então no meu caso é 20, está vendo?
Você vê que o 10 é pequenininho ali. E

05:03
dá para usar aqui também o row-gap. E aí aqui
mais uma vez eu quero mostrar só para vocês

05:09
que a margem nos itens ela influencia. Olha
aqui, deixa eu ver aqui. Então eu peguei

05:16
alguns itens daqui, eu coloquei uma tag
chamada margem, botei a margem aqui nesse

05:19
advert, e olha só o que acontece. Ele cria o
gap aqui normal, e agora o item está aqui. Se

05:26
eu tirar essa margem daqui toda, que eu
defini, olha o item está aqui, certinho, isso

05:30
aqui são os gaps de 20 pixels que eu defini.
Agora, se eu boto aqui nesse item aqui que eu

05:37
defini margem no advert, se eu boto margin-top de
50 pixels, olha o que ele faz, ele criou

05:45
aqui, olha, margin-top de 50, isso aqui é 50.
E ainda tem o gap aqui, entendeu? Então ele

05:51
vai encolhendo o tamanho da célula, do conteúdo
ali, mas ela está aqui ainda, nada

05:56
Está vindo para aqui, nada, isso aqui ainda existe
como se fosse um valor vazio, é um

06:01
valor fantasma aqui que nada vai influenciar. Margem
left, posso botar aqui 10 pixels, ele

06:08
diminui aqui, se eu botar o right, ele vai diminuir
aqui do lado. E aqui, olha só, eu não

06:15
boto o right 50 pixels, ele vai chegar para
aqui, 50 pixels, ele sumiu com o item, porque

06:21
não dá mais. Olha o 30, está vendo, ele
vai diminuindo para aqui, ele nunca vai chegar lá

06:26
e influenciar nos outros itens do grid, os outros
itens do grid não estão nem aí para a

06:30
margem aqui do restante. O que é diminuído ali,
o que diminui é a célula, a margem

06:35
diminui a célula, o conteúdo em relação à
célula. Então é isso daí contra o GridViewer.