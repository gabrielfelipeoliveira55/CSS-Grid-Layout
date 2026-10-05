````markdown
# CSS Grid Layout — `grid-area`

> **Objetivo:** compreender `grid-area` como shorthand de posicionamento de um grid item, entender a ordem dos quatro valores, trabalhar com `grid-template-areas`, nomes de áreas, linhas nomeadas, expansão, sobreposição de itens e a relação com `z-index`.

---

# 1. O que é `grid-area`?

A propriedade:

```css
grid-area
````

é uma forma abreviada (**shorthand**) utilizada para posicionar um `grid item` dentro do Grid.

Ela reúne quatro propriedades:

```css
grid-row-start
grid-column-start
grid-row-end
grid-column-end
```

Portanto:

```css
grid-area: 2 / 3 / 4 / 5;
```

é equivalente a:

```css
grid-row-start: 2;
grid-column-start: 3;
grid-row-end: 4;
grid-column-end: 5;
```

A documentação do MDN define `grid-area` como o shorthand responsável por especificar o tamanho e a localização da área de um grid item.

---

# 2. A primeira coisa que você precisa memorizar

A ordem dos quatro valores é:

```text
1º → grid-row-start
2º → grid-column-start
3º → grid-row-end
4º → grid-column-end
```

Visualmente:

```text
grid-area:
     1       2       3       4
     │       │       │       │
     ▼       ▼       ▼       ▼

   row     column    row    column
  start     start    end      end
```

Ou:

```text
┌──────────────────────────────────────┐
│                                      │
│  1 = ROW START                       │
│  2 = COLUMN START                    │
│  3 = ROW END                         │
│  4 = COLUMN END                      │
│                                      │
└──────────────────────────────────────┘
```

Essa é provavelmente a parte mais importante de `grid-area`.

A especificação e a documentação técnica utilizam exatamente essa ordem.

---

# 3. Por que a ordem parece estranha?

Ao encontrar:

```css
grid-area: 2 / 3 / 5 / 6;
```

é comum pensar:

```text
top / right / bottom / left
```

como fazemos mentalmente com algumas propriedades de box model.

Mas `grid-area` não segue essa sequência.

A ordem é:

```text
ROW START
COLUMN START
ROW END
COLUMN END
```

No modo de escrita mais comum, da esquerda para a direita, isso pode ser visualizado como:

```text
       ROW START
            ↓
      ┌───────────────┐
      │               │
      │      ITEM     │
      │               │
      └───────────────┘
            ↑
         ROW END

COLUMN START → lado esquerdo
COLUMN END   → lado direito
```

Portanto:

```text
grid-area
=
row-start / column-start / row-end / column-end
```

Essa ordem é documentada pelo MDN como uma das diferenças que mais confundem quem está começando com a propriedade.

---

# 4. `grid-area` utiliza grid lines

Antes de usar `grid-area` numericamente, precisamos lembrar como funciona o Grid.

Imagine:

```text
             COLUNAS
        1        2        3
        │        │        │
   1 ───┼────────┼────────┼───
        │        │        │
   2 ───┼────────┼────────┼───
        │        │        │
   3 ───┼────────┼────────┼───
        │        │        │
   4 ───┼────────┼────────┼───
```

Aqui temos:

```text
4 grid lines horizontais
4 grid lines verticais
```

As regiões entre essas linhas são as tracks.

Portanto:

```text
GRID LINE
    ↓
delimita

GRID TRACK
    ↓
ocupa o espaço entre linhas
```

`grid-area` utiliza justamente essas linhas para determinar os limites da área do item.

---

# 5. Exemplo mais simples

Considere:

```css
.grid {
  display: grid;

  grid-template-columns:
    repeat(3, 1fr);

  grid-template-rows:
    repeat(3, 100px);
}
```

Temos:

```text
             1       2       3       4
             │       │       │       │
        1 ───┼───────┼───────┼───────┼
             │       │       │       │
        2 ───┼───────┼───────┼───────┼
             │       │       │       │
        3 ───┼───────┼───────┼───────┼
             │       │       │       │
        4 ───┼───────┼───────┼───────┼
```

Agora:

```css
.item {
  grid-area: 2 / 2 / 4 / 4;
}
```

Leia:

```text
row-start    = 2
column-start = 2
row-end      = 4
column-end   = 4
```

---

# 6. Visualizando `grid-area: 2 / 2 / 4 / 4`

```text
             coluna
          1       2       3       4
          │       │       │       │
     1 ───┼───────┼───────┼───────┼──
          │       │       │       │
     2 ───┼───────┼───────┼───────┼──
          │       │███████│███████│
          │       │███████████████│
     3 ───┼───────┼████ ITEM █████┼──
          │       │███████████████│
     4 ───┼───────┼───────┼───────┼──
```

O item ocupa:

```text
2 column tracks
×
2 row tracks
```

Porque:

```text
row:
4 - 2 = 2

column:
4 - 2 = 2
```

Portanto:

```text
2 × 2
```

---

# 7. Como pensar nos quatro valores

Quando encontrar:

```css
grid-area: 2 / 3 / 5 / 6;
```

não tente memorizar como uma sequência de números.

Separe:

```text
2 → ROW START
3 → COLUMN START
5 → ROW END
6 → COLUMN END
```

Visualmente:

```text
             ROW START
                  ↓
          ┌────────────────┐
          │                │
          │                │
          │      ITEM      │
          │                │
          │                │
          └────────────────┘
                         ↑
                      ROW END

COLUMN START → esquerda
COLUMN END   → direita
```

---

# 8. Equivalência com as propriedades individuais

Este código:

```css
.item {
  grid-area: 2 / 3 / 5 / 6;
}
```

equivale a:

```css
.item {
  grid-row-start: 2;
  grid-column-start: 3;
  grid-row-end: 5;
  grid-column-end: 6;
}
```

Portanto:

```text
grid-area
    ↓
┌──────────────────────────────┐
│ grid-row-start               │
│ grid-column-start            │
│ grid-row-end                 │
│ grid-column-end              │
└──────────────────────────────┘
```

---

# 9. `grid-area` não controla apenas uma direção

Diferentemente de:

```css
grid-row
```

e:

```css
grid-column
```

`grid-area` pode controlar os dois eixos simultaneamente.

Temos:

```text
grid-row
↓
ROW

grid-column
↓
COLUMN

grid-area
↓
ROW + COLUMN
```

Podemos imaginar:

```text
grid-row
    ↓
posição vertical

grid-column
    ↓
posição horizontal

grid-area
    ↓
área completa
```

---

# 10. Exemplo completo

```html
<div class="grid">
  <div class="item">Item</div>
</div>
```

```css
.grid {
  display: grid;

  grid-template-columns:
    repeat(4, 1fr);

  grid-template-rows:
    repeat(4, 100px);
}

.item {
  grid-area: 2 / 2 / 4 / 4;
}
```

Visualmente:

```text
┌───────┬───────┬───────┬───────┐
│       │       │       │       │
├───────┼───────┼───────┼───────┤
│       │       │       │       │
│       │       │       │       │
├───────┼───────┼───────┼───────┤
│       │       │       │       │
│       │       │       │       │
├───────┼───────┼───────┼───────┤
│       │       │       │       │
└───────┴───────┴───────┴───────┘
```

O item ocupa a região entre:

```text
row 2 → row 4
column 2 → column 4
```

---

# 11. A propriedade também aceita `auto`

Podemos escrever:

```css
grid-area: auto;
```

Nesse caso, a propriedade contribui com posicionamento automático.

Também podemos misturar valores automáticos:

```css
grid-area: auto / auto / auto / auto;
```

`auto` significa, de forma simplificada:

```text
"deixe essa parte do posicionamento ser resolvida automaticamente"
```

A especificação também trata `auto` como contribuição de `span` padrão de uma track quando necessário.

---

# 12. Usando apenas dois valores

Também existem formas abreviadas.

Por exemplo:

```css
grid-area: 2 / 3;
```

Nesse caso:

```text
row-start    = 2
column-start = 3
```

As demais partes são resolvidas pelas regras de valor padrão da propriedade.

Não devemos interpretar:

```text
2 / 3
```

como:

```text
row 2 até row 3
```

porque `grid-area` possui uma ordem própria.

---

# 13. Formato completo

A forma mais clara para estudar os quatro valores é:

```css
grid-area:
  row-start /
  column-start /
  row-end /
  column-end;
```

Exemplo:

```css
grid-area: 2 / 3 / 5 / 6;
```

Visualmente:

```text
              3
              ↓
      ┌───────────────┐
      │               │
  2 → │      ITEM     │ ← 6
      │               │
      └───────────────┘
              ↑
              5
```

Aqui:

```text
2 = row-start
3 = column-start
5 = row-end
6 = column-end
```

---

# 14. Como calcular o tamanho da área

Depois de entender os quatro valores, podemos calcular quantas tracks o item ocupa.

Exemplo:

```css
grid-area: 2 / 3 / 6 / 7;
```

No eixo das rows:

```text
6 - 2 = 4
```

Então:

```text
4 row tracks
```

No eixo das colunas:

```text
7 - 3 = 4
```

Então:

```text
4 column tracks
```

Resultado:

```text
4 × 4
```

Portanto:

```text
row-span    = row-end - row-start
column-span = column-end - column-start
```

---

# 15. `span` dentro de `grid-area`

Assim como vimos em `grid-row` e `grid-column`, `grid-area` também pode utilizar `span`.

Exemplo:

```css
grid-area: 2 / 3 / span 2 / span 3;
```

Interpretando:

```text
ROW START    = 2
COLUMN START = 3

ROW SPAN     = 2
COLUMN SPAN  = 3
```

Ou seja:

```text
começa na row line 2
ocupa 2 row tracks

começa na column line 3
ocupa 3 column tracks
```

---

# 16. Visualizando `span`

```text
             1       2       3       4       5       6
             │       │       │       │       │       │
        1 ───┼───────┼───────┼───────┼───────┼───────┼
             │       │       │       │       │       │
        2 ───┼───────┼───────┼████████████████████───┼
             │       │       │████████████████████   │
             │       │       │████████████████████   │
        3 ───┼───────┼───────┼████████████████████───┼
             │       │       │████████████████████   │
             │       │       │████████████████████   │
        4 ───┼───────┼───────┼───────────────────────┼
```

A área:

```css
grid-area: 2 / 3 / span 2 / span 3;
```

ocupa:

```text
2 rows
×
3 columns
```

---

# 17. `grid-area` também pode representar uma área nomeada

Aqui está uma das partes mais importantes da aula.

Além de receber linhas numéricas, `grid-area` pode receber um nome:

```css
grid-area: header;
```

Mas esse código possui um significado diferente da forma numérica.

Quando usamos:

```css
grid-area: header;
```

estamos fornecendo um `<custom-ident>`.

Esse identificador pode representar uma área nomeada definida por:

```css
grid-template-areas
```

A documentação do MDN destaca justamente essa segunda função da propriedade.

---

# 18. `grid-template-areas`

Podemos criar um layout:

```css
.grid {
  display: grid;

  grid-template-columns:
    repeat(3, 1fr);

  grid-template-rows:
    repeat(3, 100px);

  grid-template-areas:
    "header header header"
    "nav    content aside"
    "footer footer footer";
}
```

Visualmente:

```text
┌─────────┬─────────┬─────────┐
│ header  │ header  │ header  │
├─────────┼─────────┼─────────┤
│ nav     │ content │ aside   │
├─────────┼─────────┼─────────┤
│ footer  │ footer  │ footer  │
└─────────┴─────────┴─────────┘
```

Aqui estamos descrevendo regiões do Grid através de nomes.

---

# 19. Associando um item à área

Agora podemos fazer:

```css
.header {
  grid-area: header;
}
```

```css
.nav {
  grid-area: nav;
}
```

```css
.content {
  grid-area: content;
}
```

```css
.aside {
  grid-area: aside;
}
```

```css
.footer {
  grid-area: footer;
}
```

Assim cada item é associado à sua área nomeada.

A aula utiliza exatamente essa abordagem como a principal vantagem prática de `grid-area`.

---

# 20. Por que isso é tão interessante?

Compare:

```css
.header {
  grid-area: 1 / 1 / 2 / 4;
}
```

com:

```css
.header {
  grid-area: header;
}
```

A segunda forma é muito mais fácil de ler.

A primeira exige que você decore:

```text
1 = row start
1 = column start
2 = row end
4 = column end
```

A segunda simplesmente diz:

```text
header
```

O código passa a comunicar a intenção do layout.

---

# 21. `grid-template-areas` funciona como um desenho

Essa propriedade costuma ser comparada a uma espécie de desenho em ASCII.

Por exemplo:

```css
grid-template-areas:
  "header header header"
  "nav    content aside"
  "footer footer footer";
```

Podemos enxergar praticamente o layout dentro do próprio CSS:

```text
"header header header"
"nav    content aside"
"footer footer footer"
```

Visualmente:

```text
┌───────────────────────┐
│        HEADER         │
├───────┬───────┬───────┤
│  NAV  │CONTENT│ ASIDE │
├───────┴───────┴───────┤
│        FOOTER         │
└───────────────────────┘
```

É por isso que `grid-template-areas` pode ser muito agradável para layouts de interface.

A documentação do MDN também destaca essa característica visual da sintaxe.

---

# 22. O nome da área não cria um item

Um detalhe importante:

```css
grid-template-areas:
  "header header header";
```

não cria automaticamente um `<header>`.

Ele cria uma **área nomeada dentro do Grid**.

Depois você precisa associar um grid item a ela:

```css
.header {
  grid-area: header;
}
```

Portanto:

```text
grid-template-areas
        ↓
cria a estrutura nomeada

grid-area: header
        ↓
coloca o item naquela área
```

---

# 23. As áreas precisam formar retângulos

Uma grid area é formada por uma ou mais grid cells e deve possuir uma forma retangular.

Uma área como:

```css
grid-template-areas:
  "a a"
  "a a";
```

é válida.

Visualmente:

```text
┌───────┬───────┐
│       │       │
│   A   │   A   │
│       │       │
├───────┼───────┤
│       │       │
│   A   │   A   │
│       │       │
└───────┴───────┘
```

Mas uma forma irregular como:

```css
grid-template-areas:
  "a a"
  "a ."
  "a a";
```

não representa uma área retangular contínua para `a`.

O conceito de `grid area` como região retangular é parte do modelo do Grid.

---

# 24. As linhas implícitas criadas pelas áreas

Quando definimos:

```css
grid-template-areas:
  "header header header"
  "content content content"
  "footer footer footer";
```

as áreas possuem bordas.

O Grid cria nomes de linhas relacionados a essas bordas, como:

```text
header-start
header-end

content-start
content-end

footer-start
footer-end
```

Visualmente:

```text
header-start
      ↓
┌──────────────────────┐
│        HEADER        │
└──────────────────────┘
      ↑
header-end
```

O mesmo ocorre com:

```text
content
footer
```

A documentação do MDN descreve explicitamente essa relação entre áreas nomeadas e linhas nomeadas.

---

# 25. Por que isso permite `grid-area: header`?

Quando fazemos:

```css
.header {
  grid-area: header;
}
```

o nome:

```text
header
```

pode ser resolvido através das linhas:

```text
header-start
header-end
```

A área inteira passa a ser utilizada.

Podemos pensar:

```text
grid-area: header
       ↓
header-start ... header-end
```

É exatamente isso que torna o uso de áreas nomeadas tão conveniente.

---

# 26. `grid-area: header` versus `grid-area: 1 / 1 / 2 / 4`

Imagine:

```css
grid-template-areas:
  "header header header"
  "content content content";
```

Então:

```css
.header {
  grid-area: header;
}
```

representa a mesma região estrutural que poderia ser obtida por:

```css
.header {
  grid-area: 1 / 1 / 2 / 4;
}
```

Considerando esse Grid específico.

Porém, o primeiro código representa a **intenção semântica**:

```text
header
```

enquanto o segundo representa:

```text
linhas numéricas
```

---

# 27. Uma vantagem importante

Imagine que você altere:

```css
grid-template-areas:
  "header header header"
  "nav content aside"
  "footer footer footer";
```

para:

```css
grid-template-areas:
  "header header"
  "nav    content"
  "aside  footer";
```

Os itens que utilizam:

```css
grid-area: header;
grid-area: nav;
grid-area: content;
grid-area: aside;
grid-area: footer;
```

podem acompanhar a nova estrutura sem que você precise recalcular manualmente os números de linha.

Isso é uma das grandes vantagens do modelo baseado em áreas nomeadas.

---

# 28. Exemplo completo

HTML:

```html
<div class="layout">
  <header class="header">Header</header>
  <nav class="nav">Nav</nav>
  <main class="content">Content</main>
  <aside class="aside">Aside</aside>
  <footer class="footer">Footer</footer>
</div>
```

CSS:

```css
.layout {
  display: grid;

  grid-template-columns:
    200px
    1fr
    200px;

  grid-template-rows:
    auto
    1fr
    auto;

  grid-template-areas:
    "header header header"
    "nav content aside"
    "footer footer footer";
}

.header {
  grid-area: header;
}

.nav {
  grid-area: nav;
}

.content {
  grid-area: content;
}

.aside {
  grid-area: aside;
}

.footer {
  grid-area: footer;
}
```

Visualmente:

```text
┌────────────┬───────────────┬────────────┐
│                  HEADER                 │
├────────────┼───────────────┼────────────┤
│    NAV     │    CONTENT    │    ASIDE   │
├────────────┴───────────────┴────────────┤
│                  FOOTER                 │
└─────────────────────────────────────────┘
```

Esse é um dos usos mais claros de `grid-area`.

---

# 29. Mudando a posição apenas pelo nome

Imagine:

```css
.item-1 {
  grid-area: header;
}
```

Depois:

```css
.item-1 {
  grid-area: sidebar;
}
```

Se:

```css
grid-template-areas:
  "header header"
  "sidebar content";
```

o mesmo item muda de posição.

Você não precisou calcular:

```text
row-start
column-start
row-end
column-end
```

Você simplesmente mudou:

```text
header
```

para:

```text
sidebar
```

---

# 30. Sobreposição de Grid Items

A aula apresenta um comportamento muito interessante:

```text
dois itens podem ocupar a mesma região do Grid
```

O Grid não impede automaticamente essa situação.

Por exemplo:

```css
.item-1 {
  grid-area: header;
}

.item-2 {
  grid-area: header;
}
```

Os dois itens podem ocupar a mesma área.

Visualmente:

```text
┌─────────────────────┐
│       ITEM 1        │
│       ITEM 2        │
│       SOBREPOSTOS   │
└─────────────────────┘
```

A especificação permite que grid items ocupem áreas que se sobrepõem.

Isso pode ser intencional.

---

# 31. O Grid não "desvia" automaticamente

Quando você força dois itens para o mesmo espaço:

```css
.item-1 {
  grid-area: a;
}

.item-2 {
  grid-area: a;
}
```

o Grid não precisa criar uma nova célula para separar os dois.

Eles podem simplesmente ser desenhados um sobre o outro.

Esse comportamento é diferente da expectativa de quem imagina que o Grid sempre tentará empurrar um item para outro espaço.

---

# 32. Quando a sobreposição pode ser útil?

A sobreposição pode ser usada para criar:

```text
camadas
sobreposições
banners
efeitos visuais
cards sobre imagens
elementos decorativos
interfaces complexas
```

Exemplo:

```text
┌────────────────────────────┐
│         IMAGEM             │
│                            │
│     ┌────────────────┐     │
│     │     TEXTO      │     │
│     └────────────────┘     │
│                            │
└────────────────────────────┘
```

A imagem e o texto podem compartilhar a mesma área do Grid.

---

# 33. Quem fica por cima?

Quando dois elementos se sobrepõem, entra em cena a ordem de empilhamento (**stacking order**).

Uma das propriedades mais importantes para controlar isso é:

```css
z-index
```

A documentação do MDN define `z-index` como a propriedade que controla a ordem no eixo Z; em elementos sobrepostos, um valor maior pode colocá-los acima de um com valor menor. `z-index` também se aplica a grid items.

---

# 34. O eixo Z

Até agora pensamos em:

```text
X
Y
```

ou:

```text
column
row
```

Mas quando existe sobreposição aparece uma terceira dimensão visual:

```text
Z
```

Podemos imaginar:

```text
          Z
          ↑
          │
          │     ITEM 3
          │
          │   ITEM 2
          │
          │ ITEM 1
          └────────────────
```

O eixo Z representa a profundidade visual.

---

# 35. `z-index`

Exemplo:

```css
.item-1 {
  z-index: 1;
}

.item-2 {
  z-index: 5;
}
```

Se os dois estiverem sobrepostos:

```text
item 2
   ↓
fica acima

item 1
   ↓
fica abaixo
```

Porque:

```text
5 > 1
```

---

# 36. Exemplo com Grid

```css
.item-1 {
  grid-area: header;
  z-index: 1;
}

.item-2 {
  grid-area: header;
  z-index: 2;
}
```

Visualmente:

```text
┌─────────────────────────┐
│                         │
│        ITEM 2           │  ← z-index 2
│        ITEM 1           │  ← z-index 1
│                         │
└─────────────────────────┘
```

O `item-2` fica na frente.

---

# 37. Valores maiores não significam "mil vezes mais perto"

Se temos:

```css
z-index: 5;
```

e:

```css
z-index: 1000000;
```

o segundo tem um nível de empilhamento maior dentro do contexto em questão.

Mas não pense em `z-index` como uma distância física.

Ele representa uma **ordem de empilhamento**.

Ou seja:

```text
menor
↓
maior
```

---

# 38. O importante é a relação entre os valores

Por exemplo:

```css
.item-1 {
  z-index: 2;
}

.item-2 {
  z-index: 5;
}
```

Temos:

```text
5 > 2
```

Portanto:

```text
item-2
   ↑
acima

item-1
   ↑
abaixo
```

---

# 39. `z-index` e stacking context

Existe um conceito mais avançado chamado:

```text
stacking context
```

Um **stacking context** é um contexto independente de empilhamento.

Isso significa que, em layouts mais complexos, não basta comparar:

```text
z-index: 100
```

com:

```text
z-index: 10;
```

se os elementos estiverem em contextos de empilhamento diferentes.

O modelo é:

```text
STACKING CONTEXT
│
├── elemento A
│
├── elemento B
│
└── elemento C
```

Cada contexto pode possuir sua própria ordem interna.

A documentação do MDN explica que stacking contexts são independentes e que seus descendentes são empilhados dentro do contexto correspondente.

Para os exemplos simples da aula, basta inicialmente pensar:

```text
z-index maior
↓
elemento fica acima
```

---

# 40. `grid-area` pode ser usada para sobreposição intencional

Uma aplicação interessante:

```css
.image {
  grid-area: 1 / 1 / 3 / 4;
}

.content {
  grid-area: 1 / 1 / 3 / 4;
}
```

Os dois elementos ocupam a mesma região.

Depois:

```css
.content {
  z-index: 2;
}
```

Resultado:

```text
┌────────────────────────────┐
│                            │
│          IMAGE             │
│       ┌─────────────┐      │
│       │   CONTENT   │      │
│       └─────────────┘      │
│                            │
└────────────────────────────┘
```

Isso transforma Grid em uma ferramenta muito útil para criar camadas.

---

# 41. Valores incompletos em `grid-area`

A sintaxe de `grid-area` permite de um a quatro valores:

```css
grid-area: 2;
```

```css
grid-area: 2 / 3;
```

```css
grid-area: 2 / 3 / 4;
```

```css
grid-area: 2 / 3 / 4 / 5;
```

Mas é importante entender que esses valores não são interpretados como uma simples sequência de "cima, direita, baixo, esquerda".

Eles seguem:

```text
1 → row-start
2 → column-start
3 → row-end
4 → column-end
```

A sintaxe formal da propriedade permite até quatro `<grid-line>` separados por `/`.

---

# 42. A forma de quatro valores é a mais fácil para estudar

Enquanto você ainda está construindo o modelo mental, prefira:

```css
grid-area: 2 / 3 / 5 / 6;
```

e traduza imediatamente:

```text
2 = row-start
3 = column-start
5 = row-end
6 = column-end
```

Depois de entender isso, as formas abreviadas ficam muito mais fáceis.

---

# 43. `grid-area` usando nomes

Também podemos escrever:

```css
grid-area: content;
```

ou:

```css
grid-area: header;
```

ou:

```css
grid-area: footer;
```

Quando esses nomes correspondem às áreas definidas em:

```css
grid-template-areas
```

podemos posicionar os itens de forma extremamente legível.

---

# 44. Exemplo com `header`

```css
.grid {
  display: grid;

  grid-template-areas:
    "header header"
    "main   main";
}

.header {
  grid-area: header;
}

.main {
  grid-area: main;
}
```

Visualmente:

```text
┌───────────────┐
│    HEADER     │
├───────────────┤
│     MAIN      │
└───────────────┘
```

---

# 45. Exemplo com sidebar

```css
.grid {
  display: grid;

  grid-template-areas:
    "header header"
    "sidebar content"
    "footer footer";
}

.header {
  grid-area: header;
}

.sidebar {
  grid-area: sidebar;
}

.content {
  grid-area: content;
}

.footer {
  grid-area: footer;
}
```

Visualmente:

```text
┌─────────────────────────┐
│          HEADER         │
├────────────┬────────────┤
│  SIDEBAR   │  CONTENT   │
├────────────┴────────────┤
│          FOOTER         │
└─────────────────────────┘
```

---

# 46. `grid-area` versus `grid-row` + `grid-column`

Podemos construir o mesmo layout de duas formas.

## Usando linhas

```css
.header {
  grid-row: 1;
  grid-column: 1 / -1;
}

.content {
  grid-row: 2;
  grid-column: 2;
}
```

## Usando áreas

```css
.header {
  grid-area: header;
}

.content {
  grid-area: content;
}
```

E no container:

```css
grid-template-areas:
  "header header"
  "sidebar content";
```

A segunda abordagem é mais orientada à estrutura semântica do layout.

---

# 47. Quando usar números

Números são especialmente úteis quando estamos pensando diretamente em:

```text
grid lines
span
áreas dinâmicas
posicionamento preciso
sobreposição
estruturas que não precisam de nomes
```

Exemplo:

```css
grid-area: 2 / 2 / 5 / 4;
```

É muito explícito geometricamente.

---

# 48. Quando usar nomes

Nomes são especialmente interessantes quando o layout possui regiões semanticamente conhecidas:

```text
header
sidebar
content
aside
footer
nav
```

Exemplo:

```css
grid-area: content;
```

A leitura é imediata.

---

# 49. Uma comparação importante

```css
grid-area: 1 / 1 / 2 / 4;
```

comunica:

```text
comece na linha 1
comece na coluna 1
termine na linha 2
termine na coluna 4
```

Já:

```css
grid-area: header;
```

comunica:

```text
este item pertence à área header
```

A segunda forma é mais semântica.

---

# 50. Exemplo da aula: mudando rapidamente as áreas

Podemos imaginar:

```css
.item-1 {
  grid-area: header;
}
```

Depois:

```css
.item-1 {
  grid-area: side-nav;
}
```

Depois:

```css
.item-1 {
  grid-area: content;
}
```

Depois:

```css
.item-1 {
  grid-area: footer;
}
```

O mesmo item pode mudar de posição apenas trocando o nome da área.

A aula destaca justamente essa possibilidade de reorganizar rapidamente o layout através dos nomes.

---

# 51. Por que isso facilita responsividade?

Uma vantagem muito interessante de `grid-template-areas` é poder alterar o desenho do layout em uma media query.

Exemplo:

```css
.layout {
  display: grid;

  grid-template-areas:
    "header header"
    "sidebar content"
    "footer footer";
}
```

Em uma tela menor:

```css
@media (max-width: 700px) {
  .layout {
    grid-template-areas:
      "header"
      "content"
      "sidebar"
      "footer";
  }
}
```

Os elementos continuam utilizando:

```css
grid-area: header;
grid-area: content;
grid-area: sidebar;
grid-area: footer;
```

Mas a estrutura muda.

Isso torna a arquitetura do layout muito flexível.

---

# 52. `grid-area` e `auto-fit`

A aula também apresenta um exemplo onde o Grid utiliza:

```css
grid-template-columns:
  repeat(auto-fit, minmax(80px, 1fr));
```

Aqui existem dois conceitos diferentes:

```text
auto-fit
+
minmax()
```

Isso não faz parte diretamente da propriedade `grid-area`, mas aparece na aula porque o exemplo cria uma quantidade dinâmica de colunas antes do posicionamento dos items.

---

# 53. O que `auto-fit` faz?

A função:

```css
repeat(auto-fit, ...)
```

permite que o Grid ajuste a quantidade de tracks que conseguem ser acomodadas dentro do espaço disponível.

Exemplo:

```css
.grid {
  grid-template-columns:
    repeat(
      auto-fit,
      minmax(80px, 1fr)
    );
}
```

A ideia é:

```text
"crie quantas colunas couberem,
respeitando os limites definidos"
```

---

# 54. O papel de `minmax()`

A função:

```css
minmax(80px, 1fr)
```

define:

```text
mínimo = 80px
máximo = 1fr
```

Ou seja:

```text
não deixe a track ficar menor que 80px
+
permita que ela cresça e distribua o espaço disponível
```

Então:

```css
repeat(auto-fit, minmax(80px, 1fr))
```

pode ser lido como:

```text
crie quantas colunas couberem,
cada uma com no mínimo 80px,
e permita que cresçam até ocupar o espaço disponível.
```

---

# 55. Relação entre `auto-fit` e `grid-area`

Aqui está o ponto importante:

```text
auto-fit
↓
muda a quantidade/estrutura das colunas

grid-area
↓
posiciona o item dentro dessa estrutura
```

São responsabilidades diferentes.

Não confunda:

```text
auto-fit
```

com:

```text
grid-area
```

---

# 56. Posicionamento fora do grid definido

A aula demonstra que, quando você força um item a uma posição que exige linhas adicionais, o Grid pode criar tracks implícitas.

Por exemplo, se a estrutura existente não possui uma determinada posição e o item é colocado além dela:

```css
.item {
  grid-column: 5;
}
```

o Grid pode precisar criar linhas/tracks implícitas para acomodar essa posição.

O conceito geral de Grid implícito é que tracks adicionais podem ser criadas quando o posicionamento ou a quantidade de conteúdo ultrapassa o grid explicitamente definido.

---

# 57. `grid-area` e Grid implícito

O mesmo princípio vale quando utilizamos:

```css
grid-area
```

Se uma posição exigir espaço adicional, o Grid pode criar tracks implícitas para conseguir realizar o posicionamento.

Por isso é importante entender:

```text
grid-area
↓
solicita uma posição

Grid
↓
precisa encontrar ou criar as tracks necessárias
```

---

# 58. Exemplo com posicionamento além da estrutura

Imagine:

```css
.grid {
  display: grid;

  grid-template-columns:
    repeat(3, 100px);
}
```

Existem três column tracks explícitas.

Agora:

```css
.item {
  grid-column: 5;
}
```

A posição solicitada está além do conjunto inicial definido.

O Grid pode criar estrutura implícita para acomodar a colocação.

---

# 59. Sobreposição em layouts dinâmicos

Quando o layout utiliza:

```css
auto-fit
minmax()
grid-area
```

podemos criar combinações interessantes.

Por exemplo:

```css
.image {
  grid-area: 1 / 1 / -1 / -1;
}

.overlay {
  grid-area: 1 / 1 / -1 / -1;
  z-index: 2;
}
```

Resultado conceitual:

```text
┌─────────────────────────────┐
│                             │
│           IMAGE             │
│                             │
│       ┌──────────────┐      │
│       │   OVERLAY    │      │
│       └──────────────┘      │
│                             │
└─────────────────────────────┘
```

---

# 60. Um detalhe sobre `z-index`

A aula mostra algo importante:

```css
z-index: 5;
```

fazendo um item aparecer sobre os demais quando eles se sobrepõem.

Isso acontece porque `z-index` controla a ordem de empilhamento.

No caso de grid items, a propriedade pode ser utilizada para controlar sua ordem de sobreposição.

Mentalmente:

```text
grid-area
↓
define ONDE

z-index
↓
define QUEM FICA ACIMA
```

---

# 61. `grid-area` + `z-index`

Essa combinação merece ser memorizada:

```text
grid-area
   ↓
posição e tamanho

z-index
   ↓
profundidade visual
```

Exemplo:

```css
.item-1 {
  grid-area: 1 / 1 / 4 / 4;
  z-index: 1;
}

.item-2 {
  grid-area: 2 / 2 / 4 / 4;
  z-index: 2;
}
```

O resultado é:

```text
ITEM 2
  ↑
  │  z-index maior
  │
ITEM 1
```

---

# 62. Um segundo assunto apresentado na aula: `mix-blend-mode`

No final da aula aparece um exemplo que não pertence diretamente ao funcionamento de `grid-area`.

Trata-se da propriedade:

```css
mix-blend-mode
```

Ela define como o conteúdo de um elemento deve ser mesclado com o conteúdo que está atrás dele dentro do contexto de empilhamento.

Portanto:

```text
grid-area
↓
layout

mix-blend-mode
↓
composição visual de cores
```

São assuntos diferentes, mas vale registrar porque aparecem na aula.

---

# 63. `mix-blend-mode: screen`

Um dos valores mostrados é:

```css
mix-blend-mode: screen;
```

O modo `screen` realiza uma operação de mesclagem entre as cores do elemento e do fundo.

A ideia simplificada é:

```text
cor clara + cor clara
↓
resultado ainda mais claro
```

O comportamento matemático do modo `screen`, trabalhando com valores normalizados, é:

```text
result = 1 - (1 - foreground) × (1 - background)
```

A documentação do MDN descreve o efeito como o inverso da multiplicação dos inversos das cores.

---

# 64. RGB

A aula aproveita o exemplo para revisar:

```text
RGB
```

RGB significa:

```text
R = Red
G = Green
B = Blue
```

No modelo RGB tradicional usado em CSS:

```text
cada canal pode variar de 0 até 255
```

Por exemplo:

```css
rgb(255, 0, 0);
```

representa:

```text
R = 255
G = 0
B = 0
```

Resultado:

```text
vermelho
```

---

# 65. Exemplos básicos de RGB

```css
rgb(255, 0, 0);
```

```text
vermelho
```

---

```css
rgb(0, 255, 0);
```

```text
verde
```

---

```css
rgb(0, 0, 255);
```

```text
azul
```

---

```css
rgb(255, 255, 255);
```

```text
branco
```

---

```css
rgb(0, 0, 0);
```

```text
preto
```

Mentalmente:

```text
           RGB

R ─────────────── 0 → 255
G ─────────────── 0 → 255
B ─────────────── 0 → 255
```

---

# 66. Combinação de canais

A cor final é produzida pela combinação dos três canais.

Por exemplo:

```css
rgb(255, 255, 0);
```

tem:

```text
R = 255
G = 255
B = 0
```

Resultado:

```text
amarelo
```

Outro exemplo:

```css
rgb(255, 0, 255);
```

produz:

```text
magenta
```

E:

```css
rgb(0, 255, 255);
```

produz:

```text
ciano
```

---

# 67. Relação entre RGB e `screen`

Com `screen`, podemos imaginar a combinação simplificada:

```text
vermelho
+
azul
=
magenta
```

```text
verde
+
vermelho
=
amarelo
```

```text
verde
+
azul
=
ciano
```

E:

```text
vermelho
+
verde
+
azul
=
branco
```

Isso ajuda a visualizar o funcionamento do modo de mistura mostrado na aula.

A operação real de `screen` é feita por canal e faz parte do modelo de composição e blending.

---

# 68. Importante: `screen` não é simplesmente "somar RGB"

Para fins didáticos, podemos dizer:

```text
screen
↓
produz um efeito de soma de luz
```

Mas tecnicamente não devemos pensar que:

```text
screen = R + R
```

ou simplesmente:

```text
screen = soma direta dos valores RGB
```

O algoritmo real é uma operação matemática de mistura:

```text
1 - (1 - A)(1 - B)
```

por canal, dentro do modelo de composição.

Por isso, a explicação "somar as cores" é uma boa analogia visual, mas não é a definição matemática exata.

---

# 69. Relação entre `grid-area`, `z-index` e `mix-blend-mode`

Os três conceitos da aula atuam em camadas diferentes:

```text
┌─────────────────────────────────┐
│            GRID                 │
│                                 │
│  grid-area                      │
│      ↓                          │
│  posiciona o item               │
│                                 │
│  z-index                        │
│      ↓                          │
│  controla a ordem de camadas    │
│                                 │
│  mix-blend-mode                 │
│      ↓                          │
│  mistura as cores               │
│                                 │
└─────────────────────────────────┘
```

Portanto:

```text
grid-area
= onde o item está

z-index
= quem fica acima

mix-blend-mode
= como as cores se misturam
```

---

# 70. Exemplo integrando os três

```html
<div class="grid">
  <div class="background"></div>
  <div class="overlay"></div>
</div>
```

```css
.grid {
  display: grid;

  grid-template-columns:
    repeat(3, 1fr);

  grid-template-rows:
    repeat(3, 150px);
}

.background {
  grid-area: 1 / 1 / 4 / 4;
  background: red;
}

.overlay {
  grid-area: 2 / 2 / 4 / 4;

  background: blue;

  z-index: 2;

  mix-blend-mode: screen;
}
```

Mentalmente:

```text
grid-area
↓
determina as regiões

z-index
↓
coloca o azul sobre o vermelho

mix-blend-mode
↓
faz a mistura de cores
```

---

# 71. Outro detalhe importante da aula: itens parcialmente transparentes

A aula utiliza:

```css
opacity: 0.2;
```

para tornar os itens parcialmente transparentes e revelar quando estão sobrepostos.

Isso é uma técnica excelente para **visualização e depuração** de layouts.

Exemplo:

```css
.item {
  opacity: 0.2;
}
```

Visualmente:

```text
ITEM 1
   ↓
┌───────────────┐
│   1           │
│      2        │
│           3   │
└───────────────┘
```

Quando as caixas ficam translúcidas, áreas sobrepostas tornam-se fáceis de identificar.

---

# 72. `opacity` não muda a posição

Um ponto importante:

```css
opacity: 0.2;
```

não altera:

```text
grid-row
grid-column
grid-area
```

Ela altera apenas a aparência/transparência do elemento.

Portanto:

```text
grid-area
↓
layout

opacity
↓
visualização
```

---

# 73. Modelo mental completo de `grid-area`

Podemos resumir a propriedade em três grandes possibilidades:

```text
                grid-area
                     │
         ┌───────────┴────────────┐
         │                        │
         ▼                       ▼
   LINHAS NUMÉRICAS          ÁREA NOMEADA
         │                        │
         │                        │
         ▼                       ▼
 row-start                   header
 column-start                content
 row-end                     footer
 column-end                  sidebar
         │                        │
         └──────────┬─────────────┘
                    ▼
                GRID ITEM
```

---

# 74. Modelo mental da sintaxe numérica

```css
grid-area: A / B / C / D;
```

Sempre transforme mentalmente em:

```text
A = row-start
B = column-start
C = row-end
D = column-end
```

Ou:

```text
┌──────────────────────────┐
│ A → onde começa a row    │
│ B → onde começa a coluna │
│ C → onde termina a row   │
│ D → onde termina coluna  │
└──────────────────────────┘
```

---

# 75. Modelo mental da sintaxe com nome

Quando encontramos:

```css
grid-area: header;
```

pense:

```text
"este item pertence à área chamada header"
```

Se a área existir em:

```css
grid-template-areas:
  "header header"
  "content content";
```

o Grid possui linhas associadas a essa área:

```text
header-start
header-end
```

e consegue resolver a área correspondente.

---

# 76. Modelo mental definitivo

```text
grid-area
│
├── FORMA NUMÉRICA
│   │
│   └── row-start
│       column-start
│       row-end
│       column-end
│
└── FORMA NOMEADA
    │
    └── grid-template-areas
            │
            ├── header
            ├── content
            ├── sidebar
            └── footer
```

---

# 77. Uma forma simples de estudar

Quando encontrar:

```css
grid-area: 2 / 3 / 5 / 6;
```

faça quatro perguntas:

```text
1. Onde começa a row?
   → 2

2. Onde começa a coluna?
   → 3

3. Onde termina a row?
   → 5

4. Onde termina a coluna?
   → 6
```

Depois:

```text
rows:
5 - 2 = 3

columns:
6 - 3 = 3
```

Resultado:

```text
3 rows × 3 columns
```

---

# 78. Quando encontrar `span`

Exemplo:

```css
grid-area: 2 / 3 / span 4 / span 2;
```

pergunte:

```text
1. Row começa em?
   → 2

2. Column começa em?
   → 3

3. Quantas rows?
   → 4

4. Quantas columns?
   → 2
```

Resultado:

```text
4 rows × 2 columns
```

---

# 79. Quando encontrar um nome

Exemplo:

```css
grid-area: content;
```

pergunte:

```text
Existe uma área chamada "content"?
```

Procure no container:

```css
grid-template-areas:
  "header header"
  "content content"
  "footer footer";
```

Se existir:

```text
content
↓
área nomeada
```

O item será associado àquela área.

---

# 80. Tabela de fixação

| Sintaxe                              | Significado                                     |
| ------------------------------------ | ----------------------------------------------- |
| `grid-area: auto`                    | posicionamento automático                       |
| `grid-area: 2`                       | primeira contribuição de posicionamento         |
| `grid-area: 2 / 3`                   | row-start = 2, column-start = 3                 |
| `grid-area: 2 / 3 / 4`               | row-start = 2, column-start = 3, row-end = 4    |
| `grid-area: 2 / 3 / 4 / 5`           | define as quatro linhas                         |
| `grid-area: 2 / 3 / span 2 / span 3` | começa em 2/3 e ocupa 2 rows × 3 columns        |
| `grid-area: header`                  | utiliza uma área nomeada chamada `header`       |
| `grid-row`                           | posiciona no eixo das rows                      |
| `grid-column`                        | posiciona no eixo das colunas                   |
| `grid-template-areas`                | define áreas nomeadas                           |
| `z-index`                            | controla a ordem de empilhamento                |
| `opacity`                            | altera a transparência visual                   |
| `mix-blend-mode`                     | controla como o conteúdo se mistura com o fundo |

---

# 81. Comparação entre os principais shorthands

| Propriedade   | Controla                         | Exemplo                    |
| ------------- | -------------------------------- | -------------------------- |
| `grid-row`    | início e fim no eixo das rows    | `grid-row: 2 / 4`          |
| `grid-column` | início e fim no eixo das colunas | `grid-column: 2 / 4`       |
| `grid-area`   | rows + columns                   | `grid-area: 2 / 2 / 4 / 4` |

Visualmente:

```text
grid-row
    ↓
┌─────────────┐
│             │
│             │
└─────────────┘

grid-column
    ↓
┌──────┐
│      │
│      │
│      │
└──────┘

grid-area
    ↓
┌────────────────┐
│                │
│      ITEM      │
│                │
└────────────────┘
```

---

# 82. `grid-area` é mais do que um "atalho"

É correto dizer:

```text
grid-area = shorthand
```

Mas não pare aí.

Ela possui duas funções muito importantes:

```text
1. posicionar um item com quatro grid lines

2. associar um item a uma área nomeada
```

Portanto:

```text
grid-area
├── line-based placement
└── named area placement
```

Essa é uma das principais razões para a propriedade ser tão importante dentro do CSS Grid.

---

# 83. Comparação final

## Posicionamento por linhas

```css
.item {
  grid-area: 2 / 2 / 4 / 4;
}
```

Significa:

```text
row-start = 2
column-start = 2
row-end = 4
column-end = 4
```

---

## Posicionamento por área

```css
.item {
  grid-area: content;
}
```

Significa:

```text
este item usa a área "content"
```

quando essa área foi definida no sistema de Grid.

---

# 84. O que realmente está acontecendo?

Quando escrevemos:

```css
grid-area: content;
```

não estamos magicamente dizendo:

```text
"o navegador sabe o que é content"
```

Estamos utilizando o sistema de nomes do Grid.

O caminho mental é:

```text
grid-template-areas
        ↓
cria áreas nomeadas
        ↓
gera linhas nomeadas relacionadas às bordas
        ↓
grid-area: content
        ↓
item é colocado naquela área
```

Esse encadeamento é muito importante para compreender por que `grid-area` funciona tão bem com `grid-template-areas`.

---

# 85. Layout completo usando áreas

```html
<div class="layout">

  <header class="header">
    Header
  </header>

  <nav class="sidebar">
    Sidebar
  </nav>

  <main class="content">
    Content
  </main>

  <aside class="aside">
    Aside
  </aside>

  <footer class="footer">
    Footer
  </footer>

</div>
```

```css
.layout {
  display: grid;

  grid-template-columns:
    200px
    1fr
    200px;

  grid-template-rows:
    auto
    1fr
    auto;

  grid-template-areas:
    "header  header  header"
    "sidebar content aside"
    "footer  footer  footer";
}

.header {
  grid-area: header;
}

.sidebar {
  grid-area: sidebar;
}

.content {
  grid-area: content;
}

.aside {
  grid-area: aside;
}

.footer {
  grid-area: footer;
}
```

Resultado:

```text
┌─────────────────────────────────────┐
│               HEADER                │
├────────────┬──────────────┬─────────┤
│            │              │         │
│  SIDEBAR   │   CONTENT    │  ASIDE  │
│            │              │         │
├────────────┴──────────────┴─────────┤
│               FOOTER                │
└─────────────────────────────────────┘
```

Essa estrutura demonstra a principal força do sistema:

```text
estrutura
↓
nome
↓
item
```

---

# 86. Layout responsivo usando `grid-area`

Podemos alterar apenas a estrutura:

```css
.layout {
  grid-template-areas:
    "header header header"
    "sidebar content aside"
    "footer footer footer";
}
```

Para:

```css
@media (max-width: 700px) {
  .layout {
    grid-template-columns: 1fr;

    grid-template-areas:
      "header"
      "content"
      "sidebar"
      "aside"
      "footer";
  }
}
```

E os componentes continuam:

```css
.header {
  grid-area: header;
}

.content {
  grid-area: content;
}

.sidebar {
  grid-area: sidebar;
}

.aside {
  grid-area: aside;
}

.footer {
  grid-area: footer;
}
```

Isso demonstra por que áreas nomeadas são muito poderosas para layouts responsivos.

---

# 87. Checklist mental

Antes de escrever `grid-area`, pergunte:

```text
□ Estou usando números ou um nome?

□ Se estou usando números:
  qual é o row-start?

□ Qual é o column-start?

□ Qual é o row-end?

□ Qual é o column-end?

□ Estou usando span?

□ Quantas rows o item vai ocupar?

□ Quantas columns o item vai ocupar?

□ Se estou usando um nome:
  essa área existe?

□ Existe grid-template-areas?

□ O item está sendo colocado sobre outro?

□ Preciso controlar a ordem visual com z-index?
```

---

# 88. Erros conceituais que devemos evitar

## Erro 1 — confundir a ordem dos valores

Errado pensar:

```text
grid-area: top / right / bottom / left
```

A ordem é:

```text
row-start
column-start
row-end
column-end
```

---

## Erro 2 — pensar que dois valores são "início e fim"

Em:

```css
grid-area: 2 / 3;
```

não leia:

```text
2 até 3
```

Leia:

```text
row-start = 2
column-start = 3
```

---

## Erro 3 — pensar que `grid-area: header` cria o header

Não.

A área precisa estar definida no Grid:

```css
grid-template-areas:
  "header";
```

ou ser resolvida por linhas nomeadas compatíveis.

---

## Erro 4 — pensar que dois itens não podem ocupar a mesma área

Podem.

Exemplo:

```css
.item-1 {
  grid-area: header;
}

.item-2 {
  grid-area: header;
}
```

Eles podem se sobrepor.

---

## Erro 5 — achar que o Grid sempre separará itens sobrepostos

Não necessariamente.

Se você definiu que dois itens ocupam a mesma região:

```text
o Grid pode simplesmente sobrepô-los.
```

---

## Erro 6 — pensar que `z-index` pertence ao Grid

Não.

`z-index` é uma propriedade geral de empilhamento que também pode ser aplicada a grid items.

```text
Grid
↓
fornece o contexto de layout

z-index
↓
controla empilhamento
```

---

# 89. Regra de ouro

```text
╔══════════════════════════════════════════════╗
║                                              ║
║              GRID-AREA                       ║
║                                              ║
║  1 → ROW START                               ║
║  2 → COLUMN START                            ║
║  3 → ROW END                                 ║
║  4 → COLUMN END                              ║
║                                              ║
║  OU                                          ║
║                                              ║
║  grid-area: nome-da-área                     ║
║                                              ║
╚══════════════════════════════════════════════╝
```

---

# 90. Modelo mental final

Quando você encontrar:

```css
grid-area: 2 / 3 / 5 / 6;
```

pense:

```text
              ROW START
                  ↓
             2 ────────────
                  │
                  │
COLUMN START      │      COLUMN END
      ↓           │           ↓
      3 ──────────┼─────────── 6
                  │
                  │
             5 ────────────
                  ↑
               ROW END
```

E quando encontrar:

```css
grid-area: content;
```

pense:

```text
grid-template-areas
        ↓
     "content"
        ↓
área chamada content
        ↓
grid-area: content
        ↓
item ocupa essa área
```

---

# 91. Resumo definitivo

```text
GRID-AREA
│
├── SHORTHAND
│   │
│   ├── grid-row-start
│   ├── grid-column-start
│   ├── grid-row-end
│   └── grid-column-end
│
├── SINTAXE
│   │
│   └── row-start /
│       column-start /
│       row-end /
│       column-end
│
├── SPAN
│   │
│   └── quantidade de tracks
│
├── NOMES
│   │
│   └── áreas nomeadas
│
├── GRID-TEMPLATE-AREAS
│   │
│   ├── header
│   ├── nav
│   ├── content
│   └── footer
│
├── SOBREPOSIÇÃO
│   │
│   └── múltiplos itens podem ocupar a mesma área
│
└── Z-INDEX
    │
    └── controla a ordem visual das camadas
```

---

# 92. O que você deve conseguir explicar sem consultar a documentação

Você deve conseguir olhar para:

```css
grid-area: 2 / 3 / 5 / 7;
```

e imediatamente responder:

```text
row-start    = 2
column-start = 3
row-end      = 5
column-end   = 7
```

e:

```text
rows:
5 - 2 = 3

columns:
7 - 3 = 4
```

Logo:

```text
3 rows × 4 columns
```

Também deve conseguir olhar para:

```css
grid-area: content;
```

e explicar:

```text
"content" é uma área nomeada do Grid,
normalmente definida por grid-template-areas
ou relacionada a linhas nomeadas.
```

E, finalmente:

```css
grid-area: 1 / 1 / -1 / -1;
```

deve ser lido como:

```text
primeira row line
+
primeira column line
+
última row line do grid explícito
+
última column line do grid explícito
```

---

# 93. Referências técnicas

## MDN Web Docs

* `grid-area`
* `grid-row`
* `grid-column`
* `grid-template-areas`
* Named grid lines
* Grid layout — line-based placement
* Grid layout — template areas
* `z-index`
* `mix-blend-mode`
* `<blend-mode>`

## Especificações

* CSS Grid Layout Module
* CSS Grid Layout Module Level 2
* CSS Compositing and Blending Module Level 2

As referências oficiais são especialmente importantes neste assunto porque `grid-area` possui uma sintaxe que pode parecer intuitiva à primeira vista, mas possui uma ordem própria e regras específicas para `<custom-ident>`, `span`, linhas nomeadas e áreas nomeadas.

---

# 94. Referência rápida

```css
/* ---------------------------------
   FORMA MAIS COMPLETA
   --------------------------------- */

grid-area: 2 / 3 / 5 / 6;


/* ---------------------------------
   EQUIVALENTE
   --------------------------------- */

grid-row-start: 2;
grid-column-start: 3;
grid-row-end: 5;
grid-column-end: 6;


/* ---------------------------------
   COM SPAN
   --------------------------------- */

grid-area: 2 / 3 / span 2 / span 3;


/* ---------------------------------
   ÁREA NOMEADA
   --------------------------------- */

grid-area: content;


/* ---------------------------------
   EXEMPLO COM TEMPLATE AREAS
   --------------------------------- */

.container {
  display: grid;

  grid-template-areas:
    "header header"
    "content sidebar"
    "footer footer";
}

.header {
  grid-area: header;
}

.content {
  grid-area: content;
}

.sidebar {
  grid-area: sidebar;
}

.footer {
  grid-area: footer;
}


/* ---------------------------------
   SOBREPOSIÇÃO
   --------------------------------- */

.background {
  grid-area: 1 / 1 / -1 / -1;
}

.overlay {
  grid-area: 1 / 1 / -1 / -1;
  z-index: 2;
}


/* ---------------------------------
   MISTURA DE CORES
   --------------------------------- */

.overlay {
  mix-blend-mode: screen;
}
```

---

# 95. GitHub

<div align="center">

### CSS Grid Layout — `grid-area`

Documentação técnica para estudo contínuo de CSS Grid Layout.

<br>

<a href="https://github.com/gabrielfelipeoliveira55" target="_blank" rel="noopener noreferrer">
Gabriel Felipe de Oliveira Rateiro
</a>

<br><br>

**CSS Grid Layout • `grid-area`**

> Não decore a sequência.
>
> Entenda:
>
> **Row Start → Column Start → Row End → Column End**
>
> e entenda que `grid-area` também pode representar uma **área nomeada**.

</div>
```
