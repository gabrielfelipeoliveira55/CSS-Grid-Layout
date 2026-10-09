# CSS Grid Layout — Propriedade `grid`

A propriedade `grid` é uma **shorthand**, ou seja, uma propriedade abreviada capaz de configurar diferentes propriedades do CSS Grid em uma única declaração.

Ela pode configurar as seguintes propriedades:

```css
grid-template-rows
grid-template-columns
grid-template-areas

grid-auto-rows
grid-auto-columns
grid-auto-flow
```

Essas propriedades representam dois grandes aspectos do Grid:

```text
                         grid
                           │
              ┌────────────┴────────────┐
              │                         │
       GRID EXPLÍCITO             GRID IMPLÍCITO
              │                         │
      ┌───────┼───────┐         ┌───────┼───────┐
      │       │       │         │       │       │
     rows   columns  areas    auto-rows auto-columns auto-flow
```

> **Importante:** `grid` é uma shorthand que reúne seis propriedades relacionadas à definição explícita e implícita do Grid. Isso não significa que uma única declaração possa escrever arbitrariamente os seis nomes ao mesmo tempo. A shorthand possui **formas sintáticas específicas**.

---

# Índice

- [1. Entendendo o Grid explícito e implícito](#1-entendendo-o-grid-explícito-e-implícito)
  - [1.1 Grid explícito](#11-grid-explícito)
  - [1.2 Grid implícito](#12-grid-implícito)
- [2. As seis propriedades configuradas por `grid`](#2-as-seis-propriedades-configuradas-por-grid)
- [3. Formas gerais da shorthand](#3-formas-gerais-da-shorthand)
- [4. Forma 1 — `grid-template-rows / grid-template-columns`](#4-forma-1--grid-template-rows--grid-template-columns)
- [5. `grid-template-rows`](#5-grid-template-rows)
- [6. `grid-template-columns`](#6-grid-template-columns)
- [7. A importância da `/`](#7-a-importância-da)
- [8. Usando `fr`](#8-usando-fr)
- [9. Usando `repeat()`](#9-usando-repeat)
- [10. Usando `minmax()`](#10-usando-minmax)
- [11. `grid-template-areas`](#11-grid-template-areas)
- [12. Colocando `grid-template-areas` dentro de `grid`](#12-colocando-grid-template-areas-dentro-de-grid)
- [13. `grid-auto-flow`](#13-grid-auto-flow)
- [14. `grid-auto-flow: row`](#14-grid-auto-flow-row)
- [15. `grid-auto-flow: column`](#15-grid-auto-flow-column)
- [16. `auto-flow` dentro da shorthand](#16-auto-flow-dentro-da-shorthand)
- [17. `auto-flow` + `grid-auto-rows`](#17-auto-flow--grid-auto-rows)
- [18. `grid-auto-rows` com múltiplos valores](#18-grid-auto-rows-com-múltiplos-valores)
- [19. `auto-flow` no lado das colunas](#19-auto-flow-no-lado-das-colunas)
- [20. Comparando as duas formas de `auto-flow`](#20-comparando-as-duas-formas-de-auto-flow)
- [21. `dense`](#21-dense)
- [22. Construindo um layout completo com `grid`](#22-construindo-um-layout-completo-com-grid)
- [23. Construindo um dashboard](#23-construindo-um-dashboard)
- [24. Construindo uma galeria](#24-construindo-uma-galeria)
- [25. Construindo uma lista vertical](#25-construindo-uma-lista-vertical)
- [26. Diferença entre `grid-template` e `grid`](#26-diferença-entre-grid-template-e-grid)
- [27. Principais formas da shorthand](#27-principais-formas-da-shorthand)
- [28. Como memorizar a sintaxe](#28-como-memorizar-a-sintaxe)
- [29. Mapa mental definitivo](#29-mapa-mental-definitivo)
- [30. Regra mental para estudar](#30-regra-mental-para-estudar)
- [31. Ponto importante sobre os resets da shorthand](#31-ponto-importante-sobre-os-resets-da-shorthand)
- [32. Resumo definitivo](#32-resumo-definitivo)
- [Referências](#referências)

---

# 1. Entendendo o Grid explícito e implícito

Antes de compreender a shorthand `grid`, é importante separar duas ideias:

- **Grid explícito**
- **Grid implícito**

Essa distinção é fundamental porque algumas propriedades definem diretamente a estrutura criada pelo desenvolvedor, enquanto outras controlam faixas criadas automaticamente pelo algoritmo de posicionamento.

---

## 1.1 Grid explícito

O **Grid explícito** é a estrutura que definimos diretamente por meio de propriedades como:

```css
grid-template-rows
grid-template-columns
grid-template-areas
```

Por exemplo:

```css
.container {
  display: grid;

  grid-template-columns: 1fr 1fr;
  grid-template-rows: 100px 200px;
}
```

Aqui definimos explicitamente:

- 2 colunas;
- 2 linhas;
- o tamanho das duas linhas;
- o dimensionamento das duas colunas.

Visualmente:

```text
┌───────────────┬───────────────┐
│               │               │
│               │               │
│     100px     │     100px     │
│               │               │
├───────────────┼───────────────┤
│               │               │
│               │               │
│     200px     │     200px     │
│               │               │
└───────────────┴───────────────┘
       1fr              1fr
```

A estrutura criada diretamente pelo `grid-template-*` faz parte do **Grid explícito**.

---

## 1.2 Grid implícito

Agora imagine que existam mais elementos do que células disponíveis no Grid explícito.

HTML:

```html
<div class="container">
  <div>1</div>
  <div>2</div>
  <div>3</div>
  <div>4</div>
  <div>5</div>
  <div>6</div>
</div>
```

CSS:

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr;
}
```

Nesse caso, definimos explicitamente apenas duas colunas.

O algoritmo de auto-placement poderá criar novas linhas para acomodar os itens.

Essas linhas adicionais fazem parte do **Grid implícito**.

```text
Grid explícito:

┌──────────────┬──────────────┐
│      1       │      2       │
├──────────────┼──────────────┤
│      3       │      4       │
├──────────────┼──────────────┤
│      5       │      6       │
└──────────────┴──────────────┘
        ↑
   linhas implícitas
```

É nesse contexto que entram:

```css
grid-auto-rows
grid-auto-columns
grid-auto-flow
```

### Corte mental

```text
grid-template-*
      ↓
"Eu desenho a estrutura"

grid-auto-*
      ↓
"Eu controlo o que acontece quando
o Grid precisa criar faixas automaticamente"
```

---

# 2. As seis propriedades configuradas por `grid`

A shorthand `grid` possui seis propriedades constituintes:

| Grupo | Propriedade | Função |
|---|---|---|
| Explícito | `grid-template-rows` | Define as faixas de linhas explícitas |
| Explícito | `grid-template-columns` | Define as faixas de colunas explícitas |
| Explícito | `grid-template-areas` | Define áreas nomeadas da grade |
| Implícito | `grid-auto-rows` | Define o tamanho das linhas implícitas |
| Implícito | `grid-auto-columns` | Define o tamanho das colunas implícitas |
| Implícito | `grid-auto-flow` | Define a direção do auto-placement |

Podemos visualizar a relação assim:

```text
                         grid
                           │
             ┌─────────────┴─────────────┐
             │                           │
      EXPLICIT GRID               IMPLICIT GRID
             │                           │
      ┌──────┼──────┐              ┌─────┼─────┐
      │      │      │              │     │     │
     rows  columns areas        auto-rows auto-columns auto-flow
```

### Atenção

É importante não interpretar essa lista como uma ordem de escrita.

Isto:

```css
grid:
  grid-template-rows
  grid-template-columns
  grid-template-areas
  grid-auto-rows
  grid-auto-columns
  grid-auto-flow;
```

**não é uma sintaxe válida da shorthand.**

Os seis nomes são as propriedades que `grid` representa.

A sintaxe da shorthand utiliza valores em formas específicas.

---

# 3. Formas gerais da shorthand

A propriedade `grid` possui três grandes famílias sintáticas:

```css
grid: <grid-template>;
```

```css
grid: <grid-template-rows> / [auto-flow && dense?] <grid-auto-columns>?;
```

```css
grid: [auto-flow && dense?] <grid-auto-rows>? / <grid-template-columns>;
```

Na prática, podemos pensar nesses três modelos:

```text
┌────────────────────────────────────────────┐
│ 1. GRID-TEMPLATE                           │
│                                            │
│    rows / columns                          │
│    ou                                     │
│    "areas" row-size / columns              │
├────────────────────────────────────────────┤
│ 2. AUTO-FLOW POR COLUNAS                   │
│                                            │
│    rows / auto-flow auto-columns           │
├────────────────────────────────────────────┤
│ 3. AUTO-FLOW POR LINHAS                    │
│                                            │
│    auto-flow auto-rows / columns           │
└────────────────────────────────────────────┘
```

Essa é a ideia central que organiza toda a shorthand.

---

# 4. Forma 1 — `grid-template-rows / grid-template-columns`

A forma mais simples é:

```css
grid: LINHAS / COLUNAS;
```

Por exemplo:

```css
.container {
  display: grid;
  grid: 100px 200px / 1fr 1fr;
}
```

Podemos interpretar assim:

```text
                  grid
                   │
                   ▼
          100px 200px / 1fr 1fr
             │             │
             │             │
            rows        columns
```

É equivalente, para essa forma, a:

```css
.container {
  display: grid;

  grid-template-rows: 100px 200px;
  grid-template-columns: 1fr 1fr;
}
```

### Regra fundamental

```text
ANTES DA /
     ↓
LINHAS

DEPOIS DA /
     ↓
COLUNAS
```

---

# 5. `grid-template-rows`

`grid-template-rows` define as **faixas de linhas explícitas** do Grid.

Pode receber valores de dimensionamento como:

```css
px
%
fr
auto
min-content
max-content
minmax()
repeat()
fit-content()
```

Exemplo:

```css
.container {
  display: grid;

  grid-template-rows:
    100px
    200px
    100px;
}
```

Visualmente:

```text
┌──────────────────────────┐
│                          │
│          100px           │
│                          │
├──────────────────────────┤
│                          │
│          200px           │
│                          │
├──────────────────────────┤
│                          │
│          100px           │
│                          │
└──────────────────────────┘
```

Na shorthand:

```css
.container {
  display: grid;

  grid: 100px 200px 100px / 1fr;
}
```

A leitura é:

```text
grid: 100px 200px 100px / 1fr
      ───────────────── ───
              │           │
             rows      columns
```

---

# 6. `grid-template-columns`

`grid-template-columns` define as **faixas de colunas explícitas**.

Exemplo:

```css
.container {
  display: grid;

  grid-template-columns:
    200px
    1fr
    100px;
}
```

Visualmente:

```text
┌────────┬──────────────────┬────────┐
│        │                  │        │
│ 200px  │       1fr        │ 100px  │
│        │                  │        │
└────────┴──────────────────┴────────┘
```

Na shorthand:

```css
.container {
  display: grid;

  grid: 200px / 200px 1fr 100px;
}
```

A leitura é:

```text
grid: 200px / 200px 1fr 100px
       ────    ─────────────────
        │              │
       rows         columns
```

Mesmo que exista apenas uma linha explícita, ela continua sendo a parte anterior à `/`.

---

# 7. A importância da `/`

A barra `/` é um dos elementos mais importantes para compreender `grid`.

Na forma explícita:

```css
grid: ROWS / COLUMNS;
```

Ela separa:

```text
LINHAS
  │
  ▼
  /
  │
  ▼
COLUNAS
```

Por exemplo:

```css
grid: 100px 200px / 1fr 2fr 1fr;
```

Significa:

```css
grid-template-rows:
  100px
  200px;

grid-template-columns:
  1fr
  2fr
  1fr;
```

Visualmente:

```text
                    COLUNAS
             ↓        ↓        ↓
            1fr      2fr      1fr

        ┌────────┬──────────┬────────┐
100px   │        │          │        │
        ├────────┼──────────┼────────┤
200px   │        │          │        │
        └────────┴──────────┴────────┘
             ↑
           LINHAS
```

### Regra mental

> **Antes da `/` → linhas. Depois da `/` → colunas.**

Essa regra funciona diretamente para a forma `rows / columns`.

> **Atenção:** quando `auto-flow` aparece, essa interpretação precisa ser ajustada porque uma das dimensões passa a representar faixas implícitas.

---

# 8. Usando `fr`

A unidade `fr` representa uma **fração do espaço livre disponível para distribuição entre as faixas flexíveis**, depois das dimensões que precisam ser consideradas pelo algoritmo de dimensionamento.

Por exemplo:

```css
grid: 100px / 1fr 1fr;
```

Temos duas colunas flexíveis:

```text
┌──────────────────┬──────────────────┐
│                  │                  │
│       1fr        │       1fr        │
│                  │                  │
└──────────────────┴──────────────────┘
```

Quando as condições permitem uma distribuição igual do espaço livre, as duas colunas terão o mesmo tamanho.

Já:

```css
grid: 100px / 1fr 2fr;
```

produz uma distribuição proporcional de `1 : 2`:

```text
┌───────────────┬─────────────────────────────┐
│      1fr      │             2fr             │
└───────────────┴─────────────────────────────┘
```

A segunda faixa recebe uma participação duas vezes maior que a primeira dentro do espaço flexível disponível.

### Corte mental

```text
1fr + 1fr
   ↓
divisão proporcional
   ↓
1 parte + 1 parte

1fr + 2fr
   ↓
divisão proporcional
   ↓
1 parte + 2 partes
```

---

# 9. Usando `repeat()`

Podemos combinar `grid` com `repeat()`.

```css
.container {
  display: grid;

  grid: repeat(3, 100px) / repeat(4, 1fr);
}
```

Isso representa:

```css
grid-template-rows:
  100px
  100px
  100px;

grid-template-columns:
  1fr
  1fr
  1fr
  1fr;
```

Visualmente:

```text
┌───────┬───────┬───────┬───────┐
│       │       │       │       │
├───────┼───────┼───────┼───────┤
│       │       │       │       │
├───────┼───────┼───────┼───────┤
│       │       │       │       │
└───────┴───────┴───────┴───────┘
```

Podemos memorizar:

```text
repeat(3, 100px)
        ↓
3 linhas de 100px

repeat(4, 1fr)
        ↓
4 colunas de 1fr
```

---

# 10. Usando `minmax()`

Também podemos utilizar `minmax()` na shorthand.

```css
grid: minmax(100px, 1fr) / 1fr 2fr;
```

Na dimensão das linhas:

```text
mínimo → 100px
máximo → 1fr
```

Isso significa que essa faixa possui um limite mínimo de `100px` e pode participar de uma distribuição flexível até o limite definido pelo máximo.

Uma aplicação prática:

```css
.container {
  display: grid;

  grid: minmax(150px, auto) / 1fr 1fr;
}
```

Podemos interpretar a linha como uma faixa que:

```text
não deve ser menor que 150px
          +
pode crescer conforme o dimensionamento automático
```

`minmax()` é especialmente útil quando queremos definir limites de dimensionamento em vez de escolher apenas um tamanho único.

---

# 11. `grid-template-areas`

Além de dimensões, o Grid permite construir layouts utilizando **áreas nomeadas**.

Exemplo:

```css
.container {
  display: grid;

  grid-template-areas:
    "header header"
    "main   aside"
    "footer footer";

  grid-template-columns: 2fr 1fr;
  grid-template-rows: auto 1fr auto;
}
```

Visualmente:

```text
┌───────────────────────────────┐
│             HEADER            │
├────────────────────┬──────────┤
│                    │          │
│        MAIN        │  ASIDE   │
│                    │          │
├────────────────────┴──────────┤
│             FOOTER            │
└───────────────────────────────┘
```

Os elementos podem receber seus respectivos nomes:

```css
.header {
  grid-area: header;
}

.main {
  grid-area: main;
}

.aside {
  grid-area: aside;
}

.footer {
  grid-area: footer;
}
```

### O conceito

`grid-template-areas` funciona como uma representação textual da estrutura visual do layout.

```text
"header header"
"main   aside"
"footer footer"
```

Pode ser visualmente interpretado como:

```text
┌─────────────┬─────────────┐
│    header   │    header   │
├─────────────┼─────────────┤
│     main    │    aside    │
├─────────────┴─────────────┤
│           footer          │
└───────────────────────────┘
```

Essa forma é frequentemente descrita como uma espécie de **ASCII art para o layout**.

---

# 12. Colocando `grid-template-areas` dentro de `grid`

A shorthand `grid` aceita a sintaxe de áreas junto com as dimensões das linhas e colunas.

```css
.container {
  display: grid;

  grid:
    "header header" auto
    "main   aside"  1fr
    "footer footer" auto
    / 2fr 1fr;
}
```

A leitura é:

```text
"header header" auto
       │          │
       │          └── tamanho da linha
       └───────────── áreas
```

Depois:

```text
"main aside" 1fr
      │         │
      │         └── tamanho da linha
      └──────────── áreas
```

E:

```text
"footer footer" auto
       │           │
       │           └── tamanho da linha
       └────────────── áreas
```

Finalmente:

```text
/ 2fr 1fr
    │
    └── colunas
```

Visualmente:

```text
┌───────────────────────────────┐
│             HEADER            │
├────────────────────┬──────────┤
│                    │          │
│        MAIN        │  ASIDE   │
│                    │          │
├────────────────────┴──────────┤
│             FOOTER            │
└───────────────────────────────┘
```

### Estrutura mental

```text
"áreas" tamanho-da-linha
"áreas" tamanho-da-linha
"áreas" tamanho-da-linha
--------------------------
/ colunas
```

Essa é a sintaxe que permite descrever o desenho do Grid diretamente na declaração.

---

# 13. `grid-auto-flow`

Agora entramos no comportamento do **Grid implícito**.

`grid-auto-flow` controla como o algoritmo de **auto-placement** distribui os itens que não foram posicionados explicitamente.

Os principais valores são:

```css
grid-auto-flow: row;
grid-auto-flow: column;
grid-auto-flow: row dense;
grid-auto-flow: column dense;
```

Também é possível utilizar:

```css
grid-auto-flow: dense;
```

Nesse caso, `row` é assumido como direção padrão.

O valor inicial de `grid-auto-flow` é:

```css
row
```

---

# 14. `grid-auto-flow: row`

Considere:

```css
.container {
  display: grid;

  grid-template-columns: repeat(3, 1fr);
  grid-auto-flow: row;
}
```

Com seis elementos:

```text
1   2   3
4   5   6
```

O auto-placement percorre o Grid linha por linha:

```text
→ → →
→ → →
```

Primeiro são preenchidas as posições disponíveis da primeira linha.

Depois, o algoritmo segue para a próxima linha.

Quando necessário, novas linhas implícitas são criadas.

```text
1 → 2 → 3
          ↓
4 → 5 → 6
          ↓
novas linhas
```

---

# 15. `grid-auto-flow: column`

Agora:

```css
.container {
  display: grid;

  grid-template-rows: repeat(3, 1fr);
  grid-auto-flow: column;
}
```

O fluxo passa a ocorrer pelas colunas:

```text
1   4
2   5
3   6
```

Visualmente:

```text
↓   ↓
↓   ↓
↓   ↓
```

O primeiro item ocupa a primeira posição da primeira coluna.

Depois o algoritmo continua verticalmente até preencher essa coluna.

Quando necessário, novas colunas implícitas são criadas.

### Corte mental

```text
row
 ↓
enche horizontalmente
 ↓
cria novas linhas

column
 ↓
enche verticalmente
 ↓
cria novas colunas
```

---

# 16. `auto-flow` dentro da shorthand

Uma das características mais importantes de `grid` é que podemos escrever `auto-flow` diretamente na shorthand.

Por exemplo:

```css
grid: auto-flow / 1fr 1fr 1fr;
```

Essa forma configura um fluxo automático pelas linhas.

Conceitualmente:

```css
grid-auto-flow: row;
grid-auto-rows: auto;
grid-template-columns: 1fr 1fr 1fr;
```

A parte mais importante da leitura é:

```text
auto-flow / 1fr 1fr 1fr
    │               │
    │               └── colunas explícitas
    │
    └── fluxo automático por linhas
```

### Regra mental

Quando `auto-flow` está **antes da `/`**:

```text
auto-flow
    ↓
fluxo pelas LINHAS
```

E o lado direito define as colunas explícitas:

```text
/ 1fr 1fr 1fr
       ↓
   colunas explícitas
```

---

# 17. `auto-flow` + `grid-auto-rows`

Podemos definir o tamanho das linhas implícitas diretamente:

```css
.container {
  display: grid;

  grid:
    auto-flow 100px
    / repeat(3, 1fr);
}
```

Essa forma corresponde conceitualmente a:

```css
grid-auto-flow: row;
grid-auto-rows: 100px;
grid-template-columns: repeat(3, 1fr);
```

Visualmente:

```text
┌────────┬────────┬────────┐
│   1    │   2    │   3    │  ← 100px
├────────┼────────┼────────┤
│   4    │   5    │   6    │  ← 100px
├────────┼────────┼────────┤
│   7    │   8    │   9    │  ← 100px
└────────┴────────┴────────┘
```

Se houver itens suficientes para exigir mais linhas:

```text
1   2   3
4   5   6
7   8   9
10  11  12
```

as novas linhas implícitas seguirão o dimensionamento definido por:

```css
grid-auto-rows: 100px;
```

---

# 18. `grid-auto-rows` com múltiplos valores

Também podemos fornecer mais de um valor:

```css
grid:
  auto-flow 100px 200px
  / repeat(3, 1fr);
```

A ideia corresponde ao funcionamento de:

```css
grid-auto-rows: 100px 200px;
```

O padrão de dimensionamento será utilizado conforme novas linhas implícitas forem criadas.

Mentalmente:

```text
Linha implícita 1 → 100px
Linha implícita 2 → 200px
Linha implícita 3 → repete o padrão
Linha implícita 4 → 100px
Linha implícita 5 → 200px
...
```

Portanto, quando usamos uma lista de valores em `grid-auto-rows`, o navegador possui um padrão de dimensionamento para as faixas implícitas sucessivas.

---

# 19. `auto-flow` no lado das colunas

Também podemos inverter a lógica.

```css
grid:
  repeat(3, 100px)
  / auto-flow 150px;
```

Agora:

```text
repeat(3, 100px)
        │
        └── linhas explícitas

auto-flow 150px
     │        │
     │        └── tamanho das colunas automáticas
     │
     └── fluxo pelas colunas
```

Essa forma corresponde conceitualmente a:

```css
grid-template-rows: repeat(3, 100px);

grid-auto-flow: column;
grid-auto-columns: 150px;
```

Visualmente:

```text
┌────┬────┬────┬────┐
│ 1  │ 4  │ 7  │ 10 │
├────┼────┼────┼────┤
│ 2  │ 5  │ 8  │ 11 │
├────┼────┼────┼────┤
│ 3  │ 6  │ 9  │ 12 │
└────┴────┴────┴────┘
 100   150  150  150
```

A primeira coluna pertence à estrutura criada pelo Grid explícito.

As próximas colunas podem ser implícitas, conforme a quantidade de itens.

### Regra mental

Quando `auto-flow` aparece **depois da `/`**:

```text
ROWS EXPLÍCITAS
       /
auto-flow
    ↓
fluxo pelas COLUNAS
```

---

# 20. Comparando as duas formas de `auto-flow`

## 20.1 Fluxo por linhas

```css
grid:
  auto-flow 100px
  / repeat(3, 1fr);
```

Interpretação:

```text
                    COLUNAS
                 1fr  1fr  1fr
                  ↓    ↓    ↓

              ┌────┬────┬────┐
              │ 1  │ 2  │ 3  │ ← 100px
              ├────┼────┼────┤
              │ 4  │ 5  │ 6  │ ← 100px
              ├────┼────┼────┤
              │ 7  │ 8  │ 9  │ ← 100px
              └────┴────┴────┘
                       ↓
                novas linhas
```

Conceitualmente:

```css
grid-auto-flow: row;
grid-auto-rows: 100px;
grid-template-columns: repeat(3, 1fr);
```

---

## 20.2 Fluxo por colunas

```css
grid:
  repeat(3, 100px)
  / auto-flow 150px;
```

Interpretação:

```text
                    ↓
               novas colunas

              ┌────┬────┬────┬────┐
              │ 1  │ 4  │ 7  │ 10 │
              ├────┼────┼────┼────┤
              │ 2  │ 5  │ 8  │ 11 │
              ├────┼────┼────┼────┤
              │ 3  │ 6  │ 9  │ 12 │
              └────┴────┴────┴────┘
                100  150  150  150
```

Conceitualmente:

```css
grid-template-rows: repeat(3, 100px);

grid-auto-flow: column;
grid-auto-columns: 150px;
```

---

# 21. `dense`

Também podemos usar:

```css
dense
```

Por exemplo:

```css
grid-auto-flow: row dense;
```

Ou diretamente na shorthand:

```css
grid:
  auto-flow dense
  / 1fr 1fr 1fr;
```

Também podemos combinar `dense` com uma dimensão automática:

```css
grid:
  auto-flow dense 150px
  / repeat(3, 1fr);
```

O algoritmo `dense` tenta preencher espaços vazios que ficaram anteriormente disponíveis.

Esse comportamento é especialmente relevante quando determinados Grid Items ocupam mais de uma célula.

### Sem `dense`

O algoritmo padrão é chamado de **sparse**.

A ideia é:

```text
o algoritmo avança
       ↓
não volta para preencher
buracos anteriores
```

Isso ajuda a preservar a ordem visual dos itens auto-posicionados, mesmo que apareçam espaços vazios.

### Com `dense`

O algoritmo pode procurar espaços anteriores:

```text
┌────┬────┬────┐
│ A  │ A  │ B  │
├────┼────┼────┤
│ C  │    │    │
└────┴────┴────┘

       ↑
   espaço vazio
```

Um item posterior que couber naquele espaço pode ser colocado ali.

Isso pode produzir uma aparência mais compacta.

### Consequência importante

`dense` pode fazer com que itens apareçam **visualmente fora da ordem esperada**, porque um item posterior pode ser colocado em um espaço anterior.

---

# 22. Construindo um layout completo com `grid`

Considere uma estrutura:

```text
HEADER
────────────────────────────
MAIN              SIDEBAR
────────────────────────────
FOOTER
```

Podemos representar esse layout com áreas nomeadas:

```css
.container {
  display: grid;

  grid:
    "header header" auto
    "main   sidebar" 1fr
    "footer footer" auto
    / 2fr 1fr;
}
```

Depois:

```css
.header {
  grid-area: header;
}

.main {
  grid-area: main;
}

.sidebar {
  grid-area: sidebar;
}

.footer {
  grid-area: footer;
}
```

Resultado conceitual:

```text
┌─────────────────────────────────┐
│             HEADER              │
├──────────────────────┬──────────┤
│                      │          │
│         MAIN         │ SIDEBAR  │
│                      │          │
├──────────────────────┴──────────┤
│             FOOTER              │
└─────────────────────────────────┘
```

Aqui a shorthand está descrevendo:

```text
grid-template-areas
        +
grid-template-rows
        +
grid-template-columns
```

em uma única declaração.

### Leitura estrutural

```text
"header header" auto
        ↓           ↓
      áreas      linha

"main sidebar" 1fr
       ↓         ↓
     áreas     linha

"footer footer" auto
       ↓           ↓
     áreas       linha

/ 2fr 1fr
     ↓
  colunas
```

---

# 23. Construindo um dashboard

Podemos utilizar a mesma ideia para um dashboard:

```css
.dashboard {
  display: grid;

  grid:
    "header header header" 80px
    "menu   main   aside"  1fr
    "footer footer footer" 60px
    / 200px 1fr 250px;
}
```

Visualmente:

```text
┌──────────────┬────────────────────────┬──────────────┐
│                         HEADER                        │
├──────────────┼────────────────────────┼──────────────┤
│              │                        │              │
│     MENU     │          MAIN          │    ASIDE     │
│              │                        │              │
│              │                        │              │
├──────────────┴────────────────────────┴──────────────┤
│                         FOOTER                        │
└──────────────────────────────────────────────────────┘
```

A estrutura pode ser lida assim:

```text
HEADER
3 colunas ocupadas
80px de altura

MENU | MAIN | ASIDE
200px | 1fr | 250px

FOOTER
3 colunas ocupadas
60px de altura
```

A shorthand está descrevendo simultaneamente:

```text
layout visual
+
altura das linhas
+
largura das colunas
```

---

# 24. Construindo uma galeria

Outra aplicação é controlar uma grade de conteúdo repetitivo.

```css
.gallery {
  display: grid;

  grid:
    auto-flow 200px
    / repeat(4, 1fr);
}
```

O resultado conceitual:

```text
┌────┬────┬────┬────┐
│ 01 │ 02 │ 03 │ 04 │
├────┼────┼────┼────┤
│ 05 │ 06 │ 07 │ 08 │
├────┼────┼────┼────┤
│ 09 │ 10 │ 11 │ 12 │
└────┴────┴────┴────┘
```

Temos:

```css
grid-template-columns: repeat(4, 1fr);
grid-auto-flow: row;
grid-auto-rows: 200px;
```

Portanto:

```text
4 colunas explícitas
        +
linhas criadas automaticamente
        +
200px por linha automática
```

---

# 25. Construindo uma lista vertical

Podemos inverter a lógica:

```css
.lista {
  display: grid;

  grid:
    repeat(4, 50px)
    / auto-flow 150px;
}
```

Agora temos:

```css
grid-template-rows: repeat(4, 50px);

grid-auto-flow: column;
grid-auto-columns: 150px;
```

Os itens passam a preencher as colunas:

```text
1   5   9
2   6   10
3   7   11
4   8   12
```

Esse padrão pode ser útil quando a estrutura deve crescer horizontalmente em vez de verticalmente.

---

# 26. Diferença entre `grid-template` e `grid`

Existe uma diferença importante entre:

```css
grid-template
```

e:

```css
grid
```

## `grid-template`

`grid-template` é a shorthand da grade explícita.

Ela configura:

```css
grid-template-rows
grid-template-columns
grid-template-areas
```

Podemos visualizar:

```text
grid-template
      │
      ├── rows
      ├── columns
      └── areas
```

Por isso ela é frequentemente chamada de **explicit grid shorthand**.

---

## `grid`

Já `grid` vai além:

```text
grid
 │
 ├── grid-template-rows
 ├── grid-template-columns
 ├── grid-template-areas
 │
 ├── grid-auto-rows
 ├── grid-auto-columns
 └── grid-auto-flow
```

Ou seja:

```text
grid-template
      ↓
somente estrutura explícita

grid
      ↓
estrutura explícita
      +
comportamento da grade implícita
```

### Uma diferença fundamental

`grid` também possui as formas de `auto-flow`, nas quais uma dimensão é definida explicitamente e a outra é controlada por faixas implícitas criadas automaticamente.

---

# 27. Principais formas da shorthand

## Forma 1 — Linhas e colunas

```css
grid: 100px 200px / 1fr 1fr;
```

Representa:

```css
grid-template-rows: 100px 200px;
grid-template-columns: 1fr 1fr;
```

---

## Forma 2 — Áreas, linhas e colunas

```css
grid:
  "header header" 100px
  "main aside"    1fr
  "footer footer" 80px
  / 2fr 1fr;
```

Representa:

```text
grid-template-areas
grid-template-rows
grid-template-columns
```

---

## Forma 3 — Fluxo automático por linhas

```css
grid:
  auto-flow 100px
  / 1fr 1fr 1fr;
```

Representa conceitualmente:

```css
grid-auto-flow: row;
grid-auto-rows: 100px;
grid-template-columns: 1fr 1fr 1fr;
```

---

## Forma 4 — Fluxo automático por colunas

```css
grid:
  repeat(3, 100px)
  / auto-flow 150px;
```

Representa conceitualmente:

```css
grid-template-rows: repeat(3, 100px);
grid-auto-flow: column;
grid-auto-columns: 150px;
```

---

## Forma 5 — Fluxo automático denso

```css
grid:
  auto-flow dense 150px
  / repeat(3, 1fr);
```

Representa conceitualmente:

```css
grid-auto-flow: row dense;
grid-auto-rows: 150px;
grid-template-columns: repeat(3, 1fr);
```

---

## Forma 6 — Fluxo por colunas com `dense`

Também podemos escrever:

```css
grid:
  repeat(3, 100px)
  / auto-flow dense 150px;
```

Representando:

```css
grid-template-rows: repeat(3, 100px);
grid-auto-flow: column dense;
grid-auto-columns: 150px;
```

---

# 28. Como memorizar a sintaxe

A propriedade `grid` pode ser entendida através de **três modelos centrais**.

## Modelo A — Grid explícito

```css
grid: LINHAS / COLUNAS;
```

Exemplo:

```css
grid: 100px 1fr / 1fr 1fr;
```

Mentalmente:

```text
LINHAS
  ↓
/
  ↓
COLUNAS
```

---

## Modelo B — Grid explícito com áreas

```css
grid:
  "ÁREA ÁREA" TAMANHO
  "ÁREA ÁREA" TAMANHO
  / COLUNAS;
```

Exemplo:

```css
grid:
  "header header" 100px
  "main aside"    1fr
  / 2fr 1fr;
```

Mentalmente:

```text
"desenho da linha" + tamanho da linha
                         ↓
                       linhas
                         ↓
                    /
                         ↓
                      colunas
```

---

## Modelo C — Grid automático por linhas

```css
grid:
  auto-flow TAMANHO-LINHAS
  / COLUNAS;
```

Exemplo:

```css
grid:
  auto-flow 150px
  / repeat(3, 1fr);
```

Mentalmente:

```text
auto-flow
    ↓
fluxo pelas linhas

150px
    ↓
linhas implícitas

repeat(3, 1fr)
    ↓
colunas explícitas
```

---

## Modelo D — Grid automático por colunas

```css
grid:
  LINHAS
  / auto-flow TAMANHO-COLUNAS;
```

Exemplo:

```css
grid:
  repeat(3, 100px)
  / auto-flow 150px;
```

Mentalmente:

```text
repeat(3, 100px)
        ↓
linhas explícitas

auto-flow
        ↓
fluxo pelas colunas

150px
        ↓
colunas implícitas
```

---

# 29. 🧠 Mapa mental definitivo

```text
                              grid
                                │
            ┌───────────────────┴───────────────────┐
            │                                       │
       GRID EXPLÍCITO                         GRID IMPLÍCITO
            │                                       │
      ┌─────┼─────┐                         ┌───────┼────────┐
      │     │     │                         │       │        │
     rows columns areas                 auto-rows auto-columns auto-flow
      │     │     │                         │       │        │
      └─────┴─────┴──────────────┬──────────┴───────┴────────┘
                                 │
                                 ▼
                              `grid`
```

Uma maneira ainda mais útil de pensar:

```text
                    grid
                      │
          ┌───────────┴───────────┐
          │                       │
      ESTRUTURA               AUTOMATIZAÇÃO
          │                       │
      rows / columns          auto-flow
      areas                  auto-rows
                              auto-columns
```

---

# 30. Regra mental para estudar

Quando encontrar:

```css
grid: ...;
```

não tente decorar todos os nomes das propriedades constituintes.

Primeiro observe a estrutura da declaração.

### Passo 1 — Procure a `/`

```text
grid: ... / ...;
```

A barra divide as duas partes da sintaxe.

---

### Passo 2 — Procure `auto-flow`

```text
Existe auto-flow?
       │
   ┌───┴───┐
   │       │
  NÃO     SIM
   │       │
   ▼       ▼
forma     observe
explícita o lado
```

---

### Passo 3 — Se não houver `auto-flow`

Exemplo:

```css
grid: 100px 1fr / 1fr 2fr;
```

Pense:

```text
ANTES DA /
    ↓
LINHAS

DEPOIS DA /
    ↓
COLUNAS
```

---

### Passo 4 — Se houver `auto-flow` antes da `/`

Exemplo:

```css
grid:
  auto-flow 150px
  / 1fr 1fr;
```

Pense:

```text
auto-flow
    ↓
fluxo pelas LINHAS

150px
    ↓
tamanho das LINHAS implícitas

1fr 1fr
    ↓
COLUNAS explícitas
```

---

### Passo 5 — Se houver `auto-flow` depois da `/`

Exemplo:

```css
grid:
  repeat(3, 100px)
  / auto-flow 150px;
```

Pense:

```text
repeat(3, 100px)
        ↓
LINHAS explícitas

auto-flow
        ↓
fluxo pelas COLUNAS

150px
        ↓
tamanho das COLUNAS implícitas
```

---

### Passo 6 — Se aparecerem strings entre aspas

Exemplo:

```css
grid:
  "header header" auto
  "main aside"    1fr
  / 2fr 1fr;
```

Pense:

```text
"header header"
        ↓
ÁREAS

auto
        ↓
tamanho da linha

"main aside"
        ↓
ÁREAS

1fr
        ↓
tamanho da linha

/
        ↓

2fr 1fr
        ↓
COLUNAS
```

---

# 31. Ponto importante sobre os resets da shorthand

Existe um detalhe fundamental das shorthands CSS.

Quando utilizamos:

```css
grid: ...;
```

as propriedades constituintes da shorthand que não forem definidas pela forma utilizada são colocadas em seus valores iniciais.

Por exemplo:

```css
.container {
  grid-auto-rows: 200px;

  grid: 100px / 1fr 1fr;
}
```

A declaração `grid` não deve ser tratada como se estivesse apenas "acrescentando" valores aos estilos anteriores.

Ela é uma shorthand e, portanto, pode redefinir propriedades relacionadas.

### Consequência

Ao escrever:

```css
grid: 100px / 1fr 1fr;
```

essa forma está configurando a parte explícita:

```css
grid-template-rows: 100px;
grid-template-columns: 1fr 1fr;
```

e as demais propriedades constituintes da shorthand assumem seus valores iniciais conforme a forma sintática utilizada.

Por exemplo:

```css
grid-auto-rows: auto;
grid-auto-columns: auto;
grid-auto-flow: row;
```

quando essas propriedades não são determinadas por aquela forma.

### Porém, `gap` é diferente

As propriedades:

```css
gap
row-gap
column-gap
```

não fazem parte das seis propriedades constituintes de `grid`.

Portanto, `grid` **não deve ser utilizado como shorthand de `gap`**.

Para controlar espaçamento entre faixas, utilizamos:

```css
gap
```

ou:

```css
row-gap
column-gap
```

Exemplo:

```css
.container {
  display: grid;

  grid:
    100px 1fr
    / 1fr 1fr;

  gap: 20px;
}
```

### Corte mental

```text
grid
 ↓
estrutura do Grid
 ↓
explicit + implicit grid

gap
 ↓
espaçamento entre tracks
```

São responsabilidades diferentes.

---

# 32. Resumo definitivo

A propriedade:

```css
grid
```

é uma shorthand para:

```text
grid-template-rows
grid-template-columns
grid-template-areas
grid-auto-rows
grid-auto-columns
grid-auto-flow
```

Mas a shorthand não é escrita colocando esses seis nomes em sequência.

Ela possui formas sintáticas próprias.

## Forma explícita

```css
grid: ROWS / COLUMNS;
```

Mentalmente:

```text
ANTES DA /
    ↓
LINHAS

DEPOIS DA /
    ↓
COLUNAS
```

---

## Forma com áreas

```css
grid:
  "A A" TAMANHO
  "B C" TAMANHO
  / COLUNAS;
```

Mentalmente:

```text
áreas
  ↓
tamanho das linhas
  ↓
/
  ↓
colunas
```

---

## Fluxo automático por linhas

```css
grid:
  auto-flow TAMANHO
  / COLUNAS;
```

Mentalmente:

```text
auto-flow
    ↓
row

TAMANHO
    ↓
grid-auto-rows

COLUNAS
    ↓
grid-template-columns
```

---

## Fluxo automático por colunas

```css
grid:
  LINHAS
  / auto-flow TAMANHO;
```

Mentalmente:

```text
LINHAS
    ↓
grid-template-rows

auto-flow
    ↓
column

TAMANHO
    ↓
grid-auto-columns
```

---

## `dense`

```css
auto-flow dense
```

significa que o algoritmo pode tentar preencher lacunas anteriores no Grid, podendo alterar a ordem visual de alguns itens auto-posicionados.

---

# 🧠 Regra de ouro

Sempre que encontrar:

```css
grid: ...;
```

pense nesta sequência:

```text
                  grid
                    │
                    ▼
             Procure a "/"
                    │
             ┌──────┴──────┐
             │             │
          sem           com
       auto-flow      auto-flow
             │             │
             ▼             ▼
        rows / cols    descubra o lado
                           │
                    ┌──────┴──────┐
                    │             │
                 antes          depois
                    │             │
                    ▼             ▼
                  row           column
```

Depois observe se existem:

```text
"áreas"
```

ou:

```text
TAMANHOS
```

ou:

```text
dense
```

Assim, `grid` deixa de ser uma shorthand difícil de decorar e passa a ser uma forma compacta de descrever:

```text
┌────────────────────────────────────────┐
│             ESTRUTURA                  │
│                                        │
│ rows                                   │
│ columns                                │
│ areas                                  │
├────────────────────────────────────────┤
│             COMPORTAMENTO              │
│                                        │
│ auto-flow                              │
│ auto-rows                              │
│ auto-columns                           │
└────────────────────────────────────────┘
```

### Síntese final

```text
grid
 │
 ├── explicita a estrutura
 │      ├── rows
 │      ├── columns
 │      └── areas
 │
 └── controla a criação automática
        ├── auto-flow
        ├── auto-rows
        └── auto-columns
```

A ideia central não é decorar uma lista de propriedades.

É reconhecer **qual forma da shorthand está sendo utilizada**.

```text
grid: ROWS / COLUMNS

grid:
  "AREAS" ROW-SIZE
  / COLUMNS

grid:
  auto-flow AUTO-ROWS
  / COLUMNS

grid:
  ROWS
  / auto-flow AUTO-COLUMNS
```

Esses quatro padrões são o núcleo prático para interpretar a maioria dos usos de `grid`.

---

# Referências

- [MDN — `grid`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid)
- [MDN — `grid-template`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-template)
- [MDN — `grid-template-areas`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-template-areas)
- [MDN — `grid-auto-flow`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-auto-flow)
- [MDN — Grid template areas](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout/Grid_template_areas)
- [W3C — CSS Grid Layout Module Level 2](https://www.w3.org/TR/css-grid/)
