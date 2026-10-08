# `justify-items` no CSS Grid

## Índice

1. [Conceito principal](#1-conceito-principal)
2. [Eixo trabalhado](#2-eixo-trabalhado)
3. [Estrutura utilizada no exemplo](#3-estrutura-utilizada-no-exemplo)
4. [`justify-items: start`](#4-justify-items-start)
5. [O que acontece com a coluna?](#5-o-que-acontece-com-a-coluna)
6. [`justify-items: end`](#6-justify-items-end)
7. [`justify-items: center`](#7-justify-items-center)
8. [Item ocupando duas colunas](#8-item-ocupando-duas-colunas)
9. [`justify-items: stretch`](#9-justify-items-stretch)
10. [`start` × `end` × `center` × `stretch`](#10-start--end--center--stretch)
11. [`justify-items` × `justify-content`](#11-justify-items--justify-content)
12. [Relação entre coluna e item](#12-relação-entre-coluna-e-item)
13. [Fluxo de raciocínio](#13-fluxo-de-raciocínio)
14. [Erros e confusões comuns](#14-erros-e-confusões-comuns)
15. [Mapa mental](#15-mapa-mental)

---

## 1. Conceito principal

`justify-items` é uma propriedade do **Grid Container** que controla o **alinhamento dos Grid Items dentro da área que cada um ocupa**, no **eixo horizontal** (eixo inline, em idiomas escritos na horizontal).

A principal diferença está naquilo que cada propriedade controla:

```text
justify-content
→ trabalha com a estrutura inteira do Grid.

justify-items
→ trabalha com os itens dentro das áreas que eles ocupam.
```

Em outras palavras:

```text
Content
→ considera a estrutura (as tracks) do Grid.

Items
→ considera o item dentro da sua área.
```

### 🧠 Corte mental

```text
`justify-content`
→ "Como distribuo a estrutura?"

`justify-items`
→ "Como alinho o item dentro da área que ele ocupa?"
```

---

## 2. Eixo trabalhado

Como a propriedade começa com `justify-`, o alinhamento ocorre no **eixo horizontal**.

```text
←──────────── eixo horizontal ────────────→

[      item      ]
```

O item pode ser alinhado com:

```text
start
center
end
```

ou pode ocupar toda a largura da sua área com:

```text
stretch
```

---

## 3. Estrutura utilizada no exemplo

Considere um Grid com 3 colunas de `1fr` e 3 linhas de `50px`:

```css
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-rows: 50px 50px 50px;
}
```

Visualmente:

```text
┌──────────┬──────────┬──────────┐
│ Coluna 1 │ Coluna 2 │ Coluna 3 │
├──────────┼──────────┼──────────┤
│          │          │          │
├──────────┼──────────┼──────────┤
│          │          │          │
└──────────┴──────────┴──────────┘
```

As colunas continuam existindo independentemente da forma como os itens são alinhados.

---

## 4. `justify-items: start`

```css
.grid {
  justify-items: start;
}
```

O item é alinhado ao **início da sua área**.

```text
┌────────────┬────────────┬────────────┐
│ Item 1     │ Item 2     │ Item 3     │
│            │            │            │
└────────────┴────────────┴────────────┘
  ↑             ↑             ↑
  início de cada coluna
```

O item deixa de se esticar e passa a ter a largura que o seu conteúdo precisa. Se os itens tiverem conteúdos de tamanhos diferentes, eles terão larguras diferentes:

```text
Item 1 → conteúdo maior   → item mais largo
Item 2 → conteúdo menor   → item mais estreito
Item 3 → conteúdo menor   → item mais estreito
```

---

## 5. O que acontece com a coluna?

Ao usar:

```css
justify-items: start;
```

a coluna **não muda**. A estrutura continua sendo:

```text
Coluna 1
Coluna 2
Coluna 3
```

O que muda é o posicionamento e a largura do item **dentro da coluna**.

```text
┌─────────────────── Coluna 1 ───────────────────┐
│ Item 1                                         │
│                                                │
└────────────────────────────────────────────────┘
```

A coluna continua ocupando sua largura completa. O item ocupa apenas a parte necessária e fica alinhado no início.

### 🧠 Corte mental

```text
A coluna continua inteira.
O item é que muda de posição e dimensão dentro dela.
```

---

## 6. `justify-items: end`

```css
.grid {
  justify-items: end;
}
```

O item é alinhado ao **final da sua área**, no eixo horizontal.

```text
┌─────────────────────────────┐
│                       Item  │
└─────────────────────────────┘
                          ↑
                        final
```

A coluna permanece com a mesma largura. O item é deslocado para o final dela.

---

## 7. `justify-items: center`

```css
.grid {
  justify-items: center;
}
```

O item é alinhado no **centro horizontal da sua área**.

```text
┌─────────────────────────────┐
│          Item               │
└─────────────────────────────┘
              ↑
            centro
```

O centro é calculado considerando **a área que o item ocupa**, o que é especialmente importante quando um item ocupa mais de uma coluna.

---

## 8. Item ocupando duas colunas

Considere um item com:

```css
.item-largo {
  grid-column: span 2;
}
```

Isso significa que o item ocupa duas colunas.

```text
┌────────────┬────────────┬────────────┐
│            │            │            │
│        Item largo       │            │
│            │            │            │
└────────────┴────────────┴────────────┘
   coluna 1     coluna 2     coluna 3
```

> A linha e a coluna em que o item aparece dependem do posicionamento automático do Grid. O ponto importante é a **largura da área** que ele ocupa: duas colunas.

Com:

```css
justify-items: center;
```

o item é centralizado considerando **as duas colunas juntas**, e não apenas uma delas.

```text
Item ocupa 1 coluna
      ↓
centralização dentro de 1 coluna

Item ocupa 2 colunas
      ↓
centralização dentro das 2 colunas
```

Isso explica por que um item que ocupa duas colunas pode parecer alinhado de forma diferente dos itens que ocupam apenas uma.

### 🧠 Corte mental

```text
`justify-items`
→ sempre considera a área que o item ocupa.

`span 2`
→ o item ocupa duas colunas.

`center`
→ o centro é calculado considerando essas duas colunas.
```

---

## 9. `justify-items: stretch`

```css
.grid {
  justify-items: stretch;
}
```

`stretch` é o **comportamento padrão** (o valor inicial `normal` se comporta como `stretch` nos Grid Items). Os itens são esticados para ocupar a largura da sua área.

```text
┌────────────┬────────────┬────────────┐
│   Item 1   │   Item 2   │   Item 3   │
├────────────┼────────────┼────────────┤
│            │            │            │
└────────────┴────────────┴────────────┘
```

> **⚠️ Atenção**
>
> O `stretch` só atua em itens cuja largura é `auto`. Se o item tiver um `width` definido, ele mantém esse tamanho e não é esticado.

---

## 10. `start` × `end` × `center` × `stretch`

| Valor | Comportamento |
| --- | --- |
| `start` | Alinha o item no início da área, com a largura do conteúdo |
| `end` | Alinha o item no final da área, com a largura do conteúdo |
| `center` | Centraliza o item na área, com a largura do conteúdo |
| `stretch` | Estica o item (com largura `auto`) para ocupar a área inteira |

---

## 11. `justify-items` × `justify-content`

Essa é uma das diferenças mais importantes para memorizar.

| Propriedade | O que controla |
| --- | --- |
| `justify-content` | Alinhamento e distribuição das tracks (a estrutura do Grid) |
| `justify-items` | Alinhamento dos itens dentro das áreas que ocupam |

Visualmente, considere uma grade com colunas fixas, menor que o container:

```text
┌─────────────────────────────────────┐
│                                     │
│   ┌────────┐ ┌────────┐ ┌────────┐  │
│   │ Item 1 │ │ Item 2 │ │ Item 3 │  │
│   └────────┘ └────────┘ └────────┘  │
│                                     │
└─────────────────────────────────────┘
```

`justify-content` posiciona **a grade como conjunto** dentro do container.

`justify-items` posiciona **cada item dentro da sua área**.

### 🧠 Corte mental definitivo

```text
`justify-content`
→ conteúdo/estrutura.

`justify-items`
→ item dentro da estrutura.
```

> **Boa prática**
>
> `justify-items` é definido no container e vale para todos os itens. Para alterar o alinhamento de um único item, use `justify-self` nele.

---

## 12. Relação entre coluna e item

```text
Grid
  ↓
colunas
  ↓
área ocupada pelo item
  ↓
`justify-items`
  ↓
posição do item dentro dessa área
```

Exemplo:

```text
Coluna
┌────────────────────────────┐
│                            │
│       ┌──────────┐         │
│       │   Item   │         │
│       └──────────┘         │
│                            │
└────────────────────────────┘
```

A coluna continua existindo. O `justify-items` determina onde o item ficará dentro dela.

---

## 13. Fluxo de raciocínio

```text
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
```

Quando o item ocupa várias colunas:

```text
`grid-column: span 2`
      ↓
item ocupa duas colunas
      ↓
`justify-items`
      ↓
alinhamento considera as duas colunas
```

---

## 14. Erros e confusões comuns

### 14.1 Confundir o item com a coluna

Quando o item não ocupa toda a área:

```text
coluna
┌─────────────────────────────┐
│ Item                        │
│                             │
│                             │
└─────────────────────────────┘
```

isso não significa que a coluna ficou menor. A coluna continua com a largura definida pelo Grid. É o **item** que deixou de ocupar toda a área.

---

### 14.2 Esquecer o `span`

Um item com:

```css
grid-column: span 1;
```

é alinhado dentro de uma coluna.

Um item com:

```css
grid-column: span 2;
```

passa a ser alinhado dentro de uma área formada por duas colunas. Por isso, seu alinhamento com:

```css
justify-items: center;
```

pode parecer diferente dos demais itens.

---

### 14.3 Confundir `stretch` com os outros valores

No `stretch`, o item ocupa a área inteira (se a largura for `auto`).

Nos valores:

```text
start
end
center
```

o item pode ficar menor que a área da coluna e apenas mudar de posição dentro dela.

---

### 14.4 Esperar que `stretch` ignore o `width`

Se o item tiver largura definida, o `stretch` não a altera:

```css
.item {
  width: 80px;
}
```

Nesse caso, o item permanece com `80px`, mesmo com `justify-items: stretch`.

---

## 15. Mapa mental

```text
`justify-items`
         │
         ↓
alinhamento horizontal do item
         │
         ├───────────────┬───────────────┬───────────────┐
         ↓               ↓               ↓               ↓
      `start`         `center`          `end`         `stretch`
         │               │               │               │
      início           centro           final        ocupa a área
                                                     (largura `auto`)

         +
         │
         ↓
área ocupada pelo item
         │
         ├── 1 coluna
         │
         └── 2 ou mais colunas
                (`span`)
´´´