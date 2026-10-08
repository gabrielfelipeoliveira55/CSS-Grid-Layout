# `justify-items` no CSS Grid

## 1. Conceito principal

`justify-items` controla o **alinhamento dos Grid Items dentro da área de cada coluna**, no **eixo horizontal (X)**.

A principal diferença está naquilo que cada propriedade está controlando:

    justify-content
    → trabalha com a estrutura inteira do Grid.

    justify-items
    → trabalha com os itens dentro das áreas que eles ocupam.

Em outras palavras:

    Content
    → considera a área/estrutura do Grid.

    Items
    → considera o item dentro dessa área.

### 🧠 Corte mental

    `justify-content`
    → "Como distribuo a estrutura?"

    `justify-items`
    → "Como alinho o item dentro da estrutura que ele ocupa?"

---

# 2. Eixo trabalhado

Como a propriedade é `justify-*`, o alinhamento ocorre no **eixo horizontal**, ou seja, no eixo **X**.

    ←──────────── eixo X ────────────→
    
    [      item      ]

O item pode ser alinhado:

    start
    center
    end

ou pode ocupar todo o espaço disponível com:

    stretch

---

# 3. Estrutura utilizada no exemplo

Considere um Grid com:

    3 colunas de 1fr
    3 linhas de 50px

Exemplo:

    .grid {
      display: grid;
      grid-template-columns: 1fr 1fr 1fr;
      grid-template-rows: 50px 50px 50px;
    }

Visualmente:

    ┌──────────┬──────────┬──────────┐
    │ Coluna 1 │ Coluna 2 │ Coluna 3 │
    ├──────────┼──────────┼──────────┤
    │          │          │          │
    ├──────────┼──────────┼──────────┤
    │          │          │          │
    └──────────┴──────────┴──────────┘

As colunas continuam existindo independentemente da forma como os itens são alinhados.

---

# 4. `justify-items: start`

    .grid {
      justify-items: start;
    }

O item é alinhado ao **início da sua coluna**.

Exemplo conceitual:

    ┌────────────┬────────────┬────────────┐
    │ Item 1     │ Item 2     │ Item 3     │
    │            │            │            │
    └────────────┴────────────┴────────────┘
      ↑             ↑             ↑
      início de cada coluna

O ponto importante é que o item não ocupa necessariamente toda a largura da coluna.

Ele fica alinhado no início e seu tamanho passa a acompanhar o espaço necessário para seu conteúdo.

Por exemplo, se:

    Item 1 → conteúdo maior
    Item 2 → conteúdo menor
    Item 3 → conteúdo menor

os itens podem apresentar larguras diferentes.

---

# 5. O que acontece com a coluna?

Ao usar:

    justify-items: start;

a coluna **não desaparece** e nem muda de existência.

A estrutura continua sendo:

    Coluna 1
    Coluna 2
    Coluna 3

O que mudou foi o posicionamento do item **dentro da coluna**.

Visualmente:

    ┌─────────────────── Coluna 1 ───────────────────┐
    │ Item 1                                         │
    │                                                │
    └────────────────────────────────────────────────┘

A coluna continua ocupando sua área completa.

O item é que passa a ocupar apenas a parte necessária e fica alinhado no início.

### 🧠 Corte mental

    A coluna continua inteira.
    O item é que muda de posição e dimensão dentro dela.

---

# 6. `justify-items: end`

    .grid {
      justify-items: end;
    }

O item é alinhado ao **final da sua coluna**, no eixo horizontal.

    ┌─────────────────────────────┐
    │                 Item        │
    └─────────────────────────────┘
                              ↑
                              final

A coluna permanece ocupando toda a sua área.

O item é deslocado para o final dessa área.

---

# 7. `justify-items: center`

    .grid {
      justify-items: center;
    }

O item é alinhado no **centro horizontal da coluna**.

    ┌─────────────────────────────┐
    │          Item               │
    └─────────────────────────────┘
                    ↑
                  centro

A posição central é calculada considerando **a coluna que o item ocupa**.

Isso se torna especialmente importante quando um item ocupa mais de uma coluna.

---

# 8. Item ocupando duas colunas

Considere o Item 5 com:

    grid-column: span 2;

Isso significa que o item ocupa duas colunas.

Visualmente:

    ┌────────────┬────────────┬────────────┐
    │            │            │            │
    │   Item 5   │   Item 5   │            │
    │            │            │            │
    └────────────┴────────────┴────────────┘
       coluna 1     coluna 2     coluna 3

Quando utilizamos:

    justify-items: center;

o Item 5 será centralizado considerando **as duas colunas que ele ocupa**, e não apenas uma delas.

Portanto:

    Item ocupa 1 coluna
          ↓
    centralização dentro de 1 coluna

    Item ocupa 2 colunas
          ↓
    centralização dentro das 2 colunas

Isso explica por que um item que ocupa duas colunas pode apresentar um alinhamento diferente dos itens que ocupam apenas uma.

### 🧠 Corte mental

    `justify-items`
    → sempre considera a área que o item ocupa.

    `span 2`
    → o item ocupa duas colunas.

    `center`
    → o centro é calculado considerando essas duas colunas.

---

# 9. `justify-items: stretch`

`stretch` é o comportamento apresentado como **padrão**.

    .grid {
      justify-items: stretch;
    }

Nesse comportamento, os itens são esticados para ocupar a largura disponível da área correspondente.

Visualmente:

    ┌────────────┬────────────┬────────────┐
    │   Item 1   │   Item 2   │   Item 3   │
    ├────────────┼────────────┼────────────┤
    │            │            │            │
    └────────────┴────────────┴────────────┘

Os itens ocupam a largura disponível dentro de suas respectivas áreas.

---

# 10. `start` × `end` × `center` × `stretch`

| Valor | Comportamento |
|---|---|
| `start` | Alinha o item no início da área |
| `end` | Alinha o item no final da área |
| `center` | Centraliza o item na área |
| `stretch` | Estica o item para ocupar a área disponível |

---

# 11. `justify-items` × `justify-content`

Essa é uma das diferenças mais importantes para memorizar.

| Propriedade | O que controla |
|---|---|
| `justify-content` | Alinhamento/distribuição da estrutura do Grid |
| `justify-items` | Alinhamento dos itens dentro das áreas que ocupam |

Visualmente:

    ┌─────────────────────────────────────┐
    │                                     │
    │   ┌────────┐ ┌────────┐ ┌────────┐ │
    │   │ Item 1 │ │ Item 2 │ │ Item 3 │ │
    │   └────────┘ └────────┘ └────────┘ │
    │                                     │
    └─────────────────────────────────────┘

`justify-content` trabalha com a **estrutura como conjunto**.

`justify-items` trabalha com **cada item dentro da área correspondente**.

### 🧠 Corte mental definitivo

    `justify-content`
    → conteúdo/estrutura.

    `justify-items`
    → item dentro da estrutura.

---

# 12. Relação entre coluna e item

O raciocínio pode ser representado assim:

    Grid
      ↓
    colunas
      ↓
    área ocupada pelo item
      ↓
    `justify-items`
      ↓
    posição do item dentro dessa área

Exemplo:

    Coluna
    ┌────────────────────────────┐
    │                            │
    │       ┌──────────┐         │
    │       │   Item   │         │
    │       └──────────┘         │
    │                            │
    └────────────────────────────┘

A coluna continua existindo.

O `justify-items` determina onde o item ficará dentro dela.

---

# 13. Fluxo de raciocínio

    `display: grid`
          ↓
    Grid Container
          ↓
    criação das colunas
          ↓
    cada item ocupa uma área
          ↓
    `justify-items`
          ↓
    alinhamento horizontal do item
          ↓
    start / end / center / stretch

Quando o item ocupa várias colunas:

    `grid-column: span 2`
          ↓
    item ocupa duas colunas
          ↓
    `justify-items`
          ↓
    alinhamento considera as duas colunas

---

# 14. ⚠️ Erros e confusões comuns

### 14.1 Confundir o item com a coluna

Quando o item não ocupa toda a coluna:

    coluna
    ┌─────────────────────────────┐
    │ Item                        │
    │                             │
    │                             │
    └─────────────────────────────┘

não significa que a coluna ficou menor.

A coluna continua tendo sua largura definida pelo Grid.

É o **item** que deixou de ocupar toda a área.

---

### 14.2 Esquecer o `span`

Um item que ocupa:

    `grid-column: span 1`

é considerado dentro de uma coluna.

Um item que ocupa:

    `grid-column: span 2`

passa a considerar uma área formada por duas colunas.

Por isso, seu alinhamento com:

    justify-items: center;

pode parecer diferente dos demais itens.

---

### 14.3 Confundir `stretch` com os outros valores

No `stretch`, o item ocupa a área disponível.

Nos outros valores:

    start
    end
    center

o item pode ficar menor que a área da coluna e apenas mudar de posição dentro dela.

---

# 15. 🧠 Mapa mental

    `justify-items`
             │
             ↓
    alinhamento horizontal
             │
             ├───────────────┬───────────────┐
             ↓               ↓               ↓
          `start`          `center`         `end`
             │               │               │
          início           centro           final
             │
             └───────────────┐
                             ↓
                         `stretch`
                             │
                             ↓
                    ocupa a área disponível

             +
             │
             ↓
       área ocupada pelo item
             │
             ├── 1 coluna
             │
             └── 2 ou mais colunas
                    (`span`)