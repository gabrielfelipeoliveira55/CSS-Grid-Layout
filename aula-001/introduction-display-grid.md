# CSS Grid Layout — Introdução e `display: grid`

## Índice

- [1. O que é CSS Grid?](#1-o-que-é-css-grid)
- [2. Grid Container](#2-grid-container)
- [3. Quem são os Grid Items?](#3-quem-são-os-grid-items)
  - [Regra importante](#regra-importante)
- [4. `display: grid`](#4-display-grid)
- [5. `display: inline-grid`](#5-display-inline-grid)
  - [Uso](#uso)
- [6. `subgrid`](#6-subgrid)
- [7. `display: grid` sozinho não define as colunas](#7-display-grid-sozinho-não-define-as-colunas)
- [8. Diferença entre `display: flex` e `display: grid`](#8-diferença-entre-display-flex-e-display-grid)
- [9. `grid-template-columns`](#9-grid-template-columns)
- [10. Removendo `grid-template-columns`](#10-removendo-grid-template-columns)
- [11. Criando três colunas](#11-criando-três-colunas)
- [12. `grid-template-columns` aceita diferentes unidades](#12-grid-template-columns-aceita-diferentes-unidades)
- [13. Usando porcentagem](#13-usando-porcentagem)
- [14. Valores diferentes para as colunas](#14-valores-diferentes-para-as-colunas)
- [15. Uma dica sobre os nomes das propriedades](#15-uma-dica-sobre-os-nomes-das-propriedades)
- [16. `columns` no plural](#16-columns-no-plural)
  - [⚠️ Atenção](#atenção)
- [17. Grid Container x Grid Item](#17-grid-container-x-grid-item)
- [18. `fr` — unidade fracional](#18-fr-unidade-fracional)
- [19. `1fr 1fr 1fr`](#19-1fr-1fr-1fr)
- [20. `2fr 2fr 2fr`](#20-2fr-2fr-2fr)
- [21. `1fr 2fr 1fr`](#21-1fr-2fr-1fr)
- [22. `3fr`](#22-3fr)
- [23. Por que usar `fr`?](#23-por-que-usar-fr)
- [24. Regra mental da unidade `fr`](#24-regra-mental-da-unidade-fr)
- [25. `grid-gap`](#25-grid-gap)
- [26. Grid dentro de Grid](#26-grid-dentro-de-grid)
- [27. Exemplo de Grid aninhado](#27-exemplo-de-grid-aninhado)
- [28. `subgrid` não é simplesmente "Grid dentro de Grid"](#28-subgrid-não-é-simplesmente-grid-dentro-de-grid)
  - [Grid aninhado](#grid-aninhado)
  - [Subgrid](#subgrid)
- [29. Exemplo completo](#29-exemplo-completo)
- [30. Estrutura mental do CSS Grid](#30-estrutura-mental-do-css-grid)
- [31. Resumo das propriedades estudadas](#31-resumo-das-propriedades-estudadas)
- [32. Resumo de `grid-template-columns`](#32-resumo-de-grid-template-columns)
  - [Duas colunas fixas](#duas-colunas-fixas)
  - [Três colunas fixas](#três-colunas-fixas)
  - [Duas colunas iguais](#duas-colunas-iguais)
  - [Três colunas iguais](#três-colunas-iguais)
  - [Uma coluna maior](#uma-coluna-maior)
- [33. 🧠 Regra mental para revisão](#33-regra-mental-para-revisão)
- [34. 📌 O que realmente precisa ficar na memória](#34-o-que-realmente-precisa-ficar-na-memória)
  - [Conceito central](#conceito-central)

---

## 1. O que é CSS Grid?

O **CSS Grid Layout** é um sistema de layout baseado na organização de elementos em **linhas e colunas**.

Assim como no Flexbox, existe a relação entre um **container** e seus **itens**.

No Grid, essa relação pode ser representada da seguinte forma:

```text
Grid Container

      │

      ├── Grid Item

      ├── Grid Item

      ├── Grid Item

      └── Grid Item
````

O elemento que estabelece o contexto do Grid é o **Grid Container**, enquanto seus filhos diretos são os **Grid Items**.

---

## 2. Grid Container

O **Grid Container** é o elemento que recebe:

```css
.container {
  display: grid;
}
```

A partir dessa declaração, o elemento passa a estabelecer um **contexto de layout Grid** para seus filhos diretos.

### Exemplo

```html
<section class="grid">
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>
</section>
```

```css
.grid {
  display: grid;
}
```

Nesse caso:

```text
section.grid

     ↓

Grid Container

     │

     ├── div → Grid Item

     ├── div → Grid Item

     └── div → Grid Item
```

---

## 3. Quem são os Grid Items?

Os **Grid Items** são os **filhos diretos** do Grid Container.

Considere o seguinte exemplo:

```html
<div class="grid">
  <div>Item 1</div>
  <div>Item 2</div>
  <section>
    <div>Item 3</div>
  </section>
</div>
```

A estrutura pode ser visualizada assim:

```text
.grid

 │

 ├── div
 │    └── Grid Item

 │

 ├── div
 │    └── Grid Item

 │

 └── section
      └── Grid Item

           │

           └── div → NÃO é Grid Item do .grid
```

O `div` que está dentro do `section` é um **filho do filho**. Por isso, ele não é um Grid Item direto do `.grid`.

### Regra importante

> **Somente os filhos diretos do Grid Container participam diretamente do Grid.**

---

## 4. `display: grid`

A propriedade utilizada para estabelecer um Grid Container é:

```css
.container {
  display: grid;
}
```

Essa declaração transforma o elemento em um **Grid Container**.

Sem ela:

```css
.container {
  /* sem display: grid */
}
```

o elemento continua seguindo o comportamento definido pelo seu valor de `display` atual.

Portanto, `display: grid` é o ponto de partida para utilizar o sistema de layout Grid.

---

## 5. `display: inline-grid`

Também é possível utilizar:

```css
.container {
  display: inline-grid;
}
```

Nesse caso, o elemento também estabelece um contexto de Grid, mas participa do fluxo externo como um elemento **inline-level**.

Por exemplo, dois elementos `inline-grid` podem aparecer lado a lado:

```text
[ Grid ] [ Grid ]
```

Já:

```css
display: grid;
```

faz com que o elemento participe do fluxo externo com comportamento de bloco.

### Uso

Em layouts convencionais, é comum utilizar:

```css
display: grid;
```

quando o objetivo é criar um Grid Container no fluxo normal da página.

---

## 6. `subgrid`

`subgrid` está relacionado à possibilidade de um Grid filho participar da estrutura de trilhas definida pelo Grid ancestral.

O conceito pode ser representado assim:

```text
Grid principal

│

├── Item

├── Item

└── Item

     │

     └── Subgrid

         ├── Item

         ├── Item

         └── Item
```

É importante diferenciar a ideia de **Grid aninhado** da ideia de **subgrid**.

> **Um Grid aninhado cria uma nova estrutura de Grid. O `subgrid` permite que determinadas trilhas do Grid filho sejam alinhadas à estrutura do Grid pai.**

---

## 7. `display: grid` sozinho não define as colunas

Utilizar:

```css
.container {
  display: grid;
}
```

não significa automaticamente:

```text
"Crie várias colunas."
```

O `display: grid` estabelece o contexto de Grid, mas outras propriedades determinam como suas trilhas serão organizadas.

Sem uma definição explícita de colunas:

```css
display: grid;
```

os elementos podem continuar sendo organizados em uma única coluna.

Exemplo:

```text
[Item 1]

[Item 2]

[Item 3]

[Item 4]
```

Por isso, ativar o Grid e definir sua estrutura são etapas conceitualmente diferentes.

---

## 8. Diferença entre `display: flex` e `display: grid`

Flexbox e Grid são sistemas de layout diferentes.

Quando utilizamos:

```css
.container {
  display: flex;
}
```

os itens passam imediatamente a participar do modelo de layout flexível.

Por padrão, a disposição ocorre ao longo de uma direção principal:

```text
[Item 1] [Item 2] [Item 3]
```

No Grid:

```css
.container {
  display: grid;
}
```

o contexto Grid é estabelecido, mas a estrutura da grade pode ser definida por propriedades como:

```css
grid-template-columns: 200px 200px;
```

Essa diferença ajuda a entender que o Grid trabalha diretamente com uma estrutura de **linhas e colunas**, enquanto o Flexbox é orientado principalmente por um **eixo**.

---

## 9. `grid-template-columns`

A propriedade:

```css
grid-template-columns
```

é utilizada para definir as **colunas do Grid Container**.

### Exemplo

```css
.container {
  display: grid;
  grid-template-columns: 200px 200px;
}
```

Isso define duas colunas com `200px` cada:

```text
┌──────────┬──────────┐
│  200px   │  200px   │
├──────────┼──────────┤
│  Item    │  Item    │
└──────────┴──────────┘
```

Temos:

```text
Coluna 1 → 200px

Coluna 2 → 200px
```

---

## 10. Removendo `grid-template-columns`

Ao remover:

```css
grid-template-columns: 200px 200px;
```

essas duas colunas explícitas deixam de fazer parte da definição do Grid.

Dependendo das demais regras aplicadas, os itens podem ser posicionados em uma única coluna:

```text
[Item 1]

[Item 2]

[Item 3]

[Item 4]
```

A ideia central é:

> **`display: grid` estabelece o contexto Grid, enquanto `grid-template-columns` define as colunas explícitas.**

---

## 11. Criando três colunas

Podemos definir três colunas com:

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
}
```

Agora temos:

```text
┌────────┬────────┬────────┐
│ 100px  │ 100px  │ 100px  │
├────────┼────────┼────────┤
│ Item 1 │ Item 2 │ Item 3 │
└────────┴────────┴────────┘
```

As três colunas possuem `100px`, totalizando `300px` de largura entre elas.

Se o container possuir largura superior a `300px`, poderá existir espaço restante:

```text
┌────────┬────────┬────────┬───────────────┐
│ 100px  │ 100px  │ 100px  │ espaço vazio  │
└────────┴────────┴────────┴───────────────┘
```

---

## 12. `grid-template-columns` aceita diferentes unidades

As colunas podem utilizar diferentes unidades CSS.

Por exemplo:

```css
grid-template-columns: 25% 25%;
```

Também é possível utilizar:

```css
grid-template-columns: 50% 50%;
```

ou combinar valores:

```css
grid-template-columns: 150px 200px;
```

Cada valor corresponde a uma coluna na ordem em que foi declarado.

---

## 13. Usando porcentagem

Considere:

```css
.container {
  display: grid;
  grid-template-columns: 25% 25%;
}
```

Temos:

```text
25% + 25% = 50%
```

Assim, as duas colunas ocupam metade da largura disponível para a referência usada pelo percentual, restando espaço para a área restante:

```text
┌──────────────┬──────────────┬──────────────────────┐
│     25%      │      25%     │   espaço restante    │
└──────────────┴──────────────┴──────────────────────┘
```

Com:

```css
grid-template-columns: 50% 50%;
```

a estrutura passa a ser:

```text
┌──────────────────────┬──────────────────────┐
│         50%          │         50%          │
└──────────────────────┴──────────────────────┘
```

---

## 14. Valores diferentes para as colunas

Também é possível combinar tamanhos diferentes:

```css
.container {
  display: grid;
  grid-template-columns: 150px 200px;
}
```

Nesse caso:

```text
Coluna 1 → 150px

Coluna 2 → 200px
```

O Grid respeita os tamanhos definidos para essas colunas.

Quando os tamanhos definidos não cabem no espaço disponível, a estrutura pode ultrapassar os limites do container ou produzir overflow, dependendo das demais condições do layout.

---

## 15. Uma dica sobre os nomes das propriedades

Muitas propriedades relacionadas ao CSS Grid possuem `grid` no próprio nome.

Por exemplo:

```css
grid-template-columns

grid-template-rows

grid-template-areas
```

Esse padrão ajuda a identificar visualmente que a propriedade está relacionada ao sistema Grid.

---

## 16. `columns` no plural

Observe:

```css
grid-template-columns
```

A palavra utilizada é:

```text
columns
```

no plural.

Isso ocorre porque a propriedade pode definir múltiplas colunas.

A forma correta é:

```css
grid-template-columns
```

e não:

```css
grid-template-column
```

### ⚠️ Atenção

```css
grid-template-columns: 200px 200px;
```

✅ Correto.

```css
grid-template-column: 200px 200px;
```

❌ Incorreto.

---

## 17. Grid Container x Grid Item

Diferenciar **Grid Container** e **Grid Item** é fundamental para compreender o CSS Grid.

Considere:

```html
<section class="grid">
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>
</section>
```

```css
.grid {
  display: grid;
}
```

A relação fica assim:

```text
<section class="grid">

        ↓

Grid Container

     ┌───────────────┐
     │      div      │ → Grid Item
     ├───────────────┤
     │      div      │ → Grid Item
     ├───────────────┤
     │      div      │ → Grid Item
     └───────────────┘
```

Podemos resumir:

```text
Container

→ controla a estrutura do Grid

Items

→ participam dessa estrutura
```

---

## 18. `fr` — unidade fracional

Uma unidade especialmente importante no CSS Grid é:

```css
fr
```

`fr` representa uma **fração do espaço disponível**.

Por exemplo:

```css
grid-template-columns: 1fr 1fr 1fr;
```

pode ser entendido como:

```text
1 parte

+

1 parte

+

1 parte
```

As três colunas possuem a mesma proporção:

```text
33,33% + 33,33% + 33,33%
```

Visualmente:

```text
┌──────────┬──────────┬──────────┐
│   1fr    │   1fr    │   1fr    │
└──────────┴──────────┴──────────┘
```

---

## 19. `1fr 1fr 1fr`

A declaração:

```css
grid-template-columns: 1fr 1fr 1fr;
```

pode ser interpretada como:

```text
1 parte | 1 parte | 1 parte
```

Todas as colunas possuem a mesma proporção.

Não é necessário calcular manualmente:

```text
100 ÷ 3 = 33,333...
```

A própria unidade `fr` permite expressar essa proporção diretamente:

```css
1fr 1fr 1fr
```

---

## 20. `2fr 2fr 2fr`

Considere:

```css
grid-template-columns: 2fr 2fr 2fr;
```

Temos:

```text
2 partes | 2 partes | 2 partes
```

Como as três colunas possuem a mesma proporção, continuam tendo o mesmo tamanho relativo:

```text
┌──────────┬──────────┬──────────┐
│   2fr    │   2fr    │   2fr    │
└──────────┴──────────┴──────────┘
```

O mais importante é observar a **relação entre os valores**, e não apenas o número isolado.

---

## 21. `1fr 2fr 1fr`

Considere:

```css
grid-template-columns: 1fr 2fr 1fr;
```

Isso significa:

```text
1 parte

+

2 partes

+

1 parte
```

O total é:

```text
1 + 2 + 1 = 4 partes
```

Portanto:

```text
Coluna 1 → 1/4

Coluna 2 → 2/4

Coluna 3 → 1/4
```

Visualmente:

```text
┌────────┬────────────────┬────────┐
│  1fr   │      2fr       │  1fr   │
└────────┴────────────────┴────────┘
```

A coluna do meio possui uma proporção duas vezes maior que cada uma das outras.

---

## 22. `3fr`

Para criar uma proporção em que uma coluna seja três vezes maior que cada uma das outras:

```css
grid-template-columns: 3fr 1fr 1fr;
```

Temos:

```text
3 partes | 1 parte | 1 parte
```

O total é:

```text
5 partes
```

Visualmente:

```text
┌───────────────────┬───────┬───────┐
│        3fr        │  1fr  │  1fr  │
└───────────────────┴───────┴───────┘
```

A primeira coluna possui uma fração três vezes maior que cada uma das demais.

---

## 23. Por que usar `fr`?

A principal vantagem da unidade `fr` é permitir trabalhar com **proporções** sem precisar calcular manualmente porcentagens.

Em vez de:

```css
grid-template-columns: 33.33% 33.33% 33.33%;
```

podemos utilizar:

```css
grid-template-columns: 1fr 1fr 1fr;
```

A ideia pode ser visualizada assim:

```text
1fr → 1 parte

1fr → 1 parte

1fr → 1 parte
```

Isso torna a intenção do layout mais clara.

---

## 24. Regra mental da unidade `fr`

Sempre que encontrar:

```css
fr
```

pense:

> **"Uma fração do espaço disponível."**

### Exemplos

```css
1fr 1fr
```

```text
1 parte | 1 parte
```

```css
1fr 2fr
```

```text
1 parte | 2 partes
```

```css
1fr 3fr
```

```text
1 parte | 3 partes
```

A ideia central é sempre observar a **proporção entre as frações**.

---

## 25. `grid-gap`

Também é possível definir espaçamento entre as células do Grid utilizando:

```css
grid-gap
```

Por exemplo:

```css
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 20px;
}
```

Nesse caso, existe um espaçamento de `20px` entre as células.

Sem espaçamento:

```text
┌────────┬────────┐
│ Item 1 │ Item 2 │
├────────┼────────┤
│ Item 3 │ Item 4 │
└────────┴────────┘
```

Com `grid-gap: 20px`:

```text
┌────────┐  20px  ┌────────┐
│ Item 1 │        │ Item 2 │
└────────┘        └────────┘

┌────────┐  20px  ┌────────┐
│ Item 3 │        │ Item 4 │
└────────┘        └────────┘
```

> `gap` é a propriedade moderna e genérica para definir espaçamento entre linhas e colunas, enquanto `grid-gap` é uma propriedade histórica específica do contexto Grid.

---

## 26. Grid dentro de Grid

Um elemento pode desempenhar dois papéis diferentes ao mesmo tempo:

**Grid Item** em relação ao Grid pai e **Grid Container** em relação aos seus próprios filhos.

Por exemplo:

```html
<section class="grid">
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>

  <div class="sub-grid">
    <div>Item A</div>
    <div>Item B</div>
    <div>Item C</div>
  </div>
</section>
```

```css
.grid {
  display: grid;
}

.sub-grid {
  display: grid;
}
```

Nesse caso, `.sub-grid` possui dois papéis:

```text
Grid Item do Grid pai

        +

Grid Container para seus próprios filhos
```

Visualmente:

```text
GRID PAI

│

├── Item 1

├── Item 2

├── Item 3

└── .sub-grid
     │
     ├── Item A
     ├── Item B
     └── Item C
```

---

## 27. Exemplo de Grid aninhado

Considere:

```css
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}

.sub-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}
```

O Grid principal possui:

```text
1fr | 1fr | 1fr
```

O `.sub-grid` possui:

```text
1fr | 1fr
```

Isso produz uma estrutura de Grid aninhado:

```text
GRID PRINCIPAL

┌──────┬──────┬──────────────────┐
│Item 1│Item 2│     SUB-GRID     │
│      │      ├────────┬─────────┤
│      │      │ Item A │ Item B  │
│      │      └────────┴─────────┘
└──────┴──────┴──────────────────┘
```

O Grid interno possui sua própria configuração de colunas e espaçamento.

---

## 28. `subgrid` não é simplesmente "Grid dentro de Grid"

É importante não confundir:

```css
display: grid;
```

em um elemento filho com o conceito de:

```text
subgrid
```

São conceitos diferentes.

### Grid aninhado

```css
.child {
  display: grid;
}
```

O elemento filho cria sua **própria estrutura de Grid**.

### Subgrid

O `subgrid` permite que determinadas linhas ou colunas do Grid filho utilizem a estrutura de trilhas definida pelo Grid pai.

Assim, o Grid filho não precisa necessariamente criar uma estrutura completamente independente para essas trilhas.

> **Grid aninhado cria uma nova estrutura. `subgrid` permite participar da estrutura de trilhas do Grid ancestral.**

---

## 29. Exemplo completo

### HTML

```html
<section class="grid">
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>
  <div>Item 4</div>
</section>
```

### CSS

```css
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 20px;
}
```

O resultado pode ser representado assim:

```text
┌──────────┬──────────┬──────────┐
│  Item 1  │  Item 2  │  Item 3  │
├──────────┼──────────┼──────────┤
│  Item 4  │          │          │
└──────────┴──────────┴──────────┘
```

Como existem três colunas:

```text
1fr | 1fr | 1fr
```

os itens são posicionados nessas colunas de acordo com o fluxo de posicionamento automático do Grid.

---

## 30. Estrutura mental do CSS Grid

O funcionamento pode ser resumido da seguinte maneira:

```text
GRID

│

├── display: grid

│      ↓

│   estabelece o Grid

│

├── Grid Container

│      ↓

│   elemento pai

│

├── Grid Items

│      ↓

│   filhos diretos

│

├── grid-template-columns

│      ↓

│   define as colunas

│

├── fr

│      ↓

│   representa proporções de espaço

│

├── gap

│      ↓

│   cria espaçamento

│

└── Grid aninhado

       ↓

    Grid dentro de Grid
```

---

## 31. Resumo das propriedades estudadas

| Propriedade             | Função                                                                 |
| ----------------------- | ---------------------------------------------------------------------- |
| `display: grid`         | Estabelece o elemento como Grid Container                              |
| `display: inline-grid`  | Estabelece um Grid Container com comportamento inline no fluxo externo |
| `grid-template-columns` | Define as colunas explícitas                                           |
| `grid-gap`              | Define espaçamento entre linhas e colunas em um Grid                   |
| `gap`                   | Forma moderna e genérica de definir espaçamento                        |
| `fr`                    | Representa uma fração do espaço disponível                             |

---

## 32. Resumo de `grid-template-columns`

### Duas colunas fixas

```css
grid-template-columns: 200px 200px;
```

```text
200px | 200px
```

### Três colunas fixas

```css
grid-template-columns: 100px 100px 100px;
```

```text
100px | 100px | 100px
```

### Duas colunas iguais

```css
grid-template-columns: 1fr 1fr;
```

```text
1 parte | 1 parte
```

### Três colunas iguais

```css
grid-template-columns: 1fr 1fr 1fr;
```

```text
1 parte | 1 parte | 1 parte
```

### Uma coluna maior

```css
grid-template-columns: 3fr 1fr 1fr;
```

```text
3 partes | 1 parte | 1 parte
```

---

## 33. 🧠 Regra mental para revisão

Ao encontrar:

```css
display: grid;
```

pense:

> **"Este elemento é um Grid Container."**

Ao encontrar:

```css
grid-template-columns
```

pense:

> **"Estou definindo as colunas do Grid."**

Ao encontrar:

```css
1fr
```

pense:

> **"Uma fração do espaço disponível."**

Ao encontrar:

```css
2fr 1fr
```

pense:

> **"A primeira coluna possui o dobro da proporção da segunda."**

Ao encontrar:

```css
gap: 20px;
```

pense:

> **"Estou criando espaçamento entre as células do Grid."**

Ao encontrar um elemento com:

```css
display: grid;
```

dentro de outro Grid, pense:

> **"Esse elemento pode ser um Grid Item do pai e, ao mesmo tempo, um Grid Container para seus próprios filhos."**

---

## 34. 📌 O que realmente precisa ficar na memória

```text
1. display: grid

   → estabelece o contexto CSS Grid.

2. Grid Container

   → é o elemento que recebeu display: grid.

3. Grid Items

   → são os filhos diretos do Grid Container.

4. grid-template-columns

   → define as colunas explícitas.

5. fr

   → representa uma fração do espaço disponível.

6. gap

   → cria espaçamento entre os itens.

7. Grid pode existir dentro de outro Grid

   → um elemento pode ser Grid Item e Grid Container ao mesmo tempo.
```

### Conceito central

> **`display: grid` estabelece o contexto do CSS Grid, enquanto propriedades como `grid-template-columns` definem como a estrutura de colunas será organizada.**

A partir desses conceitos, é possível construir layouts utilizando **colunas, proporções, espaçamento e estruturas de Grid aninhadas**.

```

Essa versão passa a funcionar como **documentação de consulta**, e não como uma transcrição reorganizada de conteúdo didático.
```
