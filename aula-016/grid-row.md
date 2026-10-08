# CSS Grid Layout — `grid-row`

> **Objetivo:** compreender como `grid-row` posiciona e dimensiona um grid item no eixo das rows do CSS Grid, utilizando números, linhas nomeadas, `span`, valores negativos e a relação com `grid-row-start`, `grid-row-end`, `grid-template-rows`, `grid-auto-rows` e `grid-template-areas`.

## Índice

1. [Antes de entender `grid-row`](#1-antes-de-entender-grid-row)
2. [CSS Grid possui dois eixos](#2-css-grid-possui-dois-eixos)
3. [O que é uma row?](#3-o-que-é-uma-row)
4. [O que é uma grid line?](#4-o-que-é-uma-grid-line)
5. [`row` e `grid line` não são a mesma coisa](#5-row-e-grid-line-não-são-a-mesma-coisa)
6. [A relação entre rows e grid lines](#6-a-relação-entre-rows-e-grid-lines)
7. [A propriedade `grid-row`](#7-a-propriedade-grid-row)
8. [`grid-row` é um shorthand](#8-grid-row-é-um-shorthand)
9. [O significado da barra `/`](#9-o-significado-da-barra-)
10. [`grid-row: 2`](#10-grid-row-2)
11. [Exemplo básico](#11-exemplo-básico)
12. [`grid-row` não controla a coluna](#12-grid-row-não-controla-a-coluna)
13. [Contando rows entre duas grid lines](#13-contando-rows-entre-duas-grid-lines)
14. [A ideia do `span`](#14-a-ideia-do-span)
15. [`grid-row: 1 / span 3`](#15-grid-row-1--span-3)
16. [`grid-row: 2 / span 3`](#16-grid-row-2--span-3)
17. [`2 / 5` versus `2 / span 3`](#17-2--5-versus-2--span-3)
18. [`grid-row: span 2`](#18-grid-row-span-2)
19. [`grid-row-start`](#19-grid-row-start)
20. [`grid-row-end`](#20-grid-row-end)
21. [Linhas positivas](#21-linhas-positivas)
22. [Linhas negativas](#22-linhas-negativas)
23. [`grid-row: 1 / -1`](#23-grid-row-1---1)
24. [Por que `-1` é útil?](#24-por-que--1-é-útil)
25. [Grid explícito](#25-grid-explícito)
26. [Grid implícito](#26-grid-implícito)
27. [Como `grid-row` pode levar à criação de rows implícitas](#27-como-grid-row-pode-levar-à-criação-de-rows-implícitas)
28. [`grid-auto-rows`](#28-grid-auto-rows)
29. [E se `grid-auto-rows` não for definido?](#29-e-se-grid-auto-rows-não-for-definido)
30. [Exemplo completo com rows implícitas](#30-exemplo-completo-com-rows-implícitas)
31. [Linhas nomeadas](#31-linhas-nomeadas)
32. [Utilizando uma linha nomeada](#32-utilizando-uma-linha-nomeada)
33. [Nomes em `grid-row-start` e `grid-row-end`](#33-nomes-em-grid-row-start-e-grid-row-end)
34. [Linhas com múltiplos nomes](#34-linhas-com-múltiplos-nomes)
35. [Nomes repetidos](#35-nomes-repetidos)
36. [`grid-template-areas`](#36-grid-template-areas)
37. [Áreas nomeadas geram linhas nomeadas](#37-áreas-nomeadas-geram-linhas-nomeadas)
38. [Utilizando uma área com `grid-row`](#38-utilizando-uma-área-com-grid-row)
39. [`grid-row: footer`](#39-grid-row-footer)
40. [`grid-area` e `grid-row`](#40-grid-area-e-grid-row)
41. [Auto-placement](#41-auto-placement)
42. [`grid-auto-flow`](#42-grid-auto-flow)
43. [O que acontece quando movemos um item?](#43-o-que-acontece-quando-movemos-um-item)
44. [Espaços vazios](#44-espaços-vazios)
45. [`grid-auto-flow: dense`](#45-grid-auto-flow-dense)
46. [`dense` não ignora posicionamento explícito](#46-dense-não-ignora-posicionamento-explícito)
47. [Exemplos com três colunas](#47-exemplos-com-três-colunas)
48. [Exemplo com `grid-template-rows`](#48-exemplo-com-grid-template-rows)
49. [`grid-row` define posição e extensão](#49-grid-row-define-posição-e-extensão)
50. [Como ler qualquer `grid-row`](#50-como-ler-qualquer-grid-row)
51. [Modelo mental completo](#51-modelo-mental-completo)
52. [Regra de ouro](#52-regra-de-ouro)
53. [Diferença entre `grid-row` e `grid-column`](#53-diferença-entre-grid-row-e-grid-column)
54. [Posicionamento bidimensional](#54-posicionamento-bidimensional)
55. [Exemplo de layout com `grid-row`](#55-exemplo-de-layout-com-grid-row)
56. [Exemplo com linhas nomeadas](#56-exemplo-com-linhas-nomeadas)
57. [Exemplo com `grid-template-areas`](#57-exemplo-com-grid-template-areas)
58. [Linha nomeada inexistente](#58-linha-nomeada-inexistente)
59. [`auto` em `grid-row`](#59-auto-em-grid-row)
60. [Os principais formatos de `grid-row`](#60-os-principais-formatos-de-grid-row)
61. [Comparação rápida](#61-comparação-rápida)
62. [Perguntas para testar a compreensão](#62-perguntas-para-testar-a-compreensão)
63. [Erros conceituais que devemos evitar](#63-erros-conceituais-que-devemos-evitar)
64. [`grid-row` e acessibilidade](#64-grid-row-e-acessibilidade)
65. [Exemplo final completo](#65-exemplo-final-completo)
66. [Exemplo usando `span`](#66-exemplo-usando-span)
67. [Mapa mental final](#67-mapa-mental-final)
68. [Referência rápida](#68-referência-rápida)
69. [Tabela de fixação](#69-tabela-de-fixação)
70. [Checklist mental](#70-checklist-mental)
71. [O conteúdo essencial para guardar](#71-o-conteúdo-essencial-para-guardar)
72. [Referências técnicas](#72-referências-técnicas)
73. [GitHub](#73-github)

---

## 1. Antes de entender `grid-row`

Para compreender `grid-row`, primeiro é preciso entender como um elemento participa de um Grid.

Quando um elemento recebe:

```css
.container {
  display: grid;
}
```

ele se torna um **grid container**. Os seus **filhos diretos** tornam-se **grid items**.

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

```text
.grid
│
├── Item 1  ← grid item
├── Item 2  ← grid item
└── Item 3  ← grid item
```

```text
GRID CONTAINER
       ↓
FILHOS DIRETOS
       ↓
GRID ITEMS
```

É sobre esses grid items que propriedades como `grid-row`, `grid-row-start` e `grid-row-end` atuam.

---

## 2. CSS Grid possui dois eixos

O CSS Grid é um sistema de layout bidimensional. Ele trabalha com:

```text
colunas
+
rows
```

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

```text
grid-column
     ↓
posicionamento no eixo das colunas

grid-row
     ↓
posicionamento no eixo das rows
```

No modo de escrita mais comum, `horizontal-tb`, as rows são horizontais e se sucedem visualmente de cima para baixo. Por isso, no estudo inicial, é comum associar:

```text
grid-row → eixo vertical
```

O conceito tecnicamente mais preciso é:

```text
grid-row → eixo das rows
```

Isso evita confusão quando começarmos a estudar `writing-mode` e outros modos de escrita.

---

## 3. O que é uma row?

Uma **grid row** é uma **track horizontal** do Grid.

```text
┌──────────┬──────────┬──────────┐
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

Aqui temos 3 row tracks (ou, informalmente, 3 rows). Tecnicamente, uma row é o espaço entre duas grid lines horizontais.

---

## 4. O que é uma grid line?

Uma **grid line** é uma linha estrutural do Grid que delimita as tracks.

Com três rows:

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

```text
3 row tracks = 4 grid lines
```

---

## 5. `row` e `grid line` não são a mesma coisa

```text
ROW
↓
é uma track

GRID LINE
↓
é uma linha que delimita as tracks
```

```text
grid line
    ↓
    ├───────────────────────┤
            ROW
    ├───────────────────────┤
    ↑
grid line
```

```text
GRID LINE ≠ ROW
```

Uma row existe **entre duas grid lines**.

---

## 6. A relação entre rows e grid lines

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

```text
ROW 1 = espaço entre as linhas 1 e 2
ROW 2 = espaço entre as linhas 2 e 3
ROW 3 = espaço entre as linhas 3 e 4
```

Por isso:

```css
grid-row: 1 / 4;
```

atravessa as três rows:

```text
linha 1 → ROW 1 → linha 2 → ROW 2 → linha 3 → ROW 3 → linha 4
```

Resultado: 3 rows.

---

## 7. A propriedade `grid-row`

`grid-row` é um **shorthand** que define o posicionamento de um grid item no eixo das rows. Ela aceita:

```text
um número de linha
um span
um nome de linha
posicionamento automático
```

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

## 8. `grid-row` é um shorthand

`grid-row` reúne `grid-row-start` e `grid-row-end`.

```css
grid-row: 2 / 4;
```

equivale a:

```css
grid-row-start: 2;
grid-row-end: 4;
```

```text
                 grid-row
                    │
          ┌─────────┴─────────┐
          │                   │
          ▼                   ▼
 grid-row-start       grid-row-end
```

```text
grid-row = START / END
```

---

## 9. O significado da barra `/`

```css
grid-row: 1 / 4;
```

A barra separa:

```text
START / END
```

ou:

```text
INÍCIO / FIM
```

Não é uma divisão matemática.

---

## 10. `grid-row: 2`

```css
.item {
  grid-row: 2;
}
```

Quando o valor único é um **número**, ele define a linha inicial. A linha final fica como `auto`, e o item ocupa uma única row:

```text
START = 2
END   = auto  (ocupa 1 row)
```

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

O item foi colocado na ROW 2.

> **Nota:** o comportamento é diferente quando o valor único é um **nome** (como `footer`). Isso é explicado na seção [`grid-row: footer`](#39-grid-row-footer).

---

## 11. Exemplo básico

```html
<div class="grid">
  <div class="item item-1">1</div>
  <div class="item item-2">2</div>
  <div class="item item-3">3</div>
</div>
```

```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 100px);
}

.item-1 {
  grid-row: 2;
}
```

```text
ITEM 1
   ↓
começa na grid line 2 (ocupa a ROW 2)
```

A posição do item nas colunas continua sendo determinada separadamente.

---

## 12. `grid-row` não controla a coluna

```css
.item {
  grid-row: 2;
}
```

não significa "coloque o item na segunda coluna". Significa:

```text
"posicione o item a partir da grid line 2 do eixo das rows"
```

A posição nas colunas é controlada por `grid-column`:

```text
grid-column
     ↓
posição no eixo das colunas

grid-row
     ↓
posição no eixo das rows
```

---

## 13. Contando rows entre duas grid lines

Com números, a quantidade de rows ocupadas é:

```text
quantidade de rows = linha final − linha inicial
```

### `grid-row: 1 / 2`

```text
grid line 1
───────────────
│     ITEM    │
───────────────
grid line 2
```

1 row.

### `grid-row: 1 / 3`

```text
grid line 1
───────────────
│             │
│    ITEM     │
│             │
───────────────
grid line 3
```

2 rows (`3 − 1`).

### `grid-row: 1 / 4`

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

3 rows (`4 − 1`).

### Resumo

```text
1 / 2 = 1 row
1 / 3 = 2 rows
1 / 4 = 3 rows
2 / 5 = 3 rows
3 / 6 = 3 rows
```

---

## 14. A ideia do `span`

`span` indica uma **quantidade de tracks** que o item deve ocupar. Em `grid-row`, são row tracks:

```css
grid-row: span 3;
```

```text
ocupe 3 row tracks
```

Não significa "ocupe 3 grid lines".

```text
grid line
↓
limite

span
↓
quantidade de tracks atravessadas
```

---

## 15. `grid-row: 1 / span 3`

```css
.item {
  grid-row: 1 / span 3;
}
```

```text
comece na grid line 1
+
ocupe 3 rows
```

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

```text
START = 1
SPAN  = 3
END   = 4
```

---

## 16. `grid-row: 2 / span 3`

```css
.item {
  grid-row: 2 / span 3;
}
```

```text
START = 2
SPAN  = 3
```

```text
2 → 3
3 → 4
4 → 5
```

Logo, `END = 5`.

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

## 17. `2 / 5` versus `2 / span 3`

```css
grid-row: 2 / 5;
```

```css
grid-row: 2 / span 3;
```

Ambas produzem a mesma área:

```text
2 → 3 = 1 row
3 → 4 = 1 row
4 → 5 = 1 row

total = 3 rows
```

Mas a intenção é diferente:

```text
2 / 5
→ COMECE NA LINHA 2 e TERMINE NA LINHA 5

2 / span 3
→ COMECE NA LINHA 2 e OCUPE 3 ROWS
```

Em outras palavras: `2 / 5` expressa posição inicial + posição final, enquanto `2 / span 3` expressa posição inicial + tamanho.

---

## 18. `grid-row: span 2`

```css
.item {
  grid-row: span 2;
}
```

```text
ocupe 2 row tracks
```

Sem linha inicial informada, a posição de partida é resolvida pelo auto-placement:

```text
posição inicial → automática
tamanho         → 2 rows
```

---

## 19. `grid-row-start`

`grid-row-start` define a linha inicial do item no eixo das rows.

```css
.item {
  grid-row-start: 2;
}
```

```text
START = grid line 2
```

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

## 20. `grid-row-end`

`grid-row-end` define a linha final do item no eixo das rows.

```css
.item {
  grid-row-end: 4;
}
```

```text
END = grid line 4
```

Juntas:

```css
.item {
  grid-row-start: 2;
  grid-row-end: 4;
}
```

equivalem a:

```css
.item {
  grid-row: 2 / 4;
}
```

`grid-row` não é uma propriedade isolada: ela resume essas duas.

---

## 21. Linhas positivas

As grid lines são numeradas a partir do início do eixo das rows.

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

```text
1 → início
2 → próxima linha
3 → próxima linha
4 → final
```

---

## 22. Linhas negativas

Também é possível contar a partir da extremidade final do **grid explícito**:

```text
1  ──────  -4
2  ──────  -3
3  ──────  -2
4  ──────  -1
```

```text
grid line 4 = -1
grid line 3 = -2
grid line 2 = -3
grid line 1 = -4
```

---

## 23. `grid-row: 1 / -1`

```css
.item {
  grid-row: 1 / -1;
}
```

```text
START = primeira grid line
END   = última grid line do grid explícito
```

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

Esse padrão é útil quando um item deve atravessar todas as rows explícitas.

---

## 24. Por que `-1` é útil?

```css
grid-template-rows: repeat(3, 100px);
```

```text
3 rows
4 grid lines
```

`grid-row: 1 / 4` funciona. Mas, se o Grid mudar para:

```css
grid-template-rows: repeat(6, 100px);
```

```text
6 rows
7 grid lines
```

`grid-row: 1 / 4` já não ocupa todo o Grid. Já:

```css
grid-row: 1 / -1;
```

continua representando "da primeira linha até a última linha do grid explícito".

---

## 25. Grid explícito

```css
grid-template-rows: repeat(3, 100px);
```

cria explicitamente três row tracks:

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

Essas rows fazem parte do **grid explícito**.

---

## 26. Grid implícito

O Grid também cria rows que não foram declaradas diretamente: as **rows implícitas**.

```css
.grid {
  display: grid;
  grid-template-rows: 100px 100px;
}
```

São 2 rows explícitas. Se algum posicionamento exigir mais espaço, o navegador cria uma row implícita:

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

## 27. Como `grid-row` pode levar à criação de rows implícitas

```css
.grid {
  display: grid;
  grid-template-rows: repeat(2, 100px);
}

.item {
  grid-row: 2 / span 3;
}
```

O item ocupa as rows:

```text
2 → 3   (ROW 2, explícita)
3 → 4   (ROW 3, implícita)
4 → 5   (ROW 4, implícita)
```

Como o grid explícito termina na linha 3, o Grid cria duas rows implícitas.

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

## 28. `grid-auto-rows`

O tamanho das rows implícitas é controlado por `grid-auto-rows`:

```css
.grid {
  display: grid;
  grid-template-rows: 100px 100px;
  grid-auto-rows: 50px;
}
```

```text
rows explícitas:  100px, 100px
rows implícitas:  50px, 50px, 50px, ...
```

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

## 29. E se `grid-auto-rows` não for definido?

O valor inicial de `grid-auto-rows` é `auto`: as rows implícitas se ajustam ao conteúdo. Se a row implícita estiver vazia, ela pode ficar com altura zero e parecer inexistente.

```text
grid-template-rows
↓
define rows explícitas

grid-auto-rows
↓
controla rows implícitas
```

---

## 30. Exemplo completo com rows implícitas

```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: 100px 100px;
  grid-auto-rows: 50px;
}

.item {
  grid-row: 2 / span 4;
}
```

O item ocupa as rows:

```text
2 → 3   (ROW 2, explícita, 100px)
3 → 4   (ROW 3, implícita, 50px)
4 → 5   (ROW 4, implícita, 50px)
5 → 6   (ROW 5, implícita, 50px)
```

O grid explícito tem duas rows. As rows 3, 4 e 5 são criadas implicitamente e recebem `50px` de `grid-auto-rows`.

---

## 31. Linhas nomeadas

Grid lines podem receber nomes entre colchetes. O nome vem **antes** da track cuja linha inicial ele identifica:

```css
.grid {
  display: grid;
  grid-template-rows:
    [inicio] 100px
    [meio] 100px
    [fim] 100px
    [final];
}
```

```text
[inicio]   → linha 1
   ├── ROW 1 ──┤
[meio]     → linha 2
   ├── ROW 2 ──┤
[fim]      → linha 3
   ├── ROW 3 ──┤
[final]    → linha 4
```

Esses nomes podem ser usados nas propriedades de posicionamento.

---

## 32. Utilizando uma linha nomeada

```css
.item {
  grid-row: inicio / fim;
}
```

equivale a:

```css
.item {
  grid-row: 1 / 3;
}
```

O item ocupa 2 rows. Para ocupar as três, use `inicio / final`, que equivale a `1 / 4`.

Em layouts grandes, nomes facilitam a leitura do código.

---

## 33. Nomes em `grid-row-start` e `grid-row-end`

```css
.item {
  grid-row-start: inicio;
  grid-row-end: fim;
}
```

Ao receber um nome (`<custom-ident>`), o Grid procura uma linha com esse nome. Se não existir, procura uma linha de área nomeada:

```text
grid-row-start: nome  →  linha "nome"  ou  "nome-start"
grid-row-end:   nome  →  linha "nome"  ou  "nome-end"
```

---

## 34. Linhas com múltiplos nomes

Uma mesma grid line pode receber vários nomes:

```css
.grid {
  display: grid;
  grid-template-rows:
    [header-start] 100px
    [header-end main-start] 1fr
    [main-end];
}
```

```text
[header-start]
      │
      ├──── 100px ────┤
[header-end main-start]
      │
      ├───── 1fr ─────┤
[main-end]
```

A linha do meio pode ser chamada de `header-end` ou de `main-start`: ela é, ao mesmo tempo, o fim do header e o início do conteúdo principal.

---

## 35. Nomes repetidos

Várias linhas podem ter o mesmo nome:

```css
.grid {
  display: grid;
  grid-template-rows: repeat(4, [row-start] 100px);
}
```

Para escolher uma ocorrência, informe o número depois do nome:

```css
.item {
  grid-row: row-start 2 / row-start 4;
}
```

```text
nome da linha
+
número da ocorrência
```

Aqui o item vai da 2ª linha chamada `row-start` (linha 2) até a 4ª (linha 4): 2 rows.

---

## 36. `grid-template-areas`

```css
.grid {
  display: grid;
  grid-template-areas:
    "header"
    "content"
    "footer";
}
```

```text
┌────────────────────┐
│       header       │
├────────────────────┤
│      content       │
├────────────────────┤
│       footer       │
└────────────────────┘
```

As áreas nomeadas podem ser usadas nas propriedades de posicionamento.

---

## 37. Áreas nomeadas geram linhas nomeadas

Uma área chamada `header` gera automaticamente as linhas nomeadas `header-start` e `header-end`:

```text
header-start
      ↓
┌────────────────────┐
│       HEADER       │
└────────────────────┘
      ↑
header-end
```

O mesmo vale para `content`, `footer`, `sidebar` e qualquer outro nome de área.

---

## 38. Utilizando uma área com `grid-row`

```css
.item {
  grid-row: content-start / content-end;
}
```

```text
começa na linha que delimita o início da área content
+
termina na linha que delimita o fim da área content
```

---

## 39. `grid-row: footer`

Quando `grid-row` recebe um único **nome** (e não um número), a linha final copia o mesmo nome:

```css
.item {
  grid-row: footer;
}
```

equivale a:

```css
.item {
  grid-row: footer / footer;
}
```

Se `footer` for uma área nomeada, o Grid resolve esses nomes para `footer-start` e `footer-end`. O item ocupa, portanto, as rows da área `footer`:

```css
.item {
  grid-row: footer-start / footer-end;
}
```

```text
número sozinho   → grid-row: 2       →  2 / auto
nome sozinho     → grid-row: footer   →  footer / footer
```

Se a intenção é posicionar o item na área completa, nos dois eixos, a forma mais direta é `grid-area: footer`.

---

## 40. `grid-area` e `grid-row`

```css
grid-area
```

pode definir uma área inteira. A ordem dos valores do shorthand é:

```text
grid-row-start
grid-column-start
grid-row-end
grid-column-end
```

Já `grid-row` controla somente `grid-row-start` e `grid-row-end`:

```text
grid-area
↓
rows + colunas

grid-row
↓
rows
```

---

## 41. Auto-placement

Nem todos os itens precisam de posição manual. Sem posicionamento explícito suficiente, o Grid usa o algoritmo de **auto-placement**.

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
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(2, 100px);
}
```

```text
┌─────┬─────┬─────┐
│  1  │  2  │  3  │
├─────┼─────┼─────┤
│  4  │  5  │  6  │
└─────┴─────┴─────┘
```

---

## 42. `grid-auto-flow`

O auto-placement é influenciado por `grid-auto-flow`. O valor inicial é:

```css
grid-auto-flow: row;
```

O algoritmo preenche o Grid seguindo as rows:

```text
→ → →
→ → →
→ → →
```

```text
primeira row
↓
segunda row
↓
terceira row
```

---

## 43. O que acontece quando movemos um item?

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
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 100px);
}

.item-1 {
  grid-row: 2;
}
```

O item 1 tem a row definida (2) e a coluna automática, então ocupa a primeira coluna livre da row 2. Os demais seguem o auto-placement a partir do início do Grid:

```text
┌─────┬─────┬─────┐
│  2  │  3  │  4  │
├─────┼─────┼─────┤
│  1  │     │     │
├─────┼─────┼─────┤
│     │     │     │
└─────┴─────┴─────┘
```

```text
alterar um item
       ↓
pode alterar a disposição dos demais
```

O Grid não move apenas aquele item: ele reaplica as regras de colocação ao conjunto.

---

## 44. Espaços vazios

Posicionamentos explícitos ou itens maiores podem deixar lacunas. Exemplo com 3 colunas, em que o item 3 ocupa duas colunas:

```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}

.item-3 {
  grid-column: span 2;
}
```

Os itens 1 e 2 ocupam as duas primeiras colunas da row 1. O item 3 precisa de duas colunas e não cabe na coluna restante, então vai para a row 2:

```text
┌───────┬───────┬───────┐
│   1   │   2   │       │  ← lacuna
├───────┴───────┼───────┤
│       3       │   4   │
└───────────────┴───────┘
```

A lacuna não é erro: é consequência do auto-placement.

---

## 45. `grid-auto-flow: dense`

```css
.grid {
  grid-auto-flow: dense;
}
```

Com `dense`, o algoritmo procura lacunas anteriores e as preenche com itens posteriores que couberem. No exemplo anterior, o item 4 volta e ocupa a lacuna:

```text
┌───────┬───────┬───────┐
│   1   │   2   │   4   │
├───────┴───────┼───────┤
│       3       │       │
└───────────────┴───────┘
```

> **⚠️ Atenção**
>
> Com `dense`, a ordem visual deixa de acompanhar a ordem do HTML (o item 4 aparece antes do 3). Isso pode atrapalhar a navegação por teclado e leitores de tela.

---

## 46. `dense` não ignora posicionamento explícito

```text
dense
↓
"o navegador pode ignorar minhas posições"   ← interpretação errada
```

O `dense` influencia apenas os itens que ainda precisam de auto-placement.

```text
posição explícita
      ↓
continua sendo respeitada

auto-placement
      ↓
pode procurar lacunas anteriores
```

---

## 47. Exemplos com três colunas

Todos os exemplos abaixo usam:

```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 100px);
}
```

### Item ocupando várias rows

```css
.item-1 {
  grid-row: 1 / 4;
}
```

```text
┌──────────┬──────────┬──────────┐
│  ITEM 1  │          │          │
│          │          │          │
├──────────┤          │          │
│  ITEM 1  │          │          │
│          │          │          │
├──────────┤          │          │
│  ITEM 1  │          │          │
│          │          │          │
└──────────┴──────────┴──────────┘
```

O item ocupa 3 rows. Na imagem acima, os demais itens (omitidos por clareza) ocupariam as outras colunas.

### Equivalente com `span`

```css
.item-1 {
  grid-row: 1 / span 3;
}
```

```text
comece na grid line 1
+
ocupe 3 rows
```

Neste exemplo, é equivalente a `grid-row: 1 / 4`.

### Iniciar depois e expandir

```css
.item-1 {
  grid-row: 2 / span 3;
}
```

```text
grid line 1
──────────────

grid line 2
     ┌───────────────┐
     │     ITEM      │
     │     ROW 2     │
     ├───────────────┤
     │     ROW 3     │
     ├───────────────┤
     │     ROW 4     │
     └───────────────┘
grid line 5
```

Como o grid explícito tem 3 rows, a row 4 é criada implicitamente.

### Ocupar toda a estrutura explícita

```css
.item {
  grid-row: 1 / -1;
}
```

```text
┌─────────────────────────┐
│                         │
│                         │
│          ITEM           │
│                         │
│                         │
└─────────────────────────┘
```

```text
primeira grid line
↓
todas as rows explícitas
↓
última grid line
```

---

## 48. Exemplo com `grid-template-rows`

```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: 100px 150px 200px;
}
```

```text
grid line 1
──────────────
ROW 1 (100px)
──────────────
grid line 2
──────────────
ROW 2 (150px)
──────────────
grid line 3
──────────────
ROW 3 (200px)
──────────────
grid line 4
```

```css
.item {
  grid-row: 1 / 4;
}
```

O item atravessa as três rows, independentemente de elas terem alturas diferentes (450px no total, mais os gaps, se existirem).

---

## 49. `grid-row` define posição e extensão

```css
grid-row: 2 / 5;
```

não diz apenas "coloque o item na row 2". Define:

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

O resultado é uma área composta por várias row tracks.

```text
2 / 5       → posição inicial + posição final
2 / span 3  → posição inicial + tamanho
```

---

## 50. Como ler qualquer `grid-row`

### Com números

```css
grid-row: 2 / 6;
```

```text
1. START = 2
2. END   = 6
3. 6 − 2 = 4
```

Resultado: 4 rows.

### Com `span`

```css
grid-row: 3 / span 4;
```

```text
START = 3
SPAN  = 4 rows
```

```text
3 → 4
4 → 5
5 → 6
6 → 7
```

Então `END = 7`.

### Uma imagem para lembrar

```text
────────────────────────  ← grid line 1
        ROW 1
────────────────────────  ← grid line 2
        ROW 2
────────────────────────  ← grid line 3
        ROW 3
────────────────────────  ← grid line 4
```

`grid-row: 1 / 3` aponta os **limites** da área (linha 1 e linha 3), e não as rows em si:

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

Ao ler `grid-row: 2 / 5`, não pense "da segunda até a quinta linha". Pense: começa na grid line 2, atravessa 3 row tracks e termina na grid line 5.

---

## 51. Modelo mental completo

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

## 52. Regra de ouro

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

```css
grid-row: 1 / 4;
```

```text
linha 1
↓
atravesse 3 rows
↓
linha 4
```

```css
grid-row: 2 / span 3;
```

```text
comece na linha 2
↓
ocupe 3 rows
↓
termine na linha 5
```

---

## 53. Diferença entre `grid-row` e `grid-column`

```text
grid-column
│
└── trabalha no eixo das colunas

grid-row
│
└── trabalha no eixo das rows
```

```css
.item {
  grid-column: 2 / 4;
  grid-row: 1 / 3;
}
```

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

## 54. Posicionamento bidimensional

```css
.card {
  grid-column: 2 / 4;
  grid-row: 2 / 5;
}
```

```text
horizontal:  2 → 4  =  2 colunas
vertical:    2 → 5  =  3 rows
```

```text
Área do item = 2 colunas × 3 rows
```

---

## 55. Exemplo de layout com `grid-row`

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

```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: 80px 1fr 1fr 80px;
}
```

Eixo das rows:

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

Eixo das colunas, separadamente:

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

As duas dimensões são controladas de forma independente.

---

## 56. Exemplo com linhas nomeadas

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

```css
.content {
  grid-row: content-start / content-end;
}
```

Compare:

```css
.item {
  grid-row: 2 / 3;
}
```

```css
.item {
  grid-row: content-start / content-end;
}
```

```text
números → posição numérica
nomes   → estrutura semântica
```

Em layouts simples, números bastam. Em layouts maiores, nomes facilitam a manutenção.

---

## 57. Exemplo com `grid-template-areas`

```css
.grid {
  display: grid;
  grid-template-rows: 100px 1fr 80px;
  grid-template-areas:
    "header"
    "content"
    "footer";
}
```

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

As áreas geram as linhas `header-start`, `header-end`, `content-start`, `content-end`, `footer-start` e `footer-end`, que podem ser usadas em `grid-row`:

```css
.item {
  grid-row: content-start / content-end;
}
```

```text
do início da área content
até
o fim da área content
```

Enquanto `grid-area: content` associa o item diretamente à área `content`, nos dois eixos.

---

## 58. Linha nomeada inexistente

```css
.grid {
  display: grid;
  grid-template-rows:
    [row1] 100px
    [row2] 100px
    [row3] 100px;
}

.item {
  grid-row: row4;
}
```

O nome `row4` não existe. O Grid não o interpreta como "a quarta linha". Pelas regras de resolução de nomes, quando a linha não é encontrada, as linhas **implícitas** são tratadas como se tivessem esse nome. O item vai parar além do grid explícito, o que cria rows implícitas.

Por isso, use sempre nomes que realmente foram definidos:

```text
NOME DEFINIDO
      ↓
NOME UTILIZADO
      ↓
devem corresponder
```

---

## 59. `auto` em `grid-row`

```css
grid-row: auto;
```

`auto` mantém essa parte da posição automática. Por isso:

```css
grid-row: 2;
```

equivale a:

```text
START = 2
END   = auto
```

---

## 60. Os principais formatos de `grid-row`

```css
grid-row: auto;
grid-row: 2;
grid-row: 2 / 5;
grid-row: 2 / -1;
grid-row: 2 / span 3;
grid-row: span 3;
grid-row: inicio / fim;
```

```text
grid-row: 2;

→ começa na grid line 2
→ fim automático (1 row)


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
→ posição inicial automática


grid-row: inicio / fim;

→ utiliza linhas nomeadas
```

---

## 61. Comparação rápida

| Declaração | Significado |
| --- | --- |
| `grid-row: 2` | começa na grid line 2 e a outra extremidade fica automática (1 row) |
| `grid-row: 1 / 3` | da grid line 1 até a 3 (2 rows) |
| `grid-row: 1 / 4` | ocupa 3 row tracks |
| `grid-row: 2 / 5` | ocupa 3 row tracks |
| `grid-row: 1 / -1` | da primeira até a última linha do grid explícito |
| `grid-row: 2 / span 3` | começa em 2 e ocupa 3 rows |
| `grid-row: span 2` | ocupa 2 rows |
| `grid-row: footer` | usa a área nomeada `footer` (de `footer-start` até `footer-end`) |
| `grid-row: inicio / fim` | utiliza linhas nomeadas |
| `grid-row-start: 2` | define a linha inicial |
| `grid-row-end: 4` | define a linha final |

---

## 62. Perguntas para testar a compreensão

### Quantas grid lines existem em um Grid com 3 rows?

```text
4 grid lines
```

```text
rows + 1 = grid lines
```

### O que existe entre duas grid lines?

```text
uma grid track
```

No eixo das rows: uma row track.

### O que faz `grid-row: 1 / 4`?

```text
linha 1 → linha 4
=
3 rows
```

### O que faz `grid-row: 2 / span 3`?

```text
começa na linha 2
+
ocupa 3 rows
```

### O que faz `grid-row: 1 / -1`?

```text
primeira grid line
até
última grid line do grid explícito
```

### Qual propriedade controla as rows implícitas?

```css
grid-auto-rows
```

### Qual propriedade controla o fluxo automático?

```css
grid-auto-flow
```

---

## 63. Erros conceituais que devemos evitar

### Erro 1 — pensar que row e grid line são a mesma coisa

```text
errado:   3 rows = 3 grid lines
correto:  3 rows = 4 grid lines
```

### Erro 2 — pensar que `1 / 4` significa quatro rows

```text
1 → 2
2 → 3
3 → 4

= 3 rows
```

### Erro 3 — pensar que `span 3` significa três grid lines

```text
span 3 = 3 tracks
```

No `grid-row`: 3 row tracks.

### Erro 4 — pensar que `grid-row` define a coluna

```text
grid-row    → eixo das rows
grid-column → eixo das colunas
```

### Erro 5 — pensar que `-1` é "qualquer última linha"

```text
-1 → última linha do grid explícito
```

Rows implícitas ficam fora dessa contagem.

### Erro 6 — pensar que `dense` muda posições explícitas

```text
dense → atua no auto-placement
```

### Erro 7 — pensar que `grid-row: footer` é só a linha inicial

```text
grid-row: footer → footer-start / footer-end
```

---

## 64. `grid-row` e acessibilidade

Mover visualmente um elemento com Grid não significa que a ordem do HTML possa ser ignorada. A estrutura HTML deve continuar fazendo sentido para:

```text
leitura
acessibilidade
tecnologias assistivas
navegação por teclado
manutenção
```

```text
HTML
↓
ordem semântica

CSS Grid
↓
organização visual
```

Não construa um HTML desordenado para "consertar" a ordem com `grid-row`.

---

## 65. Exemplo final completo

```html
<div class="layout">
  <header class="header">Header</header>
  <nav class="nav">Navegação</nav>
  <main class="content">Conteúdo</main>
  <aside class="aside">Aside</aside>
  <footer class="footer">Footer</footer>
</div>
```

```css
.layout {
  display: grid;
  grid-template-columns: 200px 1fr 200px;
  grid-template-rows: 80px 1fr 80px;
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

```text
                    COLUNAS
        1             2             3
        │             │             │
        ▼             ▼             ▼
┌───────────────────────────────────────────┐
│                  HEADER                   │
├────────────┬─────────────────┬────────────┤
│            │                 │            │
│    NAV     │     CONTENT     │   ASIDE    │
│            │                 │            │
├────────────┴─────────────────┴────────────┤
│                  FOOTER                   │
└───────────────────────────────────────────┘
```

```text
HEADER  → grid-row: 1
NAV     → grid-row: 2
CONTENT → grid-row: 2
ASIDE   → grid-row: 2
FOOTER  → grid-row: 3
```

---

## 66. Exemplo usando `span`

Para fazer o conteúdo ocupar duas rows, o Grid precisa de uma row a mais. O footer desce para a row 4, para não se sobrepor ao conteúdo:

```css
.layout {
  display: grid;
  grid-template-columns: 200px 1fr 200px;
  grid-template-rows: 80px 1fr 1fr 80px;
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
  grid-row: 2 / span 2;
}

.aside {
  grid-column: 3;
  grid-row: 2;
}

.footer {
  grid-column: 1 / -1;
  grid-row: 4;
}
```

```text
┌───────────┬───────────────┬───────────┐
│                 HEADER                │
├───────────┼───────────────┼───────────┤
│           │               │           │
│    NAV    │               │   ASIDE   │
│           │               │           │
├───────────┤    CONTENT    ├───────────┤
│           │               │           │
│  (vazio)  │               │  (vazio)  │
│           │               │           │
├───────────┴───────────────┴───────────┤
│                 FOOTER                │
└───────────────────────────────────────┘
```

O conteúdo ocupa as rows 2 e 3.

> **⚠️ Atenção**
>
> Se o footer permanecesse em `grid-row: 3`, ele se sobreporia ao conteúdo, já que ambos ocupariam a row 3 em colunas que se cruzam. Ao ampliar um item com `span`, confira sempre o que já ocupa as rows atravessadas.

---

## 67. Mapa mental final

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

## 68. Referência rápida

```css
/* Posicionamento automático */
grid-row: auto;

/* Começa na linha 2 (1 row) */
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

/* Área nomeada (footer-start / footer-end) */
grid-row: footer;

/* Longhands */
grid-row-start: 2;
grid-row-end: 4;
```

---

## 69. Tabela de fixação

| Conceito | O que significa | Exemplo |
| --- | --- | --- |
| `grid-row` | shorthand de `grid-row-start` e `grid-row-end` | `grid-row: 1 / 4` |
| `grid-row-start` | define a linha inicial | `grid-row-start: 2` |
| `grid-row-end` | define a linha final | `grid-row-end: 4` |
| `row` | row track no eixo das rows | `ROW 1` |
| `grid line` | linha estrutural que delimita tracks | `1`, `2`, `3`, `4` |
| `grid track` | espaço entre duas grid lines | `1 → 2` |
| `span` | quantidade de tracks | `span 3` |
| `-1` | última grid line do grid explícito | `1 / -1` |
| número positivo | conta linhas a partir do início | `2` |
| número negativo | conta linhas a partir da extremidade final | `-1` |
| linha nomeada | grid line identificada por um nome | `[content-start]` |
| `grid-template-rows` | define rows explícitas | `repeat(3, 100px)` |
| `grid-auto-rows` | define o tamanho das rows implícitas | `50px` |
| `grid-template-areas` | define áreas nomeadas | `"header"` |
| `grid-auto-flow` | controla o auto-placement | `row` |
| `dense` | permite ao auto-placement preencher lacunas anteriores | `dense` |

---

## 70. Checklist mental

Antes de escrever `grid-row: ...`, pergunte:

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
□ O item vai se sobrepor a outro?
```

---

## 71. O conteúdo essencial para guardar

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

14. grid-row: 2 → 2 / auto.
    grid-row: nome → nome / nome.

15. Rows implícitas podem ser criadas quando necessárias.

16. grid-auto-rows controla o dimensionamento das
    rows implícitas.

17. Itens não posicionados explicitamente são
    organizados pelo auto-placement.

18. grid-auto-flow controla esse comportamento.

19. dense permite ao auto-placement preencher
    lacunas anteriores, mas altera a ordem visual.
```

---

## 72. Referências técnicas

### MDN Web Docs

- [`grid-row`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-row)
- [`grid-row-start`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-row-start)
- [`grid-row-end`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-row-end)
- [`grid-template-rows`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-template-rows)
- [`grid-template-areas`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-template-areas)
- [Grid layout — Basic concepts](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout/Basic_concepts)
- [Grid layout — Line-based placement](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout/Line-based_placement)
- [Grid layout — Named grid lines](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout/Named_grid_lines)

### Especificação

- [CSS Grid Layout Module Level 2 — W3C](https://www.w3.org/TR/css-grid-2/)

---

## 73. GitHub

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