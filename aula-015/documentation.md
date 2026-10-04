# CSS Grid Layout — `grid-column`

> **Objetivo:** compreender como `grid-column` posiciona e dimensiona um `grid item` no eixo das colunas, utilizando números, linhas nomeadas, `span`, valores negativos e a relação com `grid-column-start` e `grid-column-end`.

---

# 1. Fundamentos necessários para entender `grid-column`

Antes de estudar `grid-column`, precisamos entender quem participa de um Grid.

Um elemento se transforma em **grid container** quando recebe:

```css
.container {
  display: grid;
}
```

Os **filhos diretos** desse elemento tornam-se **grid items**.

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

Podemos visualizar:

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

Nesse caso:

```text
.grid
│
└── .item
    │
    └── .interno
```

`.item` é filho direto de `.grid`, portanto participa diretamente daquele Grid.

`.interno` não é filho direto de `.grid`.

Isso é importante porque `grid-column` trabalha com o posicionamento do **grid item naquele contexto de Grid**.

## Regra mental

```text
GRID CONTAINER
      │
      └── FILHOS DIRETOS
                ↓
           GRID ITEMS
```

---

# 2. Grid explícito e grid implícito

O CSS Grid pode trabalhar com uma estrutura definida explicitamente pelo autor e também criar tracks implicitamente quando necessário.

## Grid explícito

É a parte definida por propriedades como:

```css
grid-template-columns
grid-template-rows
grid-template-areas
```

Exemplo:

```css
.grid {
  display: grid;

  grid-template-columns:
    1fr
    1fr
    1fr;
}
```

Temos:

```text
┌──────────┬──────────┬──────────┐
│ Coluna 1 │ Coluna 2 │ Coluna 3 │
└──────────┴──────────┴──────────┘
```

Essas três colunas fazem parte do **grid explícito**.

## Grid implícito

O Grid pode precisar de tracks que não foram definidas diretamente.

Por exemplo:

```css
.grid {
  display: grid;

  grid-template-columns:
    100px
    100px;
}
```

O grid explícito possui duas colunas.

Se o posicionamento dos itens exigir uma terceira coluna, o navegador pode criar uma coluna implícita.

Visualmente:

```text
GRID EXPLÍCITO

┌─────────┬─────────┐
│         │         │
│ Coluna 1│ Coluna 2│
│         │         │
└─────────┴─────────┘

              ↓

NECESSIDADE DE MAIS ESPAÇO

              ↓

GRID COM TRACK IMPLÍCITA

┌─────────┬─────────┬────────────┐
│         │         │            │
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

# 3. Estrutura interna do Grid

Agora precisamos separar três conceitos:

```text
Grid Line
Grid Track
Grid Column
```

Eles estão relacionados, mas não são a mesma coisa.

---

# 4. Grid line

Uma **grid line** é uma linha estrutural que delimita as tracks do Grid.

Imagine três colunas:

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

Portanto:

```text
3 colunas → 4 linhas verticais
```

---

# 5. Grid track

Uma **grid track** é o espaço existente entre duas grid lines.

Observe:

```text
linha 1             linha 2
   │                   │
   ▼                   ▼
   ├───────────────────┤
          TRACK
```

Portanto:

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

# 6. Grid column

No eixo horizontal do Grid, cada **column track** representa uma coluna.

Por exemplo:

```text
┌──────────┬──────────┬──────────┐
│ Coluna 1 │ Coluna 2 │ Coluna 3 │
└──────────┴──────────┴──────────┘
```

Observe a relação:

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

# 7. A propriedade `grid-column`

A propriedade:

```css
grid-column
```

é utilizada para posicionar um grid item no eixo das colunas.

Exemplo:

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

Visualmente:

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

Portanto:

```text
2 / 4 = 2 tracks
```

---

# 8. `grid-column` é um shorthand

`grid-column` é uma forma abreviada de:

```css
grid-column-start
grid-column-end
```

Assim:

```css
grid-column: 2 / 4;
```

equivale a:

```css
grid-column-start: 2;
grid-column-end: 4;
```

Podemos visualizar:

```text
              grid-column
                   │
          ┌────────┴────────┐
          │                 │
          ▼                 ▼
 grid-column-start   grid-column-end
```

---

# 9. O significado da barra `/`

Quando escrevemos:

```css
grid-column: 1 / 4;
```

a barra separa dois valores:

```text
START / END
```

ou:

```text
INÍCIO / FIM
```

Não é uma divisão matemática.

Ela separa:

```css
grid-column-start
```

de:

```css
grid-column-end
```

---

# 10. Entendendo `1 / 4`

Imagine um Grid com três colunas:

```text
      1          2          3          4
      │          │          │          │
      ▼          ▼          ▼          ▼
      ┌──────────┬──────────┬──────────┐
      │          │          │          │
      │          │          │          │
      └──────────┴──────────┴──────────┘
```

Se escrevermos:

```css
grid-column: 1 / 4;
```

o item ocupará:

```text
1 → 2
2 → 3
3 → 4
```

Portanto:

```text
3 tracks
```

Atenção:

```text
1 / 4
```

não significa:

```text
coluna 1 até coluna 4
```

Significa:

```text
linha 1 até linha 4
```

---

# 11. Uma regra fundamental

```text
grid-column posiciona usando GRID LINES.

As tracks ficam ENTRE essas linhas.
```

Exemplo:

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

Logo:

```text
1 / 4
```

significa:

```text
3 tracks
```

---

# 12. Quando apenas um valor é informado

Também podemos escrever:

```css
grid-column: 2;
```

Nesse caso, estamos indicando a linha inicial e deixando a outra parte para resolução automática.

Um comportamento comum é o item ocupar uma única track.

Podemos pensar:

```text
grid-column: 2;

START = 2
END = automático
```

Visualmente:

```text
linha 1     linha 2     linha 3     linha 4
   │           │           │           │
               ├───────────┤
                   ITEM
```

---

# 13. Posicionamento por números

Podemos utilizar números positivos para identificar grid lines.

Exemplo:

```css
.item {
  grid-column: 2;
}
```

Ou:

```css
.item {
  grid-column: 2 / 4;
}
```

A contagem positiva começa pelo lado inicial do eixo:

```text
1 → 2 → 3 → 4 → 5
```

---

# 14. Relação entre linhas e quantidade de colunas

Se temos:

```css
grid-template-columns: repeat(3, 1fr);
```

temos:

```text
3 column tracks
```

e:

```text
4 grid lines verticais
```

Visualmente:

```text
1          2          3          4
│          │          │          │
├──────────┼──────────┼──────────┤
```

Se tivermos cinco colunas:

```css
grid-template-columns: repeat(5, 1fr);
```

teremos seis linhas verticais:

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

# 15. Números negativos

O Grid também permite utilizar números negativos.

Exemplo:

```css
grid-column: -1;
```

Os índices negativos são contados a partir da extremidade final do grid explícito.

Por exemplo:

```text
1          2          3          4
│          │          │          │
├──────────┼──────────┼──────────┤
│          │          │          │
├──────────┼──────────┼──────────┤
-4        -3         -2         -1
```

Podemos pensar:

```text
linha 4 = -1
linha 3 = -2
linha 2 = -3
linha 1 = -4
```

---

# 16. O caso especial de `-1`

Um padrão muito utilizado é:

```css
grid-column: 1 / -1;
```

Isso significa:

```text
comece na primeira linha
termine na última linha do grid explícito
```

Exemplo:

```css
.grid {
  display: grid;

  grid-template-columns:
    repeat(4, 1fr);
}

.header {
  grid-column: 1 / -1;
}
```

Resultado:

```text
┌─────────────────────────────────────┐
│               HEADER                │
├───────────┬───────────┬─────────────┤
│           │           │             │
│           │           │             │
├───────────┼───────────┼─────────────┤
│           │           │             │
└───────────┴───────────┴─────────────┘
```

Para o entendimento técnico, lembre-se:

```text
-1 = última linha do grid explícito
```

---

# 17. Por que `-1` é tão útil?

Imagine:

```css
grid-template-columns:
  repeat(4, 1fr);
```

Você poderia fazer:

```css
grid-column: 1 / 5;
```

Mas depois poderia alterar o Grid para:

```css
grid-template-columns:
  repeat(6, 1fr);
```

Nesse caso teria que alterar também:

```css
grid-column: 1 / 5;
```

para:

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

# 18. O valor `span`

A palavra:

```css
span
```

indica uma quantidade de tracks que o item deve ocupar.

Por exemplo:

```css
grid-column: span 2;
```

significa:

```text
ocupe 2 tracks
```

Portanto:

```text
span = extensão / quantidade de tracks
```

---

# 19. `grid-column: 1 / span 2`

Considere:

```css
grid-column: 1 / span 2;
```

Leia:

```text
comece na linha 1
+
ocupe 2 tracks
```

Visualmente:

```text
linha 1      linha 2      linha 3
   │            │            │
   ├────────────┼────────────┤
   │            ITEM         │
   │         span 2          │
   └────────────┴────────────┘
```

O resultado é:

```text
1 → 2
2 → 3
```

Portanto:

```text
2 tracks
```

---

# 20. `2 / 4` e `2 / span 2`

Estas duas formas podem chegar ao mesmo resultado:

```css
grid-column: 2 / 4;
```

e:

```css
grid-column: 2 / span 2;
```

Porque:

```text
2 / 4

2 → 3
3 → 4

= 2 tracks
```

Enquanto:

```text
2 / span 2

começa em 2
+
2 tracks

= termina em 4
```

Mas a intenção expressa é diferente.

### `2 / 4`

```text
comece em 2 e termine em 4
```

### `2 / span 2`

```text
comece em 2 e ocupe 2 tracks
```

---

# 21. `grid-column: span 2`

Podemos ainda escrever:

```css
grid-column: span 2;
```

Aqui a quantidade está definida:

```text
2 tracks
```

mas a linha inicial não foi especificada diretamente nessa declaração.

O Grid poderá determinar a posição inicial pelo algoritmo de posicionamento automático, quando aplicável.

Mentalmente:

```text
posição inicial → pode ser automática
quantidade      → 2 tracks
```

---

# 22. `grid-column-start`

A propriedade:

```css
grid-column-start
```

define a linha inicial do item no eixo das colunas.

Exemplo:

```css
.item {
  grid-column-start: 2;
}
```

Podemos interpretar como:

```text
START = linha 2
```

Visualmente:

```text
1          2          3          4
│          │          │          │
           ▼
           ┌──────────┐
           │   ITEM   │
           └──────────┘
```

---

# 23. `grid-column-end`

A propriedade:

```css
grid-column-end
```

define a linha final do item no eixo das colunas.

Exemplo:

```css
.item {
  grid-column-end: 4;
}
```

Aqui estamos definindo:

```text
END = linha 4
```

Quando queremos controle explícito do início e do fim:

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

# 24. Linhas nomeadas

Até agora utilizamos números:

```text
1
2
3
4
```

Mas as grid lines podem receber nomes.

Exemplo:

```css
.grid {
  display: grid;

  grid-template-columns:
    [inicio] 1fr
    [meio] 1fr
    [fim] 1fr;
}
```

Podemos imaginar:

```text
[inicio]        [meio]        [fim]        [linha final]
    │              │             │               │
    ├──────────────┼─────────────┼───────────────┤
```

Agora as linhas possuem nomes que podem ser utilizados para posicionamento.

---

# 25. Utilizando as linhas nomeadas

Podemos escrever:

```css
.item {
  grid-column: inicio / fim;
}
```

em vez de:

```css
.item {
  grid-column: 1 / 4;
}
```

Isso pode tornar o CSS mais semântico.

Compare:

```css
grid-column: 1 / 4;
```

com:

```css
grid-column: inicio / fim;
```

A segunda forma comunica melhor a intenção estrutural do layout.

---

# 26. Linhas nomeadas com `grid-column-start` e `grid-column-end`

Também podemos usar:

```css
.item {
  grid-column-start: inicio;
  grid-column-end: fim;
}
```

que corresponde a:

```css
.item {
  grid-column: inicio / fim;
}
```

---

# 27. Uma mesma linha pode possuir vários nomes

É possível colocar vários nomes em uma mesma grid line.

Exemplo:

```css
.grid {
  display: grid;

  grid-template-columns:
    [sidebar-end main-start] 200px
    [main-end] 1fr;
}
```

Nesse exemplo, uma única linha pode ser referenciada tanto como:

```text
sidebar-end
```

quanto:

```text
main-start
```

Isso é útil porque uma mesma linha pode representar semanticamente o limite de duas regiões diferentes.

---

# 28. Linhas nomeadas repetidas

Também podemos ter nomes repetidos.

Exemplo:

```css
.grid {
  display: grid;

  grid-template-columns:
    repeat(4, [col-start] 1fr);
}
```

Agora existem múltiplas linhas chamadas:

```text
col-start
```

É possível especificar uma ocorrência determinada do nome:

```css
.item {
  grid-column: col-start 2 / col-start 4;
}
```

A ideia é:

```text
nome + índice da ocorrência
```

---

# 29. `grid-template-areas`

Outra forma importante de organizar um Grid é:

```css
grid-template-areas
```

Podemos definir:

```css
.grid {
  display: grid;

  grid-template-columns:
    repeat(3, 1fr);

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

Aqui estamos nomeando regiões do Grid.

---

# 30. Áreas nomeadas e suas bordas

Uma área como:

```text
header
```

possui bordas estruturais.

Conceitualmente:

```text
header-start
      ↓
┌───────┬───────┬───────┐
│ header │ header│ header│
└───────┴───────┴───────┘
                        ↑
                    header-end
```

As áreas nomeadas estão relacionadas a linhas que podem ser utilizadas no posicionamento.

---

# 31. Usando o nome de uma área

Podemos associar um item a uma área utilizando:

```css
.header {
  grid-area: header;
}
```

Isso posiciona o item na área completa, considerando os dois eixos.

Também podemos trabalhar diretamente com as linhas relacionadas à área:

```css
.item {
  grid-column: header-start / header-end;
}
```

A ideia é:

```text
header-start
     ↓
 início da área no eixo das colunas

header-end
     ↓
 fim da área no eixo das colunas
```

---

# 32. `grid-area` e `grid-column`

Essas propriedades não são a mesma coisa.

```text
grid-area
├── eixo das colunas
└── eixo das linhas
```

Enquanto:

```text
grid-column
└── eixo das colunas
```

Portanto:

```css
grid-area: header;
```

pode representar uma área inteira.

Já:

```css
grid-column: 1 / -1;
```

trabalha apenas com a dimensão horizontal do posicionamento.

---

# 33. Auto-placement

Nem todos os grid items precisam ter seu posicionamento definido manualmente.

Quando um item não possui um posicionamento explícito suficiente para determinar sua posição, o Grid utiliza o algoritmo de **auto-placement**.

Exemplo:

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

  grid-template-columns:
    repeat(3, 1fr);
}
```

O navegador distribui os itens de acordo com o algoritmo de posicionamento automático.

---

# 34. `grid-auto-flow`

A propriedade:

```css
grid-auto-flow
```

controla como os itens com posicionamento automático são inseridos no Grid.

O valor padrão é:

```css
grid-auto-flow: row;
```

Podemos pensar:

```text
→ → →
→ → →
→ → →
```

Ou seja, o preenchimento ocorre seguindo as linhas do Grid.

---

# 35. O que acontece quando um item é colocado manualmente?

Imagine:

```css
.item-1 {
  grid-column: 2;
}
```

Temos algo como:

```text
┌─────────┬─────────┬─────────┐
│         │ Item 1  │         │
├─────────┼─────────┼─────────┤
│         │         │         │
└─────────┴─────────┴─────────┘
```

Os outros itens ainda precisam ser posicionados.

O navegador precisa continuar executando o auto-placement para eles.

Dependendo da situação, isso pode resultar em espaços vazios.

---

# 36. `grid-auto-flow: dense`

Podemos solicitar um comportamento mais agressivo na tentativa de preencher espaços:

```css
.grid {
  grid-auto-flow: dense;
}
```

O modo `dense` faz com que o algoritmo procure oportunidades de preencher espaços anteriores que ficaram vazios, quando isso for possível.

Conceitualmente:

```text
SEM dense

┌───────┬───────┬───────┐
│   1   │   2   │       │
├───────┼───────┼───────┤
│   3   │       │       │
└───────┴───────┴───────┘
```

Com uma disposição apropriada:

```text
COM dense

┌───────┬───────┬───────┐
│   1   │   2   │   4   │
├───────┼───────┼───────┤
│   3   │       │       │
└───────┴───────┴───────┘
```

O objetivo do `dense` é melhorar o preenchimento visual do Grid.

---

# 37. Cuidado com `dense`

`dense` não significa:

```text
"o Grid ignora tudo que eu defini"
```

Ele atua no algoritmo de auto-placement.

Itens com posicionamento explícito continuam respeitando a posição que foi determinada.

---

# 38. Exemplo completo com três colunas

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

  grid-template-columns:
    repeat(3, 1fr);

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

Visualmente:

```text
linha 1    linha 2    linha 3    linha 4
   │          │          │          │
   ▼          ▼          ▼          ▼
┌──────────┬──────────┬──────────┐
│    1     │    2     │    3     │
└──────────┴──────────┴──────────┘
```

---

# 39. Exemplo: ocupar duas colunas

```css
.item-1 {
  grid-column: 1 / 3;
}
```

Visualização:

```text
linha 1       linha 2       linha 3       linha 4
   │             │             │             │
   ├─────────────┼─────────────┤
   │             ITEM          │
   │          2 tracks         │
   └─────────────┴─────────────┘
```

---

# 40. Exemplo equivalente usando `span`

```css
.item-1 {
  grid-column: 1 / span 2;
}
```

A leitura é:

```text
comece na linha 1
+
ocupe 2 tracks
```

Resultado:

```text
linha 1       linha 2       linha 3
   │             │             │
   ├─────────────┼─────────────┤
   │             ITEM          │
   │          span 2           │
   └─────────────┴─────────────┘
```

---

# 41. Exemplo: começar em 2 e ocupar 2 tracks

```css
.item {
  grid-column: 2 / span 2;
}
```

Visualmente:

```text
1        2        3        4        5
│        │        │        │        │
         ├────────┼────────┤
         │       ITEM      │
         │     span 2      │
         └────────┴────────┘
```

O item:

```text
começa na linha 2
ocupa 2 tracks
termina na linha 4
```

---

# 42. Exemplo: largura completa

```css
.header {
  grid-column: 1 / -1;
}
```

Com quatro colunas:

```text
1        2        3        4        5
│        │        │        │        │
┌────────┴────────┴────────┴────────┐
│               HEADER              │
└────────┬────────┬────────┬────────┘
```

O item ocupa todas as tracks do grid explícito naquele eixo.

---

# 43. Exemplo completo de layout

Podemos representar este layout:

```text
┌──────────────────────────────────────┐
│                HEADER                │
├────────────┬────────────┬────────────┤
│    NAV     │  CONTENT   │    ASIDE   │
├────────────┴────────────┴────────────┤
│               FOOTER                 │
└──────────────────────────────────────┘
```

Utilizando apenas posicionamento por Grid:

```css
.grid {
  display: grid;

  grid-template-columns:
    repeat(3, 1fr);
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

---

# 44. Se quisermos controlar o eixo vertical

`grid-column` controla:

```text
eixo das colunas
```

Para controlar o eixo das linhas, existe:

```css
grid-row
```

Modelo:

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

Assim:

```text
grid-column
     ↓
eixo das colunas

grid-row
     ↓
eixo das linhas
```

---

# 45. Como ler qualquer `grid-column`

Quando encontrar:

```css
grid-column: 2 / 5;
```

faça mentalmente:

### Primeiro

```text
START = 2
```

### Depois

```text
END = 5
```

### Depois

Conte as tracks:

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

# 46. Como ler um `span`

Quando encontrar:

```css
grid-column: 3 / span 2;
```

pense:

```text
START = 3
SPAN  = 2 tracks
```

Depois:

```text
3 → 4
4 → 5
```

Resultado:

```text
END = 5
```

---

# 47. Modelo mental definitivo

Todo esse assunto pode ser reduzido a uma sequência:

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

# 48. Uma analogia simples

Imagine o Grid como um estacionamento dividido por linhas:

```text
linha
│
▼
| vaga | vaga | vaga |
        ↑
      linha
```

As linhas são as divisões.

As vagas são os espaços entre essas divisões.

Quando você diz:

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

Assim o item ocupa os espaços existentes entre elas.

```text
1      2      3
│      │      │
├──────┼──────┤
│      ITEM   │
└──────┴──────┘
```

---

# 49. Resumo visual dos principais formatos

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

# 50. Relação entre `start`, `end` e `span`

Compare:

```css
grid-column: 2 / 5;
```

com:

```css
grid-column: 2 / span 3;
```

Os dois podem representar:

```text
2 → 3
3 → 4
4 → 5
```

Ou seja:

```text
3 tracks
```

Mas:

```text
2 / 5
```

define:

```text
START + END
```

Enquanto:

```text
2 / span 3
```

define:

```text
START + QUANTIDADE
```

---

# 51. Tabela de fixação

| Conceito              | Significado                                                   |
| --------------------- | ------------------------------------------------------------- |
| `grid-column`         | shorthand para `grid-column-start` e `grid-column-end`        |
| `grid-column-start`   | define a linha inicial                                        |
| `grid-column-end`     | define a linha final                                          |
| `/`                   | separa início e fim                                           |
| `grid line`           | linha estrutural que delimita tracks                          |
| `grid track`          | espaço entre duas grid lines                                  |
| `grid column`         | track no eixo das colunas                                     |
| número positivo       | referencia linhas contando do início                          |
| número negativo       | referencia linhas contando do final do grid explícito         |
| `-1`                  | última linha do grid explícito                                |
| `span`                | indica quantidade de tracks                                   |
| `[nome]`              | cria nome para uma grid line                                  |
| `nome-start`          | linha inicial associada a uma área nomeada                    |
| `nome-end`            | linha final associada a uma área nomeada                      |
| `grid-template-areas` | define áreas nomeadas do Grid                                 |
| `grid-auto-flow`      | controla o auto-placement                                     |
| `dense`               | permite ao auto-placement tentar preencher lacunas anteriores |

---

# 52. Checklist mental

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

# 53. Erros conceituais que devemos evitar

## Erro 1

Pensar:

```text
grid-column: 1 / 4;
```

como:

```text
"quatro colunas"
```

O correto é:

```text
linha 1 → linha 4
```

resultando em:

```text
3 tracks
```

## Erro 2

Pensar que:

```text
grid line
```

e:

```text
grid track
```

são a mesma coisa.

Não são.

```text
LINE = limite
TRACK = espaço entre limites
```

## Erro 3

Pensar que `span` significa linha.

Não.

```text
span = quantidade de tracks
```

## Erro 4

Pensar que `-1` representa qualquer linha final criada pelo Grid.

Para o modelo de estudo:

```text
-1 = última linha do grid explícito
```

## Erro 5

Pensar que `grid-column` controla os dois eixos.

Ele trabalha no eixo das colunas.

Para o outro eixo temos:

```css
grid-row
```

---

# 54. Mapa mental final

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

# 55. Regra de ouro

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

Portanto:

```css
grid-column: 1 / -1;
```

pode ser entendido como:

```text
comece na primeira linha
↓
atravesse todas as tracks
↓
termine na última linha do grid explícito
```

E:

```css
grid-column: 2 / span 3;
```

como:

```text
comece na linha 2
↓
ocupe 3 tracks
↓
termine na linha correspondente
```

---

# 56. Referências técnicas

A base conceitual desta documentação deve ser conferida principalmente nas especificações e documentações técnicas de CSS Grid Layout, incluindo:

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

# 57. GitHub

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
