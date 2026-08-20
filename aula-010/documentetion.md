# CSS Grid — Propriedade `grid`

A propriedade `grid` é uma **shorthand**, ou seja, uma propriedade abreviada capaz de configurar diferentes propriedades do CSS Grid em uma única declaração.

Ela pode representar as seguintes propriedades:

```css
grid-template-rows
grid-template-columns
grid-template-areas

grid-auto-rows
grid-auto-columns
grid-auto-flow
```

Essas propriedades podem ser divididas em dois grupos:

```text
GRID
│
├── Grid explícito
│   ├── grid-template-rows
│   ├── grid-template-columns
│   └── grid-template-areas
│
└── Grid implícito
    ├── grid-auto-rows
    ├── grid-auto-columns
    └── grid-auto-flow
```

A documentação do MDN define `grid` justamente como a shorthand que configura as propriedades da grade explícita e implícita.

---

# 1. Antes de entender `grid`, entenda os dois tipos de Grid

Para compreender a shorthand `grid`, primeiro precisamos separar duas ideias:

* **Grid explícito**
* **Grid implícito**

## Grid explícito

É a estrutura que definimos diretamente.

Por exemplo:

```css
.container {
  display: grid;

  grid-template-columns: 1fr 1fr;
  grid-template-rows: 100px 200px;
}
```

Aqui estamos dizendo explicitamente:

```text
┌───────────────┬───────────────┐
│               │               │  ← 100px
├───────────────┼───────────────┤
│               │               │  ← 200px
└───────────────┴───────────────┘
      1fr              1fr
```

Nós decidimos:

* quantas colunas existem;
* quantas linhas existem;
* o tamanho de cada uma.

---

## Grid implícito

Agora imagine que existem mais elementos do que células disponíveis:

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

E temos:

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr;
}
```

Definimos apenas duas colunas.

O Grid precisará criar novas linhas para acomodar os elementos restantes.

Essas linhas adicionais são **implícitas**.

É nesse contexto que entram:

```css
grid-auto-rows
grid-auto-columns
grid-auto-flow
```

---

# 2. As seis propriedades que `grid` pode configurar

A shorthand `grid` trabalha com seis propriedades principais:

| Grupo     | Propriedade             | Função                                       |
| --------- | ----------------------- | -------------------------------------------- |
| Explícito | `grid-template-rows`    | Define as linhas                             |
| Explícito | `grid-template-columns` | Define as colunas                            |
| Explícito | `grid-template-areas`   | Define áreas nomeadas                        |
| Implícito | `grid-auto-rows`        | Define o tamanho das linhas automáticas      |
| Implícito | `grid-auto-columns`     | Define o tamanho das colunas automáticas     |
| Implícito | `grid-auto-flow`        | Define a direção do preenchimento automático |

A grande vantagem de `grid` é poder trabalhar com esses dois grupos através de uma única propriedade.

---

# 3. Primeira forma: `grid-template-rows / grid-template-columns`

A forma mais simples da shorthand é:

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
       ───────┬─────┬──────
              │     │
            rows  columns
```

Ou seja:

```css
grid-template-rows: 100px 200px;
grid-template-columns: 1fr 1fr;
```

---

# 4. `grid-template-rows`

`grid-template-rows` define as **faixas de linhas explícitas** do Grid. Ela aceita valores como `px`, `%`, `fr`, `auto`, `minmax()`, `repeat()` e outros valores de dimensionamento.

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

Temos:

```text
┌──────────────────────────┐
│                          │
│          100px           │
├──────────────────────────┤
│                          │
│          200px           │
├──────────────────────────┤
│                          │
│          100px           │
└──────────────────────────┘
```

Na shorthand:

```css
.container {
  display: grid;
  grid: 100px 200px 100px / 1fr;
}
```

Aqui:

```text
grid-template-rows
        ↓
100px 200px 100px
        │
        ▼
       "/"
        │
        ▼
grid-template-columns
        ↓
       1fr
```

---

# 5. `grid-template-columns`

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

---

# 6. A importância da `/`

A barra `/` é fundamental.

Ela separa:

```css
grid: ROWS / COLUMNS;
```

Exemplo:

```css
grid: 100px 200px / 1fr 2fr 1fr;
```

Significa:

```css
grid-template-rows: 100px 200px;

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
100px     │        │          │        │
          ├────────┼──────────┼────────┤
200px     │        │          │        │
          └────────┴──────────┴────────┘
              ↑
            LINHAS
```

Portanto:

> **Antes da `/` → linhas. Depois da `/` → colunas.**

---

# 7. Usando `fr`

A unidade `fr` representa uma fração do espaço disponível.

Por exemplo:

```css
grid: 100px / 1fr 1fr;
```

Temos duas colunas que dividem o espaço restante igualmente:

```text
┌──────────────────┬──────────────────┐
│                  │                  │
│       1fr        │       1fr        │
│                  │                  │
└──────────────────┴──────────────────┘
```

Já:

```css
grid: 100px / 1fr 2fr;
```

produz uma divisão proporcional:

```text
┌───────────────┬─────────────────────────────┐
│      1fr      │             2fr             │
└───────────────┴─────────────────────────────┘
```

A segunda coluna recebe aproximadamente o dobro do espaço da primeira.

---

# 8. Usando `repeat()`

Podemos combinar `grid` com `repeat()`.

```css
.container {
  display: grid;
  grid: repeat(3, 100px) / repeat(4, 1fr);
}
```

Isso significa:

```css
grid-template-rows:
  100px 100px 100px;

grid-template-columns:
  1fr 1fr 1fr 1fr;
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

---

# 9. Usando `minmax()`

Também podemos utilizar:

```css
grid: minmax(100px, 1fr) / 1fr 2fr;
```

Aqui a linha pode ter:

```text
mínimo → 100px
máximo → 1fr
```

Uma aplicação prática:

```css
.container {
  display: grid;
  grid: minmax(150px, auto) / 1fr 1fr;
}
```

Isso é útil quando queremos evitar que determinada área fique pequena demais, mas ainda permitir que ela cresça de acordo com o conteúdo.

---

# 10. `grid-template-areas`

Além de tamanhos, podemos construir layouts utilizando **áreas nomeadas**.

Por exemplo:

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
│            HEADER             │
├────────────────────┬──────────┤
│                    │          │
│        MAIN        │  ASIDE   │
│                    │          │
├────────────────────┴──────────┤
│            FOOTER             │
└───────────────────────────────┘
```

Os elementos podem então receber:

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

---

# 11. Colocando `grid-template-areas` dentro de `grid`

A shorthand `grid` também aceita essa estrutura:

```css
.container {
  display: grid;

  grid:
    "header header" auto
    "main aside" 1fr
    "footer footer" auto
    / 2fr 1fr;
}
```

A ordem é extremamente importante.

Podemos ler:

```text
"header header"  auto
        ↑          ↑
      áreas      altura
```

Depois:

```text
"main aside"     1fr
     ↑             ↑
   áreas         altura
```

Depois:

```text
"footer footer" auto
      ↑           ↑
    áreas       altura
```

E finalmente:

```text
/ 2fr 1fr
     ↑
  colunas
```

A sintaxe de áreas funciona como uma espécie de **desenho do layout em texto**. O MDN utiliza exatamente essa abordagem para construir layouts completos.

---

# 12. `grid-auto-flow`

Agora entramos na parte implícita da shorthand.

`grid-auto-flow` controla **como os itens posicionados automaticamente percorrem o Grid**.

Os principais valores são:

```css
grid-auto-flow: row;
grid-auto-flow: column;
grid-auto-flow: row dense;
grid-auto-flow: column dense;
```

O valor padrão é:

```css
grid-auto-flow: row;
```

---

# 13. `grid-auto-flow: row`

Imagine:

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

O preenchimento ocorre:

```text
→ → →
→ → →
```

Primeiro completa uma linha e depois passa para a próxima.

---

# 14. `grid-auto-flow: column`

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

O preenchimento acontece:

```text
↓ ↓
↓ ↓
↓ ↓
```

Esse comportamento é especialmente útil quando queremos controlar a direção em que novos itens são adicionados ao Grid.

---

# 15. `auto-flow` dentro da shorthand

Aqui está uma das partes mais importantes da propriedade `grid`.

Podemos escrever:

```css
grid: auto-flow / 1fr 1fr 1fr;
```

Isso significa:

```css
grid-auto-flow: row;
grid-auto-rows: auto;
grid-template-columns: 1fr 1fr 1fr;
```

Ou seja:

```text
auto-flow / 1fr 1fr 1fr
    │              │
    │              └── colunas explícitas
    │
    └── fluxo automático pelas linhas
```

O MDN descreve essa forma como a configuração de um fluxo automático de linhas, enquanto as colunas são definidas explicitamente.

---

# 16. `auto-flow` + `grid-auto-rows`

Podemos definir também o tamanho das linhas automáticas:

```css
.container {
  display: grid;

  grid:
    auto-flow 100px
    / repeat(3, 1fr);
}
```

Isso equivale conceitualmente a:

```css
grid-auto-flow: row;
grid-auto-rows: 100px;
grid-template-columns: repeat(3, 1fr);
```

Imagine:

```text
┌────────┬────────┬────────┐
│   1    │   2    │   3    │  ← 100px
├────────┼────────┼────────┤
│   4    │   5    │   6    │  ← 100px
├────────┼────────┼────────┤
│   7    │   8    │   9    │  ← 100px
└────────┴────────┴────────┘
```

Se novos elementos aparecerem, novas linhas implícitas serão criadas seguindo a configuração de `grid-auto-rows`.

---

# 17. `auto-flow` + `grid-auto-rows` com valores diferentes

Também podemos fornecer vários tamanhos:

```css
grid:
  auto-flow 100px 200px
  / repeat(3, 1fr);
```

Isso permite configurar a dimensão das linhas automáticas através de uma lista de valores.

A ideia é:

```text
Linha automática 1 → 100px
Linha automática 2 → 200px
Linha automática 3 → ...
```

Essa lógica corresponde ao uso de `grid-auto-rows`, que define o tamanho das faixas de linha criadas automaticamente.

---

# 18. `auto-flow` no lado das colunas

Também podemos inverter a lógica:

```css
grid:
  repeat(3, 100px)
  / auto-flow 150px;
```

Agora temos:

```text
repeat(3, 100px)
        │
        └── linhas explícitas

auto-flow 150px
     │       │
     │       └── tamanho das colunas automáticas
     │
     └── fluxo pelas colunas
```

Isso equivale conceitualmente a:

```css
grid-template-rows: repeat(3, 100px);

grid-auto-flow: column;
grid-auto-columns: 150px;
```

Essa é justamente uma das formas previstas na sintaxe oficial da shorthand `grid`.

---

# 19. Comparando as duas formas de `auto-flow`

## Fluxo por linhas

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

## Fluxo por colunas

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

---

# 20. `dense`

Podemos adicionar:

```css
dense
```

Por exemplo:

```css
grid-auto-flow: row dense;
```

ou:

```css
grid: auto-flow dense / 1fr 1fr 1fr;
```

O `dense` permite que o algoritmo de posicionamento tente preencher espaços vazios que surgiram anteriormente.

Isso pode ser útil em layouts nos quais alguns elementos ocupam mais de uma célula.

---

# 21. Construindo um layout completo com `grid`

Imagine um site com:

```text
HEADER
────────────────────────────
MAIN             SIDEBAR
────────────────────────────
FOOTER
```

Podemos construir isso usando:

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

O resultado conceitual é:

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

Aqui a shorthand está definindo:

```text
grid-template-areas
        +
grid-template-rows
        +
grid-template-columns
```

em uma única declaração.

---

# 22. Construindo um dashboard

Podemos ir além:

```css
.dashboard {
  display: grid;

  grid:
    "header header header" 80px
    "menu   main   aside" 1fr
    "footer footer footer" 60px
    / 200px 1fr 250px;
}
```

Temos:

```text
┌──────────────┬────────────────────────┬──────────────┐
│                       HEADER                         │
├──────────────┼────────────────────────┼──────────────┤
│              │                        │              │
│     MENU     │          MAIN          │    ASIDE     │
│              │                        │              │
│              │                        │              │
├──────────────┴────────────────────────┴──────────────┤
│                       FOOTER                         │
└──────────────────────────────────────────────────────┘
```

A estrutura é:

```text
HEADER
3 colunas ocupadas
80px de altura

MENU | MAIN | ASIDE
200px | 1fr | 250px
altura: 1fr

FOOTER
3 colunas ocupadas
60px de altura
```

---

# 23. Construindo uma galeria

Outra aplicação:

```css
.gallery {
  display: grid;

  grid:
    auto-flow 200px
    / repeat(4, 1fr);
}
```

Isso cria:

```text
┌────┬────┬────┬────┐
│ 01 │ 02 │ 03 │ 04 │
├────┼────┼────┼────┤
│ 05 │ 06 │ 07 │ 08 │
├────┼────┼────┼────┤
│ 09 │ 10 │ 11 │ 12 │
└────┴────┴────┴────┘

      ↑
   200px
```

As colunas são:

```css
1fr 1fr 1fr 1fr
```

E as linhas criadas automaticamente possuem:

```css
200px
```

---

# 24. Construindo uma lista vertical

Podemos inverter:

```css
.lista {
  display: grid;

  grid:
    repeat(4, 50px)
    / auto-flow 150px;
}
```

Agora o preenchimento acontece pelas colunas.

Isso pode ser útil quando queremos criar estruturas que crescem horizontalmente.

---

# 25. A diferença entre `grid-template` e `grid`

Existe uma diferença importante entre:

```css
grid-template
```

e:

```css
grid
```

`grid-template` trabalha com:

```css
grid-template-rows
grid-template-columns
grid-template-areas
```

Já `grid` também trabalha com:

```css
grid-auto-rows
grid-auto-columns
grid-auto-flow
```

Ou seja:

```text
grid-template
│
├── rows
├── columns
└── areas


grid
│
├── rows
├── columns
├── areas
│
├── auto-rows
├── auto-columns
└── auto-flow
```

Além disso, a shorthand `grid` redefine as propriedades que não são especificadas, enquanto `grid-template` é focada na grade explícita. Essa diferença é importante quando existem regras de Grid herdadas de outras declarações CSS.

---

# 26. As principais formas da shorthand

## Forma 1 — linhas e colunas

```css
grid: 100px 200px / 1fr 1fr;
```

Equivale a:

```css
grid-template-rows: 100px 200px;
grid-template-columns: 1fr 1fr;
```

---

## Forma 2 — áreas, linhas e colunas

```css
grid:
  "header header" 100px
  "main aside" 1fr
  "footer footer" 80px
  / 2fr 1fr;
```

Configura:

```text
grid-template-areas
grid-template-rows
grid-template-columns
```

---

## Forma 3 — fluxo automático por linhas

```css
grid:
  auto-flow 100px
  / 1fr 1fr 1fr;
```

Configura:

```text
grid-auto-flow: row
grid-auto-rows: 100px
grid-template-columns: 1fr 1fr 1fr
```

---

## Forma 4 — fluxo automático por colunas

```css
grid:
  repeat(3, 100px)
  / auto-flow 150px;
```

Configura:

```text
grid-template-rows: repeat(3, 100px)
grid-auto-flow: column
grid-auto-columns: 150px
```

---

## Forma 5 — fluxo automático denso

```css
grid:
  auto-flow dense 150px
  / repeat(3, 1fr);
```

Configura um fluxo automático pelas linhas utilizando o algoritmo `dense`.

---

# 27. Uma maneira simples de memorizar

A shorthand `grid` pode ser entendida através de **três modelos principais**.

### Modelo A — Grid explícito

```css
grid: LINHAS / COLUNAS;
```

Exemplo:

```css
grid: 100px 1fr / 1fr 1fr;
```

---

### Modelo B — Grid explícito com áreas

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
  "main aside" 1fr
  / 2fr 1fr;
```

---

### Modelo C — Grid automático

```css
grid:
  auto-flow TAMANHO-LINHAS
  / COLUNAS;
```

ou:

```css
grid:
  LINHAS
  / auto-flow TAMANHO-COLUNAS;
```

Essas formas correspondem à sintaxe formal da propriedade `grid` especificada pelo CSS Grid.

---

# 🧠 Mapa mental definitivo

```text
                            grid
                             │
             ┌───────────────┴───────────────┐
             │                               │
        GRID EXPLÍCITO                  GRID IMPLÍCITO
             │                               │
     ┌───────┼───────┐               ┌───────┼───────┐
     │       │       │               │       │       │
   rows   columns  areas          auto-rows auto-columns auto-flow
     │       │       │               │       │       │
     │       │       │               │       │       │
     └───────┴───────┴───────────────┴───────┴───────┘
                             │
                             ▼
                       propriedade
                          `grid`
```

---

# 📌 Ordem dos valores

## Linhas → colunas

```css
grid: ROWS / COLUMNS;
```

Exemplo:

```css
grid: 100px 200px / 1fr 2fr;
```

---

## Áreas → tamanho das linhas → colunas

```css
grid:
  "A A" TAMANHO
  "B C" TAMANHO
  / COLUNAS;
```

Exemplo:

```css
grid:
  "header header" 100px
  "main aside" 1fr
  / 2fr 1fr;
```

---

## Fluxo automático pelas linhas

```css
grid:
  auto-flow TAMANHO
  / COLUNAS;
```

Exemplo:

```css
grid:
  auto-flow 150px
  / repeat(3, 1fr);
```

---

## Fluxo automático pelas colunas

```css
grid:
  LINHAS
  / auto-flow TAMANHO;
```

Exemplo:

```css
grid:
  repeat(3, 100px)
  / auto-flow 150px;
```

---

# ⚠️ Ponto importante

Não devemos pensar que existe uma única ordem em que obrigatoriamente precisamos escrever os seis nomes:

```css
grid:
  grid-template-rows
  grid-template-columns
  grid-template-areas
  grid-auto-rows
  grid-auto-columns
  grid-auto-flow;
```

**Não é assim que a shorthand é escrita.**

Esses são os **componentes que `grid` consegue configurar**, mas a sintaxe da shorthand possui formas específicas.

As principais são:

```css
grid: ROWS / COLUMNS;
```

```css
grid:
  AREAS TAMANHO-DAS-LINHAS
  / COLUMNS;
```

```css
grid:
  auto-flow TAMANHO-DAS-LINHAS
  / COLUMNS;
```

```css
grid:
  ROWS
  / auto-flow TAMANHO-DAS-COLUNAS;
```

Essa distinção é fundamental para não decorar a propriedade de maneira errada.

---

# 🎯 Regra mental para estudar

Quando encontrar:

```css
grid: ...;
```

primeiro procure a `/`.

Depois pergunte:

```text
Existe auto-flow?
        │
   ┌────┴────┐
  NÃO       SIM
   │          │
   ▼          ▼
ROWS/COLUMNS  Qual lado?
              │
        ┌─────┴─────┐
        │           │
       antes       depois
        │           │
        ▼           ▼
      row         column
```

Se não houver `auto-flow`:

```css
grid: 100px 1fr / 1fr 2fr;
```

pense:

```text
ANTES DA /
     ↓
LINHAS

DEPOIS DA /
     ↓
COLUNAS
```

Se houver `auto-flow`:

```css
grid: auto-flow 150px / 1fr 1fr;
```

pense:

```text
auto-flow
    ↓
fluxo automático

150px
    ↓
tamanho das linhas automáticas

1fr 1fr
    ↓
colunas explícitas
```

E:

```css
grid: repeat(3, 100px) / auto-flow 150px;
```

pense:

```text
repeat(3, 100px)
        ↓
linhas explícitas

auto-flow
        ↓
fluxo pelas colunas

150px
        ↓
colunas automáticas
```

Assim, a propriedade `grid` deixa de ser apenas uma abreviação difícil de decorar e passa a ser uma forma compacta de **descrever a estrutura explícita e o comportamento automático de um layout Grid**.
