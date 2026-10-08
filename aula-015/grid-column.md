# CSS Grid Layout — `grid-column`

> **Objetivo:** compreender como `grid-column` posiciona e dimensiona um grid item no eixo das colunas, utilizando números, linhas nomeadas, `span`, valores negativos e a relação com `grid-column-start` e `grid-column-end`.

## Índice

1. [Fundamentos necessários para entender `grid-column`](#1-fundamentos-necessários-para-entender-grid-column)
2. [Grid explícito e grid implícito](#2-grid-explícito-e-grid-implícito)
3. [Estrutura interna do Grid](#3-estrutura-interna-do-grid)
4. [Grid line](#4-grid-line)
5. [Grid track](#5-grid-track)
6. [Grid column](#6-grid-column)
7. [A propriedade `grid-column`](#7-a-propriedade-grid-column)
8. [`grid-column` é um shorthand](#8-grid-column-é-um-shorthand)
9. [O significado da barra `/`](#9-o-significado-da-barra-)
10. [Entendendo `1 / 4`](#10-entendendo-1--4)
11. [Uma regra fundamental](#11-uma-regra-fundamental)
12. [Quando apenas um valor é informado](#12-quando-apenas-um-valor-é-informado)
13. [Posicionamento por números](#13-posicionamento-por-números)
14. [Relação entre linhas e quantidade de colunas](#14-relação-entre-linhas-e-quantidade-de-colunas)
15. [Números negativos](#15-números-negativos)
16. [O caso especial de `-1`](#16-o-caso-especial-de--1)
17. [Por que `-1` é tão útil?](#17-por-que--1-é-tão-útil)
18. [O valor `span`](#18-o-valor-span)
19. [`grid-column: 1 / span 2`](#19-grid-column-1--span-2)
20. [`2 / 4` e `2 / span 2`](#20-2--4-e-2--span-2)
21. [`grid-column: span 2`](#21-grid-column-span-2)
22. [`grid-column-start`](#22-grid-column-start)
23. [`grid-column-end`](#23-grid-column-end)
24. [Linhas nomeadas](#24-linhas-nomeadas)
25. [Utilizando as linhas nomeadas](#25-utilizando-as-linhas-nomeadas)
26. [Linhas nomeadas com `grid-column-start` e `grid-column-end`](#26-linhas-nomeadas-com-grid-column-start-e-grid-column-end)
27. [Uma mesma linha pode possuir vários nomes](#27-uma-mesma-linha-pode-possuir-vários-nomes)
28. [Linhas nomeadas repetidas](#28-linhas-nomeadas-repetidas)
29. [`grid-template-areas`](#29-grid-template-areas)
30. [Áreas nomeadas e suas bordas](#30-áreas-nomeadas-e-suas-bordas)
31. [Usando o nome de uma área](#31-usando-o-nome-de-uma-área)
32. [`grid-area` e `grid-column`](#32-grid-area-e-grid-column)
33. [Auto-placement](#33-auto-placement)
34. [`grid-auto-flow`](#34-grid-auto-flow)
35. [O que acontece quando um item é colocado manualmente?](#35-o-que-acontece-quando-um-item-é-colocado-manualmente)
36. [`grid-auto-flow: dense`](#36-grid-auto-flow-dense)
37. [Cuidado com `dense`](#37-cuidado-com-dense)
38. [Exemplo completo com três colunas](#38-exemplo-completo-com-três-colunas)
39. [Exemplo: ocupar duas colunas](#39-exemplo-ocupar-duas-colunas)
40. [Exemplo equivalente usando `span`](#40-exemplo-equivalente-usando-span)
41. [Exemplo: começar em 2 e ocupar 2 tracks](#41-exemplo-começar-em-2-e-ocupar-2-tracks)
42. [Exemplo: largura completa](#42-exemplo-largura-completa)
43. [Exemplo completo de layout](#43-exemplo-completo-de-layout)
44. [Se quisermos controlar o eixo vertical](#44-se-quisermos-controlar-o-eixo-vertical)
45. [Como ler qualquer `grid-column`](#45-como-ler-qualquer-grid-column)
46. [Como ler um `span`](#46-como-ler-um-span)
47. [Modelo mental definitivo](#47-modelo-mental-definitivo)
48. [Uma analogia simples](#48-uma-analogia-simples)
49. [Resumo visual dos principais formatos](#49-resumo-visual-dos-principais-formatos)
50. [Relação entre `start`, `end` e `span`](#50-relação-entre-start-end-e-span)
51. [Tabela de fixação](#51-tabela-de-fixação)
52. [Checklist mental](#52-checklist-mental)
53. [Erros conceituais que devemos evitar](#53-erros-conceituais-que-devemos-evitar)
54. [Mapa mental final](#54-mapa-mental-final)
55. [Regra de ouro](#55-regra-de-ouro)
56. [Referências técnicas](#56-referências-técnicas)
57. [GitHub](#57-github)

---

## 1. Fundamentos necessários para entender `grid-column`

Antes de estudar `grid-column`, é preciso entender quem participa de um Grid.

Um elemento se transforma em **grid container** quando recebe:

```css
.container {
  display: grid;
}
```

Os **filhos diretos** desse elemento tornam-se **grid items**.

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
├── Item 1 ← grid item
├── Item 2 ← grid item
└── Item 3 ← grid item
```

Agora observe um elemento aninhado:

```html
<div class="grid">
  <div class="item">
    <div class="interno">Conteúdo</div>
  </div>
</div>
```

```text
.grid
│
└── .item
    │
    └── .interno
```

`.item` é filho direto de `.grid`, portanto participa diretamente daquele Grid.

`.interno` não é filho direto de `.grid`, então não é um grid item dele.

Isso é importante porque `grid-column` trabalha com o posicionamento do **grid item naquele contexto de Grid**.

### Regra mental

```text
GRID CONTAINER
      │
      └── FILHOS DIRETOS
                ↓
           GRID ITEMS
```

---

## 2. Grid explícito e grid implícito

O CSS Grid trabalha com uma estrutura definida pelo autor (explícita) e também cria tracks automaticamente quando necessário (implícitas).

### Grid explícito

É a parte definida por propriedades como:

```css
grid-template-columns
grid-template-rows
grid-template-areas
```

```css
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}
```

```text
┌──────────┬──────────┬──────────┐
│ Coluna 1 │ Coluna 2 │ Coluna 3 │
└──────────┴──────────┴──────────┘
```

Essas três colunas fazem parte do **grid explícito**.

### Grid implícito

O Grid pode precisar de tracks que não foram definidas diretamente.

```css
.grid {
  display: grid;
  grid-template-columns: 100px 100px;
}

.item {
  grid-column: 3;
}
```

O grid explícito possui duas colunas. Como o item pede a coluna 3, o navegador cria uma coluna implícita:

```text
GRID EXPLÍCITO

┌─────────┬─────────┐
│ Coluna 1│ Coluna 2│
└─────────┴─────────┘

              ↓

item posicionado na coluna 3

              ↓

GRID COM TRACK IMPLÍCITA

┌─────────┬─────────┬────────────┐
│ Coluna 1│ Coluna 2│ Coluna 3   │
│         │         │ implícita  │
└─────────┴─────────┴────────────┘
```

As dimensões das tracks implícitas podem ser controladas por:

```css
grid-auto-columns
grid-auto-rows
```

---

## 3. Estrutura interna do Grid

Três conceitos precisam ser separados:

```text
Grid Line
Grid Track
Grid Column
```

Eles estão relacionados, mas não são a mesma coisa.

---

## 4. Grid line

Uma **grid line** é uma linha estrutural que delimita as tracks do Grid.

Com três colunas:

```text
      1          2          3          4
      │          │          │          │
      ▼          ▼          ▼          ▼
      ┌──────────┬──────────┬──────────┐
      │          │          │          │
      │ Coluna 1 │ Coluna 2 │ Coluna 3 │
      │          │          │          │
      └──────────┴──────────┴──────────┘
```

Existem quatro linhas verticais:

```text
linha 1
linha 2
linha 3
linha 4
```

```text
3 colunas → 4 linhas verticais
```

---

## 5. Grid track

Uma **grid track** é o espaço existente entre duas grid lines.

```text
linha 1             linha 2
   │                   │
   ▼                   ▼
   ├───────────────────┤
          TRACK
```

```text
linha = delimita
track = espaço entre linhas
```

Em três colunas:

```text
linha 1     linha 2     linha 3     linha 4
   │           │           │           │
   ├── track ──┤
               ├── track ──┤
                           ├── track ──┤
```

---

## 6. Grid column

No eixo horizontal do Grid, cada **column track** representa uma coluna.

```text
┌──────────┬──────────┬──────────┐
│ Coluna 1 │ Coluna 2 │ Coluna 3 │
└──────────┴──────────┴──────────┘
```

```text
Grid line
   ↓
delimita

Grid track
   ↓
ocupa o espaço

Grid column
   ↓
track no eixo das colunas
```

---

## 7. A propriedade `grid-column`

A propriedade `grid-column` posiciona um grid item no eixo das colunas.

```css
.item {
  grid-column: 2 / 4;
}
```

O significado é:

```text
comece na linha 2
termine na linha 4
```

```text
linha 1    linha 2    linha 3    linha 4
   │          │          │          │
   │          ├──────────┼──────────┤
   │          │          ITEM       │
   │          └──────────┴──────────┘
```

O item ocupa as tracks:

```text
2 → 3
3 → 4
```

```text
2 / 4 = 2 tracks
```

---

## 8. `grid-column` é um shorthand

`grid-column` é uma forma abreviada de:

```css
grid-column-start
grid-column-end
```

```css
grid-column: 2 / 4;
```

equivale a:

```css
grid-column-start: 2;
grid-column-end: 4;
```

```text
              grid-column
                   │
          ┌────────┴────────┐
          │                 │
          ▼                 ▼
 grid-column-start   grid-column-end
```

---

## 9. O significado da barra `/`

```css
grid-column: 1 / 4;
```

A barra separa dois valores:

```text
START / END
```

ou:

```text
INÍCIO / FIM
```

Não é uma divisão matemática. Ela separa `grid-column-start` de `grid-column-end`.

---

## 10. Entendendo `1 / 4`

Considere um Grid com três colunas:

```text
      1          2          3          4
      │          │          │          │
      ▼          ▼          ▼          ▼
      ┌──────────┬──────────┬──────────┐
      │          │          │          │
      │          │          │          │
      └──────────┴──────────┴──────────┘
```

```css
grid-column: 1 / 4;
```

O item ocupará:

```text
1 → 2
2 → 3
3 → 4
```

```text
3 tracks
```

> **⚠️ Atenção**
>
> `1 / 4` **não** significa "coluna 1 até coluna 4". Significa **linha 1 até linha 4**.

---

## 11. Uma regra fundamental

```text
grid-column posiciona usando GRID LINES.

As tracks ficam ENTRE essas linhas.
```

```text
linha 1
   │
   ├──── track 1 ────┤
                     │
                   linha 2
                     │
   ├──── track 2 ────┤
                     │
                   linha 3
                     │
   ├──── track 3 ────┤
                     │
                   linha 4
```

Logo, `1 / 4` significa 3 tracks.

---

## 12. Quando apenas um valor é informado

```css
grid-column: 2;
```

Nesse caso, a linha inicial é 2 e a linha final é `auto`. Com `auto`, o item ocupa **uma única track**, como se fosse `span 1`.

```text
grid-column: 2;

START = 2
END   = auto (ocupa 1 track)
```

```text
linha 1     linha 2     linha 3     linha 4
   │           │           │           │
               ├───────────┤
                   ITEM
```

---

## 13. Posicionamento por números

Números positivos identificam grid lines, contando a partir do início do eixo.

```css
.item {
  grid-column: 2;
}
```

```css
.item {
  grid-column: 2 / 4;
}
```

```text
1 → 2 → 3 → 4 → 5
```

> **Nota:** o "início" do eixo depende da direção de escrita. Em idiomas escritos da esquerda para a direita, a linha 1 fica à esquerda. Em idiomas da direita para a esquerda (como o árabe), a linha 1 fica à direita.

---

## 14. Relação entre linhas e quantidade de colunas

```css
grid-template-columns: repeat(3, 1fr);
```

```text
3 column tracks
4 grid lines verticais
```

```text
1          2          3          4
│          │          │          │
├──────────┼──────────┼──────────┤
```

Com cinco colunas:

```css
grid-template-columns: repeat(5, 1fr);
```

```text
1    2    3    4    5    6
│    │    │    │    │    │
├────┼────┼────┼────┼────┤
```

Regra:

```text
número de linhas = número de tracks + 1
```

---

## 15. Números negativos

O Grid também permite números negativos:

```css
grid-column: -1;
```

Os índices negativos são contados a partir da extremidade final do grid explícito.

```text
1          2          3          4
│          │          │          │
├──────────┼──────────┼──────────┤
│          │          │          │
├──────────┼──────────┼──────────┤
-4        -3         -2         -1
```

```text
linha 4 = -1
linha 3 = -2
linha 2 = -3
linha 1 = -4
```

---

## 16. O caso especial de `-1`

Um padrão muito utilizado é:

```css
grid-column: 1 / -1;
```

```text
comece na primeira linha
termine na última linha do grid explícito
```

```css
.grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
}

.header {
  grid-column: 1 / -1;
}
```

Resultado, com quatro colunas:

```text
┌────────────────────────────────────────┐
│                 HEADER                 │
├──────────┬──────────┬─────────┬────────┤
│          │          │         │        │
│          │          │         │        │
├──────────┼──────────┼─────────┼────────┤
│          │          │         │        │
└──────────┴──────────┴─────────┴────────┘
```

```text
-1 = última linha do grid explícito
```

---

## 17. Por que `-1` é tão útil?

```css
grid-template-columns: repeat(4, 1fr);
```

Você poderia fazer:

```css
grid-column: 1 / 5;
```

Mas, se depois o Grid mudar para:

```css
grid-template-columns: repeat(6, 1fr);
```

será preciso alterar também o valor para:

```css
grid-column: 1 / 7;
```

Com:

```css
grid-column: 1 / -1;
```

o posicionamento continua representando:

```text
primeira linha → última linha
```

---

## 18. O valor `span`

A palavra `span` indica uma quantidade de tracks que o item deve ocupar.

```css
grid-column: span 2;
```

```text
ocupe 2 tracks
```

```text
span = extensão / quantidade de tracks
```

---

## 19. `grid-column: 1 / span 2`

```css
grid-column: 1 / span 2;
```

```text
comece na linha 1
+
ocupe 2 tracks
```

```text
linha 1      linha 2      linha 3
   │            │            │
   ├────────────┼────────────┤
   │            ITEM         │
   │         span 2          │
   └────────────┴────────────┘
```

```text
1 → 2
2 → 3
```

Resultado: 2 tracks.

---

## 20. `2 / 4` e `2 / span 2`

Estas duas formas chegam ao mesmo resultado:

```css
grid-column: 2 / 4;
```

```css
grid-column: 2 / span 2;
```

```text
2 / 4

2 → 3
3 → 4

= 2 tracks
```

```text
2 / span 2

começa em 2
+
2 tracks

= termina em 4
```

Mas a intenção expressa é diferente:

```text
2 / 4
→ comece em 2 e termine em 4

2 / span 2
→ comece em 2 e ocupe 2 tracks
```

---

## 21. `grid-column: span 2`

```css
grid-column: span 2;
```

A quantidade está definida (2 tracks), mas a linha inicial não foi especificada nessa declaração.

O Grid determina a posição inicial pelo algoritmo de posicionamento automático.

```text
posição inicial → automática
quantidade      → 2 tracks
```

---

## 22. `grid-column-start`

`grid-column-start` define a linha inicial do item no eixo das colunas.

```css
.item {
  grid-column-start: 2;
}
```

```text
START = linha 2
```

```text
1          2          3          4
│          │          │          │
           ▼
           ┌──────────┐
           │   ITEM   │
           └──────────┘
```

---

## 23. `grid-column-end`

`grid-column-end` define a linha final do item no eixo das colunas.

```css
.item {
  grid-column-end: 4;
}
```

```text
END = linha 4
```

Para controlar início e fim explicitamente:

```css
.item {
  grid-column-start: 2;
  grid-column-end: 4;
}
```

Isso corresponde a:

```css
.item {
  grid-column: 2 / 4;
}
```

---

## 24. Linhas nomeadas

Até aqui foram usados números. Mas as grid lines também podem receber nomes, escritos entre colchetes:

```css
.grid {
  display: grid;
  grid-template-columns:
    [inicio] 1fr
    [meio] 1fr
    [terceira] 1fr
    [fim];
}
```

Cada nome é colocado **antes** da track cuja linha ele identifica. O último nome, `[fim]`, identifica a linha que fecha a terceira coluna.

```text
[inicio]       [meio]       [terceira]       [fim]
    │             │              │              │
    ├─────────────┼──────────────┼──────────────┤
         1fr           1fr            1fr
```

Equivalência com os números:

```text
inicio    = linha 1
meio      = linha 2
terceira  = linha 3
fim       = linha 4
```

---

## 25. Utilizando as linhas nomeadas

```css
.item {
  grid-column: inicio / fim;
}
```

equivale a:

```css
.item {
  grid-column: 1 / 4;
}
```

Isso torna o CSS mais semântico:

```css
grid-column: 1 / 4;
```

```css
grid-column: inicio / fim;
```

A segunda forma comunica melhor a intenção estrutural do layout.

---

## 26. Linhas nomeadas com `grid-column-start` e `grid-column-end`

```css
.item {
  grid-column-start: inicio;
  grid-column-end: fim;
}
```

equivale a:

```css
.item {
  grid-column: inicio / fim;
}
```

---

## 27. Uma mesma linha pode possuir vários nomes

Uma mesma grid line pode receber vários nomes, separados por espaço:

```css
.grid {
  display: grid;
  grid-template-columns:
    [sidebar-start] 200px
    [sidebar-end main-start] 1fr
    [main-end];
}
```

```text
[sidebar-start]   [sidebar-end main-start]   [main-end]
       │                     │                    │
       ├─────── 200px ───────┼───────── 1fr ──────┤
```

A linha do meio pode ser referenciada como `sidebar-end` ou como `main-start`. Isso é útil porque ela representa, ao mesmo tempo, o limite final da sidebar e o limite inicial do conteúdo principal.

```css
.sidebar {
  grid-column: sidebar-start / sidebar-end;
}

.main {
  grid-column: main-start / main-end;
}
```

---

## 28. Linhas nomeadas repetidas

Um mesmo nome pode se repetir em várias linhas:

```css
.grid {
  display: grid;
  grid-template-columns: repeat(4, [col-start] 1fr);
}
```

Agora existem várias linhas chamadas `col-start`. Para escolher uma ocorrência específica, informe o **número da ocorrência** depois do nome:

```css
.item {
  grid-column: col-start 2 / col-start 4;
}
```

```text
nome + índice da ocorrência
```

Aqui, `col-start 2` é a segunda linha com esse nome (linha 2) e `col-start 4` é a quarta (linha 4). O item ocupa 2 tracks.

---

## 29. `grid-template-areas`

Outra forma de organizar um Grid é `grid-template-areas`:

```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
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

Aqui estamos nomeando regiões do Grid.

---

## 30. Áreas nomeadas e suas bordas

Cada área nomeada gera automaticamente linhas nomeadas nas suas bordas, com os sufixos `-start` e `-end`.

Para a área `header`:

```text
header-start                         header-end
      │                                   │
      ├───────────┬───────────┬───────────┤
      │  header   │  header   │  header   │
      └───────────┴───────────┴───────────┘
```

Essas linhas podem ser usadas no posicionamento.

---

## 31. Usando o nome de uma área

Podemos associar um item a uma área com `grid-area`:

```css
.header {
  grid-area: header;
}
```

Isso posiciona o item na área completa, considerando os dois eixos.

Também podemos usar as linhas geradas pela área:

```css
.item {
  grid-column: header-start / header-end;
}
```

```text
header-start
     ↓
 início da área no eixo das colunas

header-end
     ↓
 fim da área no eixo das colunas
```

---

## 32. `grid-area` e `grid-column`

Essas propriedades não são a mesma coisa.

```text
grid-area
├── eixo das colunas
└── eixo das linhas
```

```text
grid-column
└── eixo das colunas
```

```css
grid-area: header;
```

pode representar uma área inteira. Já:

```css
grid-column: 1 / -1;
```

trabalha apenas com a dimensão horizontal do posicionamento.

---

## 33. Auto-placement

Nem todo grid item precisa ter a posição definida manualmente.

Quando um item não possui posicionamento explícito suficiente, o Grid utiliza o algoritmo de **auto-placement**.

```html
<div class="grid">
  <div>1</div>
  <div>2</div>
  <div>3</div>
  <div>4</div>
</div>
```

```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}
```

O navegador distribui os itens automaticamente, seguindo a ordem do HTML.

---

## 34. `grid-auto-flow`

`grid-auto-flow` controla como os itens com posicionamento automático são inseridos no Grid.

O valor padrão é:

```css
grid-auto-flow: row;
```

```text
→ → →
→ → →
→ → →
```

O preenchimento ocorre seguindo as linhas do Grid.

---

## 35. O que acontece quando um item é colocado manualmente?

```css
.item-1 {
  grid-column: 2;
}
```

```text
┌─────────┬─────────┬─────────┐
│         │ Item 1  │         │
├─────────┼─────────┼─────────┤
│         │         │         │
└─────────┴─────────┴─────────┘
```

Os outros itens ainda precisam ser posicionados, e o navegador continua executando o auto-placement para eles.

Dependendo da situação, isso pode deixar espaços vazios.

---

## 36. `grid-auto-flow: dense`

```css
.grid {
  grid-auto-flow: dense;
}
```

O modo `dense` faz o algoritmo procurar espaços anteriores que ficaram vazios e preenchê-los com itens que vêm depois no HTML.

Exemplo com 3 colunas, em que o item 3 ocupa duas colunas:

```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}

.item-3 {
  grid-column: span 2;
}
```

Os itens 1 e 2 ocupam as duas primeiras colunas da linha 1. O item 3 precisa de duas colunas e não cabe na coluna 3 restante, então vai para a linha 2.

```text
SEM dense (padrão)

┌───────┬───────┬───────┐
│   1   │   2   │       │  ← lacuna
├───────┴───────┼───────┤
│       3       │   4   │
└───────────────┴───────┘
```

Com `grid-auto-flow: dense`, o item 4 volta e ocupa a lacuna:

```text
COM dense

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

## 37. Cuidado com `dense`

`dense` não significa:

```text
"o Grid ignora tudo que eu defini"
```

Ele atua no algoritmo de auto-placement. Itens com posicionamento explícito continuam respeitando a posição determinada.

---

## 38. Exemplo completo com três colunas

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
  gap: 8px;
}

.item-1 {
  grid-column: 1;
}

.item-2 {
  grid-column: 2;
}

.item-3 {
  grid-column: 3;
}
```

```text
linha 1    linha 2    linha 3    linha 4
   │          │          │          │
   ▼          ▼          ▼          ▼
┌──────────┬──────────┬──────────┐
│    1     │    2     │    3     │
└──────────┴──────────┴──────────┘
```

---

## 39. Exemplo: ocupar duas colunas

```css
.item-1 {
  grid-column: 1 / 3;
}
```

```text
linha 1       linha 2       linha 3       linha 4
   │             │             │             │
   ├─────────────┼─────────────┤
   │             ITEM          │
   │          2 tracks         │
   └─────────────┴─────────────┘
```

---

## 40. Exemplo equivalente usando `span`

```css
.item-1 {
  grid-column: 1 / span 2;
}
```

```text
comece na linha 1
+
ocupe 2 tracks
```

```text
linha 1       linha 2       linha 3
   │             │             │
   ├─────────────┼─────────────┤
   │             ITEM          │
   │          span 2           │
   └─────────────┴─────────────┘
```

---

## 41. Exemplo: começar em 2 e ocupar 2 tracks

```css
.item {
  grid-column: 2 / span 2;
}
```

```text
1        2        3        4        5
│        │        │        │        │
         ├────────┼────────┤
         │       ITEM      │
         │     span 2      │
         └────────┴────────┘
```

```text
começa na linha 2
ocupa 2 tracks
termina na linha 4
```

---

## 42. Exemplo: largura completa

```css
.header {
  grid-column: 1 / -1;
}
```

Com quatro colunas:

```text
1        2        3        4        5
│        │        │        │        │
├────────┴────────┴────────┴────────┤
│               HEADER              │
└───────────────────────────────────┘
```

O item ocupa todas as tracks do grid explícito naquele eixo.

---

## 43. Exemplo completo de layout

```text
┌──────────────────────────────────────┐
│                HEADER                │
├────────────┬────────────┬────────────┤
│    NAV     │  CONTENT   │    ASIDE   │
├────────────┴────────────┴────────────┤
│               FOOTER                 │
└──────────────────────────────────────┘
```

Utilizando apenas posicionamento por colunas:

```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}

.header {
  grid-column: 1 / -1;
}

.nav {
  grid-column: 1;
}

.content {
  grid-column: 2;
}

.aside {
  grid-column: 3;
}

.footer {
  grid-column: 1 / -1;
}
```

> **Nota:** aqui as linhas não foram definidas com `grid-row`. O resultado depende do auto-placement e da ordem dos itens no HTML (header, nav, content, aside, footer).

---

## 44. Se quisermos controlar o eixo vertical

`grid-column` controla o eixo das colunas. Para controlar o eixo das linhas, existe `grid-row`:

```text
                 COLUNAS
            1      2      3      4
            │      │      │      │
linha 1 ────┼──────┼──────┼──────┼────
            │      │      │      │
linha 2 ────┼──────┼──────┼──────┼────
            │      │      │      │
linha 3 ────┼──────┼──────┼──────┼────
            │      │      │      │
linha 4 ────┼──────┼──────┼──────┼────
```

```text
grid-column
     ↓
eixo das colunas

grid-row
     ↓
eixo das linhas
```

---

## 45. Como ler qualquer `grid-column`

```css
grid-column: 2 / 5;
```

Primeiro:

```text
START = 2
```

Depois:

```text
END = 5
```

Depois, conte as tracks:

```text
2 → 3
3 → 4
4 → 5
```

Resultado:

```text
3 tracks
```

---

## 46. Como ler um `span`

```css
grid-column: 3 / span 2;
```

```text
START = 3
SPAN  = 2 tracks
```

```text
3 → 4
4 → 5
```

Resultado:

```text
END = 5
```

---

## 47. Modelo mental definitivo

```text
GRID CONTAINER
      ↓
GRID ITEMS
      ↓
GRID TRACKS
      ↓
GRID LINES
      ↓
POSICIONAMENTO
      ↓
grid-column
      ↓
START / END
     ou
START / SPAN
```

---

## 48. Uma analogia simples

Imagine o Grid como um estacionamento dividido por linhas:

```text
linha
│
▼
| vaga | vaga | vaga |
        ↑
      linha
```

As linhas são as divisões. As vagas são os espaços entre essas divisões.

Quando você escreve:

```css
grid-column: 1 / 3;
```

não está dizendo:

```text
"quero a vaga 1 até a vaga 3"
```

Está dizendo:

```text
"comece na divisão 1 e termine na divisão 3"
```

Assim, o item ocupa os espaços existentes entre elas.

```text
1      2      3
│      │      │
├──────┼──────┤
│      ITEM   │
└──────┴──────┘
```

---

## 49. Resumo visual dos principais formatos

```text
grid-column: 2;

1        2        3        4
│        │        │        │
         ├────────┤
           ITEM


grid-column: 1 / 3;

1        2        3        4
│        │        │        │
├────────┼────────┤
│        ITEM     │


grid-column: 2 / 4;

1        2        3        4
│        │        │        │
         ├────────┼────────┤
         │  ITEM  │


grid-column: 2 / span 2;

1        2        3        4
│        │        │        │
         ├────────┼────────┤
         │  ITEM  │


grid-column: 1 / -1;

1        2        3        4        5
│        │        │        │        │
├────────┴────────┴────────┴────────┤
│                ITEM               │
```

---

## 50. Relação entre `start`, `end` e `span`

```css
grid-column: 2 / 5;
```

```css
grid-column: 2 / span 3;
```

Os dois representam:

```text
2 → 3
3 → 4
4 → 5
```

Ou seja, 3 tracks. Mas:

```text
2 / 5
→ START + END

2 / span 3
→ START + QUANTIDADE
```

---

## 51. Tabela de fixação

| Conceito | Significado |
| --- | --- |
| `grid-column` | shorthand para `grid-column-start` e `grid-column-end` |
| `grid-column-start` | define a linha inicial |
| `grid-column-end` | define a linha final |
| `/` | separa início e fim |
| `grid line` | linha estrutural que delimita tracks |
| `grid track` | espaço entre duas grid lines |
| `grid column` | track no eixo das colunas |
| número positivo | referencia linhas contando do início |
| número negativo | referencia linhas contando do final do grid explícito |
| `-1` | última linha do grid explícito |
| `span` | indica quantidade de tracks |
| `[nome]` | cria nome para uma grid line |
| `nome-start` | linha inicial associada a uma área nomeada |
| `nome-end` | linha final associada a uma área nomeada |
| `grid-template-areas` | define áreas nomeadas do Grid |
| `grid-auto-flow` | controla o auto-placement |
| `dense` | permite ao auto-placement tentar preencher lacunas anteriores |

---

## 52. Checklist mental

Antes de escrever um `grid-column`, pense:

```text
□ Quantas column tracks existem?
□ Quantas grid lines existem?
□ Qual é a linha inicial?
□ Qual é a linha final?
□ Quero definir END ou usar SPAN?
□ Preciso utilizar -1?
□ Existem linhas nomeadas?
□ O item será posicionado explicitamente?
□ Os outros itens continuarão em auto-placement?
□ Existem tracks implícitas?
```

---

## 53. Erros conceituais que devemos evitar

### Erro 1

Pensar que:

```css
grid-column: 1 / 4;
```

significa "quatro colunas".

O correto é:

```text
linha 1 → linha 4
```

resultando em 3 tracks.

### Erro 2

Pensar que `grid line` e `grid track` são a mesma coisa.

```text
LINE  = limite
TRACK = espaço entre limites
```

### Erro 3

Pensar que `span` significa linha.

```text
span = quantidade de tracks
```

### Erro 4

Pensar que `-1` representa qualquer linha final criada pelo Grid.

```text
-1 = última linha do grid explícito
```

Tracks implícitas ficam fora dessa contagem.

### Erro 5

Pensar que `grid-column` controla os dois eixos.

Ele trabalha no eixo das colunas. Para o outro eixo existe `grid-row`.

---

## 54. Mapa mental final

```text
CSS GRID
│
├── GRID CONTAINER
│   │
│   └── GRID ITEMS
│
├── GRID EXPLÍCITO
│   ├── grid-template-columns
│   ├── grid-template-rows
│   └── grid-template-areas
│
├── GRID IMPLÍCITO
│   ├── grid-auto-columns
│   └── grid-auto-rows
│
├── GRID STRUCTURE
│   │
│   ├── GRID LINE
│   │      ↓
│   │    delimita
│   │
│   └── GRID TRACK
│          ↓
│        ocupa o espaço
│
└── POSICIONAMENTO
    │
    ├── grid-column
    │   │
    │   ├── start
    │   ├── end
    │   ├── span
    │   ├── números
    │   └── nomes
    │
    └── grid-row
```

---

## 55. Regra de ouro

```text
╔══════════════════════════════════════════════╗
║                                              ║
║  GRID-COLUMN PENSA EM GRID LINES.            ║
║                                              ║
║  AS TRACKS EXISTEM ENTRE ESSAS LINHAS.       ║
║                                              ║
║  START = onde começa                         ║
║  END   = onde termina                        ║
║  SPAN  = quantas tracks ocupa                ║
║                                              ║
╚══════════════════════════════════════════════╝
```

```css
grid-column: 1 / -1;
```

```text
comece na primeira linha
↓
atravesse todas as tracks
↓
termine na última linha do grid explícito
```

```css
grid-column: 2 / span 3;
```

```text
comece na linha 2
↓
ocupe 3 tracks
↓
termine na linha 5
```

---

## 56. Referências técnicas

A base conceitual desta documentação pode ser conferida nas especificações e documentações técnicas de CSS Grid Layout:

* CSS Grid Layout Module
* MDN Web Docs — `grid-column`
* MDN Web Docs — `grid-column-start`
* MDN Web Docs — `grid-column-end`
* MDN Web Docs — `grid-template-columns`
* MDN Web Docs — `grid-template-areas`
* MDN Web Docs — `grid-auto-flow`

Conceitos técnicos utilizados:

```text
grid container
grid item
grid line
grid track
explicit grid
implicit grid
auto-placement
named grid lines
```

---

## 57. GitHub

<div align="center">

### CSS Grid Layout — `grid-column`

Documentação técnica para estudo de CSS Grid Layout.

<br>

<a href="https://github.com/gabrielfelipeoliveira55" target="_blank" rel="noopener noreferrer">
Gabriel Felipe de Oliveira Rateiro
</a>

<br><br>

**CSS Grid Layout • `grid-column`**

> Não decore os números.
> Entenda as linhas, as tracks e a área que o item ocupa.

</div>