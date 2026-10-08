# CSS Grid Layout — `align-self`

> [!NOTE]
> Esta documentação explica o funcionamento da propriedade `align-self` no CSS Grid, com base na documentação oficial do CSS (MDN e CSS Box Alignment).

## Índice

1. [O que é `align-self`?](#1-o-que-é-align-self)
2. [A ideia central](#2-a-ideia-central)
3. [O significado de `self`](#3-o-significado-de-self)
4. [`align-items` × `align-self`](#4-align-items--align-self)
5. [Onde `align-self` atua?](#5-onde-align-self-atua)
6. [Não confunda Grid Area com item](#6-não-confunda-grid-area-com-item)
7. [Relação com `grid-row`](#7-relação-com-grid-row)
8. [Sintaxe](#8-sintaxe)
9. [`align-self: start`](#9-align-self-start)
10. [`align-self: end`](#10-align-self-end)
11. [`align-self: center`](#11-align-self-center)
12. [`align-self: stretch`](#12-align-self-stretch)
13. [Comparação dos quatro valores](#13-comparação-dos-quatro-valores)
14. [Exemplo prático](#14-exemplo-prático)
15. [`align-self` não altera as linhas](#15-align-self-não-altera-as-linhas)
16. [`align-items` como regra geral](#16-align-items-como-regra-geral)
17. [Modelo "regra geral + exceção"](#17-modelo-regra-geral--exceção)
18. [`align-self` × `justify-self`](#18-align-self--justify-self)
19. [Por que falamos em eixo, e não em "horizontal/vertical"?](#19-por-que-falamos-em-eixo-e-não-em-horizontalvertical)
20. [`align-self: auto`](#20-align-self-auto)
21. [O comportamento padrão](#21-o-comportamento-padrão)
22. [Exceção: imagens e proporção](#22-exceção-imagens-e-proporção)
23. [`align-self` e `height`](#23-align-self-e-height)
24. [`place-self`](#24-place-self)
25. [Valores adicionais](#25-valores-adicionais)
26. [Mapa mental](#26-mapa-mental)
27. [Regra de ouro](#27-regra-de-ouro)
28. [Como raciocinar diante de um problema](#28-como-raciocinar-diante-de-um-problema)
29. [Tabela de fixação](#29-tabela-de-fixação)
30. [Tabela de comparação](#30-tabela-de-comparação)
31. [Checklist de compreensão](#31-checklist-de-compreensão)
32. [Resumo final](#32-resumo-final)
33. [Regra definitiva para memorizar](#33-regra-definitiva-para-memorizar)
34. [Referências oficiais](#34-referências-oficiais)
35. [GitHub](#35-github)

---

## 1. O que é `align-self`?

A propriedade `align-self` define **como um único Grid Item é alinhado dentro da Grid Area que ele ocupa**, no **eixo block**.

Em um layout horizontal tradicional:

```text
EIXO BLOCK
    ↑
    │
    ↓

EIXO INLINE
←────────────────────→
```

```text
justify-self
    ↓
eixo inline
    ↓
normalmente horizontal


align-self
    ↓
eixo block
    ↓
normalmente vertical
```

A MDN define `align-self` como a propriedade que alinha um item dentro da sua Grid Area no eixo block.

---

## 2. A ideia central

Imagine uma Grid Area alta e um item com altura menor que ela. Com `start`, `center` ou `end`, o item ocupa apenas a altura do seu conteúdo, e `align-self` decide **onde** ele fica dentro da área.

```css
.item {
  align-self: start;
}
```

```text
┌───────────────────────────────┐
│       ┌──────────┐            │
│       │   ITEM   │            │
│       └──────────┘            │
│                               │
│                               │
└───────────────────────────────┘
```

```css
.item {
  align-self: end;
}
```

```text
┌───────────────────────────────┐
│                               │
│                               │
│       ┌──────────┐            │
│       │   ITEM   │            │
│       └──────────┘            │
└───────────────────────────────┘
```

```css
.item {
  align-self: center;
}
```

```text
┌───────────────────────────────┐
│                               │
│       ┌──────────┐            │
│       │   ITEM   │            │
│       └──────────┘            │
│                               │
└───────────────────────────────┘
```

> **Nota:** esse é o comportamento com `start`, `center` e `end`. O comportamento padrão no Grid é `stretch`, que estica o item (seção 12).

---

## 3. O significado de `self`

```text
align → alinhamento
self  → o próprio elemento
```

`align-self` responde à pergunta:

> "Como **este** item deve ser alinhado?"

Isso o diferencia de `align-items`.

---

## 4. `align-items` × `align-self`

### `align-items`

É aplicada no **Grid Container** e define o alinhamento padrão de todos os itens.

```css
.container {
  display: grid;
  align-items: center;
}
```

### `align-self`

É aplicada no **Grid Item** e altera o alinhamento de **um item específico**.

```css
.item {
  align-self: end;
}
```

A MDN descreve `align-items` como a propriedade que define o valor de `align-self` para todos os filhos como grupo.

```text
                 GRID CONTAINER
                       │
                align-items: center
                       │
              ┌────────┼────────┐
              ↓        ↓        ↓
            ITEM 1   ITEM 2   ITEM 3
              │        │        │
              │        └── align-self: end
              │
              └── segue o padrão
```

```text
ITEM 1 → center
ITEM 2 → end
ITEM 3 → center
```

```text
align-items → regra geral
align-self  → regra individual
```

---

## 5. Onde `align-self` atua?

```text
align-self
      ↓
alinhamento individual
      ↓
dentro da Grid Area
      ↓
no eixo block
```

```text
┌────────────────────────────────────┐
│             GRID AREA              │
│                                    │
│                                    │
│               ┌───────┐            │
│               │ ITEM  │            │
│               └───────┘            │
│                                    │
│                                    │
└────────────────────────────────────┘
```

A Grid Area continua no mesmo lugar. O que muda é a posição do item **dentro dela**.

---

## 6. Não confunda Grid Area com item

```css
.item {
  grid-row: 1 / 3;
}
```

O item ocupa duas rows. Toda essa região é a área disponível para ele.

```text
┌───────────────┐
│               │ ← row 1
│               │
│               │ ← row 2
│               │
└───────────────┘
```

Agora:

```css
.item {
  align-self: start;
}
```

```text
┌───────────────┐
│    ┌──────┐   │
│    │ ITEM │   │
│    └──────┘   │
│               │
│               │
└───────────────┘
```

`align-self` não alterou `grid-row`. Ele apenas alinhou o item dentro da área já determinada.

---

## 7. Relação com `grid-row`

```text
grid-row
→ "Em quais rows o item ficará?"

align-self
→ "Como o item ficará dentro dessa área?"
```

```text
grid-row define a área

┌───────────────┐
│               │
│   GRID AREA   │
│               │
└───────────────┘

align-self posiciona o item

┌───────────────┐
│    ┌──────┐   │
│    │ ITEM │   │
│    └──────┘   │
│               │
└───────────────┘
```

---

## 8. Sintaxe

```css
.item {
  align-self: valor;
}
```

Os quatro valores principais:

```css
align-self: start;
align-self: end;
align-self: center;
align-self: stretch;
```

A propriedade também aceita `auto`, `normal`, `self-start`, `self-end`, `baseline` e combinações com `safe` e `unsafe` (seção 25).

---

## 9. `align-self: start`

```css
.item {
  align-self: start;
}
```

Coloca o item no **início do eixo block** da sua Grid Area. Em um layout horizontal tradicional, é a parte superior.

```text
┌───────────────────────────────┐
│       ┌──────────┐            │
│       │   ITEM   │            │
│       └──────────┘            │
│                               │
│                               │
└───────────────────────────────┘
```

```text
start = início
```

---

## 10. `align-self: end`

```css
.item {
  align-self: end;
}
```

Coloca o item no **final do eixo block**.

```text
┌───────────────────────────────┐
│                               │
│                               │
│       ┌──────────┐            │
│       │   ITEM   │            │
│       └──────────┘            │
└───────────────────────────────┘
```

```text
end = final
```

---

## 11. `align-self: center`

```css
.item {
  align-self: center;
}
```

Centraliza o item dentro da Grid Area, no eixo block.

```text
┌───────────────────────────────┐
│                               │
│       ┌──────────┐            │
│       │   ITEM   │            │
│       └──────────┘            │
│                               │
└───────────────────────────────┘
```

```text
center = meio
```

---

## 12. `align-self: stretch`

```css
.item {
  align-self: stretch;
}
```

Em vez de posicionar o item, `stretch` o **estica** para ocupar o espaço disponível no eixo block.

```text
┌───────────────────────────────┐
│ ┌───────────────────────────┐ │
│ │                           │ │
│ │           ITEM            │ │
│ │                           │ │
│ └───────────────────────────┘ │
└───────────────────────────────┘
```

> **⚠️ Atenção**
>
> O `stretch` só atua em itens com `height: auto`. Se o item tiver `height` definido, ele mantém essa altura e é posicionado como `start`. Restrições como `min-height` e `max-height` também limitam o resultado.

---

## 13. Comparação dos quatro valores

```text
START

┌───────────────────────────────┐
│       ┌──────────┐            │
│       │   ITEM   │            │
│       └──────────┘            │
│                               │
│                               │
└───────────────────────────────┘
```

```text
CENTER

┌───────────────────────────────┐
│                               │
│       ┌──────────┐            │
│       │   ITEM   │            │
│       └──────────┘            │
│                               │
└───────────────────────────────┘
```

```text
END

┌───────────────────────────────┐
│                               │
│                               │
│       ┌──────────┐            │
│       │   ITEM   │            │
│       └──────────┘            │
└───────────────────────────────┘
```

```text
STRETCH

┌───────────────────────────────┐
│ ┌───────────────────────────┐ │
│ │           ITEM            │ │
│ │                           │ │
│ └───────────────────────────┘ │
└───────────────────────────────┘
```

```text
start   → para o começo
center  → para o meio
end     → para o final
stretch → ocupa o espaço disponível
```

---

## 14. Exemplo prático

```html
<div class="container">
  <div class="item item-1">1</div>
  <div class="item item-2">2</div>
  <div class="item item-3">3</div>
  <div class="item item-4">4</div>
</div>
```

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-auto-rows: 120px;
}

.item-1 {
  align-self: start;
}

.item-2 {
  align-self: end;
}

.item-3 {
  align-self: center;
}

.item-4 {
  align-self: stretch;
}
```

Os itens 1 e 2 ficam na primeira row, e os itens 3 e 4, na segunda. Cada item está em sua própria célula, com um alinhamento diferente no eixo block:

```text
┌───────────────────┬───────────────────┐
│ ┌───────┐         │                   │
│ │   1   │         │                   │
│ └───────┘         │                   │
│                   │         ┌───────┐ │
│                   │         │   2   │ │
│                   │         └───────┘ │
├───────────────────┼───────────────────┤
│                   │ ┌───────────────┐ │
│ ┌───────┐         │ │               │ │
│ │   3   │         │ │       4       │ │
│ └───────┘         │ │               │ │
│                   │ └───────────────┘ │
└───────────────────┴───────────────────┘

1 → start    2 → end
3 → center   4 → stretch
```

---

## 15. `align-self` não altera as linhas

```css
.item {
  grid-row: 1 / 3;
}
```

A área do item permanece a mesma. Ao adicionar:

```css
.item {
  align-self: end;
}
```

não estamos dizendo "mova o item para a row 3". Estamos dizendo:

> "alinhe o item no final da área que ele já ocupa."

---

## 16. `align-items` como regra geral

```css
.container {
  display: grid;
  align-items: center;
}
```

Todos os itens são alinhados ao centro das suas Grid Areas.

```text
┌───────────────┬───────────────┐
│               │               │
│    ITEM 1     │    ITEM 2     │
│               │               │
├───────────────┼───────────────┤
│               │               │
│    ITEM 3     │    ITEM 4     │
│               │               │
└───────────────┴───────────────┘
```

Para abrir uma exceção:

```css
.item-2 {
  align-self: end;
}
```

```text
ITEM 1 → center
ITEM 2 → end
ITEM 3 → center
ITEM 4 → center
```

---

## 17. Modelo "regra geral + exceção"

```text
align-items
     ↓
regra geral
     ↓
todos os itens

align-self
     ↓
exceção
     ↓
um item específico
```

```css
.container {
  align-items: center;
}

.item-destaque {
  align-self: end;
}
```

```text
TODOS → center
EXCETO .item-destaque → end
```

---

## 18. `align-self` × `justify-self`

```text
              GRID ITEM
                  │
          ┌───────┴───────┐
          │               │
          ▼               ▼
   justify-self       align-self
          │               │
          ▼               ▼
    eixo inline      eixo block
          │               │
          ▼               ▼
   geralmente         geralmente
   horizontal          vertical
```

Em um layout horizontal tradicional:

```text
┌─────────────────────────────────┐
│               ↑                 │
│               │ align-self      │
│               ↓                 │
│          ┌────────┐             │
│ ←──────→ │  ITEM  │ ←──────→    │
│          └────────┘             │
│       justify-self              │
└─────────────────────────────────┘
```

Combinando os dois:

```css
.item {
  justify-self: center;
  align-self: center;
}
```

```text
┌─────────────────────────────────┐
│                                 │
│                                 │
│            ┌──────┐             │
│            │ ITEM │             │
│            └──────┘             │
│                                 │
│                                 │
└─────────────────────────────────┘
```

O item fica centralizado nos dois eixos.

---

## 19. Por que falamos em eixo, e não em "horizontal/vertical"?

É comum aprender:

```text
justify = horizontal
align   = vertical
```

Isso funciona em layouts tradicionais, mas o CSS trabalha com:

```text
inline axis → acompanha a direção do texto no writing-mode
block axis  → eixo em que os blocos são empilhados
```

```text
justify-self → eixo inline
align-self   → eixo block
```

Em idiomas e modos de escrita tradicionais, esses eixos aparecem como horizontal e vertical, mas isso não vale para todos os `writing-mode`.

---

## 20. `align-self: auto`

```css
align-self: auto;
```

É o valor inicial de `align-self`. No Grid, `auto` usa o valor de `align-items` do elemento pai.

```text
GRID CONTAINER
       │
       │ align-items: center
       ↓
GRID ITEM
       │
       │ align-self: auto
       ↓
usa o alinhamento do pai
       ↓
center
```

---

## 21. O comportamento padrão

O valor inicial de `align-items` é `normal`. No Grid, `normal` se comporta como `stretch`. Como `align-self` começa em `auto`, os itens normalmente se esticam pela área disponível.

```text
align-items: normal
    ↓
no Grid
    ↓
stretch
    ↓
itens (com height: auto) ocupam a área
```

---

## 22. Exceção: imagens e proporção

```html
<img src="foto.jpg" alt="Foto">
```

Uma imagem tem uma proporção natural:

```text
1920 × 1080  →  16 : 9
```

Esticá-la livremente poderia distorcê-la. Por isso, no comportamento `normal`, itens com proporção intrínseca (aspect ratio) se comportam como `start`, em vez de `stretch`.

```text
ITEM NORMAL
    ↓
stretch

IMAGEM COM PROPORÇÃO
    ↓
evita distorção
    ↓
comportamento semelhante a start
```

---

## 23. `align-self` e `height`

`align-self: stretch` não é o mesmo que `height: 100%`. São mecanismos diferentes.

```text
align-self: stretch → faz parte do Box Alignment,
                      respeita as restrições do item

height: 100%        → define uma altura explícita
```

```css
.item {
  align-self: stretch;
  max-height: 100px;
}
```

O `max-height` limita o quanto o item cresce. O que sobrar da área fica livre, e o item é posicionado como `start`.

---

## 24. `place-self`

`place-self` é o shorthand de `align-self` e `justify-self`:

```css
.item {
  place-self: center;
}
```

equivale a:

```css
.item {
  align-self: center;
  justify-self: center;
}
```

Com dois valores:

```css
.item {
  place-self: end center;
}
```

```text
place-self: <align-self> <justify-self>

primeiro valor → align-self   (end)
segundo valor  → justify-self (center)
```

---

## 25. Valores adicionais

Além de `start`, `end`, `center` e `stretch`:

```css
align-self: auto;
align-self: normal;

align-self: self-start;
align-self: self-end;

align-self: baseline;
align-self: first baseline;
align-self: last baseline;

align-self: safe center;
align-self: unsafe center;
```

Esses valores fazem parte do CSS Box Alignment e são úteis em situações mais avançadas. Para o estudo inicial, o essencial é:

```text
start
center
end
stretch
```

---

## 26. Mapa mental

```text
                    CSS GRID
                       │
                       ▼
                GRID CONTAINER
                       │
                       ▼
                   GRID ITEM
                       │
                       ▼
                   GRID AREA
                       │
                       ▼
                   ALINHAR
                       │
        ┌──────────────┴──────────────┐
        │                             │
        ▼                             ▼
   EIXO INLINE                   EIXO BLOCK
        │                             │
        ▼                             ▼
  justify-self                   align-self
                                      │
                    ┌───────┬─────────┼─────────┐
                    ▼       ▼         ▼         ▼
                  start   center     end     stretch
```

---

## 27. Regra de ouro

> **`align-self` controla o alinhamento de um único Grid Item dentro da sua Grid Area, no eixo block.**

```text
align-self
    ↓
vertical (em layouts horizontais)
    ↓
dentro da Grid Area
    ↓
item individual
```

---

## 28. Como raciocinar diante de um problema

```css
.item {
  align-self: center;
}
```

```text
1. Este elemento é um Grid Item?
        ↓
       SIM

2. Qual é a Grid Area dele?
        ↓
   descubra as rows e
   colunas que ele ocupa

3. Em qual eixo estamos trabalhando?
        ↓
   eixo block

4. Qual valor foi utilizado?
        ↓
      center

5. Resultado:
        ↓
   o item é centralizado
   dentro da Grid Area
```

Esse processo é mais útil do que decorar "align = vertical", porque explica **por que** o resultado acontece.

---

## 29. Tabela de fixação

| Valor | Comportamento | Mentalidade |
| --- | --- | --- |
| `start` | alinha o item no início do eixo block | "vai para o começo" |
| `center` | centraliza o item dentro da área | "vai para o meio" |
| `end` | alinha o item no final do eixo block | "vai para o final" |
| `stretch` | estica o item (com `height: auto`) até preencher a área, respeitando `min-height` e `max-height` | "preenche o espaço" |
| `auto` | usa o valor de `align-items` do pai | "segue a regra geral" |
| `normal` | no Grid, se comporta como `stretch` (ou `start`, com aspect ratio) | "comportamento normal" |

---

## 30. Tabela de comparação

| Propriedade | Aplicada em | Controla | Eixo |
| --- | --- | --- | --- |
| `align-content` | Grid Container | conjunto de tracks do Grid | block |
| `align-items` | Grid Container | alinhamento padrão dos itens | block |
| `align-self` | Grid Item | alinhamento de um item específico | block |
| `justify-content` | Grid Container | conjunto de tracks do Grid | inline |
| `justify-items` | Grid Container | alinhamento padrão dos itens | inline |
| `justify-self` | Grid Item | alinhamento de um item específico | inline |

---

## 31. Checklist de compreensão

```text
[ ] O que significa "self" em align-self?
[ ] Em qual elemento coloco align-self?
[ ] Qual é a diferença entre align-items e align-self?
[ ] Em qual eixo align-self trabalha?
[ ] O que acontece com align-self: start?
[ ] O que acontece com align-self: center?
[ ] O que acontece com align-self: end?
[ ] O que acontece com align-self: stretch?
[ ] Por que stretch não funciona com height definido?
[ ] align-self muda a Grid Area?
[ ] align-self muda as linhas do Grid?
[ ] Qual é a relação entre align-self e grid-row?
[ ] Qual é a relação entre align-self e justify-self?
[ ] Qual é a função de place-self?
```

---

## 32. Resumo final

```text
align-self
│
├── aplicado ao Grid Item
│
├── controla o alinhamento individual
│
├── atua dentro da Grid Area
│
├── trabalha no eixo block
│   └── em layouts horizontais, geralmente o eixo vertical
│
├── valores fundamentais
│   ├── start
│   ├── center
│   ├── end
│   └── stretch (só com height: auto)
│
├── pode sobrescrever align-items
│
└── não define onde a Grid Area está
    └── define como o item fica dentro dela
```

---

## 33. Regra definitiva para memorizar

```text
grid-row / grid-column / grid-area
        ↓
"ONDE O ITEM ESTÁ?"

align-self
        ↓
"COMO O ITEM FICA DENTRO DESSE ESPAÇO, NO EIXO BLOCK?"

justify-self
        ↓
"COMO O ITEM FICA DENTRO DESSE ESPAÇO, NO EIXO INLINE?"
```

---

## 34. Referências oficiais

- **MDN — `align-self`:** definição, sintaxe, valores e comportamento em Grid.
- **MDN — Box alignment in grid layout:** eixos inline e block, `align-items` e `align-self` no Grid.
- **MDN — `align-items`:** relação entre `align-items` e `align-self`.
- **MDN — `place-self`:** shorthand de `align-self` e `justify-self`.
- **W3C — CSS Box Alignment Module Level 3:** especificação oficial do sistema de alinhamento.

---

## 35. GitHub

<div align="center">

### CSS Grid Layout — `align-self`

Documentação técnica para estudo contínuo de CSS Grid Layout.

<br>

<a href="https://github.com/gabrielfelipeoliveira55" target="_blank" rel="noopener noreferrer">
Gabriel Felipe de Oliveira Rateiro
</a>

<br><br>

**CSS Grid Layout • `align-self`**

> `grid-area` define **onde** o item está.
>
> `align-self` define **como** ele fica dentro da área, no eixo block.

</div>