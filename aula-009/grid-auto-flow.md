# CSS Grid — `grid-auto-flow`

## Índice

1. [O que é `grid-auto-flow`?](#1-o-que-é-grid-auto-flow)
2. [O comportamento padrão](#2-o-comportamento-padrão)
3. [`grid-auto-flow: row`](#3-grid-auto-flow-row)
4. [`grid-auto-flow: column`](#4-grid-auto-flow-column)
5. [`row` x `column`](#5-row-x-column)
6. [`grid-auto-flow: column` e a definição das linhas](#6-grid-auto-flow-column-e-a-definição-das-linhas)
7. [Por que novas colunas podem aparecer?](#7-por-que-novas-colunas-podem-aparecer)
8. [`grid-auto-flow: column` + `grid-auto-columns`](#8-grid-auto-flow-column--grid-auto-columns)
9. [Relação com `grid-auto-columns`](#9-relação-com-grid-auto-columns)
10. [Exemplo com `grid-template-columns`](#10-exemplo-com-grid-template-columns)
11. [O fluxo não altera a definição das colunas](#11-o-fluxo-não-altera-a-definição-das-colunas)
12. [`grid-auto-flow` não define tamanho](#12-grid-auto-flow-não-define-tamanho)
13. [Exemplo comparativo](#13-exemplo-comparativo)
14. [`grid-auto-flow: dense`](#14-grid-auto-flow-dense)
15. [O problema que `dense` tenta resolver](#15-o-problema-que-dense-tenta-resolver)
16. [Espaços vazios](#16-espaços-vazios)
17. [O que `dense` tenta fazer?](#17-o-que-dense-tenta-fazer)
18. [`dense` pode alterar a ordem visual](#18-dense-pode-alterar-a-ordem-visual)
19. [Quando `dense` é útil?](#19-quando-dense-é-útil)
20. [Quando evitar `dense`?](#20-quando-evitar-dense)
21. [`dense` é diferente de `column`](#21-dense-é-diferente-de-column)
22. [Combinações possíveis](#22-combinações-possíveis)
23. [Mapa mental — `grid-auto-flow`](#23-mapa-mental--grid-auto-flow)
24. [Mapa mental — relação com outras propriedades](#24-mapa-mental--relação-com-outras-propriedades)
25. [Exemplo completo — fluxo por linhas](#25-exemplo-completo--fluxo-por-linhas)
26. [Exemplo completo — fluxo por colunas](#26-exemplo-completo--fluxo-por-colunas)
27. [Exemplo completo — `dense`](#27-exemplo-completo--dense)
28. [Relação com `grid-column`](#28-relação-com-grid-column)
29. [`dense` é uma estratégia de preenchimento](#29-dense-é-uma-estratégia-de-preenchimento)
30. [⚠️ Cuidado com a ordem](#30-️-cuidado-com-a-ordem)
31. [`grid-auto-flow` e responsividade](#31-grid-auto-flow-e-responsividade)
32. [Mapa mental — escolha rápida](#32-mapa-mental--escolha-rápida)
33. [Regra mental definitiva](#33-regra-mental-definitiva)
34. [Resumo das propriedades relacionadas](#34-resumo-das-propriedades-relacionadas)
35. [📌 Resumo final](#35--resumo-final)
36. [🧠 Três frases para guardar](#36--três-frases-para-guardar)

---

# 1. O que é `grid-auto-flow`?

A propriedade:

```css
grid-auto-flow
```

controla **como o algoritmo de posicionamento automático distribui os Grid Items** pela grade.

Quando os itens não possuem uma posição explícita, o Grid utiliza o algoritmo de auto-placement para decidir onde cada item será colocado.

Por padrão, o fluxo utiliza:

```css
grid-auto-flow: row;
```

Isso significa que o Grid preenche cada linha e, quando necessário, cria novas linhas.

Também podemos alterar o fluxo para:

```css
grid-auto-flow: column;
```

Nesse caso, o Grid preenche cada coluna e, quando necessário, cria novas colunas.

### Ideia principal

```text
grid-auto-flow

        ↓

controla o fluxo automático

        ↓

como os itens são distribuídos
```

---

# 2. O comportamento padrão

Por padrão, `grid-auto-flow` possui o valor:

```css
grid-auto-flow: row;
```

Ou seja, os itens são colocados **preenchendo cada linha sucessivamente**.

Imagine:

```css
.grid {
  display: grid;

  grid-template-columns:
    1fr
    1fr;
}
```

Com vários itens:

```text
┌──────────┬──────────┐
│ Item 1   │ Item 2   │
├──────────┼──────────┤
│ Item 3   │ Item 4   │
├──────────┼──────────┤
│ Item 5   │ Item 6   │
└──────────┴──────────┘
```

O preenchimento acontece assim:

```text
1 → 2
3 → 4
5 → 6
```

### Regra mental

> **`row` preenche cada linha por vez e cria novas linhas quando necessário.**

---

# 3. `grid-auto-flow: row`

Podemos declarar explicitamente:

```css
.grid {
  grid-auto-flow: row;
}
```

Esse é o comportamento padrão.

Visualmente:

```text
1 → 2 → 3

4 → 5 → 6

7 → 8 → 9
```

O Grid preenche uma linha, passa para a próxima e cria novas linhas implícitas quando necessário.

---

# 4. `grid-auto-flow: column`

Podemos alterar o fluxo:

```css
.grid {
  grid-auto-flow: column;
}
```

Agora o Grid tenta preencher **cada coluna por vez**, adicionando novas colunas quando necessário.

Para visualizar claramente esse comportamento, é comum definir previamente a quantidade de linhas:

```css
.grid {
  display: grid;

  grid-template-rows:
    100px
    100px;

  grid-auto-flow: column;
}
```

Nesse cenário, os itens podem ser distribuídos assim:

```text
┌──────────┬──────────┬──────────┐
│ Item 1   │ Item 3   │ Item 5   │
├──────────┼──────────┼──────────┤
│ Item 2   │ Item 4   │ Item 6   │
└──────────┴──────────┴──────────┘
```

A direção mudou:

```text
1
↓

2

→

3
↓

4
```

e assim por diante.

---

# 5. `row` x `column`

A diferença principal:

```text
grid-auto-flow: row;

↓

preenche por linhas
```

```text
grid-auto-flow: column;

↓

preenche por colunas
```

### Mapa mental

```text
                grid-auto-flow

                       │

              ┌────────┴────────┐
              ↓                 ↓

            row              column
              │                 │
              ↓                 ↓

         novas linhas      novas colunas
```

### Corte mental ①

```text
row

→ "continue na próxima linha"

column

→ "continue na próxima coluna"
```

---

# 6. `grid-auto-flow: column` e a definição das linhas

Esse ponto é importante.

Quando utilizamos:

```css
grid-auto-flow: column;
```

o algoritmo preenche as colunas utilizando as linhas disponíveis.

Para controlar quantos itens podem ser colocados em cada coluna antes de uma nova coluna ser criada, podemos definir as linhas explicitamente:

```css
grid-template-rows:
  100px
  100px;
```

Agora temos:

```text
Linha 1 → 100px
Linha 2 → 100px
```

E os itens podem ser distribuídos:

```text
┌──────────┬──────────┬──────────┐
│ Item 1   │ Item 3   │ Item 5   │
├──────────┼──────────┼──────────┤
│ Item 2   │ Item 4   │ Item 6   │
└──────────┴──────────┴──────────┘
```

O fluxo ocorre por colunas:

```text
Coluna 1
→ Item 1
→ Item 2

Coluna 2
→ Item 3
→ Item 4

Coluna 3
→ Item 5
→ Item 6
```

---

# 7. Por que novas colunas podem aparecer?

Imagine:

```css
.grid {
  display: grid;

  grid-template-rows:
    100px
    100px;

  grid-auto-flow: column;
}
```

Temos duas linhas.

Se existem seis itens:

```text
1
2
3
4
5
6
```

o Grid distribui:

```text
Coluna 1:
1
2

Coluna 2:
3
4

Coluna 3:
5
6
```

Visualmente:

```text
┌─────────┬─────────┬─────────┐
│    1    │    3    │    5    │
├─────────┼─────────┼─────────┤
│    2    │    4    │    6    │
└─────────┴─────────┴─────────┘
```

As colunas adicionais são criadas conforme necessário.

---

# 8. `grid-auto-flow: column` + `grid-auto-columns`

Podemos combinar:

```css
.grid {
  display: grid;

  grid-template-rows:
    100px
    100px;

  grid-auto-flow:
    column;

  grid-auto-columns:
    100px;
}
```

Agora:

```text
linhas:

100px
100px
```

e:

```text
colunas implícitas:

100px
100px
100px
...
```

Resultado:

```text
┌─────────┬─────────┬─────────┐
│    1    │    3    │    5    │
├─────────┼─────────┼─────────┤
│    2    │    4    │    6    │
└─────────┴─────────┴─────────┘
```

---

# 9. Relação com `grid-auto-columns`

Isso conecta diretamente os conceitos anteriores.

```text
grid-auto-flow: column

        ↓

o fluxo precisa avançar por colunas

        ↓

novas colunas podem ser criadas

        ↓

essas colunas são implícitas

        ↓

grid-auto-columns

        ↓

define o tamanho dessas colunas
```

### Mapa mental

```text
grid-auto-flow

      │

      ↓

"Como vou preencher?"

      │

      ├── row
      │    ↓
      │  novas linhas
      │
      └── column
           ↓
        novas colunas
             │
             ↓
      grid-auto-columns
```

---

# 10. Exemplo com `grid-template-columns`

Podemos combinar o fluxo por colunas com colunas explícitas:

```css
.grid {
  display: grid;

  grid-template-columns:
    100px
    200px
    100px;

  grid-auto-flow:
    column;
}
```

As colunas definidas continuam existindo.

O `grid-auto-flow` determina **como os itens auto-posicionados serão distribuídos**.

Se forem necessárias novas colunas, elas poderão ser adicionadas ao Grid implícito.

---

# 11. O fluxo não altera a definição das colunas

Considere:

```css
grid-template-columns:
  100px
  200px
  100px;
```

Temos:

```text
Coluna 1 → 100px
Coluna 2 → 200px
Coluna 3 → 100px
```

Se utilizarmos:

```css
grid-auto-flow: column;
```

o Grid continuará respeitando essa estrutura.

Se novas colunas implícitas forem necessárias, o tamanho delas poderá ser controlado por:

```css
grid-auto-columns
```

---

# 12. `grid-auto-flow` não define tamanho

É importante não confundir:

```css
grid-auto-flow
```

com:

```css
grid-auto-columns
```

ou:

```css
grid-auto-rows
```

Cada propriedade possui uma responsabilidade diferente.

```text
grid-auto-flow

→ define COMO os itens fluem
```

```text
grid-auto-columns

→ define O TAMANHO das colunas implícitas
```

```text
grid-auto-rows

→ define O TAMANHO das linhas implícitas
```

### Corte mental ②

```text
FLOW

→ "como os itens são distribuídos?"

AUTO-COLUMNS

→ "qual tamanho terão as colunas implícitas?"

AUTO-ROWS

→ "qual tamanho terão as linhas implícitas?"
```

---

# 13. Exemplo comparativo

## Fluxo por linhas

```css
grid-auto-flow: row;
```

```text
1 → 2 → 3

4 → 5 → 6

7 → 8 → 9
```

## Fluxo por colunas

```css
grid-auto-flow: column;
```

Com uma estrutura de linhas definida, podemos ter:

```text
1 ↓ 2 ↓ 3

4 ↓ 5 ↓ 6

7 ↓ 8 ↓ 9
```

Visualmente:

```text
ROW

┌───┬───┬───┐
│ 1 │ 2 │ 3 │
├───┼───┼───┤
│ 4 │ 5 │ 6 │
├───┼───┼───┤
│ 7 │ 8 │ 9 │
└───┴───┴───┘
```

```text
COLUMN

┌───┬───┬───┐
│ 1 │ 4 │ 7 │
├───┼───┼───┤
│ 2 │ 5 │ 8 │
├───┼───┼───┤
│ 3 │ 6 │ 9 │
└───┴───┴───┘
```

---

# 14. `grid-auto-flow: dense`

Existe também:

```css
grid-auto-flow: dense;
```

`dense` ativa um algoritmo de preenchimento que tenta ocupar **espaços vazios anteriores da grade** com itens posteriores que possam caber nesses espaços.

O objetivo é melhorar o aproveitamento da área disponível.

---

# 15. O problema que `dense` tenta resolver

Imagine três colunas:

```css
grid-template-columns:
  1fr
  1fr
  1fr;
```

Temos:

```text
┌──────┬──────┬──────┐
│  1   │  2   │  3   │
├──────┼──────┼──────┤
│  4   │  5   │  6   │
└──────┴──────┴──────┘
```

Agora imagine que o item `2` precise ocupar três colunas:

```text
┌──────┬──────┬──────┐
│  1   │      │      │
├──────┼──────┼──────┤
│  2 →──────→──────── │
└──────┴──────┴──────┘
```

Como o item `2` não cabe na primeira posição disponível, ele pode avançar para uma posição posterior.

Isso pode deixar espaços vazios.

---

# 16. Espaços vazios

Imagine:

```text
┌──────┬──────┬──────┐
│  1   │      │      │
├──────┼──────┼──────┤
│  2   │  2   │  2   │
├──────┼──────┼──────┤
│  4   │  5   │  6   │
└──────┴──────┴──────┘
```

Existem espaços vazios na primeira linha.

Sem `dense`, o algoritmo utiliza o comportamento **sparse** padrão: ele segue adiante no Grid e não volta para preencher esses espaços com itens posteriores.

---

# 17. O que `dense` tenta fazer?

Com:

```css
grid-auto-flow: dense;
```

o Grid tenta encontrar espaços vazios anteriores nos quais itens posteriores possam caber.

A ideia é:

```text
espaço vazio

     ↓

existem itens depois?

     ↓

algum deles cabe aqui?

     ↓

SIM

     ↓

tente preencher
```

Visualmente, podemos representar:

```text
ANTES

┌──────┬──────┬──────┐
│  1   │      │      │
├──────┼──────┼──────┤
│  2   │  2   │  2   │
├──────┼──────┼──────┤
│  4   │  5   │  6   │
└──────┴──────┴──────┘
```

Com `dense`, itens menores posteriores podem ocupar os espaços disponíveis:

```text
DEPOIS — conceito

┌──────┬──────┬──────┐
│  1   │  4   │  5   │
├──────┼──────┼──────┤
│  2   │  2   │  2   │
├──────┼──────┼──────┤
│  6   │  7   │  8   │
└──────┴──────┴──────┘
```

A disposição exata depende do tamanho e das posições dos itens.

---

# 18. `dense` pode alterar a ordem visual

Esse é um ponto muito importante.

Sem `dense`, o algoritmo sparse preserva a progressão normal do auto-placement e não volta para preencher lacunas anteriores.

Com `dense`, um item posterior pode ocupar uma lacuna anterior.

Assim:

```text
ordem no HTML

↓

1, 2, 3, 4, 5, 6
```

pode resultar em uma disposição visual diferente da sequência original.

### Regra mental

> **`dense` prioriza o preenchimento dos espaços disponíveis e pode fazer os itens parecerem fora da ordem original.**

Isso altera a **posição visual**, não a ordem dos elementos no HTML ou no DOM.

---

# 19. Quando `dense` é útil?

`dense` é especialmente interessante quando o aproveitamento visual do espaço é mais importante do que manter uma ordem visual estritamente sequencial.

Um exemplo típico:

```text
galeria de imagens
```

Imagine:

```text
┌───────┬───────┬───────┐
│ Foto  │ Foto  │ Foto  │
├───────┼───────┼───────┤
│      Foto grande       │
└────────────────────────┘
```

Uma galeria pode priorizar o aproveitamento visual da grade.

Nesse cenário:

```css
grid-auto-flow: dense;
```

pode ser útil.

---

# 20. Quando evitar `dense`?

Se a ordem visual dos elementos for importante, devemos ter cuidado.

Por exemplo:

```text
documento

texto

artigo

lista

conteúdo sequencial
```

Imagine:

```text
Item 1
Item 2
Item 3
```

Com `dense`, um item posterior pode ocupar uma lacuna anterior.

Isso pode fazer a ordem visual parecer diferente da sequência original.

### Regra prática

```text
ordem visual importa

→ cuidado com dense
```

```text
aproveitamento visual importa mais

→ dense pode ser útil
```

---

# 21. `dense` é diferente de `column`

Não confunda:

```css
grid-auto-flow: column;
```

com:

```css
grid-auto-flow: dense;
```

`column` define:

> **A direção principal do fluxo automático.**

`dense` define:

> **Uma estratégia de preenchimento que tenta ocupar lacunas anteriores.**

São conceitos diferentes.

---

# 22. Combinações possíveis

Podemos combinar:

```css
grid-auto-flow: row dense;
```

ou:

```css
grid-auto-flow: column dense;
```

A ideia é:

```text
row dense

→ fluxo por linhas + preenchimento denso
```

```text
column dense

→ fluxo por colunas + preenchimento denso
```

---

# 23. Mapa mental — `grid-auto-flow`

```text
                    grid-auto-flow

                           │

             ┌─────────────┼─────────────┐
             ↓             ↓             ↓

            row          column         dense
             │             │             │
             ↓             ↓             ↓

        novas linhas   novas colunas   preenche
                                      espaços
                                       vazios
```

### Corte mental ①

```text
row

→ "desce"

column

→ "vai para o lado"

dense

→ "procura espaços vazios"
```

---

# 24. Mapa mental — relação com outras propriedades

```text
                     GRID

                      │

             ┌────────┴────────┐
             ↓                 ↓

        AUTO FLOW          AUTO SIZE

             │                 │

             ↓                 ↓

          direção           tamanho

             │                 │

      ┌──────┴──────┐     ┌────┴──────────────┐
      ↓             ↓     ↓                   ↓

     row          column  auto-rows      auto-columns
```

A distinção:

```text
grid-auto-flow

→ COMO os itens são distribuídos

grid-auto-rows

→ QUANTO mede uma linha implícita

grid-auto-columns

→ QUANTO mede uma coluna implícita
```

---

# 25. Exemplo completo — fluxo por linhas

```css
.grid {
  display: grid;

  grid-template-columns:
    repeat(3, 1fr);

  grid-auto-flow:
    row;
}
```

Fluxo:

```text
1 → 2 → 3

4 → 5 → 6

7 → 8 → 9
```

Novas linhas podem ser criadas automaticamente conforme a quantidade de itens.

---

# 26. Exemplo completo — fluxo por colunas

```css
.grid {
  display: grid;

  grid-template-rows:
    repeat(3, 100px);

  grid-auto-flow:
    column;
}
```

Fluxo:

```text
1
↓
2
↓
3

4
↓
5
↓
6

7
↓
8
↓
9
```

Visualmente:

```text
┌───┬───┬───┐
│ 1 │ 4 │ 7 │
├───┼───┼───┤
│ 2 │ 5 │ 8 │
├───┼───┼───┤
│ 3 │ 6 │ 9 │
└───┴───┴───┘
```

---

# 27. Exemplo completo — `dense`

```css
.grid {
  display: grid;

  grid-template-columns:
    repeat(3, 1fr);

  grid-auto-flow:
    dense;
}
```

Agora o algoritmo tenta aproveitar melhor espaços vazios deixados por elementos que ocupam mais de uma célula.

---

# 28. Relação com `grid-column`

Uma situação comum para entender `dense` é quando um item ocupa várias colunas.

Por exemplo:

```css
.item-2 {
  grid-column:
    span 3;
}
```

Isso significa:

> O item 2 deve ocupar três colunas.

Se ele não couber na posição atual, o Grid poderá colocá-lo em uma posição posterior adequada.

Isso pode deixar uma lacuna.

Com:

```css
grid-auto-flow: dense;
```

outros itens posteriores podem tentar ocupar esse espaço.

---

# 29. `dense` é uma estratégia de preenchimento

Podemos pensar em:

```css
grid-auto-flow: row;
```

como:

```text
"Continue seguindo o fluxo normal."
```

Enquanto:

```css
grid-auto-flow: dense;
```

pode ser entendido como:

```text
"Continue seguindo o fluxo, mas tente preencher
lacunas anteriores quando outros itens couberem."
```

### Corte mental ②

```text
normal

→ segue adiante no fluxo

dense

→ tenta aproveitar lacunas anteriores
```

---

# 30. ⚠️ Cuidado com a ordem

Imagine:

```text
HTML

1

2

3

4

5

6
```

A ordem estrutural continua:

```text
1
2
3
4
5
6
```

Mas com:

```css
grid-auto-flow: dense;
```

um item posterior pode ocupar uma lacuna que apareceu anteriormente.

Portanto:

```text
ordem do HTML

≠

necessariamente ordem visual
```

quando o algoritmo `dense` reorganiza visualmente o preenchimento.

### Regra prática

```text
conteúdo sequencial

→ tenha cuidado com dense
```

```text
layout visual em que a ordem importa menos

→ dense pode ser útil
```

---

# 31. `grid-auto-flow` e responsividade

Podemos utilizar o fluxo automático em layouts que precisam acomodar quantidades diferentes de itens.

Por exemplo:

```css
.gallery {
  display: grid;

  grid-template-columns:
    repeat(3, 1fr);

  grid-auto-flow:
    row;
}
```

Os itens adicionais continuam preenchendo as linhas seguintes.

Também podemos combinar `grid-auto-flow` com:

```css
grid-template-columns
grid-template-rows
grid-auto-columns
grid-auto-rows
```

para criar comportamentos mais específicos.

---

# 32. Mapa mental — escolha rápida

```text
                    PRECISO CONTROLAR

                    O FLUXO DOS ITENS?

                           │

                    ┌──────┴──────┐
                    ↓             ↓

                   SIM           NÃO
                    │
                    ↓
             grid-auto-flow
                    │
        ┌───────────┼───────────┐
        ↓           ↓           ↓

       row        column      dense
        │           │           │
        ↓           ↓           ↓

      linhas      colunas     lacunas
```

---

# 33. Regra mental definitiva

```text
grid-auto-flow

      ↓

"Como os itens serão distribuídos automaticamente?"
```

### `row`

```text
→ → →

↓
→ → →
```

### `column`

```text
↓
↓
↓

→

↓
↓
↓
```

### `dense`

```text
"Existe uma lacuna anterior que outro item consegue ocupar?"

          ↓

      tente preencher
```

---

# 34. Resumo das propriedades relacionadas

| Propriedade             | Função                                                      |
| ----------------------- | ----------------------------------------------------------- |
| `grid-auto-flow`        | Define a direção e a estratégia do preenchimento automático |
| `grid-auto-rows`        | Define o tamanho das linhas implícitas                      |
| `grid-auto-columns`     | Define o tamanho das colunas implícitas                     |
| `grid-template-rows`    | Define as linhas explícitas                                 |
| `grid-template-columns` | Define as colunas explícitas                                |

Podemos memorizar:

```text
TEMPLATE

→ estrutura que você define

AUTO

→ estrutura criada automaticamente

FLOW

→ direção/estratégia usada no preenchimento
```

---

# 35. 📌 Resumo final

```text
                    GRID AUTO FLOW

                           │

            ┌──────────────┼──────────────┐
            ↓              ↓              ↓

           row           column          dense
            │              │              │
            ↓              ↓              ↓

      preenche por    preenche por    tenta aproveitar
         linhas          colunas      espaços vazios
```

### `row`

```css
grid-auto-flow: row;
```

> Preenche os itens seguindo as linhas e cria novas linhas quando necessário.

### `column`

```css
grid-auto-flow: column;
```

> Preenche os itens seguindo as colunas e cria novas colunas quando necessário.

### `dense`

```css
grid-auto-flow: dense;
```

> Tenta preencher lacunas anteriores da grade com itens posteriores que possam caber.

### Combinações

```css
grid-auto-flow: row dense;
```

ou:

```css
grid-auto-flow: column dense;
```

> Combina uma direção de fluxo com o algoritmo de preenchimento denso.

---

# 36. 🧠 Três frases para guardar

> **`grid-auto-flow: row` → os itens fluem preenchendo as linhas.**

> **`grid-auto-flow: column` → os itens fluem preenchendo as colunas.**

> **`dense` → o Grid tenta aproveitar lacunas anteriores, podendo alterar a ordem visual dos itens.**

A relação geral fica:

```text
grid-auto-flow

→ COMO os itens serão distribuídos?

grid-auto-rows

→ QUAL TAMANHO terão as linhas implícitas?

grid-auto-columns

→ QUAL TAMANHO terão as colunas implícitas?
```

### Corte mental final

```text
FLOW

→ direção/estratégia do preenchimento

AUTO-ROWS

→ tamanho das linhas implícitas

AUTO-COLUMNS

→ tamanho das colunas implícitas
```

---

# Referências

* [MDN — `grid-auto-flow`](https://developer.mozilla.org/pt-BR/docs/Web/CSS/Reference/Properties/grid-auto-flow)
* [MDN — Auto-placement in grid layout](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout/Auto-placement)
* [MDN — Basic concepts of grid layout](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout/Basic_concepts)
* [MDN — `grid-auto-rows`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-auto-rows)
* [MDN — `grid-auto-columns`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-auto-columns)
* [W3C — CSS Grid Layout Module Level 2](https://www.w3.org/TR/css-grid-2/)
