# CSS Grid — `grid-template-columns`, `minmax()`, `repeat()`, `auto-fit` e `auto-fill`

## Índice

- [1. `grid-template-columns`](#1-grid-template-columns)
- [2. `px` define o tamanho da coluna](#2-px-define-o-tamanho-da-coluna)
- [3. Coluna e conteúdo do item são conceitos diferentes](#3-coluna-e-conteúdo-do-item-são-conceitos-diferentes)
- [4. Conteúdo maior que a coluna](#4-conteúdo-maior-que-a-coluna)
- [5. `width` e a célula do Grid](#5-width-e-a-célula-do-grid)
- [6. `min-width` pode fazer o item ultrapassar a coluna](#6-min-width-pode-fazer-o-item-ultrapassar-a-coluna)
- [7. Unidade `fr`](#7-unidade-fr)
- [8. `fr` e o conteúdo](#8-fr-e-o-conteúdo)
  - [Regra mental](#regra-mental)
- [9. `minmax()`](#9-minmax)
- [10. Exemplo de `minmax()`](#10-exemplo-de-minmax)
- [11. `minmax(100px, 1fr)`](#11-minmax100px-1fr)
- [12. `minmax(50px, 100px)`](#12-minmax50px-100px)
- [13. `minmax()` resolve um problema importante](#13-minmax-resolve-um-problema-importante)
- [14. `minmax()` e conteúdo grande](#14-minmax-e-conteúdo-grande)
- [15. `repeat()`](#15-repeat)
- [16. Sintaxe do `repeat()`](#16-sintaxe-do-repeat)
- [17. `repeat()` economiza código](#17-repeat-economiza-código)
- [18. `repeat()` aceita diferentes valores](#18-repeat-aceita-diferentes-valores)
- [19. `repeat()` também pode ser combinado com outras colunas](#19-repeat-também-pode-ser-combinado-com-outras-colunas)
- [20. `auto-fit`](#20-auto-fit)
- [21. Como `auto-fit` se comporta](#21-como-auto-fit-se-comporta)
- [22. `auto-fit` com `1fr`](#22-auto-fit-com-1fr)
- [23. `auto-fit` com um tamanho fixo](#23-auto-fit-com-um-tamanho-fixo)
- [24. O padrão mais útil: `auto-fit` + `minmax()`](#24-o-padrão-mais-útil-auto-fit--minmax)
- [25. Como funciona `repeat(auto-fit, minmax())`](#25-como-funciona-repeatauto-fit-minmax)
- [26. Por que `minmax()` ajuda o `auto-fit`?](#26-por-que-minmax-ajuda-o-auto-fit)
- [27. `auto` como valor máximo](#27-auto-como-valor-máximo)
- [28. `auto-fit` x `auto-fill`](#28-auto-fit-x-auto-fill)
- [29. `auto-fit`](#29-auto-fit)
- [30. `auto-fill`](#30-auto-fill)
- [31. Diferença mental entre `auto-fit` e `auto-fill`](#31-diferença-mental-entre-auto-fit-e-auto-fill)
- [32. O que acontece quando o container aumenta?](#32-o-que-acontece-quando-o-container-aumenta)
- [33. Layout responsivo sem várias Media Queries](#33-layout-responsivo-sem-várias-media-queries)
- [34. Padrão de Grid responsivo](#34-padrão-de-grid-responsivo)
- [35. Exemplo completo](#35-exemplo-completo)
- [36. Combinações importantes](#36-combinações-importantes)
  - [Colunas fixas](#colunas-fixas)
  - [Colunas proporcionais](#colunas-proporcionais)
  - [Repetição](#repetição)
  - [Limite mínimo e máximo](#limite-mínimo-e-máximo)
  - [Grid responsivo](#grid-responsivo)
- [37. Estrutura mental completa](#37-estrutura-mental-completa)
- [38. 🧠 Como memorizar](#38-como-memorizar)
  - [`fr`](#fr)
  - [`minmax()`](#minmax-1)
  - [`repeat()`](#repeat-1)
  - [`auto-fit`](#auto-fit-1)
  - [`auto-fill`](#auto-fill-1)
- [39. 📌 O padrão mais importante](#39-o-padrão-mais-importante)
- [40. Resumo final](#40-resumo-final)
  - [Regra mental principal](#regra-mental-principal)

---

## 1. `grid-template-columns`

A propriedade:

```css
grid-template-columns
````

define a estrutura das **colunas do Grid**.

### Exemplo

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px 100px;
}
```

Nesse caso, são definidas quatro colunas de `100px`:

```text
┌────────┬────────┬────────┬────────┐
│ 100px  │ 100px  │ 100px  │ 100px  │
└────────┴────────┴────────┴────────┘
```

### Regra importante

> Cada valor informado em `grid-template-columns` representa uma coluna.

---

## 2. `px` define o tamanho da coluna

Considere:

```css
grid-template-columns: 100px 100px 100px;
```

Temos:

```text
100px + 100px + 100px

        ↓

      300px
```

Esses valores definem o tamanho das colunas.

É importante não confundir a largura de uma coluna com o tamanho total do conteúdo colocado dentro dela.

---

## 3. Coluna e conteúdo do item são conceitos diferentes

Uma coluna pode ser definida como:

```css
grid-template-columns: 100px;
```

mesmo que o conteúdo do item seja maior.

Por exemplo:

```css
.grid {
  display: grid;
  grid-template-columns: 100px;
}
```

Se o conteúdo não conseguir se ajustar dentro de `100px`, ele poderá ultrapassar a largura da coluna.

```text
┌──────────────┐
│ coluna 100px │
│ conteúdo     │───────────────>
└──────────────┘
```

Portanto:

> **A coluna continua com `100px`; o conteúdo é que pode ultrapassar seus limites.**

---

## 4. Conteúdo maior que a coluna

Considere:

```css
.grid {
  display: grid;
  grid-template-columns: 100px;
}
```

Com um conteúdo como:

```text
Uma palavra muito grande
```

o navegador pode quebrar o texto quando existem oportunidades naturais de quebra, como espaços.

```text
┌──────────────┐
│ Uma palavra  │
│ muito        │
│ grande       │
└──────────────┘
```

Uma palavra única e muito grande pode não possuir um ponto natural de quebra.

Nesse caso:

```text
┌──────────────┐
│ superpalavraque│────────>
└──────────────┘
```

O conteúdo pode ultrapassar a largura da coluna.

---

## 5. `width` e a célula do Grid

É útil diferenciar os níveis envolvidos:

```text
Grid
 └── coluna
      └── célula
           └── item
                └── conteúdo
```

Quando definimos:

```css
grid-template-columns: 100px;
```

estamos definindo a estrutura da coluna.

O item continua tendo suas próprias propriedades, como:

```css
width
min-width
padding
margin
```

Essas propriedades podem influenciar o comportamento visual do item sem necessariamente alterar a definição da coluna.

---

## 6. `min-width` pode fazer o item ultrapassar a coluna

Suponha:

```css
.grid {
  display: grid;
  grid-template-columns: 100px;
}
```

e:

```css
.item {
  min-width: 200px;
}
```

Agora temos:

```text
coluna = 100px

item   = mínimo de 200px
```

O resultado pode ser entendido assim:

```text
┌──────────────┐
│              │
│    coluna    │
│    100px     │
│              │
└──────────────┘

┌────────────────────────┐
│      item 200px        │
└────────────────────────┘
```

O Grid mantém a coluna em `100px`, enquanto o item respeita seu `min-width`.

---

## 7. Unidade `fr`

A unidade:

```css
fr
```

representa uma **fração proporcional do espaço disponível**.

Por exemplo:

```css
grid-template-columns: 1fr 2fr;
```

significa:

```text
1 parte | 2 partes
```

O espaço é distribuído proporcionalmente:

```text
┌──────────┬────────────────────┐
│   1fr    │        2fr         │
└──────────┴────────────────────┘
```

A segunda coluna possui o dobro da proporção da primeira.

---

## 8. `fr` e o conteúdo

Uma faixa `fr` não deve ser interpretada apenas como uma porcentagem fixa do container.

Por exemplo:

```css
grid-template-columns: 1fr 2fr;
```

não deve ser mentalmente reduzido simplesmente a:

```text
33% | 66%
```

sem considerar outras restrições do layout.

Um conteúdo muito grande pode influenciar o tamanho mínimo necessário para uma faixa.

Isso significa que a distribuição visual pode não corresponder exatamente à proporção imaginada inicialmente.

### Regra mental

> **`fr` representa uma proporção flexível, mas o conteúdo e as restrições mínimas dos itens também podem influenciar o resultado final.**

---

## 9. `minmax()`

A função:

```css
minmax()
```

permite definir um **valor mínimo** e um **valor máximo** para uma faixa do Grid.

A estrutura é:

```css
minmax(mínimo, máximo)
```

Por exemplo:

```css
grid-template-columns: minmax(200px, 1fr);
```

significa:

```text
mínimo → 200px

máximo → 1fr
```

Ou seja:

> A coluna pode crescer, mas não deve ficar menor que `200px`.

---

## 10. Exemplo de `minmax()`

```css
.grid {
  display: grid;
  grid-template-columns:
    minmax(200px, 1fr) 1fr 1fr;
}
```

Temos três colunas:

```text
Coluna 1 → mínimo de 200px, máximo flexível

Coluna 2 → 1fr

Coluna 3 → 1fr
```

Quando o container diminui:

```text
espaço disponível ↓

        ↓

colunas flexíveis diminuem

        ↓

coluna com mínimo chega a 200px

        ↓

não pode diminuir mais
```

---

## 11. `minmax(100px, 1fr)`

Também podemos utilizar:

```css
grid-template-columns:
  minmax(100px, 1fr) 1fr 1fr;
```

Agora:

```text
mínimo → 100px

máximo → 1fr
```

A coluna pode diminuir até:

```text
100px
```

mas não abaixo disso.

---

## 12. `minmax(50px, 100px)`

O valor máximo também pode ser fixo:

```css
grid-template-columns:
  minmax(50px, 100px);
```

Agora temos:

```text
mínimo → 50px

máximo → 100px
```

Portanto:

```text
50px ≤ coluna ≤ 100px
```

Com espaço suficiente:

```text
coluna → 100px
```

À medida que o espaço diminui:

```text
100px
  ↓
90px
  ↓
70px
  ↓
50px
  ↓
não diminui mais
```

---

## 13. `minmax()` resolve um problema importante

Imagine:

```css
grid-template-columns: 200px 1fr 1fr;
```

Quando o espaço disponível diminui, a primeira coluna continua com:

```text
200px
```

Isso pode consumir uma quantidade significativa do espaço disponível.

Com:

```css
grid-template-columns:
  minmax(100px, 1fr) 1fr 1fr;
```

essa coluna pode diminuir até `100px`.

Isso torna a estrutura mais flexível.

---

## 14. `minmax()` e conteúdo grande

Considere:

```css
grid-template-columns:
  minmax(200px, 1fr) 1fr 1fr;
```

Se o conteúdo da primeira coluna for muito grande, o Grid ainda precisa lidar com as necessidades mínimas desse conteúdo.

A função `minmax()` limita a faixa conforme a definição realizada, mas o resultado final também pode ser influenciado pelas necessidades mínimas do conteúdo e por outras propriedades do layout.

Isso é especialmente importante quando trabalhamos com:

* palavras muito grandes;
* imagens;
* elementos com largura mínima;
* conteúdo que não pode ser reduzido.

---

## 15. `repeat()`

A função:

```css
repeat()
```

permite repetir uma mesma configuração de coluna várias vezes.

Sem `repeat()`:

```css
grid-template-columns:
  1fr 1fr 1fr 1fr;
```

Com `repeat()`:

```css
grid-template-columns:
  repeat(4, 1fr);
```

As duas declarações representam a mesma estrutura:

```text
1fr | 1fr | 1fr | 1fr
```

---

## 16. Sintaxe do `repeat()`

A estrutura é:

```css
repeat(quantidade, valor)
```

Exemplo:

```css
repeat(4, 100px)
```

significa:

```text
100px | 100px | 100px | 100px
```

Outro exemplo:

```css
repeat(3, 1fr)
```

significa:

```text
1fr | 1fr | 1fr
```

---

## 17. `repeat()` economiza código

Sem `repeat()`:

```css
grid-template-columns:
  1fr 1fr 1fr 1fr 1fr 1fr;
```

Com `repeat()`:

```css
grid-template-columns:
  repeat(6, 1fr);
```

A segunda forma é mais curta e facilita a manutenção quando a mesma configuração precisa ser repetida.

---

## 18. `repeat()` aceita diferentes valores

### Pixels

```css
grid-template-columns:
  repeat(4, 100px);
```

### Frações

```css
grid-template-columns:
  repeat(4, 1fr);
```

### Porcentagens

```css
grid-template-columns:
  repeat(4, 25%);
```

### Funções

Também é possível combinar `repeat()` com funções como:

```css
minmax()
```

---

## 19. `repeat()` também pode ser combinado com outras colunas

`repeat()` não precisa representar toda a declaração.

Por exemplo:

```css
grid-template-columns:
  3fr repeat(3, 1fr) 2fr;
```

Isso significa:

```text
3fr | 1fr | 1fr | 1fr | 2fr
```

O `repeat()` apenas repete a parte especificada.

---

## 20. `auto-fit`

Entre os valores usados com `repeat()` estão:

```css
auto-fit
```

Seu objetivo é permitir que a quantidade de colunas se adapte ao espaço disponível.

Por exemplo:

```css
grid-template-columns:
  repeat(auto-fit, 100px);
```

A ideia é:

> **Tente colocar a maior quantidade possível de colunas de `100px` dentro do espaço disponível.**

---

## 21. Como `auto-fit` se comporta

Considere um container com espaço suficiente para quatro colunas:

```text
┌─────┬─────┬─────┬─────┐
│ 100 │ 100 │ 100 │ 100 │
└─────┴─────┴─────┴─────┘
```

Ao aumentar o espaço e permitir mais uma coluna:

```text
┌─────┬─────┬─────┬─────┬─────┐
│ 100 │ 100 │ 100 │ 100 │ 100 │
└─────┴─────┴─────┴─────┴─────┘
```

Ao diminuir:

```text
┌─────┬─────┬─────┐
│ 100 │ 100 │ 100 │
└─────┴─────┴─────┘
```

A quantidade de colunas se adapta ao espaço disponível.

---

## 22. `auto-fit` com `1fr`

Podemos escrever:

```css
grid-template-columns:
  repeat(auto-fit, 1fr);
```

Nesse modelo, as colunas tentam se expandir para ocupar o espaço disponível.

A ideia pode ser separada em duas partes:

```text
auto-fit

→ quantas colunas cabem?

1fr

→ como distribuir o espaço entre elas?
```

Isso produz uma estrutura bastante flexível.

---

## 23. `auto-fit` com um tamanho fixo

Também podemos utilizar:

```css
grid-template-columns:
  repeat(auto-fit, 200px);
```

Agora cada coluna possui `200px` de largura.

O `auto-fit` determina quantas delas podem caber.

```text
Espaço grande

→ várias colunas de 200px

Espaço menor

→ menos colunas de 200px
```

---

## 24. O padrão mais útil: `auto-fit` + `minmax()`

Uma combinação importante é:

```css
grid-template-columns:
  repeat(auto-fit, minmax(100px, 1fr));
```

A declaração pode ser lida em partes:

```text
repeat()

→ repita

auto-fit

→ crie quantas colunas couberem

minmax(100px, 1fr)

→ cada coluna possui no mínimo 100px

→ e pode crescer de forma flexível
```

Essa combinação é muito útil para criar grades responsivas com poucas regras.

---

## 25. Como funciona `repeat(auto-fit, minmax())`

Considere:

```css
.grid {
  display: grid;
  grid-template-columns:
    repeat(auto-fit, minmax(100px, 1fr));
}
```

Em um container grande:

```text
┌────────┬────────┬────────┬────────┐
│   A    │   B    │   C    │   D    │
└────────┴────────┴────────┴────────┘
```

Ao diminuir:

```text
┌────────┬────────┬────────┐
│   A    │   B    │   C    │
├────────┼────────┼────────┤
│   D    │   E    │   F    │
└────────┴────────┴────────┘
```

Diminuindo novamente:

```text
┌────────┬────────┐
│   A    │   B    │
├────────┼────────┤
│   C    │   D    │
└────────┴────────┘
```

A quantidade de colunas se ajusta de acordo com o espaço disponível e as restrições definidas.

---

## 26. Por que `minmax()` ajuda o `auto-fit`?

Sem um limite mínimo, uma coluna pode continuar ficando pequena conforme o espaço disponível diminui.

Com:

```css
minmax(100px, 1fr)
```

estabelecemos:

```text
"Cada coluna deve ter pelo menos 100px."
```

Quando não houver espaço suficiente para manter mais colunas:

```text
coluna 1 → 100px

coluna 2 → 100px

coluna 3 → 100px
```

o Grid pode reorganizar os itens em novas linhas.

---

## 27. `auto` como valor máximo

Também é possível encontrar:

```css
minmax(100px, auto)
```

Nesse caso:

```text
mínimo → 100px

máximo → auto
```

O valor `auto` permite que o tamanho máximo seja determinado automaticamente.

O tamanho necessário do conteúdo pode influenciar esse comportamento.

Por exemplo, uma coluna pode ficar maior para acomodar seu conteúdo.

---

## 28. `auto-fit` x `auto-fill`

Além de:

```css
auto-fit
```

existe:

```css
auto-fill
```

Os dois são usados em combinação com a criação automática de colunas, mas existe uma diferença importante quando há espaço para mais colunas do que elementos disponíveis.

---

## 29. `auto-fit`

Considere:

```css
repeat(auto-fit, minmax(100px, 1fr))
```

O `auto-fit` tenta ajustar as colunas às necessidades dos itens existentes.

Quando todos os itens já foram acomodados, o espaço adicional tende a ser utilizado para expandir as colunas existentes.

Visualmente:

```text
┌──────────┬──────────┬──────────┐
│    A     │    B     │    C     │
└──────────┴──────────┴──────────┘
```

Ao aumentar o container:

```text
┌────────────┬────────────┬────────────┐
│     A      │     B      │     C      │
└────────────┴────────────┴────────────┘
```

Os itens podem crescer.

---

## 30. `auto-fill`

Considere:

```css
repeat(auto-fill, minmax(100px, 1fr))
```

O `auto-fill` pode criar tantas faixas quanto couberem, mesmo quando algumas delas não possuem itens.

Por exemplo:

```text
┌────────┬────────┬────────┬────────┬────────┐
│   A    │   B    │   C    │        │        │
└────────┴────────┴────────┴────────┴────────┘
```

As últimas colunas podem estar vazias, mas continuam fazendo parte da estrutura de faixas criada pelo Grid.

---

## 31. Diferença mental entre `auto-fit` e `auto-fill`

Uma forma simples de memorizar:

```text
auto-fit

→ ajuste as colunas aos itens existentes
```

```text
auto-fill

→ preencha o espaço criando todas as colunas possíveis
```

### Visualmente

```text
AUTO-FIT

[A] [B] [C]

   ↑

colunas existentes se expandem
```

```text
AUTO-FILL

[A] [B] [C] [ ] [ ]

             ↑

      colunas podem existir
      mesmo sem conteúdo
```

---

## 32. O que acontece quando o container aumenta?

Considere:

```css
grid-template-columns:
  repeat(auto-fit, minmax(100px, 1fr));
```

Quando o container é pequeno:

```text
container pequeno

→ 2 colunas
```

Aumentando:

```text
container maior

→ 3 colunas
```

Aumentando novamente:

```text
container ainda maior

→ mais colunas ou expansão das existentes
```

O comportamento depende do espaço disponível, da quantidade de itens e das restrições definidas.

---

## 33. Layout responsivo sem várias Media Queries

A combinação:

```css
grid-template-columns:
  repeat(auto-fit, minmax(100px, 1fr));
```

é especialmente útil para estruturas responsivas.

Em vez de definir manualmente diferentes quantidades de colunas com várias Media Queries:

```css
@media (...) {
  /* 4 colunas */
}

@media (...) {
  /* 3 colunas */
}

@media (...) {
  /* 2 colunas */
}

@media (...) {
  /* 1 coluna */
}
```

podemos permitir que o próprio Grid adapte a quantidade de colunas ao espaço disponível.

Isso não significa que Media Queries deixaram de ser necessárias. Significa apenas que determinados layouts podem precisar de menos regras explícitas.

---

## 34. Padrão de Grid responsivo

Um padrão bastante útil é:

```css
.grid {
  display: grid;
  grid-template-columns:
    repeat(auto-fit, minmax(100px, 1fr));
  gap: 20px;
}
```

Podemos interpretar assim:

```text
display: grid

→ ativa o Grid

repeat()

→ repete a definição

auto-fit

→ ajusta a quantidade de colunas

minmax(100px, 1fr)

→ mínimo de 100px

→ máximo flexível

gap

→ cria espaçamento
```

---

## 35. Exemplo completo

### HTML

```html
<section class="products">
  <article>Produto 1</article>
  <article>Produto 2</article>
  <article>Produto 3</article>
  <article>Produto 4</article>
  <article>Produto 5</article>
  <article>Produto 6</article>
</section>
```

### CSS

```css
.products {
  display: grid;
  grid-template-columns:
    repeat(auto-fit, minmax(150px, 1fr));
  gap: 20px;
}
```

O resultado se adapta ao espaço disponível.

### Container maior

```text
┌────────┬────────┬────────┬────────┐
│   1    │   2    │   3    │   4    │
├────────┼────────┼────────┼────────┤
│   5    │   6    │        │        │
└────────┴────────┴────────┴────────┘
```

### Container menor

```text
┌────────┬────────┬────────┐
│   1    │   2    │   3    │
├────────┼────────┼────────┤
│   4    │   5    │   6    │
└────────┴────────┴────────┘
```

### Container ainda menor

```text
┌────────┬────────┐
│   1    │   2    │
├────────┼────────┤
│   3    │   4    │
├────────┼────────┤
│   5    │   6    │
└────────┴────────┘
```

---

## 36. Combinações importantes

### Colunas fixas

```css
grid-template-columns: 100px 100px 100px;
```

Use quando quiser definir tamanhos específicos para as colunas.

### Colunas proporcionais

```css
grid-template-columns: 1fr 1fr 1fr;
```

Use quando quiser distribuir o espaço proporcionalmente.

### Repetição

```css
grid-template-columns: repeat(4, 1fr);
```

Use quando a mesma configuração se repete.

### Limite mínimo e máximo

```css
grid-template-columns:
  minmax(100px, 1fr) 1fr 1fr;
```

Use quando uma coluna precisa respeitar determinados limites de tamanho.

### Grid responsivo

```css
grid-template-columns:
  repeat(auto-fit, minmax(150px, 1fr));
```

Use quando quiser que a quantidade de colunas se adapte ao espaço disponível.

---

## 37. Estrutura mental completa

```text
grid-template-columns

        │

        ├── valores fixos
        │   └── 100px 100px

        ├── valores proporcionais
        │   └── 1fr 2fr

        ├── repeat()
        │   └── repeat(4, 1fr)

        ├── minmax()
        │   └── minmax(100px, 1fr)

        └── auto-fit / auto-fill
            └── criação dinâmica de faixas
```

---

## 38. 🧠 Como memorizar

### `fr`

> **Parte proporcional do espaço.**

```css
1fr 2fr
```

```text
1 parte : 2 partes
```

---

### `minmax()`

> **Define mínimo e máximo.**

```css
minmax(100px, 1fr)
```

```text
mínimo → 100px

máximo → 1fr
```

---

### `repeat()`

> **Repete uma configuração.**

```css
repeat(4, 1fr)
```

```text
1fr 1fr 1fr 1fr
```

---

### `auto-fit`

> **Tenta ajustar a quantidade de colunas aos itens e ao espaço disponível.**

```css
repeat(auto-fit, ...)
```

---

### `auto-fill`

> **Tenta preencher o espaço com o máximo de faixas possível, mesmo que algumas possam ficar vazias.**

```css
repeat(auto-fill, ...)
```

---

## 39. 📌 O padrão mais importante

Uma das combinações mais úteis para layouts responsivos é:

```css
.container {
  display: grid;
  grid-template-columns:
    repeat(auto-fit, minmax(150px, 1fr));
  gap: 20px;
}
```

A leitura dessa declaração pode ser feita assim:

```text
Grid

 ↓

repita automaticamente

 ↓

quantas colunas couberem

 ↓

cada uma deve ter pelo menos 150px

 ↓

e pode crescer proporcionalmente

 ↓

com 20px de espaçamento
```

Esse padrão permite construir uma grade que se adapta ao espaço disponível sem determinar manualmente uma quantidade fixa de colunas para cada largura.

---

## 40. Resumo final

```text
grid-template-columns

        ↓

define as colunas

        │

        ├── px
        │   → tamanho fixo
        │
        ├── %
        │   → proporção relativa
        │
        ├── fr
        │   → fração proporcional do espaço
        │
        ├── minmax()
        │   → define mínimo e máximo
        │
        ├── repeat()
        │   → repete uma configuração
        │
        ├── auto-fit
        │   → ajusta as colunas aos itens e ao espaço
        │
        └── auto-fill
            → cria todas as faixas possíveis
```

### Regra mental principal

> **`grid-template-columns` define as colunas. `fr` define proporções. `minmax()` define limites. `repeat()` evita repetição de código. `auto-fit` e `auto-fill` permitem criar estruturas de colunas dinâmicas.**