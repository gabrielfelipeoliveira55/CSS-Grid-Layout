# CSS Grid Layout — `justify-self`

> [!NOTE]
> Esta documentação apresenta o funcionamento da propriedade `justify-self`, com foco principal no **CSS Grid Layout**, relacionando o conceito com `justify-items`, `justify-content`, `align-self` e `place-self`.

## Índice

1. [Objetivo](#1-objetivo)
2. [Antes de entender `justify-self`](#2-antes-de-entender-justify-self)
3. [O que significa `self`?](#3-o-que-significa-self)
4. [`justify-items` × `justify-self`](#4-justify-items--justify-self)
5. [Onde `justify-self` atua?](#5-onde-justify-self-atua)
6. [O que exatamente é alinhado?](#6-o-que-exatamente-é-alinhado)
7. [Sintaxe](#7-sintaxe)
8. [`justify-self: start`](#8-justify-self-start)
9. [`justify-self: center`](#9-justify-self-center)
10. [`justify-self: end`](#10-justify-self-end)
11. [Visualizando os três principais valores](#11-visualizando-os-três-principais-valores)
12. [`justify-self: stretch`](#12-justify-self-stretch)
13. [Por que `stretch` parece ser o padrão?](#13-por-que-stretch-parece-ser-o-padrão)
14. [`justify-self: auto`](#14-justify-self-auto)
15. [`justify-self: normal`](#15-justify-self-normal)
16. [`start` e `self-start`](#16-start-e-self-start)
17. [`self-end`](#17-self-end)
18. [`left` e `right`](#18-left-e-right)
19. [`baseline`](#19-baseline)
20. [`safe` e `unsafe`](#20-safe-e-unsafe)
21. [Exemplo completo](#21-exemplo-completo)
22. [`justify-self` não altera o posicionamento do Grid](#22-justify-self-não-altera-o-posicionamento-do-grid)
23. [`justify-self` × `justify-items` × `justify-content`](#23-justify-self--justify-items--justify-content)
24. [Exemplo para diferenciar `justify-content`](#24-exemplo-para-diferenciar-justify-content)
25. [`justify-self` × `align-self`](#25-justify-self--align-self)
26. [`place-self`](#26-place-self)
27. [Um detalhe importante sobre `stretch`](#27-um-detalhe-importante-sobre-stretch)
28. [Quando `stretch` não funciona como esperado?](#28-quando-stretch-não-funciona-como-esperado)
29. [Exemplo prático: botão dentro de uma célula](#29-exemplo-prático-botão-dentro-de-uma-célula)
30. [Exemplo com imagem](#30-exemplo-com-imagem)
31. [Relação com `grid-area`](#31-relação-com-grid-area)
32. [Relação com `span`](#32-relação-com-span)
33. [Erro comum: pensar que `justify-self` move a coluna](#33-erro-comum-pensar-que-justify-self-move-a-coluna)
34. [Erro comum: confundir com `text-align`](#34-erro-comum-confundir-com-text-align)
35. [Erro comum: confundir `justify-items` com `justify-self`](#35-erro-comum-confundir-justify-items-com-justify-self)
36. [Erro comum: pensar somente em "horizontal"](#36-erro-comum-pensar-somente-em-horizontal)
37. [Fluxo mental definitivo](#37-fluxo-mental-definitivo)
38. [Mapa mental](#38-mapa-mental)
39. [Regra de ouro](#39-regra-de-ouro)
40. [Tabela de fixação](#40-tabela-de-fixação)
41. [Tabela de comparação das propriedades](#41-tabela-de-comparação-das-propriedades)
42. [Código de referência](#42-código-de-referência)
43. [Perguntas para verificar a compreensão](#43-perguntas-para-verificar-a-compreensão)
44. [Resumo técnico](#44-resumo-técnico)
45. [Referências oficiais](#45-referências-oficiais)
46. [GitHub](#46-github)

---

## 1. Objetivo

A propriedade `justify-self` controla **como um único item é alinhado dentro do espaço que foi reservado para ele**.

No CSS Grid, esse espaço é a **grid area** ocupada pelo item.

```text
GRID CONTAINER
└── GRID AREA DO ITEM
    └── ITEM
```

O `justify-self` decide **onde o item ficará dentro dessa área, no eixo inline**.

Em uma escrita horizontal comum, como a do português:

```text
Eixo inline
←────────────────────────────→

┌──────────────────────────────┐
│          GRID AREA           │
│                              │
│      ┌──────────────┐        │
│      │     ITEM     │        │
│      └──────────────┘        │
│                              │
└──────────────────────────────┘
```

> `justify-self` não move a célula do Grid. Ele posiciona o **item dentro da área que esse item já ocupa**.

A especificação CSS Box Alignment define `justify-self` como uma propriedade de **self-alignment**: o alinhamento da própria caixa dentro do seu container de alinhamento.

---

## 2. Antes de entender `justify-self`

Para compreender essa propriedade, é preciso separar quatro conceitos.

```text
┌─────────────────────────────────────────────┐
│              GRID CONTAINER                 │
│                                             │
│   ┌─────────────────────────────────────┐   │
│   │             GRID AREA               │   │
│   │                                     │   │
│   │           ┌─────────┐               │   │
│   │           │  ITEM   │               │   │
│   │           └─────────┘               │   │
│   │                                     │   │
│   └─────────────────────────────────────┘   │
│                                             │
└─────────────────────────────────────────────┘
```

### Grid Container

É o elemento que possui `display: grid`. Ele cria o contexto de Grid.

```css
.container {
  display: grid;
}
```

### Grid Track

É uma coluna ou uma linha criada pelo Grid.

```text
      coluna 1        coluna 2
         ↓               ↓

      ┌───────┬───────────┐
linha │       │           │
  1   │       │           │
      ├───────┼───────────┤
linha │       │           │
  2   │       │           │
      └───────┴───────────┘
```

Uma coluna é um **column track**. Uma linha é um **row track**.

### Grid Area

É a região formada por uma ou mais linhas e colunas ocupada por um item.

```css
.item {
  grid-column: 1 / 3;
}
```

Nesse caso, o item ocupa duas colunas.

```text
┌───────────────┬───────────────┐
│                               │
│      ÁREA DO ITEM (2 colunas) │
│                               │
└───────────────┴───────────────┘
```

A área disponível pode ser maior do que o conteúdo do item precisa. É nesse espaço que `justify-self` atua.

### Grid Item

É o elemento filho direto do Grid Container.

```html
<div class="container">
  <div class="item"></div>
</div>
```

Como `.item` é filho direto do elemento com `display: grid`, ele é um grid item.

---

## 3. O que significa `self`?

```text
justify → alinhar no eixo inline
self    → o próprio item
```

```text
justify-items → define uma regra para os itens

justify-self  → define a regra para um item específico
```

Essa diferença é essencial.

---

## 4. `justify-items` × `justify-self`

### `justify-items`

É definido no **Grid Container** e estabelece a regra padrão de alinhamento para os itens.

```css
.container {
  display: grid;
  justify-items: center;
}
```

### `justify-self`

É definido diretamente no **Grid Item**. Só esse item passa a ter uma regra própria.

```css
.item {
  justify-self: end;
}
```

A MDN descreve `justify-items` como a propriedade que define o comportamento padrão de `justify-self` para os itens, enquanto `justify-self` altera o alinhamento de um item individualmente.

Com `justify-items: center` e duas colunas:

```text
┌──────────────────┬──────────────────┐
│    ┌────────┐    │    ┌────────┐    │
│    │ ITEM 1 │    │    │ ITEM 2 │    │
│    └────────┘    │    └────────┘    │
└──────────────────┴──────────────────┘
       center              center
```

Se o segundo item receber:

```css
.item-2 {
  justify-self: end;
}
```

o item continua **na mesma célula e na mesma linha**. Só muda a posição horizontal dentro dela:

```text
┌──────────────────┬──────────────────┐
│    ┌────────┐    │        ┌────────┐│
│    │ ITEM 1 │    │        │ ITEM 2 ││
│    └────────┘    │        └────────┘│
└──────────────────┴──────────────────┘
       center                end
```

> `justify-items` define o padrão para todos. `justify-self` permite que um item tenha o seu próprio alinhamento.

---

## 5. Onde `justify-self` atua?

No Grid, `justify-self` atua no **eixo inline**. Em um documento com `writing-mode` horizontal tradicional:

```text
EIXO INLINE
←──────────────────────────────→

EIXO BLOCK
↑
│
↓
```

```text
justify-self → direção horizontal
align-self   → direção vertical
```

Mas `justify-self` é definido em termos do **eixo inline**, e não simplesmente como "horizontal". O comportamento depende do `writing-mode`. A forma tecnicamente correta de pensar é:

```text
justify-self → eixo inline
align-self   → eixo block
```

---

## 6. O que exatamente é alinhado?

`justify-self` não alinha o texto. Ele alinha a **caixa do elemento** dentro do seu espaço de alinhamento.

```text
GRID AREA

┌──────────────────────────────────┐
│                                  │
│        ┌──────────────┐          │
│        │     ITEM     │          │
│        └──────────────┘          │
│                                  │
└──────────────────────────────────┘
```

O Grid possui uma área disponível. O elemento possui a sua própria caixa. `justify-self` decide onde essa caixa ficará.

Isso é diferente de:

```css
text-align: center;
```

```text
justify-self → alinha a CAIXA do elemento
text-align   → alinha o CONTEÚDO TEXTUAL dentro da caixa
```

```css
.item {
  justify-self: center;
  text-align: center;
}
```

```text
GRID AREA
┌─────────────────────────────────────┐
│                                     │
│             ┌───────────┐           │
│             │   ITEM    │           │
│             │   texto   │           │
│             └───────────┘           │
│                                     │
└─────────────────────────────────────┘

justify-self
       ↓
move a caixa do ITEM

text-align
       ↓
alinha o texto dentro da caixa
```

---

## 7. Sintaxe

```css
.item {
  justify-self: valor;
}
```

```css
.item {
  justify-self: center;
}
```

Os principais valores para aprender primeiro:

```css
justify-self: auto;
justify-self: normal;
justify-self: stretch;

justify-self: start;
justify-self: center;
justify-self: end;
```

Também existem:

```css
justify-self: self-start;
justify-self: self-end;
justify-self: left;
justify-self: right;
justify-self: baseline;
justify-self: first baseline;
justify-self: last baseline;
justify-self: safe center;
justify-self: unsafe center;
```

A sintaxe atual da especificação também inclui `anchor-center`, relacionado ao posicionamento por âncora. Nem todos os valores mais recentes têm o mesmo suporte nos navegadores.

---

## 8. `justify-self: start`

```css
.item {
  justify-self: start;
}
```

Coloca o item junto ao início do eixo inline. Em uma escrita horizontal da esquerda para a direita:

```text
INÍCIO →                                     FIM

┌──────────────────────────────────────────────┐
│ ┌──────────┐                                 │
│ │   ITEM   │                                 │
│ └──────────┘                                 │
└──────────────────────────────────────────────┘
```

```text
start = começo
```

---

## 9. `justify-self: center`

```css
.item {
  justify-self: center;
}
```

Centraliza o item dentro da sua área de alinhamento.

```text
┌──────────────────────────────────────────────┐
│                                              │
│              ┌──────────┐                    │
│              │   ITEM   │                    │
│              └──────────┘                    │
│                                              │
└──────────────────────────────────────────────┘
```

```text
center = meio
```

---

## 10. `justify-self: end`

```css
.item {
  justify-self: end;
}
```

Coloca o item junto ao final do eixo inline.

```text
INÍCIO                                     FIM
                                            ↓

┌──────────────────────────────────────────────┐
│                                 ┌──────────┐ │
│                                 │   ITEM   │ │
│                                 └──────────┘ │
└──────────────────────────────────────────────┘
```

```text
end = final
```

---

## 11. Visualizando os três principais valores

```text
start

┌──────────────────────────────────┐
│ ┌───────┐                        │
│ │ ITEM  │                        │
│ └───────┘                        │
└──────────────────────────────────┘
```

```text
center

┌──────────────────────────────────┐
│            ┌───────┐             │
│            │ ITEM  │             │
│            └───────┘             │
└──────────────────────────────────┘
```

```text
end

┌──────────────────────────────────┐
│                        ┌───────┐ │
│                        │ ITEM  │ │
│                        └───────┘ │
└──────────────────────────────────┘
```

A área não mudou. O item não mudou de célula. Apenas a posição **dentro da área** mudou.

---

## 12. `justify-self: stretch`

Esse valor tem uma diferença fundamental em relação aos valores de posicionamento.

```css
.item {
  justify-self: stretch;
}
```

O item pode ocupar o espaço disponível no eixo de alinhamento.

```text
stretch

┌─────────────────────────────────────┐
│ ┌─────────────────────────────────┐ │
│ │              ITEM               │ │
│ └─────────────────────────────────┘ │
└─────────────────────────────────────┘
```

Compare com:

```text
start

┌─────────────────────────────────────┐
│ ┌─────────┐                         │
│ │  ITEM   │                         │
│ └─────────┘                         │
└─────────────────────────────────────┘
```

```text
center

┌─────────────────────────────────────┐
│          ┌─────────┐                │
│          │  ITEM   │                │
│          └─────────┘                │
└─────────────────────────────────────┘
```

```text
end

┌─────────────────────────────────────┐
│                         ┌─────────┐ │
│                         │  ITEM   │ │
│                         └─────────┘ │
└─────────────────────────────────────┘
```

A especificação explica `stretch` como o comportamento que aumenta o tamanho de itens dimensionados como `auto` para preencher o espaço disponível, respeitando restrições como `min-width` e `max-width`.

---

## 13. Por que `stretch` parece ser o padrão?

O valor inicial de `justify-items` é `normal`, que no Grid se comporta como `stretch`. O valor inicial de `justify-self` é `auto`, que usa o `justify-items` do elemento pai.

```text
GRID CONTAINER
    │
    │ justify-items: normal  (comporta-se como stretch)
    ↓
GRID ITEM
    │
    │ justify-self: auto
    ↓
usa o padrão do pai
    ↓
stretch
```

Por isso é comum ver os itens ocupando todo o espaço disponível na célula.

---

## 14. `justify-self: auto`

```css
.item {
  justify-self: auto;
}
```

`auto` não significa "centralizar", "esticar" ou "não fazer nada". Significa:

> use o valor de `justify-items` do elemento pai.

```css
.container {
  display: grid;
  justify-items: center;
}
```

```css
.item {
  justify-self: auto;
}
```

```text
justify-items: center
          ↓
      ITEM 1 → center
      ITEM 2 → center
      ITEM 3 → center
```

Se um item receber uma regra própria:

```css
.item-2 {
  justify-self: end;
}
```

```text
ITEM 1 → center
ITEM 2 → end
ITEM 3 → center
```

```text
justify-items = regra padrão
justify-self  = regra específica
```

---

## 15. `justify-self: normal`

```css
.item {
  justify-self: normal;
}
```

O significado de `normal` depende do modelo de layout.

No Grid, `normal` se comporta como `stretch`, com uma exceção: em caixas com **proporção intrínseca** (aspect ratio), como imagens, ele se comporta como `start`.

```html
<div class="container">
  <img src="imagem.jpg" alt="">
</div>
```

Uma imagem tem uma proporção natural, por exemplo:

```text
largura : altura
   16   :   9
```

Esticá-la sem respeitar essa proporção produziria distorção.

---

## 16. `start` e `self-start`

À primeira vista:

```css
justify-self: start;
```

```css
justify-self: self-start;
```

parecem iguais. Mas há uma diferença conceitual.

```text
start
→ lado inicial do container de alinhamento,
  segundo o modo de escrita do container

self-start
→ lado do container correspondente ao início do próprio item,
  segundo o modo de escrita do item
```

Em situações simples, ambos produzem o mesmo resultado:

```text
┌────────────────────────────┐
│ ITEM                       │
└────────────────────────────┘
```

A diferença aparece quando o item e o container têm **writing modes** ou direções de escrita diferentes.

---

## 17. `self-end`

```css
.item {
  justify-self: self-end;
}
```

Alinha o item ao lado do container correspondente ao fim do próprio item. Em layouts simples:

```text
┌──────────────────────────────────┐
│                         ┌──────┐ │
│                         │ITEM  │ │
│                         └──────┘ │
└──────────────────────────────────┘
```

Ele fica mais interessante com diferentes sistemas de escrita.

---

## 18. `left` e `right`

```css
justify-self: left;
justify-self: right;
```

Esses valores são **físicos**:

```text
left  → esquerda física
right → direita física
```

Isso é diferente de `start` e `end`, que são valores **lógicos**.

```text
left
┌────────────────────────────────┐
│ ITEM                           │
└────────────────────────────────┘
```

```text
right
┌────────────────────────────────┐
│                           ITEM │
└────────────────────────────────┘
```

Em interfaces modernas, `start` e `end` costumam ser mais adequados, porque respeitam diferentes direções e modos de escrita.

---

## 19. `baseline`

```css
justify-self: baseline;
justify-self: first baseline;
justify-self: last baseline;
```

Esse alinhamento é mais avançado. Em vez de colocar a caixa no início, meio ou fim, o navegador considera a **baseline**, a linha de base tipográfica.

```text
ITEM 1          ITEM 2

Texto grande    Texto pequeno
      ────────────────
          baseline
```

A especificação CSS Box Alignment define `baseline`, `first baseline` e `last baseline` como formas de alinhamento por linha de base.

---

## 20. `safe` e `unsafe`

Alguns valores posicionais podem ser combinados com `safe` ou `unsafe`:

```css
.item {
  justify-self: safe center;
}
```

### `safe`

Se o alinhamento escolhido fizer o item ultrapassar o container de alinhamento, o navegador usa um alinhamento equivalente a `start`, para que o conteúdo não fique inacessível.

```text
SAFE

┌──────────────────────────────┐
│ ITEM██████████████           │
└──────────────────────────────┘

o item ultrapassaria o espaço,
então fica alinhado ao início
```

### `unsafe`

O alinhamento solicitado é mantido, mesmo que haja overflow.

```text
UNSAFE

   ┌──────────────────────────────┐
 ██│█████████ ITEM ██████████████│██
   └──────────────────────────────┘

o item continua centralizado e
ultrapassa o espaço nos dois lados
```

---

## 21. Exemplo completo

```html
<div class="container">
  <div class="item item-1">1</div>
  <div class="item item-2">2</div>
  <div class="item item-3">3</div>
  <div class="item item-4">4</div>
</div>
```

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}

.item {
  padding: 20px;
}

.item-1 {
  justify-self: start;
}

.item-2 {
  justify-self: center;
}

.item-3 {
  justify-self: end;
}

.item-4 {
  justify-self: stretch;
}
```

```text
┌─────────────────────┐   ┌─────────────────────┐
│ ┌──────┐            │   │      ┌──────┐       │
│ │  1   │            │   │      │  2   │       │
│ └──────┘            │   │      └──────┘       │
└─────────────────────┘   └─────────────────────┘
        start                    center

┌─────────────────────┐   ┌─────────────────────┐
│            ┌──────┐ │   │ ┌───────────────────┐│
│            │  3   │ │   │ │         4         ││
│            └──────┘ │   │ └───────────────────┘│
└─────────────────────┘   └─────────────────────┘
         end                    stretch
```

Cada item continua na sua própria célula. O que muda é a posição ou o tamanho da caixa do item dentro da respectiva área.

---

## 22. `justify-self` não altera o posicionamento do Grid

Não confunda `justify-self` com `grid-column`, `grid-row` e `grid-area`.

```text
grid-column → onde o item é colocado nas colunas
grid-row    → onde o item é colocado nas rows
grid-area   → a região ocupada pelo item
justify-self → como o item é alinhado dentro dessa área
```

```text
1. Primeiro descubra ONDE o item está.

grid-column
grid-row
grid-area
       ↓
┌───────────────────────┐
│      GRID AREA        │
└───────────────────────┘

2. Depois descubra COMO o item ficará dentro dela.

justify-self
       ↓
┌───────────────────────┐
│    ┌────────────┐     │
│    │    ITEM    │     │
│    └────────────┘     │
└───────────────────────┘
```

---

## 23. `justify-self` × `justify-items` × `justify-content`

Essas três propriedades têm nomes parecidos, mas trabalham em níveis diferentes.

| Propriedade | Definida em | Atua sobre | Ideia principal |
| --- | --- | --- | --- |
| `justify-content` | Grid Container | o conjunto das tracks do Grid | distribui o Grid dentro do container |
| `justify-items` | Grid Container | todos os Grid Items | define o alinhamento padrão dos itens |
| `justify-self` | Grid Item | um único Grid Item | define o alinhamento individual do item |

```text
GRID CONTAINER
│
├── justify-content
│       ↓
│   posiciona/distribui
│   o conjunto do Grid
│
├── justify-items
│       ↓
│   define o padrão
│   para os itens
│
└── GRID ITEM
        │
        └── justify-self
                ↓
           altera somente
           este item
```

---

## 24. Exemplo para diferenciar `justify-content`

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px;
  width: 400px;
}
```

O Grid ocupa:

```text
100px + 100px = 200px
```

mas o container tem `400px`. Sobra espaço.

`justify-content` decide onde o **conjunto das colunas** ficará. `justify-self` trabalha com o item dentro da área que já foi criada.

```text
justify-content

┌─────────────────────────────────────────┐
│      [ COLUNA ][ COLUNA ]               │
└─────────────────────────────────────────┘
         ↑
      conjunto
      do Grid
```

```text
justify-self

┌──────────────────────┐
│   ┌──────────────┐   │
│   │     ITEM     │   │
│   └──────────────┘   │
└──────────────────────┘
          ↑
       item individual
```

---

## 25. `justify-self` × `align-self`

As duas propriedades usam o conceito de **self-alignment**. A diferença está no eixo:

```text
justify-self → eixo inline → horizontal (em escrita horizontal)

align-self   → eixo block  → vertical   (em escrita horizontal)
```

```css
.item {
  justify-self: center;
  align-self: center;
}
```

```text
┌──────────────────────────────────┐
│                                  │
│                                  │
│            ┌────────┐            │
│            │  ITEM  │            │
│            └────────┘            │
│                                  │
│                                  │
└──────────────────────────────────┘
```

---

## 26. `place-self`

`place-self` é a forma abreviada de `align-self` e `justify-self`:

```css
.item {
  place-self: center;
}
```

Com um único valor, ele vale para os dois eixos, e o exemplo acima equivale a:

```css
.item {
  align-self: center;
  justify-self: center;
}
```

Com dois valores:

```css
.item {
  place-self: start end;
}
```

```text
place-self: <align-self> <justify-self>
```

Aqui, `align-self: start` e `justify-self: end`.

---

## 27. Um detalhe importante sobre `stretch`

`stretch` não significa "faça o elemento ficar com `width: 100%`". São mecanismos diferentes.

```text
justify-self: stretch → atua no sistema de alinhamento,
                        respeitando as restrições de tamanho do item

width: 100%           → define uma largura explícita
```

O `stretch` usa o espaço livre no eixo de alinhamento **quando o tamanho do item permite**.

---

## 28. Quando `stretch` não funciona como esperado?

### Largura definida

```css
.item {
  width: 100px;
  justify-self: stretch;
}
```

Com `width` definido (diferente de `auto`), o item mantém `100px` e não é esticado. Como o `stretch` não atua, ele é posicionado como `start`.

### Limite com `max-width`

```css
.item {
  max-width: 200px;
  justify-self: stretch;
}
```

Com largura `auto`, o item estica, mas só até `200px`. O espaço que sobra fica livre, e o item é posicionado como `start`.

```text
┌──────────────────────────────────────────┐
│ ┌────────────────────┐                   │
│ │  ITEM (max 200px)  │                   │
│ └────────────────────┘                   │
└──────────────────────────────────────────┘
```

---

## 29. Exemplo prático: botão dentro de uma célula

```html
<div class="card">
  <h2>Título</h2>
  <p>Descrição</p>
  <button>Comprar</button>
</div>
```

```css
.card {
  display: grid;
}

button {
  justify-self: end;
}
```

```text
┌────────────────────────────────┐
│ Título                         │
│                                │
│ Descrição                      │
│                                │
│                     ┌────────┐ │
│                     │Comprar │ │
│                     └────────┘ │
└────────────────────────────────┘
```

O botão continua na mesma área do Grid, mas a sua caixa é alinhada ao final do eixo inline.

---

## 30. Exemplo com imagem

```css
img {
  justify-self: center;
}
```

```text
┌─────────────────────────────────────┐
│                                     │
│           ┌────────────┐            │
│           │    IMG     │            │
│           └────────────┘            │
│                                     │
└─────────────────────────────────────┘
```

Isso posiciona a imagem dentro da área sem modificar a estrutura das colunas. Como a imagem tem proporção intrínseca, o comportamento `normal` não a estica, e isso evita distorções.

---

## 31. Relação com `grid-area`

```css
.item {
  grid-area: 1 / 1 / 3 / 3;
}
```

```text
grid-area
    ↓
define a área ocupada
```

```css
.item {
  justify-self: center;
}
```

```text
┌───────────────────────────────┐
│                               │
│           GRID AREA           │
│                               │
│        ┌──────────┐           │
│        │   ITEM   │           │
│        └──────────┘           │
│                               │
└───────────────────────────────┘
             ↑
       justify-self
```

Primeiro: **onde o item está?** Depois: **como ele fica dentro daquele espaço?**

---

## 32. Relação com `span`

```css
.item {
  grid-column: span 2;
}
```

```text
┌───────────────┬───────────────┐
│                               │
│             ITEM              │
│                               │
└───────────────┴───────────────┘
```

```css
.item {
  justify-self: center;
}
```

O item é centralizado dentro dessa área maior.

```text
span
 ↓
aumenta a área ocupada

justify-self
 ↓
alinha o item dentro dessa área
```

---

## 33. Erro comum: pensar que `justify-self` move a coluna

```css
.item {
  justify-self: end;
}
```

Isso **não** significa "mova a coluna para a direita". Significa "alinhe este item no final do espaço que ele possui".

```text
Antes:

┌───────────────┐
│    ITEM       │
└───────────────┘

Depois:

┌───────────────┐
│          ITEM │
└───────────────┘

mesma área
item em posição diferente
```

---

## 34. Erro comum: confundir com `text-align`

```css
.item {
  justify-self: center;
}
```

não significa "centralizar o texto". Significa "centralizar a caixa do item".

Para o texto:

```css
.item {
  text-align: center;
}
```

É possível usar os dois:

```css
.item {
  justify-self: center;
  text-align: center;
}
```

```text
justify-self → posição da caixa
text-align   → posição do texto dentro da caixa
```

---

## 35. Erro comum: confundir `justify-items` com `justify-self`

```css
.container {
  justify-items: center;
}
```

Afeta o padrão de todos os itens.

```css
.item {
  justify-self: end;
}
```

Afeta apenas aquele item.

```text
justify-items
      ↓
"Como os itens devem se alinhar?"

justify-self
      ↓
"E este item especificamente?"
```

---

## 36. Erro comum: pensar somente em "horizontal"

É comum decorar:

```text
justify = horizontal
align   = vertical
```

Isso funciona como simplificação inicial em layouts horizontais. A definição técnica é:

```text
justify-self → eixo inline
align-self   → eixo block
```

Essa forma é mais correta porque respeita diferentes `writing-mode` e direções de escrita.

---

## 37. Fluxo mental definitivo

Ao encontrar `justify-self: center`, não pense apenas "centraliza". Pense nesta sequência:

```text
1. Tenho um Grid Container
            ↓
2. Tenho um Grid Item
            ↓
3. O item recebeu uma Grid Area
            ↓
4. Existe espaço disponível dentro dessa área
            ↓
5. justify-self controla o alinhamento da caixa
            ↓
6. O alinhamento acontece no eixo inline
            ↓
7. center coloca o item no centro desse espaço
```

---

## 38. Mapa mental

```text
                    CSS GRID
                       │
                       ▼
                GRID CONTAINER
                       │
                       ▼
                   GRID ITEM
                       │
                       ▼
                   GRID AREA
                       │
        ┌──────────────┴──────────────┐
        │                             │
        ▼                             ▼
   EIXO INLINE                   EIXO BLOCK
        │                             │
        ▼                             ▼
  justify-self                   align-self
        │                             │
  ┌─────┼─────┐                 ┌─────┼─────┐
  ▼     ▼     ▼                 ▼     ▼     ▼
start center end              start center end
        │
        ▼
 posiciona o ITEM
 dentro da GRID AREA
```

---

## 39. Regra de ouro

> **`justify-self` não decide onde a Grid Area fica. Ele decide onde o item fica dentro da Grid Area, no eixo inline.**

```text
grid-column
grid-row
grid-area
      ↓
ONDE O ITEM ESTÁ

justify-self
      ↓
COMO O ITEM FICA DENTRO DESSE ESPAÇO
```

---

## 40. Tabela de fixação

| Valor | O que faz | Mentalidade |
| --- | --- | --- |
| `auto` | usa o `justify-items` do pai | "siga o padrão" |
| `normal` | comportamento padrão do modelo de layout (no Grid: `stretch`, ou `start` com aspect ratio) | "comportamento normal" |
| `start` | alinha no início do eixo | "começo" |
| `center` | centraliza o item | "meio" |
| `end` | alinha no final do eixo | "fim" |
| `stretch` | preenche o espaço quando o tamanho é `auto` | "preencher" |
| `self-start` | alinha no início do próprio item | "início do próprio item" |
| `self-end` | alinha no fim do próprio item | "fim do próprio item" |
| `left` | alinha à esquerda física | "esquerda" |
| `right` | alinha à direita física | "direita" |
| `baseline` | alinha pela linha de base | "alinhar pela baseline" |
| `safe center` | centraliza, evitando overflow quando aplicável | "centralizar com segurança" |
| `unsafe center` | mantém o alinhamento mesmo com overflow | "forçar o alinhamento" |

---

## 41. Tabela de comparação das propriedades

| Propriedade | Aplicada em | Principal função |
| --- | --- | --- |
| `justify-content` | Grid Container | posicionar e distribuir as tracks do Grid no eixo inline |
| `align-content` | Grid Container | posicionar e distribuir as tracks do Grid no eixo block |
| `justify-items` | Grid Container | definir o alinhamento padrão dos itens no eixo inline |
| `align-items` | Grid Container | definir o alinhamento padrão dos itens no eixo block |
| `justify-self` | Grid Item | alinhar um item individual no eixo inline |
| `align-self` | Grid Item | alinhar um item individual no eixo block |
| `place-self` | Grid Item | shorthand de `align-self` + `justify-self` |

---

## 42. Código de referência

```css
.container {
  display: grid;
  grid-template-columns: repeat(2, 1fr);

  /* Regra padrão para os itens */
  justify-items: stretch;
}

.item-start {
  justify-self: start;
}

.item-center {
  justify-self: center;
}

.item-end {
  justify-self: end;
}

.item-stretch {
  justify-self: stretch;
}

.item-auto {
  justify-self: auto;
}
```

---

## 43. Perguntas para verificar a compreensão

1. O que significa `self` em `justify-self`?
2. `justify-self` é aplicado ao Grid Container ou ao Grid Item?
3. Qual é a diferença entre `justify-items` e `justify-self`?
4. `justify-self` posiciona o item em relação a quê?
5. Qual eixo é controlado por `justify-self`?
6. Qual a diferença entre `justify-self` e `text-align`?
7. O que faz `justify-self: start`?
8. O que faz `justify-self: center`?
9. O que faz `justify-self: end`?
10. O que diferencia `stretch` dos valores posicionais?
11. O que significa `justify-self: auto`?
12. Qual a diferença conceitual entre `grid-area` e `justify-self`?

Se essas perguntas estiverem claras, o conceito central da propriedade está consolidado.

---

## 44. Resumo técnico

```text
justify-self
│
├── propriedade de alinhamento individual
│
├── aplicada ao Grid Item
│
├── trabalha no eixo inline
│
├── alinha a caixa do item
│
├── não altera a posição da Grid Area
│
├── não é equivalente a text-align
│
├── pode sobrescrever o padrão de justify-items
│
└── principais valores
      ├── auto
      ├── normal
      ├── start
      ├── center
      ├── end
      └── stretch
```

---

## 45. Referências oficiais

- **MDN — `justify-self`:** definição, sintaxe, valores, exemplos e compatibilidade.
- **MDN — `justify-items`:** relação entre `justify-items` e `justify-self`.
- **MDN — Box alignment in grid layout:** diferença entre `*-items` e `*-self` no CSS Grid.
- **MDN — `align-self`:** propriedade correspondente para o outro eixo.
- **MDN — `place-self`:** shorthand de `align-self` e `justify-self`.
- **W3C — CSS Box Alignment Module Level 3:** especificação oficial do sistema de alinhamento.

---

## 46. GitHub

<div align="center">

### CSS Grid Layout — `justify-self`

Documentação técnica para estudo contínuo de CSS Grid Layout.

<br>

<a href="https://github.com/gabrielfelipeoliveira55" target="_blank" rel="noopener noreferrer">
Gabriel Felipe de Oliveira Rateiro
</a>

<br><br>

**CSS Grid Layout • `justify-self`**

> Conteúdo do Grid → conjunto de itens → item individual:
>
> `justify-content` → `justify-items` → `justify-self`

</div>