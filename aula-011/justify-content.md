# CSS Grid Layout — `justify-content`

## Índice

1. [O que é `justify-content`?](#1-o-que-é-justify-content)
2. [Onde `justify-content` atua?](#2-onde-justify-content-atua)
3. [Estrutura usada no exemplo](#3-estrutura-usada-no-exemplo)
4. [Sintaxe](#4-sintaxe)
5. [Valores demonstrados](#5-valores-demonstrados)
6. [O caso mais importante: `stretch`](#6-o-caso-mais-importante-stretch)
7. [Relação entre `justify-content` e `grid-template-columns`](#7-relação-entre-justify-content-e-grid-template-columns)
8. [Ajuste aplicado no exemplo](#8-ajuste-aplicado-no-exemplo)
9. [Comparação direta](#9-comparação-direta)
10. [Fluxo de raciocínio](#10-fluxo-de-raciocínio)
11. [Regras mentais para memorizar](#11-regras-mentais-para-memorizar)
12. [Erros e confusões comuns](#12-erros-e-confusões-comuns)
13. [Mapa mental final](#13-mapa-mental-final)
14. [Resumo final](#14-resumo-final)
15. [Regra mental definitiva](#15-regra-mental-definitiva)

---

## 1. O que é `justify-content`?

A propriedade:

```css
justify-content
```

controla o alinhamento do conjunto de colunas do Grid dentro do espaço disponível do container, ao longo do eixo horizontal (o eixo inline, em idiomas escritos na horizontal).

Ela não reorganiza o HTML e não muda a ordem dos itens. O que ela faz é decidir **como a estrutura do Grid se posiciona no eixo horizontal** quando sobra espaço dentro do container.

Os pontos centrais desta documentação são:

- como o Grid se desloca para a esquerda, direita ou centro;
- como o espaço livre pode ser distribuído entre as colunas;
- por que `stretch` depende do tipo de tamanho definido nas tracks.

---

## 2. Onde `justify-content` atua?

Primeiro existe o Grid Container:

```css
.grid {
  display: grid;
}
```

Depois existe a estrutura das tracks (linhas e colunas):

```css
grid-template: repeat(3, 8.75rem) / repeat(3, 8.75rem);
```

Essa declaração significa:

```text
repeat(3, 8.75rem)
→ 3 linhas com 8.75rem

/

repeat(3, 8.75rem)
→ 3 colunas com 8.75rem
```

Ou seja, a grade deste exemplo tem tamanho fixo:

```text
3 colunas fixas
3 linhas fixas
```

Visualmente:

```text
┌──────────┬──────────┬──────────┐
│    1     │    2     │    3     │
├──────────┼──────────┼──────────┤
│    4     │    5     │    6     │
├──────────┼──────────┼──────────┤
│          │          │          │
└──────────┴──────────┴──────────┘
```

O `justify-content` atua sobre esse bloco de colunas. Ele só tem efeito visível quando **sobra espaço horizontal** dentro do Grid Container.

### Regra mental

> **`justify-content` responde à pergunta: "onde a grade deve ficar no eixo horizontal?"**

---

## 3. Estrutura usada no exemplo

Todas as grades compartilham a mesma base:

```css
.grid {
  display: grid;
  grid-template: repeat(3, 8.75rem) / repeat(3, 8.75rem);
}
```

O HTML repete essa grade com uma classe diferente para cada valor de `justify-content`:

```html
<div class="grid start">
  <div>1</div>
  <div>2</div>
  <div>3</div>
  <div>4</div>
  <div>5</div>
  <div>6</div>
</div>
```

As demais grades seguem o mesmo modelo, trocando apenas a classe:

```html
<div class="grid end">...</div>
<div class="grid center">...</div>
<div class="grid stretch">...</div>
<div class="grid space-around">...</div>
<div class="grid space-between">...</div>
<div class="grid space-evenly">...</div>
```

Cada classe altera apenas a forma como o Grid se alinha horizontalmente.

---

## 4. Sintaxe

```css
justify-content: start;
```

Decompondo:

```text
justify-content
→ propriedade que controla o alinhamento horizontal da grade

start
→ encosta a grade no início do eixo horizontal
```

Outros valores demonstrados:

```css
justify-content: end;
justify-content: center;
justify-content: stretch;
justify-content: space-around;
justify-content: space-between;
justify-content: space-evenly;
```

---

## 5. Valores demonstrados

### 5.1 `start`

```css
justify-content: start;
```

Posiciona a grade no início do eixo horizontal.

```text
┌──────────────────────────────────────────────┐
│┌──────┬──────┬──────┐                        │
││ GRID │ GRID │ GRID │                        │
│└──────┴──────┴──────┘                        │
└──────────────────────────────────────────────┘
```

---

### 5.2 `end`

```css
justify-content: end;
```

Posiciona a grade no final do eixo horizontal.

```text
┌──────────────────────────────────────────────┐
│                        ┌──────┬──────┬──────┐│
│                        │ GRID │ GRID │ GRID ││
│                        └──────┴──────┴──────┘│
└──────────────────────────────────────────────┘
```

---

### 5.3 `center`

```css
justify-content: center;
```

Centraliza a grade horizontalmente.

```text
┌──────────────────────────────────────────────┐
│        ┌──────┬──────┬──────┐                │
│        │ GRID │ GRID │ GRID │                │
│        └──────┴──────┴──────┘                │
└──────────────────────────────────────────────┘
```

---

### 5.4 `space-around`

```css
justify-content: space-around;
```

Distribui o espaço livre **entre as colunas**, reservando metade desse espaço em cada extremidade.

```text
[½][col][1][col][1][col][½]
```

O espaço entre duas colunas é o dobro do espaço em cada extremidade.

---

### 5.5 `space-between`

```css
justify-content: space-between;
```

Encosta a primeira coluna no início, a última no fim, e divide o espaço livre igualmente entre as colunas.

```text
[col][1][col][1][col]
```

---

### 5.6 `space-evenly`

```css
justify-content: space-evenly;
```

Divide o espaço livre em partes exatamente iguais: entre as colunas e também nas extremidades.

```text
[1][col][1][col][1][col][1]
```

> No Grid, os valores `space-*` distribuem espaço entre as **tracks** (colunas), e não ao redor da grade como um bloco único.

### 🧠 Corte mental

```text
space-around
→ metade do espaço nas pontas

space-between
→ nenhum espaço nas pontas

space-evenly
→ espaço igual em todos os intervalos
```

---

## 6. O caso mais importante: `stretch`

### 6.1 O que `stretch` faz?

```css
justify-content: stretch;
```

O `stretch` aumenta as tracks de tamanho `auto` para que ocupem o espaço livre do container. Tracks com tamanho fixo não são alteradas.

```text
há espaço sobrando no container
        ↓
existem tracks `auto`?
        ↓
sim → o espaço livre é dividido entre elas
não → nada cresce
```

---

### 6.2 Por que `stretch` não funciona com colunas fixas?

A grade do exemplo foi definida assim:

```css
.grid {
  grid-template: repeat(3, 8.75rem) / repeat(3, 8.75rem);
}
```

Isso cria colunas com largura fixa:

```text
8.75rem
8.75rem
8.75rem
```

Colunas fixas não crescem, então o `stretch` não tem o que esticar.

### 🧠 Corte mental

```text
coluna fixa
→ não cresce

stretch
→ precisa de espaço livre + tracks `auto`
```

O problema não está na sintaxe do `justify-content`. Está na combinação entre:

```text
justify-content: stretch
        +
grid-template-columns com tamanho fixo
```

---

## 7. Relação entre `justify-content` e `grid-template-columns`

Esse é o ponto central desta documentação:

```text
justify-content
→ alinha a grade no espaço disponível

grid-template-columns
→ define como as colunas são construídas
```

Existem três situações principais.

### Colunas fixas

```css
grid-template-columns: repeat(3, 8.75rem);
```

O alinhamento horizontal move a grade (`start`, `end`, `center`, `space-*`), mas não altera a largura das colunas. O `stretch` não tem efeito.

### Colunas `auto`

```css
grid-template-columns: repeat(3, auto);
```

As colunas começam com o tamanho do conteúdo. Com `stretch`, elas crescem e dividem o espaço livre.

### Colunas flexíveis (`fr`)

```css
grid-template-columns: repeat(3, 1fr);
```

ou:

```css
grid-template-columns: repeat(3, minmax(0, 1fr));
```

As colunas com `fr` já consomem todo o espaço livre do container. Como não sobra espaço, `justify-content` deixa de ter efeito visível.

### Mapa mental

```text
                 JUSTIFY-CONTENT
                        │
                        ↓
          "Como a grade se comporta no eixo horizontal?"
                        │
        ┌───────────────┼───────────────┐
        ↓               ↓               ↓
   tracks fixas    tracks `auto`    tracks `fr`
        │               │               │
        ↓               ↓               ↓
  só desloca a     stretch faz      não sobra espaço
  grade            as tracks        livre para
                   crescerem        alinhar
```

---

## 8. Ajuste aplicado no exemplo

Para que o `stretch` tenha efeito visível, a classe `.stretch` troca as colunas fixas por colunas `auto`:

```css
.stretch {
  grid-template-columns: repeat(3, auto);
  justify-content: stretch;
}
```

A grade continua com 3 colunas, mas agora elas podem crescer e dividir o espaço livre do container.

> **⚠️ Atenção**
>
> Usar `grid-template-columns: auto;` (sem `repeat`) cria **uma única coluna**. Os itens passam a ser empilhados em uma coluna só, e a grade deixa de representar o layout 3×3 do exemplo. Para manter 3 colunas, use `repeat(3, auto)` ou `auto auto auto`.

---

## 9. Comparação direta

| Situação | Resultado com `stretch` |
| --- | --- |
| `grid-template-columns: repeat(3, 8.75rem);` | sem efeito: colunas fixas não crescem |
| `grid-template-columns: repeat(3, auto);` | as colunas crescem e dividem o espaço livre |
| `grid-template-columns: repeat(3, 1fr);` | sem efeito visível: `fr` já ocupa todo o espaço |
| `grid-template-columns: repeat(3, minmax(0, 1fr));` | igual a `1fr`, mas as colunas podem encolher abaixo do tamanho mínimo do conteúdo, o que evita overflow |

---

## 10. Fluxo de raciocínio

```text
1. display: grid
        ↓
2. o elemento vira Grid Container
        ↓
3. grid-template-columns define as colunas
        ↓
4. sobra espaço horizontal no container?
        ↓
5. justify-content decide como esse espaço é usado
        ↓
6. stretch só atua sobre tracks `auto`
```

---

## 11. Regras mentais para memorizar

> **Regra mental:** `justify-content` não corrige uma estrutura rígida. Ele trabalha sobre a estrutura que já foi criada.

> **Regra mental:** colunas fixas favorecem diferenças visuais entre `start`, `end` e `center`, mas não permitem que `stretch` tenha efeito.

> **Regra mental:** se a ideia é expandir colunas, olhe primeiro para `grid-template-columns`, não apenas para `justify-content`.

---

## 12. Erros e confusões comuns

Não confunda:

```text
justify-content
→ alinhamento horizontal da grade

grid-template-columns
→ definição estrutural das colunas
```

Também não é uma boa leitura pensar:

```text
"stretch não funciona"
```

O comportamento mais correto é:

```text
"stretch só estica tracks `auto`"
```

> **⚠️ Atenção**
>
> Quando a grade é construída com medidas fixas, muitos efeitos de alinhamento aparecem apenas como deslocamento horizontal, não como expansão.

> **⚠️ Atenção**
>
> Em Grid, o valor padrão de `justify-content` é `normal`, que se comporta como `stretch`. Por isso, tracks `auto` já crescem mesmo sem declarar a propriedade.

---

## 13. Mapa mental final

```text
                    CSS GRID
                       │
                       ↓
                justify-content
                       │
       ┌───────────────┼────────────────┐
       ↓               ↓                ↓
     posição       distribuição      expansão
       │               │                │
       ↓               ↓                ↓
 start/end/center  space-*          stretch
                                         │
                                         ↓
                         depende do tamanho das tracks
                                         │
                 ┌───────────────────────┼───────────────────────┐
                 ↓                       ↓                       ↓
            tracks fixas            tracks `auto`            tracks `fr`
                 │                       │                       │
                 ↓                       ↓                       ↓
           sem expansão           expandem com stretch    já ocupam o espaço
```

---

## 14. Resumo final

```text
CONCEITO
→ `justify-content` alinha a grade no eixo horizontal.

PROPRIEDADE
→ define como o conjunto de colunas se comporta dentro do espaço disponível.

VALORES
→ `start`, `end`, `center`, `stretch`, `space-around`, `space-between`, `space-evenly`.

RELAÇÃO
→ o resultado visual depende diretamente de como as colunas foram definidas em `grid-template-columns`.
```

No exemplo estudado:

```text
`start`, `end` e `center`
→ deslocam visualmente a grade

`space-*`
→ distribuem o espaço horizontal entre as colunas

`stretch`
→ só tem efeito sobre tracks `auto`
```

---

## 15. Regra mental definitiva

```text
justify-content
→ "como a grade se alinha no eixo horizontal?"

grid-template-columns
→ "essas colunas são fixas, auto ou flexíveis?"

stretch
→ "só estica o que pode crescer"
```