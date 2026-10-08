# `align-content` no CSS Grid

## Índice

1. [Conceito principal](#1-conceito-principal)
2. [Quando `align-content` produz efeito?](#2-quando-align-content-produz-efeito)
3. [Sintaxe](#3-sintaxe)
4. [`align-content: start`](#4-align-content-start)
5. [`align-content: end`](#5-align-content-end)
6. [`align-content: center`](#6-align-content-center)
7. [`align-content: stretch`](#7-align-content-stretch)
8. [`align-content: space-around`](#8-align-content-space-around)
9. [`align-content: space-between`](#9-align-content-space-between)
10. [`align-content: space-evenly`](#10-align-content-space-evenly)
11. [Comparação dos valores](#11-comparação-dos-valores)
12. [`space-around` × `space-between` × `space-evenly`](#12-space-around--space-between--space-evenly)
13. [Relação entre espaço disponível e `align-content`](#13-relação-entre-espaço-disponível-e-align-content)
14. [Relação com o tamanho das tracks](#14-relação-com-o-tamanho-das-tracks)
15. [Tracks e `gap`](#15-tracks-e-gap)
16. [Exemplo completo](#16-exemplo-completo)
17. [`justify-content` × `align-content`](#17-justify-content--align-content)
18. [Erros e confusões comuns](#18-erros-e-confusões-comuns)
19. [Fluxo mental](#19-fluxo-mental)
20. [Mapa mental](#20-mapa-mental)
21. [Resumo final](#21-resumo-final)
22. [Regra mental definitiva](#22-regra-mental-definitiva)

---

## 1. Conceito principal

`align-content` é uma propriedade do **Grid Container** usada para distribuir e alinhar o conjunto de **tracks (linhas e colunas do Grid)** dentro do espaço disponível no **eixo de bloco**, que é o eixo vertical em idiomas escritos na horizontal.

Neste contexto:

- `justify-content` → distribui o conteúdo no eixo horizontal.
- `align-content` → distribui o conteúdo no eixo vertical.

A ideia central é semelhante entre as duas propriedades: ambas controlam **como o conjunto de tracks é distribuído quando existe espaço livre no container**.

### 🧠 Corte mental

```text
justify-content
→ distribuição no eixo horizontal.

align-content
→ distribuição no eixo vertical.
```

> **Importante:** `align-content` não serve para alinhar individualmente cada Grid Item. Ele controla a distribuição das tracks do Grid dentro do espaço disponível do container.

---

## 2. Quando `align-content` produz efeito?

Para perceber o funcionamento de `align-content`, o Grid precisa possuir **espaço livre** para distribuir.

Considere um Grid com:

- 3 colunas de `1fr`;
- 3 linhas de `50px`;
- um `height` maior que o espaço ocupado pelas linhas.

```css
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-rows: 50px 50px 50px;
  height: 300px;
}
```

As três linhas ocupam:

```text
50px + 50px + 50px = 150px
```

Mas o Grid possui:

```text
300px de altura
```

Portanto, existe espaço vertical disponível para ser distribuído. A ilustração abaixo mostra esse espaço com `align-content: center`:

```text
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
```

Quando o `height` do Grid é exatamente igual ao tamanho necessário para as tracks (ou quando não há `height` definido e o container se ajusta ao conteúdo), não existe espaço para distribuir. Nesse caso, as diferentes opções de `align-content` não produzem efeito visual.

---

## 3. Sintaxe

```css
.grid {
  align-content: valor;
}
```

A propriedade recebe valores que determinam **como o espaço disponível será distribuído entre as tracks do Grid**.

Valores demonstrados:

```text
start
end
center
stretch
space-around
space-between
space-evenly
```

O valor padrão é `normal`, que no Grid se comporta como `stretch`.

---

## 4. `align-content: start`

```css
.grid {
  align-content: start;
}
```

As tracks são posicionadas no **início do eixo vertical**.

```text
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
```

A estrutura do Grid permanece agrupada no início, deixando o espaço livre no final.

---

## 5. `align-content: end`

```css
.grid {
  align-content: end;
}
```

As tracks são posicionadas no **final do eixo vertical**.

```text
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
```

Todo o conjunto de tracks é deslocado para o final do espaço disponível.

---

## 6. `align-content: center`

```css
.grid {
  align-content: center;
}
```

As tracks são agrupadas no **centro do eixo vertical**.

```text
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
```

O espaço livre é dividido igualmente entre o início e o final do Grid.

---

## 7. `align-content: stretch`

```css
.grid {
  align-content: stretch;
}
```

`stretch` expande as tracks de tamanho `auto` para que ocupem o espaço livre do container. **Tracks com tamanho fixo não são alteradas.**

```text
espaço livre no container
          ↓
existem tracks `auto`?
          ↓
sim → o espaço livre é dividido entre elas
não → nada cresce
```

Exemplo em que o `stretch` tem efeito:

```css
.grid {
  display: grid;
  grid-template-rows: auto auto auto;
  height: 300px;
  align-content: stretch;
}
```

As três linhas `auto` crescem e dividem os `300px`:

```text
┌──────────────────────┐
│     Linha expandida  │
├──────────────────────┤
│     Linha expandida  │
├──────────────────────┤
│     Linha expandida  │
└──────────────────────┘
```

Já com `grid-template-rows: 50px 50px 50px`, as linhas continuam com `50px` e o espaço restante permanece livre.

> **⚠️ Atenção**
>
> `stretch` só atua sobre tracks `auto`. Com linhas de tamanho fixo (como `50px`), ele não produz efeito, e a grade se comporta como `start`. O mesmo vale para `justify-content: stretch` nas colunas.

---

## 8. `align-content: space-around`

```css
.grid {
  align-content: space-around;
}
```

O espaço livre é distribuído **ao redor de cada track**: cada track recebe metade do espaço de cada lado.

Isso gera espaço entre as tracks e também nas extremidades, mas o espaço nas extremidades é **menor** que o espaço entre as tracks:

```text
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
```

Conceitualmente:

```text
extremidade  = 1 parte
entre tracks = 2 partes
extremidade  = 1 parte
```

---

## 9. `align-content: space-between`

```css
.grid {
  align-content: space-between;
}
```

O espaço livre é distribuído **somente entre as tracks**. Não existe espaço adicional nas extremidades.

```text
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
```

A primeira track encosta no início e a última encosta no final.

### 🧠 Corte mental

```text
space-between
→ espaço somente ENTRE as tracks.
```

---

## 10. `align-content: space-evenly`

```css
.grid {
  align-content: space-evenly;
}
```

O espaço disponível é dividido em **partes iguais em todos os intervalos**:

- espaço antes da primeira track;
- espaço entre a primeira e a segunda;
- espaço entre a segunda e a terceira;
- espaço depois da terceira.

```text
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
```

### 🧠 Corte mental

```text
space-evenly
→ todos os espaços são iguais.
```

---

## 11. Comparação dos valores

| Valor | Comportamento |
| --- | --- |
| `start` | Coloca as tracks no início |
| `end` | Coloca as tracks no final |
| `center` | Centraliza as tracks |
| `stretch` | Expande as tracks `auto` para ocupar o espaço disponível |
| `space-around` | Distribui espaço ao redor de cada track (as pontas recebem metade) |
| `space-between` | Distribui espaço somente entre as tracks |
| `space-evenly` | Distribui espaços iguais em todos os intervalos |

---

## 12. `space-around` × `space-between` × `space-evenly`

Esses três valores são facilmente confundidos.

### `space-around`

```text
│ espaço menor │
│    Track     │
│    espaço    │
│    Track     │
│    espaço    │
│    Track     │
│ espaço menor │
```

O espaço entre as tracks é maior que o espaço nas extremidades.

---

### `space-between`

```text
│ Track │
│ espaço│
│ Track │
│ espaço│
│ Track │
```

O espaço existe somente entre as tracks.

---

### `space-evenly`

```text
│ espaço │
│ Track  │
│ espaço │
│ Track  │
│ espaço │
│ Track  │
│ espaço │
```

Todos os espaços são iguais.

### 🧠 Corte mental

```text
space-around
→ espaço ao redor.

space-between
→ espaço entre.

space-evenly
→ espaços iguais.
```

---

## 13. Relação entre espaço disponível e `align-content`

O comportamento pode ser entendido através deste fluxo:

```text
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
nenhum     align-content
efeito          ↓
          distribui o espaço
```

O ponto fundamental é:

> **`align-content` precisa de espaço disponível para que sua distribuição possa ser percebida.**

---

## 14. Relação com o tamanho das tracks

Suponha:

```css
grid-template-rows: 50px 50px 50px;
```

Temos três tracks de `50px`:

```text
Linha 1 → 50px
Linha 2 → 50px
Linha 3 → 50px
```

Total:

```text
150px
```

Agora suponha:

```css
height: 300px;
```

Existe:

```text
300px - 150px = 150px
```

de espaço adicional.

É esse espaço que permite observar os diferentes comportamentos de `align-content`.

---

## 15. Tracks e `gap`

O `gap` define um espaço fixo entre as tracks. Esse espaço faz parte da estrutura do Grid e é descontado do espaço livre antes de `align-content` distribuí-lo:

```text
espaço livre = altura do container − altura das tracks − gaps
```

Exemplo:

```css
.grid {
  display: grid;
  grid-template-rows: 50px 50px 50px;
  gap: 10px;
  height: 300px;
}
```

```text
tracks: 50px + 50px + 50px = 150px
gaps:   10px + 10px        =  20px
espaço livre: 300px − 150px − 20px = 130px
```

Com `space-between`, `space-around` ou `space-evenly`, os `130px` são **somados** ao `gap`, e o `gap` funciona como o espaço mínimo entre as tracks.

```text
Linha 1
─────────────
   gap + espaço distribuído
─────────────
Linha 2
─────────────
   gap + espaço distribuído
─────────────
Linha 3
```

> **Importante:** o `gap` não desaparece quando `align-content` distribui o espaço. Ele é a base sobre a qual o espaço extra é acrescentado.

---

## 16. Exemplo completo

```css
.grid {
  display: grid;

  grid-template-columns: 1fr 1fr 1fr;
  grid-template-rows: 50px 50px 50px;

  height: 300px;

  align-content: center;
}
```

Neste exemplo:

```text
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
```

---

## 17. `justify-content` × `align-content`

| Propriedade | Eixo trabalhado neste contexto | Função |
| --- | --- | --- |
| `justify-content` | Horizontal | Distribui o conjunto de tracks horizontalmente |
| `align-content` | Vertical | Distribui o conjunto de tracks verticalmente |

### 🧠 Corte mental

```text
JUSTIFY
→ horizontal.

ALIGN
→ vertical.
```

A lógica das propriedades é semelhante; o que muda é o eixo em que a distribuição acontece.

---

## 18. Erros e confusões comuns

### 18.1 Confundir `align-content` com alinhamento individual

`align-content` não controla diretamente a posição de cada Grid Item.

Ele trabalha com a **distribuição das tracks do Grid** dentro do espaço disponível.

---

### 18.2 Usar `align-content` sem espaço disponível

Se o Grid já possuir exatamente o tamanho necessário para suas tracks:

```text
conteúdo = tamanho do container
```

não haverá espaço livre para distribuir. Por isso, alterar:

```css
align-content: start;
align-content: center;
align-content: end;
```

não produz nenhuma diferença visual.

---

### 18.3 Esperar que `stretch` expanda linhas fixas

`stretch` só atua sobre tracks `auto`. Com `grid-template-rows: 50px 50px 50px`, as linhas continuam com `50px`, mesmo havendo espaço livre.

---

### 18.4 Confundir `align-content` com `align-items`

Apesar dos nomes semelhantes, elas possuem responsabilidades diferentes.

```text
align-content
→ distribuição do conjunto de tracks.

align-items
→ alinhamento dos Grid Items dentro de suas áreas.
```

Essa distinção é fundamental para não escolher a propriedade errada.

---

## 19. Fluxo mental

```text
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
```

---

## 20. Mapa mental

```text
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
```

---

## 21. Resumo final

`align-content` controla a **distribuição das tracks do Grid no eixo vertical** quando existe espaço disponível no Grid Container.

```text
`align-content`
→ distribui o conjunto de tracks.

`start`
→ início.

`end`
→ final.

`center`
→ centro.

`stretch`
→ expansão das tracks `auto`.

`space-around`
→ espaço ao redor.

`space-between`
→ espaço somente entre.

`space-evenly`
→ espaços iguais.
```

A propriedade só se torna visualmente relevante quando existe **espaço livre dentro do container**.

---

## 22. Regra mental definitiva

```text
`justify-content`
→ "Como distribuo a estrutura horizontalmente?"

`align-content`
→ "Como distribuo a estrutura verticalmente?"

sem espaço livre
→ nenhuma diferença visível.

com espaço livre
→ `align-content` distribui esse espaço entre as tracks.
```