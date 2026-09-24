# `align-content` no CSS Grid

## 1. Conceito principal

`align-content` é uma propriedade do **Grid Container** usada para distribuir e alinhar o conjunto de **tracks (linhas e colunas do Grid)** dentro do espaço disponível no **eixo transversal**.

Neste contexto:

- `justify-content` → distribui o conteúdo no eixo horizontal.
- `align-content` → distribui o conteúdo no eixo vertical.

A ideia central é semelhante entre as duas propriedades: ambas controlam **como o conjunto de tracks é distribuído quando existe espaço livre no container**.

### 🧠 Corte mental

    justify-content
    → distribuição no eixo horizontal.

    align-content
    → distribuição no eixo vertical.

> **Importante:** `align-content` não serve para alinhar individualmente cada Grid Item. Ele controla a distribuição das tracks do Grid dentro do espaço disponível do container.

---

# 2. Quando `align-content` produz efeito?

Para perceber o funcionamento de `align-content`, o Grid precisa possuir **espaço livre** para distribuir.

Considere um Grid com:

- 3 colunas de `1fr`;
- 3 linhas de `50px`;
- um `height` maior que o espaço ocupado pelas linhas.

Exemplo:

    .grid {
      display: grid;
      grid-template-columns: 1fr 1fr 1fr;
      grid-template-rows: 50px 50px 50px;
      height: 300px;
    }

As três linhas ocupam:

    50px + 50px + 50px = 150px

Mas o Grid possui:

    300px de altura

Portanto, existe espaço vertical disponível para ser distribuído.

Visualmente:

    ┌─────────────────────────────┐
    │                             │
    │          espaço livre       │
    │                             │
    ├─────────────────────────────┤
    │        Linha 1 — 50px       │
    ├─────────────────────────────┤
    │        Linha 2 — 50px       │
    ├─────────────────────────────┤
    │        Linha 3 — 50px       │
    ├─────────────────────────────┤
    │                             │
    │          espaço livre       │
    │                             │
    └─────────────────────────────┘

Sem espaço sobrando, não existe espaço para distribuir.

Por isso, quando o `height` do Grid é exatamente igual ao tamanho necessário para o conteúdo, as diferentes opções de `align-content` podem não produzir um efeito visual perceptível.

---

# 3. Sintaxe

    .grid {
      align-content: valor;
    }

A propriedade recebe valores que determinam **como o espaço disponível será distribuído entre as tracks do Grid**.

Entre os valores apresentados estão:

    start
    end
    center
    stretch
    space-around
    space-between
    space-evenly

---

# 4. `align-content: start`

    .grid {
      align-content: start;
    }

As tracks são posicionadas no **início do eixo vertical**.

Visualmente:

    ┌──────────────────────┐
    │ Linha 1              │
    ├──────────────────────┤
    │ Linha 2              │
    ├──────────────────────┤
    │ Linha 3              │
    ├──────────────────────┤
    │                      │
    │   espaço restante    │
    │                      │
    └──────────────────────┘

A estrutura do Grid permanece agrupada no início, deixando o espaço livre no final.

---

# 5. `align-content: end`

    .grid {
      align-content: end;
    }

As tracks são posicionadas no **final do eixo vertical**.

    ┌──────────────────────┐
    │                      │
    │   espaço restante    │
    │                      │
    ├──────────────────────┤
    │ Linha 1              │
    ├──────────────────────┤
    │ Linha 2              │
    ├──────────────────────┤
    │ Linha 3              │
    └──────────────────────┘

Todo o conjunto de tracks é deslocado para o final do espaço disponível.

---

# 6. `align-content: center`

    .grid {
      align-content: center;
    }

As tracks são agrupadas no **centro do eixo vertical**.

    ┌──────────────────────┐
    │                      │
    │   espaço restante    │
    ├──────────────────────┤
    │ Linha 1              │
    ├──────────────────────┤
    │ Linha 2              │
    ├──────────────────────┤
    │ Linha 3              │
    ├──────────────────────┤
    │   espaço restante    │
    │                      │
    └──────────────────────┘

O espaço livre é dividido entre o início e o final do Grid.

---

# 7. `align-content: stretch`

    .grid {
      align-content: stretch;
    }

`stretch` faz com que as tracks sejam **expandidas para ocupar o espaço disponível**.

Em vez de simplesmente deixar o espaço livre separado das tracks, o espaço disponível é utilizado para aumentar o tamanho das tracks.

Conceitualmente:

    espaço disponível
           ↓
    ┌──────────────────────┐
    │     Linha expandida  │
    ├──────────────────────┤
    │     Linha expandida  │
    ├──────────────────────┤
    │     Linha expandida  │
    └──────────────────────┘

> **Importante:** quando o `stretch` atua, o tamanho das tracks pode aumentar para absorver o espaço disponível.

---

# 8. `align-content: space-around`

    .grid {
      align-content: space-around;
    }

O espaço disponível é distribuído **ao redor das tracks**.

Isso gera uma quantidade de espaço entre as tracks e também nas extremidades.

Uma forma de visualizar a distribuição é:

    espaço menor
          ↓
    ┌───────────────┐
    │   Linha 1     │
    ├───────────────┤
    │               │
    │   espaço      │
    │               │
    ├───────────────┤
    │   Linha 2     │
    ├───────────────┤
    │               │
    │   espaço      │
    │               │
    ├───────────────┤
    │   Linha 3     │
    └───────────────┘
          ↑
    espaço menor

A característica importante é que o espaço nas extremidades é menor que o espaço entre as tracks.

Conceitualmente:

    extremidade = 1 parte
    entre tracks = 2 partes
    extremidade = 1 parte

---

# 9. `align-content: space-between`

    .grid {
      align-content: space-between;
    }

O espaço livre é distribuído **entre as tracks**.

Não existe espaço adicional nas extremidades.

    ┌──────────────────────┐
    │ Linha 1              │
    ├──────────────────────┤
    │                      │
    │      espaço          │
    │                      │
    ├──────────────────────┤
    │ Linha 2              │
    ├──────────────────────┤
    │                      │
    │      espaço          │
    │                      │
    ├──────────────────────┤
    │ Linha 3              │
    └──────────────────────┘

O primeiro elemento encosta no início e o último encosta no final.

### 🧠 Corte mental

    space-between
    → espaço somente ENTRE as tracks.

---

# 10. `align-content: space-evenly`

    .grid {
      align-content: space-evenly;
    }

O espaço disponível é dividido em **partes iguais em todos os intervalos**.

Isso significa que:

- espaço antes da primeira track;
- espaço entre a primeira e a segunda;
- espaço entre a segunda e a terceira;
- espaço depois da terceira;

possuem a mesma dimensão.

    ┌──────────────────────┐
    │      espaço          │
    ├──────────────────────┤
    │ Linha 1              │
    ├──────────────────────┤
    │      espaço          │
    ├──────────────────────┤
    │ Linha 2              │
    ├──────────────────────┤
    │      espaço          │
    ├──────────────────────┤
    │ Linha 3              │
    ├──────────────────────┤
    │      espaço          │
    └──────────────────────┘

### 🧠 Corte mental

    space-evenly
    → todos os espaços são iguais.

---

# 11. Comparação dos valores

| Valor | Comportamento |
| --- | --- |
| `start` | Coloca as tracks no início |
| `end` | Coloca as tracks no final |
| `center` | Centraliza as tracks |
| `stretch` | Expande as tracks para ocupar o espaço disponível |
| `space-around` | Distribui espaço ao redor das tracks |
| `space-between` | Distribui espaço somente entre as tracks |
| `space-evenly` | Distribui espaços iguais em todos os intervalos |

---

# 12. `space-around` × `space-between` × `space-evenly`

Esses três valores são facilmente confundidos.

## `space-around`

    │ espaço menor │
    │    Track     │
    │    espaço    │
    │    Track     │
    │    espaço    │
    │    Track     │
    │ espaço menor │

O espaço entre as tracks é maior que o espaço nas extremidades.

---

## `space-between`

    │ Track │
    │ espaço│
    │ Track │
    │ espaço│
    │ Track │

O espaço existe somente entre as tracks.

---

## `space-evenly`

    │ espaço │
    │ Track  │
    │ espaço │
    │ Track  │
    │ espaço │
    │ Track  │
    │ espaço │

Todos os espaços são iguais.

### 🧠 Corte mental

    space-around
    → espaço ao redor.

    space-between
    → espaço entre.

    space-evenly
    → espaços iguais.

---

# 13. Relação entre espaço disponível e `align-content`

O comportamento pode ser entendido através deste fluxo:

    Grid Container
          ↓
    possui uma determinada altura
          ↓
    Grid possui tracks
          ↓
    tracks ocupam uma parte dessa altura
          ↓
    sobra espaço?
       ↙       ↘
     não        sim
      ↓          ↓
    pouco      align-content
    efeito          ↓
              distribui o espaço

O ponto fundamental é:

> **`align-content` precisa de espaço disponível para que sua distribuição possa ser percebida.**

---

# 14. Relação com o tamanho das tracks

Suponha:

    grid-template-rows: 50px 50px 50px;

Temos três tracks de `50px`:

    Linha 1 → 50px
    Linha 2 → 50px
    Linha 3 → 50px

Total:

    150px

Agora suponha:

    height: 300px;

Existe:

    300px - 150px = 150px

de espaço adicional.

É esse espaço que permite observar os diferentes comportamentos de `align-content`.

---

# 15. Tracks e `gap`

`align-content` atua sobre a distribuição da estrutura do Grid, e essa estrutura pode envolver também espaços definidos por `gap`.

Quando uma track ocupa uma área maior por conta da distribuição do Grid, o espaço do `gap` continua fazendo parte da estrutura ocupada entre as linhas.

Visualmente:

    Linha 1
    ─────────────

         gap
    ─────────────

    Linha 2
    ─────────────

         gap
    ─────────────

    Linha 3
    ─────────────

Por isso, ao observar uma configuração em que um item ocupa múltiplas linhas, não se deve imaginar que o `gap` simplesmente desaparece.

---

# 16. Exemplo completo

    .grid {
      display: grid;

      grid-template-columns: 1fr 1fr 1fr;
      grid-template-rows: 50px 50px 50px;

      height: 300px;

      align-content: center;
    }

Neste exemplo:

    display: grid
    → transforma o elemento em Grid Container.

    grid-template-columns
    → cria três colunas proporcionais.

    grid-template-rows
    → cria três linhas de 50px.

    height
    → fornece espaço vertical adicional ao Grid.

    align-content: center
    → centraliza o conjunto de tracks verticalmente nesse espaço.

---

# 17. `justify-content` × `align-content`

| Propriedade | Eixo trabalhado neste contexto | Função |
| --- | --- | --- |
| `justify-content` | Horizontal | Distribui o conjunto de tracks horizontalmente |
| `align-content` | Vertical | Distribui o conjunto de tracks verticalmente |

### 🧠 Corte mental

    JUSTIFY
    → horizontal.

    ALIGN
    → vertical.

A lógica das propriedades é semelhante; o que muda é o eixo em que a distribuição acontece.

---

# 18. ⚠️ Erros e confusões comuns

### 18.1 Confundir `align-content` com alinhamento individual

`align-content` não controla diretamente a posição de cada Grid Item.

Ele trabalha com a **distribuição do conteúdo estrutural do Grid** dentro do espaço disponível.

---

### 18.2 Usar `align-content` sem espaço disponível

Se o Grid já possuir exatamente o tamanho necessário para suas tracks:

    conteúdo = tamanho do container

não haverá espaço livre relevante para distribuir.

Por isso, alterar:

    align-content: start;
    align-content: center;
    align-content: end;

pode parecer não produzir nenhuma diferença.

---

### 18.3 Confundir `align-content` com `align-items`

Apesar dos nomes semelhantes, elas possuem responsabilidades diferentes.

    align-content
    → distribuição do conjunto de tracks.

    align-items
    → alinhamento dos Grid Items dentro de suas áreas.

Essa distinção é fundamental para não escolher a propriedade errada.

---

# 19. Fluxo mental

    `display: grid`
          ↓
    Grid Container
          ↓
    criação das tracks
          ↓
    `grid-template-rows`
          ↓
    tamanho das linhas
          ↓
    container possui espaço adicional
          ↓
    `align-content`
          ↓
    distribuição das tracks no eixo vertical
          ↓
    start / end / center / stretch
    space-around / space-between / space-evenly

---

# 20. Mapa mental

    CSS GRID
        │
        ├── Eixo horizontal
        │      │
        │      └── `justify-content`
        │
        └── Eixo vertical
               │
               └── `align-content`
                      │
                      ├── `start`
                      │
                      ├── `end`
                      │
                      ├── `center`
                      │
                      ├── `stretch`
                      │
                      ├── `space-around`
                      │
                      ├── `space-between`
                      │
                      └── `space-evenly`

---

# 21. 📌 Resumo final

`align-content` controla a **distribuição das tracks do Grid no eixo vertical** quando existe espaço disponível no Grid Container.

    `align-content`
    → distribui o conjunto de tracks.

    `start`
    → início.

    `end`
    → final.

    `center`
    → centro.

    `stretch`
    → expansão das tracks.

    `space-around`
    → espaço ao redor.

    `space-between`
    → espaço somente entre.

    `space-evenly`
    → espaços iguais.

A propriedade só se torna visualmente relevante quando existe **espaço livre dentro do container**.

---

# 🧠 Regra mental definitiva

    `justify-content`
    → "Como distribuo a estrutura horizontalmente?"

    `align-content`
    → "Como distribuo a estrutura verticalmente?"

    E lembre:

    sem espaço livre
    → pouca ou nenhuma diferença visível.

    com espaço livre
    → `align-content` pode distribuir esse espaço entre as tracks.
