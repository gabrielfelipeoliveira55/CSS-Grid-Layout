# CSS Grid — `grid-template-rows`

A propriedade `grid-template-rows` define as **linhas explícitas** de um Grid e especifica como cada uma dessas linhas deve ser dimensionada.

Ela trabalha em conjunto com `grid-template-columns`, permitindo controlar a estrutura bidimensional do Grid:

```text
grid-template-columns
          ↓
       COLUNAS

grid-template-rows
          ↓
        LINHAS
```

> **Ideia central**
>
> `grid-template-columns` define a estrutura das colunas.
>
> `grid-template-rows` define a estrutura das linhas.

---

## 📚 Índice

- [1. O que é `grid-template-rows`?](#1-o-que-é-grid-template-rows)
- [2. Linhas explícitas e linhas implícitas](#2-linhas-explícitas-e-linhas-implícitas)
- [3. Definindo linhas explicitamente](#3-definindo-linhas-explicitamente)
- [4. Quando apenas algumas linhas são definidas](#4-quando-apenas-algumas-linhas-são-definidas)
- [5. Linhas automáticas e o conteúdo](#5-linhas-automáticas-e-o-conteúdo)
- [6. Quando o conteúdo é maior que a linha](#6-quando-o-conteúdo-é-maior-que-a-linha)
- [7. Linhas, faixas, células e itens](#7-linhas-faixas-células-e-itens)
- [8. `fr` em `grid-template-rows`](#8-fr-em-grid-template-rows)
- [9. `grid-template-columns` + `grid-template-rows`](#9-grid-template-columns--grid-template-rows)
- [10. Células vazias](#10-células-vazias)
- [11. Fluxo automático dos itens](#11-fluxo-automático-dos-itens)
- [12. Grid Lines e posicionamento](#12-grid-lines-e-posicionamento)
- [13. `repeat()` em `grid-template-rows`](#13-repeat-em-grid-template-rows)
- [14. `minmax()` em `grid-template-rows`](#14-minmax-em-grid-template-rows)
- [15. Valores aceitos](#15-valores-aceitos)
- [16. Exemplo completo](#16-exemplo-completo)
- [17. Mapas mentais](#17-mapas-mentais)
- [18. Pontos essenciais](#18-pontos-essenciais)
- [19. Resumo final](#19-resumo-final)
- [20. Referências](#20-referências)

---

## 1. O que é `grid-template-rows`?

`grid-template-rows` é uma propriedade aplicada ao **Grid Container** para definir os tamanhos das linhas que pertencem ao **Grid explícito**.

Exemplo:

```css
.grid {
  display: grid;

  grid-template-rows:
    50px
    100px
    200px;
}
```

Nesse caso, foram definidas três linhas:

```text
Linha 1 → 50px
Linha 2 → 100px
Linha 3 → 200px
```

A quantidade de valores declarados determina a quantidade de **faixas de linha explícitas**.

### Relação com `grid-template-columns`

As duas propriedades funcionam com o mesmo princípio, mas controlam dimensões diferentes:

```text
grid-template-columns
          ↓
       COLUNAS
          ↓
   estrutura no eixo das colunas


grid-template-rows
          ↓
        LINHAS
          ↓
    estrutura no eixo das linhas
```

No sistema de escrita horizontal mais comum:

```text
COLUMN
←──────→
largura

ROW
↕
altura
```

> Essa associação com largura e altura é uma forma prática de visualizar o conceito. Tecnicamente, Grid trabalha com eixos e *writing modes*, portanto o significado de "horizontal" e "vertical" pode variar conforme a escrita utilizada.

---

## 2. Linhas explícitas e linhas implícitas

Uma das características importantes do CSS Grid é que **não precisamos declarar todas as linhas manualmente**.

Quando alguns itens precisam de mais espaço do que o Grid explícito fornece, o Grid pode criar **linhas implícitas** automaticamente.

Por exemplo:

```css
.grid {
  display: grid;

  grid-template-columns:
    1fr
    1fr;
}
```

Nenhum `grid-template-rows` foi declarado.

Mesmo assim, os itens podem ser organizados em várias linhas:

```text
┌──────────┬──────────┐
│ Item 1   │ Item 2   │
├──────────┼──────────┤
│ Item 3   │ Item 4   │
├──────────┼──────────┤
│ Item 5   │ Item 6   │
└──────────┴──────────┘
```

As linhas necessárias podem ser criadas automaticamente pelo Grid.

Nesse cenário:

```text
grid-template-columns
        ↓
    2 colunas

grid-template-rows
        ↓
 não foi definido

linhas adicionais
        ↓
 criadas implicitamente
```

O tamanho dessas linhas implícitas é controlado por `grid-auto-rows`. Por padrão, o valor de `grid-auto-rows` é `auto`.

> **Importante**
>
> `grid-template-rows` não é obrigatório para que existam linhas.
>
> Ele é utilizado quando queremos definir explicitamente a estrutura e o dimensionamento das linhas.

---

## 3. Definindo linhas explicitamente

Podemos definir quantas linhas explícitas queremos e o tamanho de cada uma:

```css
.grid {
  display: grid;

  grid-template-columns:
    1fr
    1fr;

  grid-template-rows:
    50px
    100px
    50px
    200px;
}
```

O resultado conceitual é:

```text
┌──────────┬──────────┐
│          │          │ 50px
├──────────┼──────────┤
│          │          │ 100px
├──────────┼──────────┤
│          │          │ 50px
├──────────┼──────────┤
│          │          │ 200px
└──────────┴──────────┘
```

Portanto:

```text
Linha 1 → 50px
Linha 2 → 100px
Linha 3 → 50px
Linha 4 → 200px
```

Cada valor representa uma **faixa de linha** do Grid explícito.

---

## 4. Quando apenas algumas linhas são definidas

Também podemos declarar apenas uma parte da estrutura:

```css
.grid {
  display: grid;

  grid-template-rows:
    100px
    100px;
}
```

Isso define explicitamente:

```text
Linha 1 → 100px
Linha 2 → 100px
```

Se forem necessárias mais linhas para acomodar os itens, elas poderão ser criadas como linhas implícitas.

Visualmente:

```text
┌──────────┬──────────┐
│          │          │ 100px
├──────────┼──────────┤
│          │          │ 100px
├──────────┼──────────┤
│          │          │ automática
├──────────┼──────────┤
│          │          │ automática
└──────────┴──────────┘
```

As linhas que não pertencem ao Grid explícito passam a fazer parte do **Grid implícito**.

---

## 5. Linhas automáticas e o conteúdo

Quando uma linha implícita é criada, seu tamanho é determinado pela configuração de `grid-auto-rows` e, no valor padrão `auto`, pelo algoritmo de dimensionamento do Grid e pelo conteúdo.

Considere:

```css
.grid {
  display: grid;

  grid-template-rows:
    100px
    100px;
}
```

Se uma terceira linha for necessária, ela poderá assumir uma altura adequada ao seu conteúdo.

Por exemplo:

```text
┌──────────┬──────────┐
│ Item 1   │ Item 2   │ 100px
├──────────┼──────────┤
│ Item 3   │ Item 4   │ 100px
├──────────┼──────────┤
│ texto    │ texto    │
│ grande   │ grande   │ ← linha implícita
│          │          │
└──────────┴──────────┘
```

Por isso, existe uma diferença importante entre:

```css
grid-template-rows: 100px 100px;
```

e:

```css
grid-auto-rows: 100px;
```

O primeiro define **linhas explícitas**.

O segundo define como as **linhas implícitas** serão dimensionadas.

---

## 6. Quando o conteúdo é maior que a linha

Definir uma altura fixa para uma linha não significa que o conteúdo será automaticamente reduzido para caber nela.

Exemplo:

```css
.grid {
  display: grid;

  grid-template-rows:
    50px;
}
```

A primeira linha foi definida com `50px`.

```text
┌─────────────────────┐
│                     │
│      conteúdo       │
│      muito grande   │
│      para a linha   │
│                     │
└─────────────────────┘
      ↑
   50px de faixa
```

Se o conteúdo exigir mais espaço, ele não transforma automaticamente os `50px` em uma altura maior apenas porque não cabe.

Dependendo das demais regras aplicadas ao Grid e ao item, o conteúdo pode ultrapassar visualmente a área da faixa.

> **Regra mental**
>
> `grid-template-rows: 50px` significa:
>
> **"Defina essa faixa do Grid com 50px."**
>
> Não significa:
>
> **"Faça todo o conteúdo caber em 50px."**

O mesmo princípio vale para colunas:

```css
grid-template-columns: 100px;
```

define uma faixa de coluna com `100px`, mas não garante que qualquer conteúdo ficará visualmente limitado a essa largura.

---

## 7. Linhas, faixas, células e itens

Para compreender Grid de forma sólida, é importante diferenciar alguns conceitos.

### 7.1 Grid Line

Uma **Grid Line** é uma linha estrutural que delimita as faixas do Grid.

Exemplo com três linhas:

```text
Linha 1
  ↓
┌──────────┐
│  Row 1   │
├──────────┤
│  Row 2   │
├──────────┤
│  Row 3   │
└──────────┘
  ↑
Linha 4
```

Observe que:

```text
3 rows
↓
4 grid lines
```

As Grid Lines ficam nas divisões da grade.

### 7.2 Grid Row

Uma **Grid Row** é a faixa existente entre duas Grid Lines.

No exemplo:

```text
Linha 1
  ↓
┌──────────┐
│  Row 1   │
├──────────┤
│  Row 2   │
├──────────┤
│  Row 3   │
└──────────┘
            ↑
         Linha 4
```

Temos:

```text
Linha 1 ────── Linha 2
       ↓
     Row 1

Linha 2 ────── Linha 3
       ↓
     Row 2

Linha 3 ────── Linha 4
       ↓
     Row 3
```

### 7.3 Grid Cell

Uma **Grid Cell** é o espaço formado pela interseção de uma faixa de coluna com uma faixa de linha.

Por exemplo:

```text
┌─────────┬─────────┬─────────┐
│ Célula  │ Célula  │ Célula  │
│    1    │    2    │    3    │
├─────────┼─────────┼─────────┤
│ Célula  │ Célula  │ Célula  │
│    4    │    5    │    6    │
├─────────┼─────────┼─────────┤
│ Célula  │ Célula  │ Célula  │
│    7    │    8    │    9    │
└─────────┴─────────┴─────────┘
```

Com:

```text
3 colunas × 3 linhas = 9 células
```

### 7.4 Grid Item

Os elementos filhos diretos do Grid Container tornam-se **Grid Items**.

Exemplo:

```html
<div class="grid">
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>
</div>
```

Quando:

```css
.grid {
  display: grid;
}
```

os `div` filhos diretos passam a participar do layout Grid.

> **Cadeia mental**
>
> ```text
> Grid Container
>       ↓
> linhas + colunas
>       ↓
> células
>       ↓
> Grid Items
> ```

---

## 8. `fr` em `grid-template-rows`

A unidade `fr` representa uma **fração do espaço disponível** para distribuição entre as faixas flexíveis.

Ela também pode ser usada nas linhas:

```css
.grid {
  display: grid;

  grid-template-rows:
    1fr
    2fr;
}
```

A proporção desejada é:

```text
1 : 2
```

Visualmente:

```text
┌──────────────┐
│              │
│     1fr      │
│              │
├──────────────┤
│              │
│              │
│     2fr      │
│              │
└──────────────┘
```

### 8.1 Um detalhe importante sobre `fr`

Em linhas, `fr` depende da existência de **espaço disponível para distribuição no eixo das linhas**.

Por isso, este exemplo:

```css
.grid {
  display: grid;
  grid-template-rows: 1fr 2fr;
}
```

não deve ser interpretado simplesmente como:

```text
33,33% / 66,66%
```

em qualquer contexto.

O comportamento de `fr` depende do espaço disponível e das demais restrições do Grid.

Quando o Grid possui uma altura definida, o efeito fica mais fácil de observar:

```css
.grid {
  height: 600px;

  display: grid;

  grid-template-rows:
    1fr
    2fr;
}
```

Nesse cenário, o espaço flexível disponível pode ser distribuído na proporção:

```text
1 parte
+
2 partes
```

resultando em:

```text
1 : 2
```

> **Regra mental**
>
> `fr` não significa simplesmente "porcentagem".
>
> Pense em `fr` como **uma fração do espaço flexível disponível**.

---

## 9. `grid-template-columns` + `grid-template-rows`

As duas propriedades podem ser utilizadas simultaneamente.

```css
.grid {
  display: grid;

  grid-template-columns:
    100px
    1fr
    50px;

  grid-template-rows:
    50px
    200px
    50px;
}
```

Temos:

```text
COLUNAS

100px | 1fr | 50px
```

e:

```text
LINHAS

50px
200px
50px
```

Visualmente:

```text
              COLUNAS
        100px     1fr     50px
          ↓        ↓        ↓
      ┌────────┬──────────┬───────┐
50px  │        │          │       │
      ├────────┼──────────┼───────┤
200px │        │          │       │
      ├────────┼──────────┼───────┤
50px  │        │          │       │
      └────────┴──────────┴───────┘
                 ↑
               LINHAS
```

Essas duas propriedades formam grande parte da estrutura básica de um Grid explícito.

---

## 10. Células vazias

Uma grade pode possuir células que não contenham nenhum item.

Exemplo:

```text
┌────────┬────────────┬───────┐
│ Item   │ Item       │ Item  │
├────────┼────────────┼───────┤
│ Item   │            │ Item  │
├────────┼────────────┼───────┤
│ Item   │ Item       │       │
└────────┴────────────┴───────┘
```

A ausência de um elemento dentro de uma célula não significa que a célula deixou de existir.

A estrutura do Grid continua sendo determinada pelas suas linhas e colunas.

> **Importante**
>
> Grid organiza uma **estrutura de faixas**.
>
> O preenchimento dessas células depende dos Grid Items e das regras de posicionamento.

---

## 11. Fluxo automático dos itens

Quando não definimos posições específicas, o Grid utiliza seu algoritmo de posicionamento automático para distribuir os itens.

Considere:

```css
.grid {
  display: grid;

  grid-template-columns:
    100px
    1fr
    50px;
}
```

Temos três colunas.

Os itens podem ser distribuídos assim:

```text
┌────────┬────────┬────────┐
│ Item 1 │ Item 2 │ Item 3 │
├────────┼────────┼────────┤
│ Item 4 │ Item 5 │ Item 6 │
├────────┼────────┼────────┤
│ Item 7 │ Item 8 │ Item 9 │
└────────┴────────┴────────┘
```

Com menos itens:

```text
┌────────┬────────┬────────┐
│ Item 1 │ Item 2 │ Item 3 │
├────────┼────────┼────────┤
│ Item 4 │ Item 5 │        │
└────────┴────────┴────────┘
```

A estrutura de três colunas continua existindo.

Se novos itens forem adicionados, eles continuarão sendo posicionados conforme o fluxo automático do Grid e as regras de posicionamento existentes.

### Regra mental

```text
grid-template-columns: 3 faixas
            ↓
o Grid trabalha com essas 3 colunas
            ↓
novos itens ocupam as próximas posições disponíveis
```

Quando necessário, novas linhas podem ser criadas implicitamente.

---

## 12. Grid Lines e posicionamento

As Grid Lines não servem apenas para visualizar a grade.

Elas também podem ser utilizadas para posicionar elementos posteriormente com propriedades como:

```css
grid-column-start
grid-column-end
grid-row-start
grid-row-end
grid-column
grid-row
```

Considere:

```text
          coluna 1   coluna 2   coluna 3

linha 1 ┌──────────┬──────────┬──────────┐
        │          │          │          │
linha 2 ├──────────┼──────────┼──────────┤
        │          │          │          │
linha 3 ├──────────┼──────────┼──────────┤
        │          │          │          │
linha 4 └──────────┴──────────┴──────────┘
```

Observe:

```text
3 linhas
↓
4 Grid Lines horizontais
```

A primeira faixa de linha está entre:

```text
Grid Line 1
     ↓
┌──────────┐
│  Row 1   │
├──────────┤
     ↑
Grid Line 2
```

Essa distinção será fundamental quando o posicionamento baseado em linhas for estudado.

> **Corte mental**
>
> **Grid Line** = limite estrutural.
>
> **Grid Row** = faixa entre duas linhas.

---

## 13. `repeat()` em `grid-template-rows`

Quando várias linhas possuem o mesmo tamanho, podemos evitar a repetição manual utilizando `repeat()`.

Exemplo:

```css
grid-template-rows:
  repeat(3, 50px);
```

É equivalente a:

```css
grid-template-rows:
  50px
  50px
  50px;
```

Visualmente:

```text
┌──────────────┐
│              │ 50px
├──────────────┤
│              │ 50px
├──────────────┤
│              │ 50px
└──────────────┘
```

### Como ler `repeat()`

```text
repeat(quantidade, valor)
```

Por exemplo:

```css
repeat(3, 50px)
```

pode ser lido como:

```text
repita 3 vezes
      ↓
    50px
```

Resultado:

```text
50px
50px
50px
```

### Repetindo apenas algumas linhas

Também podemos escrever:

```css
grid-template-rows:
  repeat(2, 50px);
```

Isso define:

```text
Linha 1 → 50px
Linha 2 → 50px
```

Outras linhas necessárias poderão ser implícitas.

---

## 14. `minmax()` em `grid-template-rows`

A função `minmax()` permite definir um intervalo de dimensionamento:

```css
grid-template-rows:
  minmax(100px, 1fr);
```

A estrutura pode ser entendida como:

```text
minmax(mínimo, máximo)
```

Neste caso:

```text
mínimo → 100px
máximo → 1fr
```

Ou seja:

```text
┌─────────────────┐
│                 │
│  pode crescer   │
│                 │
└─────────────────┘
↑
não deve ser menor
que 100px
```

O comportamento final depende do espaço disponível e das demais regras do algoritmo de dimensionamento do Grid.

Um uso comum é combinar uma dimensão mínima com uma faixa flexível:

```css
.grid {
  display: grid;

  grid-template-rows:
    minmax(100px, 1fr)
    200px;
}
```

---

## 15. Valores aceitos

`grid-template-rows` pode utilizar diferentes tipos de valores para dimensionar suas faixas.

### Valores fixos

```css
grid-template-rows:
  50px
  100px
  200px;
```

Cada linha recebe um tamanho definido.

### Frações

```css
grid-template-rows:
  1fr
  2fr;
```

As linhas flexíveis participam da distribuição do espaço disponível.

### Porcentagens

```css
grid-template-rows:
  20%
  40%
  40%;
```

As porcentagens são calculadas em relação à dimensão correspondente do **content area** do Grid Container.

O comportamento pode exigir atenção quando a dimensão usada como referência não é definida de forma determinada.

### `repeat()`

```css
grid-template-rows:
  repeat(3, 50px);
```

Permite repetir uma definição.

### `minmax()`

```css
grid-template-rows:
  minmax(100px, 1fr)
  200px;
```

Permite estabelecer um valor mínimo e um limite superior para uma faixa.

### `auto`

Também podemos utilizar:

```css
grid-template-rows:
  auto
  1fr
  auto;
```

`auto` permite que o tamanho da faixa seja determinado pelo algoritmo de dimensionamento do Grid, levando em consideração fatores como o conteúdo.

> **Resumo**
>
> ```text
> px        → tamanho específico
> %         → tamanho relativo
> fr        → fração do espaço flexível
> auto      → dimensionamento automático
> repeat()  → repetição
> minmax()  → intervalo mínimo/máximo
> ```

---

## 16. Exemplo completo

Considere:

```css
.grid {
  display: grid;

  grid-template-columns:
    100px
    1fr
    50px;

  grid-template-rows:
    50px
    200px
    50px;

  gap: 20px;
}
```

A estrutura pode ser interpretada assim:

```text
GRID
│
├── 3 COLUNAS
│     │
│     ├── 100px
│     ├── 1fr
│     └── 50px
│
└── 3 LINHAS
      │
      ├── 50px
      ├── 200px
      └── 50px
```

Visualmente:

```text
                 COLUNAS
          100px     1fr     50px
            ↓        ↓        ↓

        ┌────────┬──────────┬───────┐
50px    │        │          │       │
        ├────────┼──────────┼───────┤
200px   │        │          │       │
        ├────────┼──────────┼───────┤
50px    │        │          │       │
        └────────┴──────────┴───────┘

                 ↑
               LINHAS
```

O `gap: 20px` acrescenta espaçamento entre as faixas.

---

## 17. Mapas mentais

### 17.1 `grid-template-rows`

```text
                        GRID
                          │
                ┌─────────┴─────────┐
                │                   │
             COLUNAS             LINHAS
                │                   │
                ↓                   ↓
  grid-template-columns    grid-template-rows
                                    │
                                    ↓
                           define as linhas
                                    │
                    ┌───────────────┼───────────────┐
                    ↓               ↓               ↓
                   px              fr              %
                    │               │               │
              tamanho fixo    espaço flexível   percentual
                                    │
                         ┌──────────┴──────────┐
                         ↓                     ↓
                     repeat()              minmax()
                         │                     │
                    repetição         mínimo + limite superior
```

---

### 17.2 Columns × Rows

```text
                         CSS GRID
                            │
              ┌─────────────┴─────────────┐
              │                           │
           COLUMNS                      ROWS
              │                           │
              ↓                           ↓
 grid-template-columns          grid-template-rows
              │                           │
              ↓                           ↓
        estrutura das               estrutura das
           colunas                     linhas
              │                           │
              ↓                           ↓
         no eixo das                no eixo das
           colunas                     linhas
```

### Corte mental

```text
COLUMN
→ faixa de coluna

ROW
→ faixa de linha
```

No sistema de escrita horizontal mais comum:

```text
COLUMN
→ esquerda ↔ direita
→ largura

ROW
→ cima ↕ baixo
→ altura
```

---

### 17.3 Construção do Grid

```text
GRID CONTAINER
       │
       ↓
display: grid
       │
       ├──────────────────────┐
       ↓                      ↓
   COLUNAS                  LINHAS
       │                      │
       ↓                      ↓
grid-template-columns  grid-template-rows
       │                      │
       ↓                      ↓
   dimensões               dimensões
       │                      │
       └──────────┬───────────┘
                  ↓
             GRID CELLS
                  │
                  ↓
             GRID ITEMS
```

### Corte mental

```text
display: grid
      ↓
"Ative o Grid"

grid-template-columns
      ↓
"Como serão as colunas?"

grid-template-rows
      ↓
"Como serão as linhas?"
```

---

### 17.4 `repeat()`

```text
repeat()
   │
   ├── quantidade
   │      ↓
   │   quantas vezes?
   │
   └── valor
          ↓
      o que repetir?
```

Exemplo:

```css
grid-template-rows:
  repeat(3, 50px);
```

Leitura:

```text
3 vezes
  +
50px
  ↓
50px
50px
50px
```

### Corte mental

```text
repeat(quantidade, valor)
=
"repita X vezes este valor"
```

---

### 17.5 Tamanho das linhas

```text
grid-template-rows
        │
        ├── 50px
        │    ↓
        │  tamanho fixo
        │
        ├── 1fr
        │    ↓
        │  espaço flexível
        │
        ├── 50%
        │    ↓
        │  porcentagem
        │
        ├── auto
        │    ↓
        │  dimensionamento automático
        │
        ├── repeat()
        │    ↓
        │  repetição
        │
        └── minmax()
             ↓
       mínimo + limite superior
```

---

## 18. Pontos essenciais

```text
┌─────────────────────────────────────────────┐
│              GRID-TEMPLATE-ROWS             │
├─────────────────────────────────────────────┤
│ Define as linhas explícitas do Grid.        │
│                                             │
│ Não é obrigatório para que existam linhas. │
│ O Grid pode criar linhas implícitas.        │
│                                             │
│ Permite definir o tamanho das linhas.       │
│                                             │
│ Pode utilizar px, %, fr, auto...            │
│                                             │
│ Pode utilizar repeat() e minmax().          │
│                                             │
│ Linhas implícitas são dimensionadas         │
│ conforme grid-auto-rows.                    │
└─────────────────────────────────────────────┘
```

### Corte mental principal

```text
grid-template-columns
        ↓
"Como são as colunas?"

grid-template-rows
        ↓
"Como são as linhas?"
```

### Atenção ao conteúdo

```css
grid-template-rows: 50px;
```

não significa:

```text
"Faça todo o conteúdo caber em 50px."
```

Significa:

```text
"Defina esta faixa do Grid com 50px."
```

O mesmo princípio vale para:

```css
grid-template-columns: 100px;
```

A dimensão da faixa e o tamanho necessário pelo conteúdo são conceitos relacionados, mas não são a mesma coisa.

---

## 19. Resumo final

```text
                         CSS GRID
                            │
              ┌─────────────┴─────────────┐
              │                           │
           COLUMNS                      ROWS
              │                           │
              ↓                           ↓
 grid-template-columns          grid-template-rows
              │                           │
              ↓                           ↓
      estrutura das colunas       estrutura das linhas
              │                           │
              ↓                           ↓
        faixas de coluna             faixas de linha
              │                           │
              └────────────┬──────────────┘
                           ↓
                       GRID CELLS
                           │
                           ↓
                       GRID ITEMS
```

### Fórmula mental principal

```text
COLUMN
→ coluna

ROW
→ linha
```

No uso cotidiano com escrita horizontal:

```text
COLUMN
→ largura da faixa

ROW
→ altura da faixa
```

### Relação entre as propriedades

```css
.grid {
  display: grid;

  grid-template-columns:
    100px
    1fr
    50px;

  grid-template-rows:
    50px
    200px
    50px;
}
```

Podemos interpretar:

```text
3 colunas
↓
100px | 1fr | 50px

3 linhas
↓
50px
200px
50px
```

### Regra final

> `grid-template-columns` define as faixas de coluna do Grid explícito.
>
> `grid-template-rows` define as faixas de linha do Grid explícito.
>
> As duas propriedades podem trabalhar juntas para formar a estrutura bidimensional do layout.
>
> Quando o Grid precisa de faixas além daquelas declaradas explicitamente, essas faixas podem ser criadas implicitamente e dimensionadas por propriedades como `grid-auto-rows` e `grid-auto-columns`.

---

## 20. Referências

- [MDN — `grid-template-rows`](https://developer.mozilla.org/pt-BR/docs/Web/CSS/Reference/Properties/grid-template-rows)
- [MDN — CSS Grid Layout](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout)
- [MDN — Grid Container](https://developer.mozilla.org/en-US/docs/Glossary/Grid_Container)
- [MDN — Grid Lines](https://developer.mozilla.org/en-US/docs/Glossary/Grid_lines)
- [W3C — CSS Grid Layout Module](https://www.w3.org/TR/css-grid/)
- [GitHub — MDN Content](https://github.com/mdn/content)
- [GitHub — Rachel Andrew / Grid by Example](https://github.com/rachelandrew/grid-by-example)
