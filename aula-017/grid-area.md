# CSS Grid Layout — `grid-area`

> **Objetivo:** compreender `grid-area` como shorthand de posicionamento de um grid item, entender a ordem dos quatro valores, trabalhar com `grid-template-areas`, nomes de áreas, linhas nomeadas, expansão, sobreposição de itens e a relação com `z-index`.

## Índice

1. [O que é grid-area](#1-o-que-é-grid-area)
2. [A ordem dos quatro valores](#2-a-ordem-dos-quatro-valores)
3. [Por que a ordem parece estranha](#3-por-que-a-ordem-parece-estranha)
4. [grid-area usa grid lines](#4-grid-area-usa-grid-lines)
5. [Exemplo simples](#5-exemplo-simples)
6. [Equivalência com as propriedades individuais](#6-equivalência-com-as-propriedades-individuais)
7. [grid-area controla os dois eixos](#7-grid-area-controla-os-dois-eixos)
8. [Valores omitidos e auto](#8-valores-omitidos-e-auto)
9. [Calculando o tamanho da área](#9-calculando-o-tamanho-da-área)
10. [span dentro de grid-area](#10-span-dentro-de-grid-area)
11. [grid-area com área nomeada](#11-grid-area-com-área-nomeada)
12. [grid-template-areas](#12-grid-template-areas)
13. [Associando um item à área](#13-associando-um-item-à-área)
14. [Nomes ou números](#14-nomes-ou-números)
15. [grid-template-areas funciona como um desenho](#15-grid-template-areas-funciona-como-um-desenho)
16. [O nome da área não cria um item](#16-o-nome-da-área-não-cria-um-item)
17. [As áreas precisam formar retângulos](#17-as-áreas-precisam-formar-retângulos)
18. [Linhas nomeadas geradas pelas áreas](#18-linhas-nomeadas-geradas-pelas-áreas)
19. [Mudando o layout sem recalcular números](#19-mudando-o-layout-sem-recalcular-números)
20. [Exemplo completo de layout](#20-exemplo-completo-de-layout)
21. [Layout responsivo](#21-layout-responsivo)
22. [Sobreposição de grid items](#22-sobreposição-de-grid-items)
23. [Eixo Z e z-index](#23-eixo-z-e-z-index)
24. [z-index e stacking context](#24-z-index-e-stacking-context)
25. [Sobreposição intencional](#25-sobreposição-intencional)
26. [Grid implícito e grid-area](#26-grid-implícito-e-grid-area)
27. [auto-fit e minmax](#27-auto-fit-e-minmax)
28. [opacity para depuração](#28-opacity-para-depuração)
29. [mix-blend-mode](#29-mix-blend-mode)
30. [RGB](#30-rgb)
31. [Exemplo integrando grid-area, z-index e mix-blend-mode](#31-exemplo-integrando-grid-area-z-index-e-mix-blend-mode)
32. [Modelo mental de grid-area](#32-modelo-mental-de-grid-area)
33. [Como ler qualquer grid-area](#33-como-ler-qualquer-grid-area)
34. [Comparação entre os shorthands](#34-comparação-entre-os-shorthands)
35. [Tabela de fixação](#35-tabela-de-fixação)
36. [Erros conceituais que devemos evitar](#36-erros-conceituais-que-devemos-evitar)
37. [Checklist mental](#37-checklist-mental)
38. [Regra de ouro](#38-regra-de-ouro)
39. [Referência rápida](#39-referência-rápida)
40. [Referências técnicas](#40-referências-técnicas)
41. [GitHub](#41-github)

---

## 1. O que é grid-area

`grid-area` é uma forma abreviada (**shorthand**) para posicionar um grid item dentro do Grid. Ela reúne quatro propriedades:

```css
grid-row-start
grid-column-start
grid-row-end
grid-column-end
```

```css
grid-area: 2 / 3 / 4 / 5;
```

equivale a:

```css
grid-row-start: 2;
grid-column-start: 3;
grid-row-end: 4;
grid-column-end: 5;
```

Segundo a MDN, `grid-area` especifica o tamanho e a localização da área de um grid item. Além de linhas numéricas, ela também aceita o **nome de uma área** definida em `grid-template-areas` (seção 11).

---

## 2. A ordem dos quatro valores

```text
1º → grid-row-start
2º → grid-column-start
3º → grid-row-end
4º → grid-column-end
```

```text
grid-area:  A  /  B  /  C  /  D
            │     │     │     │
            ▼     ▼     ▼     ▼
          row   column  row  column
          start  start  end    end
```

```text
┌──────────────────────────────────────┐
│  1 = ROW START                       │
│  2 = COLUMN START                    │
│  3 = ROW END                         │
│  4 = COLUMN END                      │
└──────────────────────────────────────┘
```

Essa é a parte mais importante de `grid-area`.

---

## 3. Por que a ordem parece estranha

Ao ler `grid-area: 2 / 3 / 5 / 6`, é comum pensar na ordem de `margin` e `padding`:

```text
top / right / bottom / left
```

Mas `grid-area` não segue essa sequência. Em idiomas escritos da esquerda para a direita, a ordem corresponde a:

```text
top / left / bottom / right
```

ou, nos termos do Grid:

```text
ROW START / COLUMN START / ROW END / COLUMN END
```

```text
                 COLUMN START
                      ↓
ROW START →  ┌────────────────┐
             │                │
             │      ITEM      │
             │                │
ROW END   →  └────────────────┘
                              ↑
                         COLUMN END
```

> **⚠️ Atenção**
>
> A ordem é do **início** dos dois eixos para o **fim** dos dois eixos, alternando linha e coluna. Em idiomas escritos da direita para a esquerda, a coluna inicial fica à direita.

---

## 4. grid-area usa grid lines

```text
             COLUNAS
        1        2        3        4
        │        │        │        │
   1 ───┼────────┼────────┼────────┼───
        │        │        │        │
   2 ───┼────────┼────────┼────────┼───
        │        │        │        │
   3 ───┼────────┼────────┼────────┼───
        │        │        │        │
   4 ───┼────────┼────────┼────────┼───
```

Aqui há 4 grid lines horizontais e 4 verticais, o que dá 3 rows e 3 columns.

```text
GRID LINE → delimita
GRID TRACK → espaço entre duas linhas
```

`grid-area` usa essas linhas para definir os limites da área do item.

---

## 5. Exemplo simples

```html
<div class="grid">
  <div class="item">Item</div>
</div>
```

```css
.grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: repeat(4, 100px);
}

.item {
  grid-area: 2 / 2 / 4 / 4;
}
```

Leitura:

```text
row-start    = 2
column-start = 2
row-end      = 4
column-end   = 4
```

O item vai da linha 2 até a linha 4 nos dois eixos, ocupando as rows 2 e 3 e as columns 2 e 3:

```text
        coluna:  1       2       3       4
                 │       │       │       │
      linha 1 ───┼───────┼───────┼───────┼───
                 │       │       │       │
      linha 2 ───┼───────┼───────┼───────┼───
                 │       │███████│███████│
                 │       │███ ITEM ██████│
      linha 3 ───┼───────┼███████┼███████┼───
                 │       │███████│███████│
                 │       │███████│███████│
      linha 4 ───┼───────┼───────┼───────┼───
```

```text
rows:    4 − 2 = 2
columns: 4 − 2 = 2

área = 2 × 2
```

---

## 6. Equivalência com as propriedades individuais

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

---

## 7. grid-area controla os dois eixos

```text
grid-row    → ROW
grid-column → COLUMN
grid-area   → ROW + COLUMN
```

```text
grid-row    → posição e extensão no eixo das rows
grid-column → posição e extensão no eixo das colunas
grid-area   → área completa, nos dois eixos
```

---

## 8. Valores omitidos e auto

`grid-area` aceita de um a quatro valores separados por `/`:

```css
grid-area: 2;
grid-area: 2 / 3;
grid-area: 2 / 3 / 4;
grid-area: 2 / 3 / 4 / 5;
```

Quando algum valor é omitido, as regras são:

| Valor omitido | Regra |
| --- | --- |
| `column-start` | copia o `row-start` se ele for um **nome**; senão, `auto` |
| `row-end` | copia o `row-start` se ele for um **nome**; senão, `auto` |
| `column-end` | copia o `column-start` se ele for um **nome**; senão, `auto` |

Consequências:

```text
grid-area: 2
→ row-start = 2; os outros três = auto

grid-area: 2 / 3
→ row-start = 2, column-start = 3; os outros dois = auto

grid-area: header
→ os quatro valores = header (área nomeada)
```

> **⚠️ Atenção**
>
> `grid-area: 2 / 3` **não** significa "de 2 até 3". Significa `row-start = 2` e `column-start = 3`.

`auto` deixa aquela parte do posicionamento para o algoritmo de auto-placement. Quando uma linha final é `auto`, o item ocupa uma única track naquele eixo:

```css
grid-area: auto;
grid-area: auto / auto / auto / auto;
```

Enquanto você constrói o modelo mental, prefira a forma de quatro valores e traduza imediatamente:

```text
2 = row-start
3 = column-start
5 = row-end
6 = column-end
```

---

## 9. Calculando o tamanho da área

```css
grid-area: 2 / 3 / 6 / 7;
```

```text
rows:    6 − 2 = 4  → 4 row tracks
columns: 7 − 3 = 4  → 4 column tracks

área = 4 × 4
```

```text
row-span    = row-end − row-start
column-span = column-end − column-start
```

---

## 10. span dentro de grid-area

`span` indica quantidade de tracks, como em `grid-row` e `grid-column`:

```css
grid-area: 2 / 3 / span 2 / span 3;
```

```text
ROW START    = 2
COLUMN START = 3
ROW SPAN     = 2  → termina na linha 4
COLUMN SPAN  = 3  → termina na linha 6
```

```text
              1       2       3       4       5       6
              │       │       │       │       │       │
         1 ───┼───────┼───────┼───────┼───────┼───────┼───
              │       │       │       │       │       │
         2 ───┼───────┼───────┼───────┼───────┼───────┼───
              │       │       │███████████████████████│
              │       │       │█████████ ITEM █████████│
         3 ───┼───────┼───────┼███████████████████████┼───
              │       │       │███████████████████████│
              │       │       │███████████████████████│
         4 ───┼───────┼───────┼───────┼───────┼───────┼───
```

O item ocupa 2 rows × 3 columns.

---

## 11. grid-area com área nomeada

Além de linhas numéricas, `grid-area` aceita um nome (`<custom-ident>`):

```css
grid-area: header;
```

Esse nome pode representar uma área definida por `grid-template-areas`. Existem, portanto, duas funções:

```text
grid-area
├── posicionamento por linhas (numérico)
└── posicionamento por área nomeada
```

---

## 12. grid-template-areas

```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 100px);
  grid-template-areas:
    "header header header"
    "nav    content aside"
    "footer footer footer";
}
```

```text
┌─────────┬─────────┬─────────┐
│ header  │ header  │ header  │
├─────────┼─────────┼─────────┤
│ nav     │ content │ aside   │
├─────────┼─────────┼─────────┤
│ footer  │ footer  │ footer  │
└─────────┴─────────┴─────────┘
```

Aqui descrevemos regiões do Grid por meio de nomes. Cada string é uma row, e cada palavra é uma coluna.

---

## 13. Associando um item à área

```css
.header  { grid-area: header; }
.nav     { grid-area: nav; }
.content { grid-area: content; }
.aside   { grid-area: aside; }
.footer  { grid-area: footer; }
```

Cada item é associado à sua área nomeada.

---

## 14. Nomes ou números

Compare:

```css
.header {
  grid-area: 1 / 1 / 2 / 4;
}
```

```css
.header {
  grid-area: header;
}
```

A primeira exige decorar `row-start`, `column-start`, `row-end` e `column-end`. A segunda comunica a intenção do layout.

**Quando usar números:** posicionamento geométrico preciso, `span`, sobreposição e estruturas sem regiões semânticas.

```css
grid-area: 2 / 2 / 5 / 4;
```

**Quando usar nomes:** regiões semanticamente conhecidas, como `header`, `sidebar`, `content`, `aside`, `footer` e `nav`.

```css
grid-area: content;
```

---

## 15. grid-template-areas funciona como um desenho

```css
grid-template-areas:
  "header header header"
  "nav    content aside"
  "footer footer footer";
```

O próprio CSS passa a se parecer com o layout:

```text
┌───────────────────────┐
│        HEADER         │
├───────┬───────┬───────┤
│  NAV  │CONTENT│ ASIDE │
├───────┴───────┴───────┤
│        FOOTER         │
└───────────────────────┘
```

Um ponto final (`.`) representa uma célula vazia, sem área:

```css
grid-template-areas:
  "header header"
  "nav    .";
```

---

## 16. O nome da área não cria um item

```css
grid-template-areas:
  "header header header";
```

não cria um elemento `<header>`. Cria uma **área nomeada dentro do Grid**. O item é associado a ela depois:

```css
.header {
  grid-area: header;
}
```

```text
grid-template-areas → cria a estrutura nomeada
grid-area: header   → coloca o item naquela área
```

---

## 17. As áreas precisam formar retângulos

Uma grid area é formada por uma ou mais células e deve ser um **retângulo**. Esta é válida:

```css
grid-template-areas:
  "a a"
  "a a";
```

```text
┌───────┬───────┐
│       │       │
│   A   │   A   │
├───────┼───────┤
│   A   │   A   │
│       │       │
└───────┴───────┘
```

Esta não é válida, porque `a` não forma um retângulo:

```css
grid-template-areas:
  "a a"
  "a ."
  "a a";
```

Nesse caso, a declaração inteira de `grid-template-areas` é considerada inválida e ignorada. Todas as strings também precisam ter o mesmo número de colunas.

---

## 18. Linhas nomeadas geradas pelas áreas

Cada área nomeada gera automaticamente linhas nomeadas nas suas bordas, com os sufixos `-start` e `-end`:

```text
header-start
      ↓
┌──────────────────────┐
│        HEADER        │
└──────────────────────┘
      ↑
header-end
```

Por isso `grid-area: header` funciona. Com um único nome, os quatro valores recebem esse nome e são resolvidos assim:

```text
row-start    → linha de row chamada header-start
column-start → linha de coluna chamada header-start
row-end      → linha de row chamada header-end
column-end   → linha de coluna chamada header-end
```

Dado este Grid:

```css
grid-template-areas:
  "header header header"
  "content content content";
```

esta declaração:

```css
.header {
  grid-area: header;
}
```

ocupa a mesma região que:

```css
.header {
  grid-area: 1 / 1 / 2 / 4;
}
```

mas o primeiro código expressa a **intenção semântica**, enquanto o segundo expressa **linhas numéricas**.

O caminho completo é:

```text
grid-template-areas
        ↓
cria áreas nomeadas
        ↓
gera linhas nomeadas nas bordas
        ↓
grid-area: content
        ↓
o item é colocado naquela área
```

---

## 19. Mudando o layout sem recalcular números

Os itens podem continuar usando os mesmos nomes quando a estrutura muda:

```css
.layout {
  grid-template-columns: repeat(2, 1fr);
  grid-template-areas:
    "header header"
    "nav    content"
    "aside  footer";
}
```

```css
.header  { grid-area: header; }
.nav     { grid-area: nav; }
.content { grid-area: content; }
.aside   { grid-area: aside; }
.footer  { grid-area: footer; }
```

Nenhum número de linha precisou ser recalculado.

> **⚠️ Atenção**
>
> `grid-template-areas` descreve as áreas, mas as colunas e rows continuam sendo definidas por `grid-template-columns` e `grid-template-rows`. Ao mudar o desenho, ajuste também essas propriedades.

Também é possível mover um item apenas trocando o nome da área:

```css
.item-1 {
  grid-area: header;
}
```

```css
.item-1 {
  grid-area: sidebar;
}
```

---

## 20. Exemplo completo de layout

```html
<div class="layout">
  <header class="header">Header</header>
  <nav class="sidebar">Sidebar</nav>
  <main class="content">Content</main>
  <aside class="aside">Aside</aside>
  <footer class="footer">Footer</footer>
</div>
```

```css
.layout {
  display: grid;
  grid-template-columns: 200px 1fr 200px;
  grid-template-rows: auto 1fr auto;
  grid-template-areas:
    "header  header  header"
    "sidebar content aside"
    "footer  footer  footer";
}

.header  { grid-area: header; }
.sidebar { grid-area: sidebar; }
.content { grid-area: content; }
.aside   { grid-area: aside; }
.footer  { grid-area: footer; }
```

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

```text
estrutura → nome → item
```

Comparação com o mesmo layout usando `grid-row` e `grid-column`:

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

A abordagem por áreas é mais orientada à estrutura semântica do layout.

---

## 21. Layout responsivo

Alterar o desenho em uma media query reorganiza o layout sem mexer nos itens:

```css
.layout {
  display: grid;
  grid-template-columns: 200px 1fr 200px;
  grid-template-areas:
    "header  header  header"
    "sidebar content aside"
    "footer  footer  footer";
}
```

```css
@media (max-width: 700px) {
  .layout {
    grid-template-columns: 1fr;
    grid-template-rows: auto;
    grid-template-areas:
      "header"
      "content"
      "sidebar"
      "aside"
      "footer";
  }
}
```

Os itens continuam com `grid-area: header`, `content`, `sidebar`, `aside` e `footer`.

> **⚠️ Atenção**
>
> No exemplo acima, `grid-template-rows: auto` redefine as rows. O valor anterior (`auto 1fr auto`) tinha três rows, mas agora existem cinco áreas empilhadas. As rows que sobram passam a ser implícitas.

---

## 22. Sobreposição de grid items

Dois itens podem ocupar a mesma região do Grid. O Grid não impede isso:

```css
.item-1 {
  grid-area: header;
}

.item-2 {
  grid-area: header;
}
```

```text
┌─────────────────────┐
│       ITEM 1        │
│       ITEM 2        │
│     SOBREPOSTOS     │
└─────────────────────┘
```

Itens com posicionamento explícito não são empurrados para outra célula: são desenhados um sobre o outro. A sobreposição pode ser intencional e é usada para:

```text
camadas
banners
efeitos visuais
cards sobre imagens
elementos decorativos
```

```text
┌────────────────────────────┐
│          IMAGEM            │
│     ┌────────────────┐     │
│     │     TEXTO      │     │
│     └────────────────┘     │
└────────────────────────────┘
```

---

## 23. Eixo Z e z-index

Com sobreposição, surge uma terceira dimensão visual: o eixo **Z**.

```text
          Z
          ↑
          │     ITEM 3
          │   ITEM 2
          │ ITEM 1
          └────────────────
```

A ordem de empilhamento é controlada por `z-index`. Um valor maior coloca o elemento acima de um com valor menor, e a propriedade também se aplica a grid items.

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

```text
┌─────────────────────────┐
│        ITEM 2           │  ← z-index 2 (na frente)
│        ITEM 1           │  ← z-index 1
└─────────────────────────┘
```

`z-index` representa uma **ordem de empilhamento**, não uma distância física. Importa a relação entre os valores:

```text
5 > 2  →  o item com z-index 5 fica acima
```

> **Nota:** sem `z-index`, itens sobrepostos são pintados na ordem do HTML: o que vem **depois** no código aparece **por cima**.

---

## 24. z-index e stacking context

Um **stacking context** é um contexto independente de empilhamento:

```text
STACKING CONTEXT
│
├── elemento A
├── elemento B
└── elemento C
```

Os descendentes são empilhados dentro do contexto a que pertencem. Por isso, em layouts complexos, `z-index: 100` pode ficar atrás de `z-index: 10` se estiverem em contextos diferentes.

Para exemplos simples, basta pensar:

```text
z-index maior → elemento fica acima
```

---

## 25. Sobreposição intencional

Quando a posição deve cobrir todo o Grid explícito, o Grid precisa ter tracks explícitas. Sem elas, `-1` aponta para a linha 1:

```css
.hero {
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: 300px;
}

.image {
  grid-area: 1 / 1 / -1 / -1;
}

.overlay {
  grid-area: 1 / 1 / -1 / -1;
  z-index: 2;
}
```

```text
┌─────────────────────────────┐
│           IMAGE             │
│       ┌──────────────┐      │
│       │   OVERLAY    │      │
│       └──────────────┘      │
└─────────────────────────────┘
```

```text
grid-area → define ONDE
z-index   → define QUEM FICA ACIMA
```

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

---

## 26. Grid implícito e grid-area

Se a posição solicitada ultrapassa o grid explícito, o Grid cria tracks implícitas para acomodá-la.

```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 100px);
}

.item {
  grid-column: 5;
}
```

O grid explícito tem 3 colunas. Para colocar o item na coluna 5, o Grid cria as colunas 4 e 5 implicitamente. O tamanho delas é controlado por `grid-auto-columns`.

O mesmo vale para `grid-area`:

```text
grid-area → solicita uma posição
Grid      → encontra ou cria as tracks necessárias
```

---

## 27. auto-fit e minmax

```css
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(80px, 1fr));
}
```

```text
auto-fit  → quantas colunas couberem
minmax()  → limites de cada coluna
```

```text
minmax(80px, 1fr)
mínimo = 80px
máximo = 1fr
```

Leitura completa:

```text
crie quantas colunas couberem,
cada uma com no mínimo 80px,
e permita que cresçam até ocupar o espaço disponível
```

Com `auto-fit`, as colunas vazias são recolhidas, e as demais crescem para ocupar o espaço. Com `auto-fill`, as colunas vazias continuam existindo.

`auto-fit` e `grid-area` têm responsabilidades diferentes:

```text
auto-fit  → define a quantidade e a estrutura das colunas
grid-area → posiciona o item dentro dessa estrutura
```

---

## 28. opacity para depuração

```css
.item {
  opacity: 0.2;
}
```

Itens translúcidos revelam quando estão sobrepostos, o que é útil para **depurar** layouts.

```text
grid-area → layout
opacity   → visualização
```

`opacity` não altera `grid-row`, `grid-column` nem `grid-area`: muda apenas a aparência.

---

## 29. mix-blend-mode

`mix-blend-mode` define como o conteúdo de um elemento se mistura com o que está atrás dele, dentro do contexto de empilhamento. Não faz parte do Grid, mas aparece junto com sobreposições:

```text
grid-area      → layout
mix-blend-mode → composição visual de cores
```

```css
mix-blend-mode: screen;
```

O modo `screen` produz um efeito de "soma de luz": cores claras resultam em cores ainda mais claras. Com valores normalizados de 0 a 1, por canal:

```text
resultado = 1 − (1 − primeiro plano) × (1 − fundo)
```

> **⚠️ Atenção**
>
> "Somar as cores" é uma boa analogia visual, mas `screen` não é uma soma direta de valores RGB. É a operação acima, aplicada a cada canal.

---

## 30. RGB

```text
R = Red
G = Green
B = Blue
```

Cada canal varia de 0 a 255:

```text
           RGB

R ─────────────── 0 → 255
G ─────────────── 0 → 255
B ─────────────── 0 → 255
```

| Valor | Cor |
| --- | --- |
| `rgb(255, 0, 0)` | vermelho |
| `rgb(0, 255, 0)` | verde |
| `rgb(0, 0, 255)` | azul |
| `rgb(255, 255, 255)` | branco |
| `rgb(0, 0, 0)` | preto |
| `rgb(255, 255, 0)` | amarelo |
| `rgb(255, 0, 255)` | magenta |
| `rgb(0, 255, 255)` | ciano |

Com `screen`, a analogia de luz ajuda a visualizar o resultado:

```text
vermelho + azul           = magenta
vermelho + verde          = amarelo
verde + azul              = ciano
vermelho + verde + azul   = branco
```

---

## 31. Exemplo integrando grid-area, z-index e mix-blend-mode

```html
<div class="grid">
  <div class="background"></div>
  <div class="overlay"></div>
</div>
```

```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 150px);
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
└─────────────────────────────────┘
```

Na região em que o azul cobre o vermelho, o `screen` produz magenta.

---

## 32. Modelo mental de grid-area

```text
                grid-area
                     │
         ┌───────────┴────────────┐
         │                        │
         ▼                        ▼
   LINHAS NUMÉRICAS          ÁREA NOMEADA
         │                        │
         ▼                        ▼
   row-start                   header
   column-start                content
   row-end                     footer
   column-end                  sidebar
         │                        │
         └───────────┬────────────┘
                     ▼
                GRID ITEM
```

Forma numérica:

```css
grid-area: A / B / C / D;
```

```text
A = row-start
B = column-start
C = row-end
D = column-end
```

Forma nomeada:

```css
grid-area: header;
```

```text
"este item pertence à área chamada header"
```

Dois shorthands, duas funções:

```text
grid-area
├── line-based placement
└── named area placement
```

---

## 33. Como ler qualquer grid-area

### Com números

```css
grid-area: 2 / 3 / 5 / 7;
```

```text
row-start    = 2
column-start = 3
row-end      = 5
column-end   = 7

rows:    5 − 3? não: 5 − 2 = 3
columns: 7 − 3 = 4

área = 3 rows × 4 columns
```

### Com `span`

```css
grid-area: 2 / 3 / span 4 / span 2;
```

```text
row-start    = 2
column-start = 3
4 rows
2 columns

área = 4 rows × 2 columns
```

### Com nome

```css
grid-area: content;
```

```text
Existe uma área chamada "content" em grid-template-areas?
→ sim: o item ocupa essa área
```

### Com `-1`

```css
grid-area: 1 / 1 / -1 / -1;
```

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

## 34. Comparação entre os shorthands

| Propriedade | Controla | Exemplo |
| --- | --- | --- |
| `grid-row` | início e fim no eixo das rows | `grid-row: 2 / 4` |
| `grid-column` | início e fim no eixo das colunas | `grid-column: 2 / 4` |
| `grid-area` | rows + columns | `grid-area: 2 / 2 / 4 / 4` |

Comparação final entre as duas formas de `grid-area`:

```css
.item {
  grid-area: 2 / 2 / 4 / 4;
}
```

```text
row-start = 2, column-start = 2, row-end = 4, column-end = 4
```

```css
.item {
  grid-area: content;
}
```

```text
este item usa a área "content"
```

---

## 35. Tabela de fixação

| Sintaxe | Significado |
| --- | --- |
| `grid-area: auto` | posicionamento automático |
| `grid-area: 2` | `row-start = 2`; os demais valores são `auto` |
| `grid-area: 2 / 3` | `row-start = 2`, `column-start = 3`; os demais são `auto` |
| `grid-area: 2 / 3 / 4` | `row-start = 2`, `column-start = 3`, `row-end = 4`; `column-end` é `auto` |
| `grid-area: 2 / 3 / 4 / 5` | define as quatro linhas |
| `grid-area: 2 / 3 / span 2 / span 3` | começa em 2/3 e ocupa 2 rows × 3 columns |
| `grid-area: header` | usa a área nomeada `header` |
| `grid-row` | posiciona no eixo das rows |
| `grid-column` | posiciona no eixo das colunas |
| `grid-template-areas` | define áreas nomeadas |
| `z-index` | controla a ordem de empilhamento |
| `opacity` | altera a transparência visual |
| `mix-blend-mode` | controla como o conteúdo se mistura com o fundo |

---

## 36. Erros conceituais que devemos evitar

### Erro 1 — confundir a ordem dos valores

```text
errado:   top / right / bottom / left
correto:  row-start / column-start / row-end / column-end
```

### Erro 2 — ler dois valores como "início e fim"

```css
grid-area: 2 / 3;
```

```text
errado:   de 2 até 3
correto:  row-start = 2, column-start = 3
```

### Erro 3 — achar que `grid-area: header` cria o header

A área precisa existir em `grid-template-areas`, ou ser resolvida por linhas nomeadas compatíveis. Se não existir, o nome é tratado como uma linha implícita, e o item vai parar além do grid explícito, o que cria tracks implícitas.

### Erro 4 — achar que dois itens não podem ocupar a mesma área

```css
.item-1 { grid-area: header; }
.item-2 { grid-area: header; }
```

Eles podem se sobrepor.

### Erro 5 — esperar que o Grid separe itens sobrepostos

Se você definiu que dois itens ocupam a mesma região, o Grid pode simplesmente sobrepô-los.

### Erro 6 — achar que `z-index` pertence ao Grid

`z-index` é uma propriedade geral de empilhamento, que também pode ser aplicada a grid items.

### Erro 7 — usar `-1` sem grid explícito

Sem `grid-template-columns` e `grid-template-rows`, o grid explícito não tem tracks, e `-1` aponta para a linha 1.

---

## 37. Checklist mental

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

□ As áreas formam retângulos?

□ O item está sendo colocado sobre outro?

□ Preciso controlar a ordem visual com z-index?
```

---

## 38. Regra de ouro

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

## 39. Referência rápida

```css
/* Forma mais completa */
grid-area: 2 / 3 / 5 / 6;

/* Equivalente */
grid-row-start: 2;
grid-column-start: 3;
grid-row-end: 5;
grid-column-end: 6;

/* Com span */
grid-area: 2 / 3 / span 2 / span 3;

/* Área nomeada */
grid-area: content;

/* Todo o grid explícito */
grid-area: 1 / 1 / -1 / -1;
```

```css
/* Exemplo com template areas */
.container {
  display: grid;
  grid-template-areas:
    "header header"
    "content sidebar"
    "footer footer";
}

.header  { grid-area: header; }
.content { grid-area: content; }
.sidebar { grid-area: sidebar; }
.footer  { grid-area: footer; }
```

```css
/* Sobreposição */
.background {
  grid-area: 1 / 1 / -1 / -1;
}

.overlay {
  grid-area: 1 / 1 / -1 / -1;
  z-index: 2;
  mix-blend-mode: screen;
}
```

---

## 40. Referências técnicas

### MDN Web Docs

- `grid-area`
- `grid-row`
- `grid-column`
- `grid-template-areas`
- Grid layout: line-based placement
- Grid layout: grid template areas
- Grid layout: named grid lines
- `z-index`
- `mix-blend-mode`
- `<blend-mode>`

### Especificações

- CSS Grid Layout Module Level 1 e Level 2
- CSS Compositing and Blending Module Level 1

`grid-area` tem uma ordem própria de valores e regras específicas para `<custom-ident>`, `span`, linhas nomeadas e áreas nomeadas, por isso vale consultar as fontes oficiais.

---

## 41. GitHub

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