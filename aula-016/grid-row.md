````markdown
# CSS Grid Layout — `grid-row`

> **Objetivo:** compreender como `grid-row` posiciona e dimensiona um `grid item` no eixo das rows do CSS Grid, utilizando números, linhas nomeadas, `span`, valores negativos e a relação com `grid-row-start`, `grid-row-end`, `grid-template-rows`, `grid-auto-rows` e `grid-template-areas`.

---

# 1. Antes de entender `grid-row`

Para compreender `grid-row`, primeiro precisamos entender como um elemento participa de um Grid.

Quando um elemento recebe:

```css
.container {
  display: grid;
}
````

ele se torna um **grid container**.

Os seus **filhos diretos** tornam-se **grid items**.

Exemplo:

```html
<div class="grid">
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>
</div>
```

```css
.grid {
  display: grid;
}
```

Podemos representar:

```text
.grid
│
├── Item 1  ← grid item
├── Item 2  ← grid item
└── Item 3  ← grid item
```

Portanto:

```text
GRID CONTAINER
       ↓
FILHOS DIRETOS
       ↓
GRID ITEMS
```

É sobre esses `grid items` que propriedades como:

```css
grid-row
grid-row-start
grid-row-end
```

atuam.

---

# 2. CSS Grid possui dois eixos

O CSS Grid é um sistema de layout bidimensional.

Isso significa que ele trabalha com:

```text
colunas
+
rows
```

Visualmente:

```text
                 COLUNAS
           1        2        3
           │        │        │
      ─────┼────────┼────────┼─────
ROW 1      │        │        │
      ─────┼────────┼────────┼─────
ROW 2      │        │        │
      ─────┼────────┼────────┼─────
ROW 3      │        │        │
      ─────┼────────┼────────┼─────
```

Podemos pensar:

```text
grid-column
     ↓
posicionamento no eixo das colunas

grid-row
     ↓
posicionamento no eixo das rows
```

No modo de escrita mais comum, `horizontal-tb`, as rows são horizontais e o posicionamento entre elas ocorre visualmente de cima para baixo.

Por isso, durante o estudo inicial, é comum associar:

```text
grid-row → eixo vertical
```

Mas o conceito técnico mais preciso é:

```text
grid-row → eixo das rows
```

Isso evita problemas quando começarmos a estudar `writing-mode` e outros modos de escrita.

---

# 3. O que é uma `row`?

Uma **grid row** é uma **track horizontal** do Grid.

Por exemplo:

```text
┌──────────┬──────────┬──────────┐
│          │          │          │
│          │          │          │
│  ROW 1   │  ROW 1   │  ROW 1   │
│          │          │          │
├──────────┼──────────┼──────────┤
│          │          │          │
│  ROW 2   │  ROW 2   │  ROW 2   │
│          │          │          │
├──────────┼──────────┼──────────┤
│          │          │          │
│  ROW 3   │  ROW 3   │  ROW 3   │
│          │          │          │
└──────────┴──────────┴──────────┘
```

Aqui temos:

```text
3 row tracks
```

ou, de forma mais informal:

```text
3 rows
```

Tecnicamente, uma row é o espaço entre duas grid lines horizontais.

---

# 4. O que é uma `grid line`?

Uma **grid line** é uma linha estrutural do Grid que delimita as tracks.

Se temos três rows:

```text
grid line 1
      ↓
┌─────────────────────────────┐
│           ROW 1             │
└─────────────────────────────┘
      ↑
grid line 2

┌─────────────────────────────┐
│           ROW 2             │
└─────────────────────────────┘
      ↑
grid line 3

┌─────────────────────────────┐
│           ROW 3             │
└─────────────────────────────┘
      ↑
grid line 4
```

Portanto:

```text
3 rows
↓
4 grid lines
```

Essa relação é fundamental.

```text
3 row tracks = 4 grid lines
```

---

# 5. `row` e `grid line` não são a mesma coisa

Essa distinção precisa ficar muito clara.

```text
ROW
↓
é uma track

GRID LINE
↓
é uma linha que delimita as tracks
```

Visualmente:

```text
grid line
    ↓
    ├───────────────────────┤
            ROW
    ├───────────────────────┤
    ↑
grid line
```

Portanto:

```text
GRID LINE ≠ ROW
```

Uma row existe **entre duas grid lines**.

---

# 6. A relação entre rows e grid lines

Considere um Grid com três rows:

```text
grid line 1
      │
      ├───────────────┤
           ROW 1
      ├───────────────┤
      │
grid line 2
      │
      ├───────────────┤
           ROW 2
      ├───────────────┤
      │
grid line 3
      │
      ├───────────────┤
           ROW 3
      ├───────────────┤
      │
grid line 4
```

Podemos escrever:

```text
ROW 1 = espaço entre linhas 1 e 2
ROW 2 = espaço entre linhas 2 e 3
ROW 3 = espaço entre linhas 3 e 4
```

Por isso:

```css
grid-row: 1 / 4;
```

significa:

```text
linha 1
   ↓
ROW 1
   ↓
linha 2
   ↓
ROW 2
   ↓
linha 3
   ↓
ROW 3
   ↓
linha 4
```

Resultado:

```text
3 rows
```

---

# 7. A propriedade `grid-row`

A propriedade:

```css
grid-row
```

é um **shorthand** utilizado para definir o posicionamento de um grid item no eixo das rows.

Ela permite informar:

```text
uma linha
um span
um nome de linha
ou posicionamento automático
```

Exemplos:

```css
grid-row: 2;
```

```css
grid-row: 1 / 4;
```

```css
grid-row: 2 / -1;
```

```css
grid-row: 1 / span 3;
```

```css
grid-row: span 2;
```

---

# 8. `grid-row` é um shorthand

`grid-row` reúne:

```css
grid-row-start
grid-row-end
```

Portanto:

```css
grid-row: 2 / 4;
```

equivale a:

```css
grid-row-start: 2;
grid-row-end: 4;
```

Podemos visualizar:

```text
                 grid-row
                    │
          ┌─────────┴─────────┐
          │                   │
          ▼                   ▼
 grid-row-start       grid-row-end
```

Mentalmente:

```text
grid-row
   =
START / END
```

---

# 9. O significado da `/`

Na declaração:

```css
grid-row: 1 / 4;
```

a barra:

```text
/
```

separa:

```text
START / END
```

ou:

```text
INÍCIO / FIM
```

Portanto:

```css
grid-row: 1 / 4;
```

equivale conceitualmente a:

```css
grid-row-start: 1;
grid-row-end: 4;
```

A barra não significa uma divisão matemática.

---

# 10. `grid-row: 2`

Podemos definir somente um valor:

```css
.item {
  grid-row: 2;
}
```

A declaração informa uma posição inicial:

```text
START = 2
```

A outra extremidade permanece em comportamento automático.

Um caso comum é o item ocupar uma row.

Visualmente:

```text
grid line 1
────────────────────────

grid line 2
────────┌───────────────┐
        │     ITEM      │
        └───────────────┘

grid line 3
────────────────────────
```

O item foi colocado na:

```text
ROW 2
```

---

# 11. Exemplo básico

HTML:

```html
<div class="grid">
  <div class="item item-1">1</div>
  <div class="item item-2">2</div>
  <div class="item item-3">3</div>
</div>
```

CSS:

```css
.grid {
  display: grid;

  grid-template-columns:
    repeat(3, 1fr);

  grid-template-rows:
    repeat(3, 100px);
}

.item-1 {
  grid-row: 2;
}
```

A ideia é:

```text
ITEM 1
   ↓
começa na grid line 2
```

Enquanto sua posição nas colunas continua sendo determinada separadamente.

---

# 12. `grid-row` não controla a coluna

Esta propriedade:

```css
grid-row
```

não diz em qual coluna o item ficará.

Ela trabalha apenas no eixo das rows.

Portanto:

```css
.item {
  grid-row: 2;
}
```

não significa:

```text
"coloque o item na segunda coluna"
```

Significa:

```text
"posicione o item a partir da grid line 2 do eixo das rows"
```

A posição nas colunas é controlada por:

```css
grid-column
```

Podemos separar:

```text
grid-column
     ↓
posição no eixo das colunas

grid-row
     ↓
posição no eixo das rows
```

---

# 13. `grid-row: 1 / 4`

Agora podemos definir início e fim:

```css
.item {
  grid-row: 1 / 4;
}
```

Isso significa:

```text
START = grid line 1
END   = grid line 4
```

Visualmente:

```text
grid line 1
──────────────┐
              │
              │
              │
              │   ITEM
              │
              │
              │
──────────────┘
grid line 4
```

O item ocupa:

```text
ROW 1
ROW 2
ROW 3
```

Portanto:

```text
1 / 4 = 3 rows
```

---

# 14. Entendendo `1 / 2`

```css
grid-row: 1 / 2;
```

Resultado:

```text
grid line 1
───────────────
│     ITEM    │
───────────────
grid line 2
```

O item ocupa:

```text
1 row
```

---

# 15. Entendendo `1 / 3`

```css
grid-row: 1 / 3;
```

Resultado:

```text
grid line 1
───────────────
│             │
│    ITEM     │
│             │
───────────────
grid line 3
```

O item ocupa:

```text
2 rows
```

Porque:

```text
3 - 1 = 2
```

---

# 16. Entendendo `1 / 4`

```css
grid-row: 1 / 4;
```

Resultado:

```text
grid line 1
───────────────
│             │
│             │
│    ITEM     │
│             │
│             │
───────────────
grid line 4
```

O item ocupa:

```text
3 rows
```

Porque:

```text
4 - 1 = 3
```

---

# 17. Regra mental para números

Quando estivermos utilizando grid lines numéricas:

```text
quantidade de rows =
linha final - linha inicial
```

Exemplos:

```text
1 / 2 = 1 row

1 / 3 = 2 rows

1 / 4 = 3 rows

2 / 5 = 3 rows

3 / 6 = 3 rows
```

Essa conta é extremamente útil para interpretar rapidamente uma declaração.

---

# 18. A ideia do `span`

A palavra:

```css
span
```

indica uma quantidade de tracks que o item deve ocupar.

Em:

```css
grid-row
```

essa quantidade corresponde a:

```text
row tracks
```

Exemplo:

```css
grid-row: span 3;
```

significa:

```text
ocupe 3 row tracks
```

Não significa:

```text
"ocupe 3 grid lines"
```

A diferença é:

```text
grid line
↓
limite

span
↓
quantidade de tracks atravessadas
```

---

# 19. `grid-row: 1 / span 3`

Considere:

```css
.item {
  grid-row: 1 / span 3;
}
```

Leia assim:

```text
comece na grid line 1
+
ocupe 3 rows
```

Visualmente:

```text
grid line 1
     │
     ├─────────────────┐
     │      ROW 1      │
     ├─────────────────┤
     │      ROW 2      │
     ├─────────────────┤
     │      ROW 3      │
     └─────────────────┘
grid line 4
```

Portanto:

```text
START = 1
SPAN  = 3
END   = 4
```

---

# 20. `grid-row: 2 / span 3`

Agora:

```css
.item {
  grid-row: 2 / span 3;
}
```

Temos:

```text
START = 2
SPAN  = 3
```

O Grid precisa atravessar:

```text
2 → 3
3 → 4
4 → 5
```

Logo:

```text
END = 5
```

Visualmente:

```text
grid line 1
───────────────

grid line 2
     ├───────────────┐
     │     ROW 1     │
     ├───────────────┤
     │     ROW 2     │
     ├───────────────┤
     │     ROW 3     │
     └───────────────┘
grid line 5
```

---

# 21. `2 / 5` versus `2 / span 3`

Estas duas declarações podem produzir a mesma área:

```css
grid-row: 2 / 5;
```

e:

```css
grid-row: 2 / span 3;
```

Porque:

```text
2 → 3 = 1 row
3 → 4 = 1 row
4 → 5 = 1 row

total = 3 rows
```

Mas a intenção é expressada de maneira diferente.

## `2 / 5`

```text
COMECE NA LINHA 2
TERMINE NA LINHA 5
```

## `2 / span 3`

```text
COMECE NA LINHA 2
OCUPE 3 ROWS
```

Essa diferença é importante quando queremos pensar no layout como:

```text
posição + tamanho
```

em vez de somente:

```text
posição inicial + posição final
```

---

# 22. `grid-row: span 2`

Também podemos utilizar:

```css
.item {
  grid-row: span 2;
}
```

Aqui estamos dizendo principalmente:

```text
ocupe 2 row tracks
```

Sem informar explicitamente uma linha inicial.

Quando o item participa do auto-placement, a posição inicial pode ser resolvida pelo algoritmo de posicionamento automático.

Mentalmente:

```text
posição inicial → automática
tamanho         → 2 rows
```

---

# 23. `grid-row-start`

A propriedade:

```css
grid-row-start
```

define a posição inicial do grid item no eixo das rows.

Exemplo:

```css
.item {
  grid-row-start: 2;
}
```

Interpretação:

```text
START = grid line 2
```

Visualmente:

```text
grid line 1
────────────────

grid line 2
──────┌────────────┐
      │    ITEM    │
      └────────────┘

grid line 3
────────────────
```

---

# 24. `grid-row-end`

A propriedade:

```css
grid-row-end
```

define a posição final do item no eixo das rows.

Exemplo:

```css
.item {
  grid-row-end: 4;
}
```

Interpretação:

```text
END = grid line 4
```

Podemos utilizar as duas propriedades:

```css
.item {
  grid-row-start: 2;
  grid-row-end: 4;
}
```

que equivale a:

```css
.item {
  grid-row: 2 / 4;
}
```

---

# 25. Por que `grid-row-start` e `grid-row-end` são importantes?

Porque `grid-row` não é uma propriedade isolada.

Ela resume duas propriedades:

```text
grid-row
│
├── grid-row-start
│
└── grid-row-end
```

Isso significa que, quando você aprende:

```css
grid-row: 2 / 5;
```

está, ao mesmo tempo, aprendendo:

```css
grid-row-start: 2;
grid-row-end: 5;
```

---

# 26. Linhas positivas

As grid lines são numeradas a partir do lado inicial do Grid.

Exemplo com três rows:

```text
grid line 1
     │
     ├───────────────┤
          ROW 1
     ├───────────────┤
     │
grid line 2
     │
     ├───────────────┤
          ROW 2
     ├───────────────┤
     │
grid line 3
     │
     ├───────────────┤
          ROW 3
     ├───────────────┤
     │
grid line 4
```

Portanto:

```text
1 → início
2 → próxima linha
3 → próxima linha
4 → final
```

---

# 27. Linhas negativas

Também podemos contar grid lines a partir da extremidade final do grid explícito.

Exemplo:

```text
1
│
2
│
3
│
4
```

Com a numeração negativa:

```text
-4
│
-3
│
-2
│
-1
```

Assim:

```text
grid line 4 = -1
grid line 3 = -2
grid line 2 = -3
grid line 1 = -4
```

Exemplo:

```css
grid-row: 1 / -1;
```

significa:

```text
primeira grid line
até
última grid line do grid explícito
```

---

# 28. `grid-row: 1 / -1`

Considere:

```css
.item {
  grid-row: 1 / -1;
}
```

Podemos interpretar:

```text
START = primeira grid line
END   = última grid line do grid explícito
```

Visualmente:

```text
grid line 1
     │
     ├────────────────┐
     │                │
     │                │
     │      ITEM      │
     │                │
     │                │
     └────────────────┘
                      ↑
                     -1
```

Esse padrão é muito útil quando um item deve atravessar todas as rows explícitas.

---

# 29. Por que `-1` é útil?

Imagine:

```css
grid-template-rows:
  repeat(3, 100px);
```

Temos:

```text
3 rows
4 grid lines
```

Se escrevemos:

```css
grid-row: 1 / 4;
```

funciona.

Mas depois podemos alterar para:

```css
grid-template-rows:
  repeat(6, 100px);
```

Agora temos:

```text
6 rows
7 grid lines
```

Se mantivermos:

```css
grid-row: 1 / 4;
```

o item não ocupará mais todo o Grid.

Porém:

```css
grid-row: 1 / -1;
```

continua representando:

```text
primeira linha
até
última linha do grid explícito
```

---

# 30. Grid explícito

Quando definimos:

```css
grid-template-rows:
  repeat(3, 100px);
```

estamos criando explicitamente três row tracks.

Visualmente:

```text
grid line 1
──────────────
ROW 1
──────────────
grid line 2
──────────────
ROW 2
──────────────
grid line 3
──────────────
ROW 3
──────────────
grid line 4
```

Essas rows fazem parte do:

```text
GRID EXPLÍCITO
```

---

# 31. Grid implícito

O CSS Grid também pode criar rows que não foram declaradas diretamente.

Essas são as:

```text
ROWS IMPLÍCITAS
```

Imagine:

```css
.grid {
  display: grid;

  grid-template-rows:
    100px
    100px;
}
```

Você definiu:

```text
2 rows explícitas
```

Mas determinado posicionamento pode exigir mais espaço.

O navegador pode então criar uma row implícita.

Visualmente:

```text
ROWS EXPLÍCITAS

┌────────────────────┐
│      ROW 1         │
├────────────────────┤
│      ROW 2         │
└────────────────────┘

          ↓

necessidade de mais espaço

          ↓

┌────────────────────┐
│      ROW 1         │
├────────────────────┤
│      ROW 2         │
├────────────────────┤
│ ROW IMPLÍCITA      │
└────────────────────┘
```

---

# 32. Como `grid-row` pode levar à criação de rows implícitas

Considere:

```css
.grid {
  display: grid;

  grid-template-rows:
    repeat(2, 100px);
}

.item {
  grid-row: 2 / span 3;
}
```

O item começa na grid line 2 e precisa ocupar:

```text
3 rows
```

O intervalo será:

```text
2 → 3
3 → 4
4 → 5
```

Portanto, ele precisa alcançar uma região além das duas rows inicialmente definidas.

O Grid pode criar rows implícitas para acomodar a colocação.

Mentalmente:

```text
GRID EXPLÍCITO
    ↓
não é suficiente

        ↓

GRID IMPLÍCITO
    ↓
novas tracks são criadas
```

---

# 33. `grid-auto-rows`

Quando rows implícitas são criadas, seu tamanho pode ser controlado por:

```css
grid-auto-rows
```

Exemplo:

```css
.grid {
  display: grid;

  grid-template-rows:
    100px
    100px;

  grid-auto-rows: 50px;
}
```

Temos:

```text
rows explícitas:
100px
100px

rows implícitas:
50px
50px
50px
...
```

Podemos visualizar:

```text
┌────────────────────┐
│      100px         │ ← explícita
├────────────────────┤
│      100px         │ ← explícita
├────────────────────┤
│       50px         │ ← implícita
├────────────────────┤
│       50px         │ ← implícita
└────────────────────┘
```

---

# 34. O que acontece se `grid-auto-rows` não for definido?

Se não definirmos:

```css
grid-auto-rows
```

as rows implícitas não recebem automaticamente um tamanho fixo escolhido por nós.

Seu tamanho será determinado pelas regras de dimensionamento automático do Grid e pelo conteúdo.

Por isso, dependendo do cenário, uma row implícita pode parecer muito pequena.

O ponto importante é:

```text
grid-template-rows
↓
define rows explícitas

grid-auto-rows
↓
controla rows implícitas
```

---

# 35. Exemplo completo com rows implícitas

```css
.grid {
  display: grid;

  grid-template-columns:
    repeat(3, 1fr);

  grid-template-rows:
    100px
    100px;

  grid-auto-rows: 50px;
}

.item {
  grid-row: 2 / span 4;
}
```

O item precisa atravessar:

```text
2 → 3
3 → 4
4 → 5
5 → 6
```

Se as linhas 5 e 6 exigirem tracks ainda não definidas no grid explícito, o Grid poderá criar tracks implícitas para satisfazer o posicionamento.

Essas tracks poderão usar:

```css
grid-auto-rows: 50px;
```

---

# 36. Linhas nomeadas

Não precisamos utilizar somente números.

Podemos nomear grid lines.

Exemplo:

```css
.grid {
  display: grid;

  grid-template-rows:
    [inicio] 100px
    [meio] 100px
    [fim] 100px;
}
```

Agora temos:

```text
[inicio]
    │
    ├───────────────
          ROW
    ├───────────────
    │
[meio]
    │
    ├───────────────
          ROW
    ├───────────────
    │
[fim]
    │
    ├───────────────
          ROW
    ├───────────────
    │
```

Esses nomes podem ser usados nas propriedades de posicionamento.

---

# 37. Utilizando uma linha nomeada

Em vez de:

```css
.item {
  grid-row: 1 / 3;
}
```

podemos ter:

```css
.item {
  grid-row: inicio / fim;
}
```

A intenção fica mais descritiva:

```text
inicio
   ↓
primeira referência

fim
   ↓
segunda referência
```

Em layouts grandes, nomes podem facilitar a leitura do código.

---

# 38. `grid-row-start` com nome

Podemos escrever:

```css
.item {
  grid-row-start: inicio;
}
```

O significado é:

```text
começar na grid line associada ao nome "inicio"
```

A especificação de posicionamento permite usar um `<custom-ident>` para procurar linhas nomeadas.

Quando esse nome corresponde a uma área nomeada, o Grid pode resolver automaticamente a linha `nome-start`.

---

# 39. `grid-row-end` com nome

Da mesma maneira:

```css
.item {
  grid-row-end: fim;
}
```

indica uma linha nomeada para a extremidade final.

Se existir uma linha com:

```text
fim-end
```

ela pode ser utilizada conforme as regras de resolução de nomes do Grid.

---

# 40. Linhas com múltiplos nomes

Uma mesma grid line pode receber vários nomes.

Exemplo:

```css
.grid {
  display: grid;

  grid-template-rows:
    [header-end main-start] 100px
    [main-end] 1fr;
}
```

A linha pode ser identificada por:

```text
header-end
```

e também:

```text
main-start
```

Isso permite representar diferentes significados estruturais para a mesma linha.

---

# 41. Nomes repetidos

Também podemos ter várias linhas com o mesmo nome.

Exemplo:

```css
.grid {
  display: grid;

  grid-template-rows:
    repeat(4, [row-start] 100px);
}
```

Agora existem várias linhas chamadas:

```text
row-start
```

Podemos especificar uma ocorrência:

```css
.item {
  grid-row: row-start 2 / row-start 4;
}
```

A ideia é:

```text
nome da linha
+
número da ocorrência
```

Isso é útil quando a mesma nomenclatura se repete ao longo de um Grid.

---

# 42. `grid-template-areas`

Outra forma de nomear a estrutura do Grid é:

```css
grid-template-areas
```

Exemplo:

```css
.grid {
  display: grid;

  grid-template-areas:
    "header"
    "content"
    "footer";
}
```

Visualmente:

```text
┌────────────────────┐
│       header       │
├────────────────────┤
│      content       │
├────────────────────┤
│       footer       │
└────────────────────┘
```

As áreas nomeadas podem ser utilizadas pelas propriedades de posicionamento do Grid.

---

# 43. Áreas nomeadas geram linhas nomeadas

Quando uma área recebe o nome:

```text
header
```

o Grid possui linhas associadas às bordas dessa área, como:

```text
header-start
header-end
```

No eixo das rows:

```text
header-start
      ↓
┌────────────────────┐
│       HEADER       │
└────────────────────┘
      ↑
header-end
```

O mesmo princípio pode ocorrer para:

```text
content
footer
sidebar
nav
```

e outros nomes definidos pelo autor.

---

# 44. Utilizando uma área com `grid-row`

Podemos aproveitar essas linhas nomeadas.

Exemplo:

```css
.item {
  grid-row: content-start / content-end;
}
```

Isso significa:

```text
começar na linha que delimita o início da área content
+
terminar na linha que delimita o fim da área content
```

Dessa maneira, o nome da estrutura pode ser mais legível do que utilizar somente números.

---

# 45. `grid-row: footer`

Aqui precisamos de uma precisão importante.

Quando escrevemos:

```css
.item {
  grid-row: footer;
}
```

não devemos simplesmente memorizar:

```text
"footer = ocupe toda a área footer"
```

O comportamento técnico de `grid-row` trata esse valor como um `<custom-ident>` usado no posicionamento baseado em linhas.

No contexto de áreas nomeadas, os nomes:

```text
footer-start
footer-end
```

são relevantes para determinar as bordas dessa área.

Quando a intenção é deixar explícito que queremos a área completa, a forma mais direta geralmente é:

```css
.item {
  grid-area: footer;
}
```

Enquanto:

```css
.item {
  grid-row: footer-start / footer-end;
}
```

deixa claro que estamos trabalhando especificamente com as duas linhas horizontais que delimitam `footer`.

---

# 46. `grid-area` e `grid-row`

É importante não confundir:

```css
grid-area
```

com:

```css
grid-row
```

`grid-area` pode definir uma área inteira.

Sua ordem completa de valores é:

```text
grid-row-start
grid-column-start
grid-row-end
grid-column-end
```

Enquanto:

```css
grid-row
```

controla somente:

```text
grid-row-start
grid-row-end
```

Portanto:

```text
grid-area
↓
linha + coluna

grid-row
↓
rows
```

---

# 47. Auto-placement

Nem todos os itens precisam receber uma posição manual.

Quando não há posicionamento explícito suficiente, o Grid pode utilizar o algoritmo de **auto-placement**.

Exemplo:

```html
<div class="grid">
  <div>1</div>
  <div>2</div>
  <div>3</div>
  <div>4</div>
  <div>5</div>
  <div>6</div>
</div>
```

```css
.grid {
  display: grid;

  grid-template-columns:
    repeat(3, 1fr);

  grid-template-rows:
    repeat(2, 100px);
}
```

O navegador posiciona automaticamente os itens.

Visualmente:

```text
┌─────┬─────┬─────┐
│  1  │  2  │  3  │
├─────┼─────┼─────┤
│  4  │  5  │  6  │
└─────┴─────┴─────┘
```

---

# 48. `grid-auto-flow`

O comportamento de auto-placement é influenciado por:

```css
grid-auto-flow
```

O valor inicial é:

```css
grid-auto-flow: row;
```

No comportamento mais comum, o algoritmo procura preencher o Grid seguindo as rows.

Podemos imaginar:

```text
→ → →
→ → →
→ → →
```

Ou seja:

```text
primeira row
↓
segunda row
↓
terceira row
```

---

# 49. O que acontece quando movemos um item?

Imagine:

```css
.item-1 {
  grid-row: 2;
}
```

Agora o item 1 foi colocado explicitamente em uma row diferente.

Os outros itens, que continuam sem posicionamento explícito completo, precisam ser processados pelo auto-placement.

Por isso:

```text
alterar um item
       ↓
pode alterar a disposição dos demais
```

O Grid não está simplesmente "movendo somente aquele item".

Ele está executando novamente as regras de colocação necessárias para organizar todo o conjunto.

---

# 50. Espaços vazios

Podemos encontrar situações como:

```text
┌───────┬───────┬───────┐
│       │   1   │       │
├───────┼───────┼───────┤
│   2   │   3   │   4   │
└───────┴───────┴───────┘
```

O espaço vazio não significa necessariamente erro.

Ele pode existir porque:

```text
um item foi posicionado explicitamente
+
os demais seguiram o auto-placement
```

A partir disso, o preenchimento natural pode deixar lacunas.

---

# 51. `grid-auto-flow: dense`

Podemos utilizar:

```css
.grid {
  grid-auto-flow: dense;
}
```

O valor:

```text
dense
```

faz o algoritmo de auto-placement tentar preencher espaços anteriores que ficaram disponíveis, quando um item posterior couber naquela posição.

Conceitualmente:

```text
SEM dense

┌───────┬───────┬───────┐
│   1   │   2   │       │
├───────┼───────┼───────┤
│   3   │       │       │
└───────┴───────┴───────┘
```

Com uma disposição compatível:

```text
COM dense

┌───────┬───────┬───────┐
│   1   │   2   │   4   │
├───────┼───────┼───────┤
│   3   │       │       │
└───────┴───────┴───────┘
```

O objetivo é:

```text
aproveitar melhor os espaços
```

---

# 52. `dense` não ignora posicionamento explícito

Uma interpretação incorreta seria:

```text
dense
↓
"o navegador pode ignorar minhas posições"
```

Não é isso.

O `dense` influencia o algoritmo de **auto-placement** dos itens que ainda precisam ser posicionados automaticamente.

Podemos pensar:

```text
posição explícita
      ↓
continua sendo respeitada

auto-placement
      ↓
pode procurar lacunas anteriores
```

---

# 53. Exemplo: `grid-row` com três columns

```html
<div class="grid">
  <div class="item item-1">1</div>
  <div class="item item-2">2</div>
  <div class="item item-3">3</div>
  <div class="item item-4">4</div>
</div>
```

```css
.grid {
  display: grid;

  grid-template-columns:
    repeat(3, 1fr);

  grid-template-rows:
    repeat(3, 100px);
}

.item-1 {
  grid-row: 2;
}
```

O item 1 começa na segunda row.

A posição das colunas continua sendo tratada pelo posicionamento das colunas e pelo auto-placement.

---

# 54. Exemplo: item ocupando várias rows

```css
.item-1 {
  grid-row: 1 / 4;
}
```

Visualmente:

```text
┌──────────┬──────────┬──────────┐
│  ITEM 1  │          │          │
│          │          │          │
├──────────┼──────────┼──────────┤
│  ITEM 1  │          │          │
│          │          │          │
├──────────┼──────────┼──────────┤
│  ITEM 1  │          │          │
│          │          │          │
└──────────┴──────────┴──────────┘
```

O item ocupa:

```text
3 rows
```

---

# 55. Exemplo: `span`

```css
.item-1 {
  grid-row: 1 / span 3;
}
```

Podemos ler:

```text
comece na grid line 1
+
ocupe 3 rows
```

Resultado equivalente neste exemplo:

```css
.item-1 {
  grid-row: 1 / 4;
}
```

---

# 56. Exemplo: iniciar depois e expandir

```css
.item-1 {
  grid-row: 2 / span 3;
}
```

Visualização:

```text
grid line 1
──────────────

grid line 2
     ┌───────────────┐
     │     ITEM      │
     │     ROW 1     │
     ├───────────────┤
     │     ROW 2     │
     ├───────────────┤
     │     ROW 3     │
     └───────────────┘
grid line 5
```

---

# 57. Exemplo: ocupar toda a estrutura explícita

```css
.item {
  grid-row: 1 / -1;
}
```

Visualmente:

```text
┌─────────────────────────┐
│                         │
│                         │
│          ITEM           │
│                         │
│                         │
│                         │
└─────────────────────────┘
```

Esse padrão significa:

```text
primeira grid line
↓
todas as rows explícitas
↓
última grid line
```

---

# 58. Exemplo com `grid-template-rows`

```css
.grid {
  display: grid;

  grid-template-columns:
    repeat(3, 1fr);

  grid-template-rows:
    100px
    150px
    200px;
}
```

Temos:

```text
grid line 1
──────────────
ROW 1
100px
──────────────
grid line 2
──────────────
ROW 2
150px
──────────────
grid line 3
──────────────
ROW 3
200px
──────────────
grid line 4
```

Se fizermos:

```css
.item {
  grid-row: 1 / 4;
}
```

o item atravessará as três rows, independentemente do fato de elas possuírem alturas diferentes.

---

# 59. `grid-row` define posição e extensão

Esse é um ponto importante.

Ao escrever:

```css
grid-row: 2 / 5;
```

não estamos apenas dizendo:

```text
"coloque o item na row 2"
```

Estamos definindo:

```text
onde começa
+
onde termina
```

ou seja:

```text
posição
+
extensão
```

O resultado será uma área composta por várias row tracks.

---

# 60. `span` como expressão de tamanho

Podemos observar uma diferença interessante:

```css
grid-row: 2 / 5;
```

expressa:

```text
posição inicial + posição final
```

Enquanto:

```css
grid-row: 2 / span 3;
```

expressa:

```text
posição inicial + tamanho
```

Essa distinção pode parecer pequena, mas ajuda muito quando começamos a construir layouts dinâmicos.

---

# 61. Um modelo mental poderoso

Quando encontrar:

```css
grid-row: A / B;
```

pense:

```text
1. Qual é a grid line inicial?
2. Qual é a grid line final?
3. Quantas rows existem entre elas?
```

Exemplo:

```css
grid-row: 2 / 6;
```

Primeiro:

```text
START = 2
```

Depois:

```text
END = 6
```

Depois:

```text
6 - 2 = 4
```

Resultado:

```text
4 rows
```

---

# 62. Modelo mental para `span`

Quando encontrar:

```css
grid-row: 3 / span 4;
```

pense:

```text
START = 3
SPAN  = 4 rows
```

Agora percorra:

```text
3 → 4
4 → 5
5 → 6
6 → 7
```

Então:

```text
END = 7
```

---

# 63. Modelo mental completo

O raciocínio pode ser:

```text
                    GRID
                      │
             ┌────────┴────────┐
             │                 │
             ▼                 ▼
          COLUMNS             ROWS
             │                 │
             │                 ▼
             │             ROW TRACKS
             │                 │
             │                 ▼
             │            GRID LINES
             │                 │
             └────────┬────────┘
                      │
                      ▼
                 GRID ITEM
                      │
                      ▼
                   grid-row
                      │
             ┌────────┴────────┐
             ▼                 ▼
           START              END
             │                 │
             └────────┬────────┘
                      ▼
                  ÁREA DO ITEM
```

---

# 64. Outra forma de pensar

Imagine o Grid como uma parede dividida por linhas horizontais:

```text
────────────────────────  ← grid line 1
        ROW 1
────────────────────────  ← grid line 2
        ROW 2
────────────────────────  ← grid line 3
        ROW 3
────────────────────────  ← grid line 4
```

Se você diz:

```css
grid-row: 1 / 3;
```

está apontando:

```text
linha 1
   ↓
┌──────────────┐
│    ITEM      │
│              │
└──────────────┘
   ↑
linha 3
```

Não está contando rows diretamente.

Está indicando:

```text
os limites da área
```

---

# 65. Regra de ouro

```text
╔══════════════════════════════════════════════╗
║                                              ║
║  GRID-ROW PENSA EM GRID LINES.               ║
║                                              ║
║  A ROW É A TRACK ENTRE ESSAS LINHAS.         ║
║                                              ║
║  START = onde começa                         ║
║  END   = onde termina                        ║
║  SPAN  = quantas ROW TRACKS ocupa            ║
║                                              ║
╚══════════════════════════════════════════════╝
```

Portanto:

```css
grid-row: 1 / 4;
```

pode ser lido como:

```text
linha 1
↓
atravesse as rows
↓
linha 4
```

Resultado:

```text
3 rows
```

E:

```css
grid-row: 2 / span 3;
```

pode ser lido como:

```text
comece na linha 2
↓
ocupe 3 rows
↓
termine na linha 5
```

---

# 66. Diferença entre `grid-row` e `grid-column`

Podemos resumir os dois conceitos:

```text
grid-column
│
└── trabalha no eixo das colunas

grid-row
│
└── trabalha no eixo das rows
```

Exemplo:

```css
.item {
  grid-column: 2 / 4;
  grid-row: 1 / 3;
}
```

Isso significa:

```text
COLUNAS
2 → 4
↓
2 column tracks

ROWS
1 → 3
↓
2 row tracks
```

Resultado:

```text
┌───────┬───────┬───────┐
│       │ ITEM  │ ITEM  │
├───────┼───────┼───────┤
│       │ ITEM  │ ITEM  │
├───────┼───────┼───────┤
│       │       │       │
└───────┴───────┴───────┘
```

---

# 67. Posicionamento bidimensional

Como Grid é bidimensional, podemos combinar:

```css
grid-column
```

com:

```css
grid-row
```

Por exemplo:

```css
.card {
  grid-column: 2 / 4;
  grid-row: 2 / 5;
}
```

Interpretando:

```text
horizontal:
2 → 4
= 2 columns

vertical:
2 → 5
= 3 rows
```

Assim:

```text
Área do item =
2 columns × 3 rows
```

---

# 68. Exemplo completo de layout

Considere:

```text
┌───────────────────────────────────┐
│              HEADER               │
├───────────────┬───────────────────┤
│               │                   │
│     NAV       │      CONTENT      │
│               │                   │
│               │                   │
├───────────────┴───────────────────┤
│              FOOTER               │
└───────────────────────────────────┘
```

Podemos controlar o eixo das rows assim:

```css
.header {
  grid-row: 1 / 2;
}

.nav {
  grid-row: 2 / 4;
}

.content {
  grid-row: 2 / 4;
}

.footer {
  grid-row: 4 / 5;
}
```

E o eixo das colunas separadamente:

```css
.header,
.footer {
  grid-column: 1 / -1;
}

.nav {
  grid-column: 1;
}

.content {
  grid-column: 2 / 4;
}
```

Assim podemos controlar as duas dimensões independentemente.

---

# 69. Exemplo com linhas nomeadas

```css
.grid {
  display: grid;

  grid-template-rows:
    [header-start] 80px
    [header-end content-start] 1fr
    [content-end footer-start] 80px
    [footer-end];
}
```

Agora podemos utilizar:

```css
.content {
  grid-row:
    content-start /
    content-end;
}
```

O código comunica melhor:

```text
CONTENT START
      ↓
     ITEM
      ↓
CONTENT END
```

em vez de depender apenas de números.

---

# 70. Por que nomes podem ser melhores?

Compare:

```css
.item {
  grid-row: 2 / 4;
}
```

com:

```css
.item {
  grid-row: content-start / content-end;
}
```

A primeira forma descreve:

```text
posição numérica
```

A segunda descreve:

```text
estrutura semântica
```

Em layouts simples, números são suficientes.

Em layouts maiores, nomes podem facilitar manutenção e leitura.

---

# 71. Exemplo com `grid-template-areas`

```css
.grid {
  display: grid;

  grid-template-rows:
    100px
    1fr
    80px;

  grid-template-areas:
    "header"
    "content"
    "footer";
}
```

Visualmente:

```text
┌──────────────────┐
│      HEADER      │
├──────────────────┤
│                  │
│     CONTENT      │
│                  │
├──────────────────┤
│      FOOTER      │
└──────────────────┘
```

Podemos fazer:

```css
.header {
  grid-area: header;
}

.content {
  grid-area: content;
}

.footer {
  grid-area: footer;
}
```

---

# 72. Linhas geradas pelas áreas

A área:

```text
header
```

possui bordas associadas:

```text
header-start
header-end
```

A área:

```text
content
```

possui:

```text
content-start
content-end
```

E:

```text
footer
```

possui:

```text
footer-start
footer-end
```

Podemos imaginar:

```text
header-start
      ↓
┌──────────────────┐
│      HEADER      │
└──────────────────┘
      ↑
header-end

content-start
      ↓
┌──────────────────┐
│     CONTENT      │
└──────────────────┘
      ↑
content-end

footer-start
      ↓
┌──────────────────┐
│      FOOTER      │
└──────────────────┘
      ↑
footer-end
```

Essas linhas nomeadas podem ser utilizadas no posicionamento baseado em linhas.

---

# 73. `grid-row` e áreas nomeadas

Podemos fazer:

```css
.item {
  grid-row:
    content-start /
    content-end;
}
```

Isso deixa explícito que o item deve ocupar:

```text
do início da área content
até
o fim da área content
```

Enquanto:

```css
.item {
  grid-area: content;
}
```

associa o item diretamente à área `content`.

---

# 74. Linha nomeada inexistente

Existe uma sutileza importante.

Imagine:

```css
.grid {
  display: grid;

  grid-template-rows:
    [row1] 100px
    [row2] 100px
    [row3] 100px;
}
```

Agora escrevemos:

```css
.item {
  grid-row: row4;
}
```

Mas:

```text
row4
```

não existe.

O CSS Grid não interpreta isso simplesmente como:

```text
"use a quarta linha porque o nome é parecido"
```

Nomes de linhas possuem regras específicas de resolução.

Quando não existe uma linha correspondente ao nome informado, o comportamento passa pelas regras de `<custom-ident>` e pode envolver a busca por linhas implícitas.

Por isso, ao utilizar linhas nomeadas, seja consistente com os nomes realmente definidos.

---

# 75. Consistência com linhas nomeadas

Se definimos:

```css
grid-template-rows:
  [header] 100px
  [content] 1fr
  [footer] 80px;
```

não devemos depois inventar:

```css
grid-row: sidebar;
```

esperando que o navegador encontre automaticamente uma linha existente.

A ideia é:

```text
NOME DEFINIDO
      ↓
NOME UTILIZADO
      ↓
deve fazer sentido dentro das regras de resolução do Grid
```

---

# 76. `auto` em `grid-row`

A propriedade também aceita:

```css
grid-row: auto;
```

O valor:

```text
auto
```

indica que essa parte da posição deve permanecer automática.

Por isso, quando encontramos:

```css
grid-row: 2;
```

a outra extremidade não foi explicitamente definida.

Podemos pensar:

```text
START = 2
END = auto
```

---

# 77. Os principais valores de `grid-row`

Os formatos mais importantes são:

```css
grid-row: auto;
```

```css
grid-row: 2;
```

```css
grid-row: 2 / 5;
```

```css
grid-row: 2 / -1;
```

```css
grid-row: 2 / span 3;
```

```css
grid-row: span 3;
```

```css
grid-row: inicio / fim;
```

Cada um comunica uma intenção diferente.

---

# 78. Resumo dos formatos

```text
grid-row: 2;

→ começa na grid line 2
→ fim automático


grid-row: 2 / 5;

→ começa na linha 2
→ termina na linha 5
→ ocupa 3 rows


grid-row: 2 / -1;

→ começa na linha 2
→ termina na última linha do grid explícito


grid-row: 2 / span 3;

→ começa na linha 2
→ ocupa 3 rows


grid-row: span 3;

→ ocupa 3 rows
→ posição inicial pode ser automática


grid-row: inicio / fim;

→ utiliza linhas nomeadas
```

---

# 79. Comparação rápida

| Declaração               | Significado                                                  |
| ------------------------ | ------------------------------------------------------------ |
| `grid-row: 2`            | começa na grid line 2 e deixa a outra extremidade automática |
| `grid-row: 1 / 3`        | da grid line 1 até a 3                                       |
| `grid-row: 1 / 4`        | ocupa 3 row tracks                                           |
| `grid-row: 2 / 5`        | ocupa 3 row tracks                                           |
| `grid-row: 1 / -1`       | da primeira até a última linha do grid explícito             |
| `grid-row: 2 / span 3`   | começa em 2 e ocupa 3 rows                                   |
| `grid-row: span 2`       | ocupa 2 rows                                                 |
| `grid-row: inicio / fim` | utiliza linhas nomeadas                                      |
| `grid-row-start: 2`      | define a linha inicial                                       |
| `grid-row-end: 4`        | define a linha final                                         |

---

# 80. Perguntas para testar a compreensão

## Quantas grid lines existem em um Grid com 3 rows?

```text
4 grid lines
```

Porque:

```text
rows + 1 = grid lines
```

---

## O que existe entre duas grid lines?

```text
uma grid track
```

No eixo das rows:

```text
uma row track
```

---

## O que faz:

```css
grid-row: 1 / 4;
```

Resposta mental:

```text
linha 1 → linha 4
=
3 rows
```

---

## O que faz:

```css
grid-row: 2 / span 3;
```

Resposta mental:

```text
começa na linha 2
+
ocupa 3 rows
```

---

## O que faz:

```css
grid-row: 1 / -1;
```

Resposta mental:

```text
primeira grid line
até
última grid line do grid explícito
```

---

## Qual propriedade controla as rows implícitas?

```css
grid-auto-rows
```

---

## Qual propriedade controla o fluxo automático?

```css
grid-auto-flow
```

---

# 81. Erros conceituais que devemos evitar

## Erro 1 — pensar que row e grid line são a mesma coisa

Errado:

```text
3 rows = 3 grid lines
```

Correto:

```text
3 rows = 4 grid lines
```

---

## Erro 2 — pensar que `1 / 4` significa quatro rows

Errado:

```text
grid-row: 1 / 4
↓
4 rows
```

Correto:

```text
1 → 2
2 → 3
3 → 4

= 3 rows
```

---

## Erro 3 — pensar que `span 3` significa três grid lines

Errado:

```text
span 3 = 3 grid lines
```

Correto:

```text
span 3 = 3 tracks
```

No `grid-row`:

```text
span 3 = 3 row tracks
```

---

## Erro 4 — pensar que `grid-row` define a coluna

Errado:

```text
grid-row
↓
coluna
```

Correto:

```text
grid-row
↓
eixo das rows
```

E:

```text
grid-column
↓
eixo das colunas
```

---

## Erro 5 — pensar que `-1` é sempre "qualquer última linha"

Melhor modelo:

```text
-1
↓
última linha do grid explícito
```

Isso evita confundir a numeração negativa com qualquer linha que possa surgir posteriormente em um grid implícito.

---

## Erro 6 — pensar que `dense` muda posições explícitas

Não.

```text
dense
↓
atua no auto-placement
```

---

# 82. `grid-row` e acessibilidade

Mover visualmente um elemento com Grid não significa que você deve ignorar a ordem do HTML.

A estrutura HTML deve continuar fazendo sentido para:

```text
leitura
acessibilidade
tecnologias assistivas
navegação
manutenção
```

Por isso:

```text
HTML
↓
ordem semântica

CSS Grid
↓
organização visual
```

é um bom modelo mental.

O fato de `grid-row` permitir reorganizar elementos visualmente não significa que devemos construir um HTML desordenado e utilizar Grid para "consertá-lo" visualmente.

---

# 83. Exemplo final completo

HTML:

```html
<div class="layout">
  <header class="header">
    Header
  </header>

  <nav class="nav">
    Navegação
  </nav>

  <main class="content">
    Conteúdo
  </main>

  <aside class="aside">
    Aside
  </aside>

  <footer class="footer">
    Footer
  </footer>
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
    80px
    1fr
    80px;

  gap: 16px;
}

.header {
  grid-column: 1 / -1;
  grid-row: 1;
}

.nav {
  grid-column: 1;
  grid-row: 2;
}

.content {
  grid-column: 2;
  grid-row: 2;
}

.aside {
  grid-column: 3;
  grid-row: 2;
}

.footer {
  grid-column: 1 / -1;
  grid-row: 3;
}
```

Estrutura visual:

```text
                    COLUNAS
        1             2             3
        │             │             │
        ▼             ▼             ▼
┌────────────┬─────────────────┬────────────┐
│                  HEADER                   │
├────────────┼─────────────────┼────────────┤
│            │                 │            │
│    NAV     │     CONTENT     │   ASIDE    │
│            │                 │            │
├────────────┴─────────────────┴────────────┤
│                  FOOTER                   │
└───────────────────────────────────────────┘
```

Aqui temos:

```text
HEADER
grid-row: 1

NAV
grid-row: 2

CONTENT
grid-row: 2

ASIDE
grid-row: 2

FOOTER
grid-row: 3
```

O eixo das colunas é controlado separadamente por:

```text
grid-column
```

e o eixo das rows por:

```text
grid-row
```

---

# 84. Exemplo usando `span`

Podemos alterar:

```css
.content {
  grid-column: 2;
  grid-row: 2 / span 2;
}
```

Agora o conteúdo ocupa:

```text
ROW 2
+
ROW 3
```

Visualmente:

```text
┌───────────┬───────────────┬───────────┐
│           │               │           │
│    NAV    │    CONTENT    │   ASIDE   │
│           │               │           │
├───────────┤               ├───────────┤
│           │               │           │
│           │    CONTENT    │           │
│           │               │           │
└───────────┴───────────────┴───────────┘
```

Esse exemplo mostra por que `span` é útil:

```text
posição inicial
+
quantidade de tracks
```

---

# 85. Modelo mental definitivo

Quando você olhar para:

```css
grid-row: 2 / 5;
```

não pense imediatamente:

```text
"segunda até quinta linha"
```

Pense:

```text
GRID LINE 2
     ↓
 ┌───────────┐
 │   ROW     │
 ├───────────┤
 │   ROW     │
 ├───────────┤
 │   ROW     │
 └───────────┘
     ↑
GRID LINE 5
```

Depois conte:

```text
5 - 2 = 3
```

Resultado:

```text
3 row tracks
```

---

# 86. Mapa mental final

```text
CSS GRID
│
├── GRID CONTAINER
│   │
│   └── GRID ITEMS
│
├── EIXOS
│   │
│   ├── COLUNAS
│   │
│   └── ROWS
│
├── ROWS
│   │
│   └── ROW TRACKS
│
├── GRID LINES
│   │
│   ├── números positivos
│   ├── números negativos
│   └── nomes
│
├── GRID EXPLÍCITO
│   │
│   └── grid-template-rows
│
├── GRID IMPLÍCITO
│   │
│   └── grid-auto-rows
│
└── POSICIONAMENTO
    │
    ├── grid-row
    │   │
    │   ├── start
    │   ├── end
    │   ├── span
    │   ├── números
    │   └── nomes
    │
    └── grid-row-start
        grid-row-end
```

---

# 87. Regra de ouro para memorizar

```text
┌───────────────────────────────────────────────┐
│                                               │
│             GRID-ROW                          │
│                                               │
│   trabalha com o eixo das ROWS                │
│                                               │
│   START → onde começa                         │
│   END   → onde termina                        │
│   SPAN  → quantas tracks atravessa            │
│                                               │
│   GRID LINE = limite                          │
│   ROW TRACK = espaço entre limites            │
│                                               │
└───────────────────────────────────────────────┘
```

Se você lembrar somente de uma imagem, lembre desta:

```text
GRID LINE 1
───────────────
      ROW 1
───────────────
GRID LINE 2
───────────────
      ROW 2
───────────────
GRID LINE 3
───────────────
      ROW 3
───────────────
GRID LINE 4
```

Então:

```css
grid-row: 1 / 4;
```

significa:

```text
linha 1
↓
3 ROW TRACKS
↓
linha 4
```

E:

```css
grid-row: 2 / span 2;
```

significa:

```text
linha 2
↓
2 ROW TRACKS
↓
linha 4
```

---

# 88. Referência rápida

```css
/* Posicionamento automático */
grid-row: auto;

/* Começa na linha 2 */
grid-row: 2;

/* Da linha 1 até a linha 4 */
grid-row: 1 / 4;

/* Da linha 2 até a última linha do grid explícito */
grid-row: 2 / -1;

/* Começa na linha 2 e ocupa 3 rows */
grid-row: 2 / span 3;

/* Ocupa 2 rows */
grid-row: span 2;

/* Linhas nomeadas */
grid-row: inicio / fim;

/* Longhands */
grid-row-start: 2;
grid-row-end: 4;
```

---

# 89. Tabela de fixação

| Conceito              | O que significa                                       | Exemplo             |
| --------------------- | ----------------------------------------------------- | ------------------- |
| `grid-row`            | shorthand de `grid-row-start` e `grid-row-end`        | `grid-row: 1 / 4`   |
| `grid-row-start`      | define a linha inicial                                | `grid-row-start: 2` |
| `grid-row-end`        | define a linha final                                  | `grid-row-end: 4`   |
| `row`                 | row track no eixo das rows                            | `ROW 1`             |
| `grid line`           | linha estrutural que delimita tracks                  | `1`, `2`, `3`, `4`  |
| `grid track`          | espaço entre duas grid lines                          | `1 → 2`             |
| `span`                | quantidade de tracks                                  | `span 3`            |
| `-1`                  | última grid line do grid explícito                    | `1 / -1`            |
| número positivo       | conta linhas a partir do início                       | `2`                 |
| número negativo       | conta linhas a partir da extremidade final            | `-1`                |
| linha nomeada         | grid line identificada por um nome                    | `[content-start]`   |
| `grid-template-rows`  | define rows explícitas                                | `repeat(3, 100px)`  |
| `grid-auto-rows`      | define o tamanho das rows implícitas                  | `50px`              |
| `grid-template-areas` | define áreas nomeadas                                 | `"header"`          |
| `grid-auto-flow`      | controla o auto-placement                             | `row`               |
| `dense`               | permite procurar lacunas anteriores no auto-placement | `dense`             |

---

# 90. Checklist mental

Antes de escrever:

```css
grid-row: ...;
```

pergunte:

```text
□ Qual é a row que quero atingir?
□ Quantas row tracks existem?
□ Quantas grid lines existem?
□ Qual é a grid line inicial?
□ Qual é a grid line final?
□ Quero indicar END ou usar SPAN?
□ Preciso usar -1?
□ Existem linhas nomeadas?
□ Existem áreas nomeadas?
□ O item está sendo colocado manualmente?
□ Os outros itens continuam no auto-placement?
□ A colocação pode exigir rows implícitas?
□ As rows implícitas precisam de um tamanho definido por grid-auto-rows?
```

---

# 91. O conteúdo essencial para guardar

```text
1. Grid é bidimensional.

2. grid-row trabalha no eixo das rows.

3. Uma row é uma track.

4. Uma grid line delimita uma track.

5. N rows possuem N + 1 grid lines.

6. grid-row é shorthand de:
   grid-row-start
   +
   grid-row-end

7. A barra / separa START e END.

8. grid-row: 1 / 4
   significa linha 1 até linha 4,
   portanto ocupa 3 tracks.

9. span representa quantidade de tracks.

10. grid-row: 2 / span 3
    significa começar em 2
    e ocupar 3 rows.

11. -1 representa a última linha do grid explícito.

12. Nomes podem ser atribuídos às grid lines.

13. grid-template-areas cria áreas nomeadas
    e linhas associadas às bordas dessas áreas.

14. Rows implícitas podem ser criadas quando necessárias.

15. grid-auto-rows controla o dimensionamento das
    rows implícitas.

16. Itens não posicionados explicitamente podem
    ser organizados pelo auto-placement.

17. grid-auto-flow controla esse comportamento.

18. dense permite ao auto-placement procurar
    oportunidades de preencher lacunas anteriores.
```

---

# 92. Referências técnicas

## MDN Web Docs

[`grid-row`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-row)

[`grid-row-start`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-row-start)

[`grid-row-end`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-row-end)

[`grid-template-rows`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-template-rows)

[`grid-template-areas`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-template-areas)

[`Grid layout — Basic concepts`](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout/Basic_concepts)

[`Grid layout — Line-based placement`](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout/Line-based_placement)

[`Grid layout — Named grid lines`](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout/Named_grid_lines)

## Especificação

[`CSS Grid Layout Module Level 2 — W3C`](https://www.w3.org/TR/css-grid-2/)

---

# 93. GitHub

<div align="center">

### CSS Grid Layout — `grid-row`

Documentação técnica para estudo contínuo de CSS Grid Layout.

<br>

<a href="https://github.com/gabrielfelipeoliveira55" target="_blank" rel="noopener noreferrer">
Gabriel Felipe de Oliveira Rateiro
</a>

<br><br>

**CSS Grid Layout • `grid-row`**

> Não decore os números.
>
> Entenda as grid lines, as row tracks e a área que o item ocupa.

</div>
```
