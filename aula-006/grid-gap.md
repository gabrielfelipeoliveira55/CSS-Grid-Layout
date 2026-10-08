# CSS Grid — `gap`, `row-gap` e `column-gap`

## 1. O que é `gap`?

A propriedade:

```css
gap
```

define o **espaçamento entre os itens de um Grid**.

Esse espaço também pode ser chamado de **gutter**.

Exemplo:

```css
.grid {
  display: grid;
  gap: 20px;
}
```

Visualmente:

```text
┌──────────┐   20px   ┌──────────┐
│  Item 1  │          │  Item 2  │
└──────────┘          └──────────┘

┌──────────┐   20px   ┌──────────┐
│  Item 3  │          │  Item 4  │
└──────────┘          └──────────┘
```

A ideia principal é:

> **`gap` cria espaço entre as células/itens do Grid.**

---

# 2. `grid-gap` x `gap`

Em códigos mais antigos, é possível encontrar:

```css
grid-gap: 20px;
```

A forma moderna é:

```css
gap: 20px;
```

Portanto:

```text
grid-gap
   ↓
nome antigo

gap
   ↓
nome atual
```

`grid-gap` continua sendo reconhecido por navegadores por questões de compatibilidade com códigos existentes.

### Para escrever código novo

Prefira:

```css
gap: 20px;
```

---

# 3. Por que `gap` substituiu `grid-gap`?

A ideia foi utilizar uma propriedade mais geral:

```css
gap
```

que não fica limitada ao Grid.

Ela também pode ser utilizada em outros contextos de layout, como o Flexbox.

Para os estudos de CSS Grid, o mais importante é reconhecer:

```css
grid-gap: 20px;
```

e entender que a forma moderna é:

```css
gap: 20px;
```

---

# 4. `gap` pertence ao container

O `gap` deve ser definido no **Grid Container**:

```css
.grid {
  display: grid;
  gap: 20px;
}
```

Não nos elementos individuais:

```css
.item {
  /* gap não representa o espaçamento do próprio item */
}
```

A estrutura mental é:

```text
Grid Container
      │
      ↓
     gap
      │
      ↓
espaço entre os Grid Items
```

---

# 5. Sem `gap`

Considere:

```css
.grid {
  display: grid;
}
```

Os itens podem ficar encostados:

```text
┌──────────┬──────────┐
│  Item 1  │  Item 2  │
├──────────┼──────────┤
│  Item 3  │  Item 4  │
└──────────┴──────────┘
```

---

# 6. Com `gap`

Agora:

```css
.grid {
  display: grid;
  gap: 20px;
}
```

Temos:

```text
┌──────────┐   20px   ┌──────────┐
│  Item 1  │          │  Item 2  │
└──────────┘          └──────────┘

┌──────────┐   20px   ┌──────────┐
│  Item 3  │          │  Item 4  │
└──────────┘          └──────────┘
```

---

# 7. `gap` é diferente de `margin`

Também podemos criar espaço utilizando:

```css
.item {
  margin: 5px;
}
```

Isso pode produzir uma aparência semelhante:

```text
[ Item ]   espaço   [ Item ]
```

Mas o mecanismo é diferente.

### `gap`

```text
Grid Container
      ↓
     gap
      ↓
espaço entre os itens
```

### `margin`

```text
Item
 ↓
margin
 ↓
espaço ao redor do próprio item
```

### Regra mental

> **`gap` pertence à estrutura do Grid; `margin` pertence ao elemento.**

---

# 8. `gap` + `margin`

Podemos utilizar ambos ao mesmo tempo:

```css
.grid {
  gap: 20px;
}

.item {
  margin: 5px;
}
```

Nesse caso, os espaços podem se somar.

Podemos visualizar:

```text
[ Item ]
   ↑
  5px
   ↑
  gap
 20px
   ↓
  5px
   ↓
[ Item ]
```

Ou seja, não devemos pensar que `gap` substitui automaticamente toda e qualquer margem.

São mecanismos diferentes.

---

# 9. `gap` é interno ao Grid

Uma característica importante é que:

```css
gap: 20px;
```

atua **entre os itens**, não necessariamente nas bordas externas do container.

Exemplo:

```text
┌───────────────────────────────────────┐
│                                       │
│  ┌─────────┐   20px   ┌─────────┐    │
│  │  Item   │          │  Item   │    │
│  └─────────┘          └─────────┘    │
│                                       │
└───────────────────────────────────────┘
```

O espaço:

```text
Item → 20px → Item
```

é responsabilidade do `gap`.

Já o espaço:

```text
borda → Item
```

pode ser controlado por:

```css
padding
```

ou:

```css
margin
```

---

# 10. `padding` no Grid Container

Podemos fazer:

```css
.grid {
  display: grid;
  gap: 20px;
  padding: 20px;
}
```

Agora temos:

```text
┌────────────────────────────────────────┐
│  padding                               │
│   ┌─────────┐   20px   ┌─────────┐    │
│   │  Item   │          │  Item   │    │
│   └─────────┘          └─────────┘    │
│                                        │
└────────────────────────────────────────┘
```

Podemos separar mentalmente:

```text
padding
→ borda do container até o conteúdo

gap
→ um item até outro item
```

---

# 11. `margin` no container

Também podemos utilizar:

```css
.grid {
  margin: 20px;
}
```

Nesse caso, a margem atua **fora do Grid Container**.

```text
espaço externo
      ↓
   margin
      ↓
┌─────────────────────────────┐
│       Grid Container        │
│                             │
└─────────────────────────────┘
```

Portanto:

```text
margin
→ externo ao elemento

padding
→ interno ao elemento

gap
→ entre os itens
```

---

# 12. `row-gap`

Podemos controlar especificamente o espaçamento entre as **linhas**.

```css
.grid {
  row-gap: 20px;
}
```

Exemplo:

```text
┌──────────┬──────────┐
│  Item 1  │  Item 2  │
└──────────┴──────────┘

          20px

┌──────────┬──────────┐
│  Item 3  │  Item 4  │
└──────────┴──────────┘
```

Portanto:

```text
row-gap
   ↓
espaço vertical entre as linhas
```

---

# 13. `column-gap`

Podemos controlar especificamente o espaçamento entre as **colunas**:

```css
.grid {
  column-gap: 20px;
}
```

Visualmente:

```text
┌──────────┐   20px   ┌──────────┐
│  Coluna 1│          │ Coluna 2 │
└──────────┘          └──────────┘
```

Portanto:

```text
column-gap
    ↓
espaço horizontal entre as colunas
```

---

# 14. `row-gap` x `column-gap`

```text
row-gap
   ↓
entre linhas
```

```text
column-gap
   ↓
entre colunas
```

Visualmente:

```text
             COLUMN GAP
                ↓
┌────────────┐       ┌────────────┐
│            │       │            │
│   Item     │       │   Item     │
│            │       │            │
└────────────┘       └────────────┘
       ↑
       │
    ROW GAP
       │
       ↓
┌────────────┐       ┌────────────┐
│            │       │            │
│   Item     │       │   Item     │
│            │       │            │
└────────────┘       └────────────┘
```

---

# 15. `gap` com um único valor

Podemos escrever:

```css
gap: 20px;
```

Nesse caso, o valor é aplicado nas duas direções:

```text
row-gap
   ↓
20px

column-gap
   ↓
20px
```

Ou seja:

```css
gap: 20px;
```

equivale conceitualmente a:

```css
row-gap: 20px;
column-gap: 20px;
```

---

# 16. `gap` com dois valores

Também podemos escrever:

```css
gap: 10px 20px;
```

A ordem é:

```text
gap: row-gap column-gap;
```

Portanto:

```css
gap: 10px 20px;
```

significa:

```css
row-gap: 10px;
column-gap: 20px;
```

### Regra para memorizar

> **Primeiro valor = linhas. Segundo valor = colunas.**

---

# 17. Exemplo de `gap` com dois valores

```css
.grid {
  display: grid;
  gap: 10px 30px;
}
```

Temos:

```text
entre linhas:
10px

entre colunas:
30px
```

Visualmente:

```text
┌─────────┐      30px      ┌─────────┐
│         │                 │         │
└─────────┘                 └─────────┘
       ↕
      10px
       ↕
┌─────────┐      30px      ┌─────────┐
│         │                 │         │
└─────────┘                 └─────────┘
```

---

# 18. `column-gap` individual

Também podemos escrever diretamente:

```css
.grid {
  column-gap: 20px;
}
```

Isso interfere somente na distância entre as colunas.

Exemplo:

```text
┌───────┐     20px     ┌───────┐
│       │              │       │
└───────┘              └───────┘
```

O espaçamento entre linhas continua sendo definido separadamente.

---

# 19. `row-gap` individual

Da mesma forma:

```css
.grid {
  row-gap: 10px;
}
```

Controla somente a distância entre as linhas.

```text
┌────────┐
│        │
└────────┘

   10px

┌────────┐
│        │
└────────┘
```

---

# 20. `gap` não significa "20px para cada lado"

Um erro comum é interpretar:

```css
column-gap: 20px;
```

como:

```text
20px do item 1
+
20px do item 2
=
40px
```

Não é assim.

O `column-gap` representa o **espaço total entre as duas colunas**.

```text
┌──────────┐
│ Coluna 1 │
└──────────┘
      ← 20px →
┌──────────┐
│ Coluna 2 │
└──────────┘
```

O valor é:

```text
20px
```

não:

```text
40px
```

---

# 21. Diferença entre `gap` e margem nos itens

Considere:

```css
.grid {
  column-gap: 20px;
}

.item {
  margin-left: 10px;
  margin-right: 10px;
}
```

Agora existem três mecanismos:

```text
margem do item 1
      +
gap
      +
margem do item 2
```

Isso pode produzir um espaço visual maior.

Portanto, quando o objetivo é simplesmente controlar a distância entre os itens, usar:

```css
gap
```

costuma ser mais simples.

---

# 22. O `gap` não altera a margem do item

Se temos:

```css
.grid {
  gap: 20px;
}

.item {
  margin: 10px;
}
```

o `gap` continua sendo:

```text
20px
```

e a margem continua sendo:

```text
10px
```

Eles não se transformam em uma única propriedade.

São duas regras diferentes.

---

# 23. Margem pode reduzir o espaço disponível dentro da célula

Suponha:

```css
.grid {
  display: grid;
  gap: 20px;
}

.item {
  margin-top: 50px;
}
```

A célula do Grid continua existindo.

O que muda é o espaço disponível para o próprio item.

Podemos imaginar:

```text
┌───────────────────────┐
│       CÉLULA          │
│                       │
│      margin-top       │
│        50px            │
│         ↓             │
│      ┌───────┐        │
│      │ item  │        │
│      └───────┘        │
└───────────────────────┘
```

O `gap` continua existindo entre as células.

---

# 24. Margens grandes podem fazer o item desaparecer

Se colocarmos:

```css
.item {
  margin-right: 50px;
}
```

e o espaço disponível ficar pequeno demais, o conteúdo do item pode deixar de aparecer adequadamente.

Isso não significa que a célula foi removida.

A célula continua fazendo parte da estrutura do Grid.

Podemos pensar:

```text
Grid Cell
│
├── espaço da margem
│
└── espaço restante para o conteúdo
```

Se a margem consumir praticamente todo o espaço:

```text
Grid Cell
│
├── margem enorme
│
└── quase nenhum espaço para conteúdo
```

---

# 25. A margem não modifica as outras células

Esse conceito é importante.

Se um item possui:

```css
margin-right: 50px;
```

a margem não faz automaticamente os outros itens do Grid se moverem para compensá-la.

O que está sendo reduzido é o espaço disponível **dentro daquela própria célula/item**.

Visualmente:

```text
┌──────────────┐ ┌──────────────┐
│     Item     │ │    Item      │
│   ← margem   │ │              │
└──────────────┘ └──────────────┘
```

A estrutura do Grid continua:

```text
Coluna 1 | Coluna 2
```

---

# 26. Mapa mental — `gap`

```text
                      GAP
                       │
            ┌──────────┴──────────┐
            ↓                     ↓
         row-gap              column-gap
            │                     │
            ↓                     ↓
      entre linhas          entre colunas
            │                     │
            └──────────┬──────────┘
                       ↓
               espaço entre
                  os itens
```

---

# 27. Mapa mental — sintaxe

```text
                    gap
                     │
        ┌────────────┴────────────┐
        ↓                         ↓
  1 valor                    2 valores
        │                         │
        ↓                         ↓
gap: 20px;                gap: 10px 20px;
        │                         │
        ↓                  ┌──────┴──────┐
   linhas = 20px           ↓             ↓
   colunas = 20px       row-gap       column-gap
                          10px           20px
```

### Corte mental ①

```text
gap: 20px
→ tudo 20px

gap: 10px 20px
→ linhas 10px
→ colunas 20px
```

---

# 28. Mapa mental — espaçamento

```text
                    ESPAÇAMENTO
                         │
          ┌──────────────┼──────────────┐
          ↓              ↓              ↓
       margin          padding          gap
          │              │              │
          ↓              ↓              ↓
      ao redor       dentro do       entre os
      do item        container       itens
```

Uma maneira muito simples de lembrar:

```text
margin
→ FORA

padding
→ DENTRO

gap
→ ENTRE
```

---

# 29. Mapa mental — Grid Container

```text
                 GRID CONTAINER
                       │
          ┌────────────┼────────────┐
          ↓            ↓            ↓
     columns       rows          padding
          │            │
          ↓            ↓
    column-gap      row-gap
          │            │
          └──────┬─────┘
                 ↓
                gap
                 ↓
        espaço entre células
```

---

# 30. Exemplo completo

```html
<div class="grid">
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>
  <div>Item 4</div>
</div>
```

```css
.grid {
  display: grid;

  grid-template-columns:
    1fr 1fr;

  gap: 20px;
}
```

Resultado:

```text
┌──────────────┐   20px   ┌──────────────┐
│    Item 1    │           │    Item 2    │
└──────────────┘           └──────────────┘

┌──────────────┐   20px   ┌──────────────┐
│    Item 3    │           │    Item 4    │
└──────────────┘           └──────────────┘
```

---

# 31. Exemplo com linhas e colunas diferentes

```css
.grid {
  display: grid;

  grid-template-columns:
    1fr 1fr 1fr;

  gap:
    10px 30px;
}
```

Interpretando:

```text
LINHAS
10px

COLUNAS
30px
```

Visualmente:

```text
┌───────┐      30px      ┌───────┐      30px      ┌───────┐
│ Item  │                 │ Item  │                 │ Item  │
└───────┘                 └───────┘                 └───────┘

                         10px

┌───────┐      30px      ┌───────┐      30px      ┌───────┐
│ Item  │                 │ Item  │                 │ Item  │
└───────┘                 └───────┘                 └───────┘
```

---

# 32. Unidades

O `gap` pode receber diferentes unidades.

Exemplo:

```css
gap: 20px;
```

ou:

```css
gap: 1em;
```

ou:

```css
gap: 2rem;
```

A escolha depende do comportamento desejado.

Para muitos layouts, um valor em `px` é bastante comum:

```css
gap: 20px;
```

---

# 33. `gap` em Grid

A ideia geral:

```css
.grid {
  display: grid;
  gap: 20px;
}
```

pode ser resumida como:

```text
Grid
 ↓
cria células
 ↓
gap
 ↓
separa as células
```

Não precisamos adicionar margem individualmente a cada item para obter o espaçamento entre eles.

---

# 34. Quando usar `gap`

Use `gap` quando o objetivo for:

```text
separar cards
separar colunas
separar linhas
criar espaçamento uniforme entre elementos
```

Exemplo:

```css
.cards {
  display: grid;
  gap: 20px;
}
```

---

# 35. Quando usar `margin`

Use `margin` quando o espaçamento fizer parte da relação do próprio elemento com outros elementos ou com o fluxo externo.

Exemplo:

```css
.title {
  margin-bottom: 30px;
}
```

Aqui não estamos dizendo:

> "Dê 30px entre todas as células do Grid."

Estamos dizendo:

> "Este elemento possui uma margem inferior de 30px."

---

# 36. Quando usar `padding`

Use `padding` quando queremos espaço **dentro do próprio container ou elemento**.

Exemplo:

```css
.card {
  padding: 20px;
}
```

Isso cria:

```text
┌─────────────────────────┐
│   padding               │
│   ┌─────────────────┐   │
│   │    conteúdo     │   │
│   └─────────────────┘   │
└─────────────────────────┘
```

---

# 37. Comparação definitiva

```text
┌─────────────────────────────────────┐
│              margin                 │
│  ┌───────────────────────────────┐  │
│  │           padding             │  │
│  │   ┌───────────────────────┐   │  │
│  │   │       conteúdo        │   │  │
│  │   └───────────────────────┘   │  │
│  └───────────────────────────────┘  │
└─────────────────────────────────────┘
```

Entre dois itens do Grid:

```text
[ Item A ] ←── gap ──→ [ Item B ]
```

Portanto:

```text
margin
→ ao redor do elemento

padding
→ entre conteúdo e borda

gap
→ entre itens do layout
```

---

# 38. ⚠️ Não confunda `gap` com `margin`

Se você possui:

```css
.grid {
  gap: 20px;
}
```

não significa:

```text
Item 1
margin: 20px

Item 2
margin: 20px
```

O `gap` é uma propriedade do **container**.

Já:

```css
.item {
  margin: 20px;
}
```

é uma propriedade do **item**.

Essa diferença é fundamental.

---

# 39. 📌 Resumo final

```text
                       GAP
                        │
        ┌───────────────┼───────────────┐
        ↓               ↓               ↓
      gap            row-gap        column-gap
        │               │               │
        ↓               ↓               ↓
   linhas +          linhas          colunas
   colunas
```

### `gap`

```css
gap: 20px;
```

```text
20px entre linhas
20px entre colunas
```

### `row-gap`

```css
row-gap: 20px;
```

```text
20px entre linhas
```

### `column-gap`

```css
column-gap: 20px;
```

```text
20px entre colunas
```

### Dois valores

```css
gap: 10px 20px;
```

```text
10px → row-gap
20px → column-gap
```

---

# 40. 🧠 Regra mental definitiva

```text
MARGIN
↓
ao redor do elemento

PADDING
↓
dentro do elemento/container

GAP
↓
entre os itens
```

E:

```text
row-gap
↓
entre LINHAS

column-gap
↓
entre COLUNAS
```

### Para memorizar

> **`gap` é o espaçamento estrutural entre os itens do layout. `row-gap` controla a distância entre linhas e `column-gap` controla a distância entre colunas.**

> **`grid-gap` é a nomenclatura antiga que você pode encontrar em códigos existentes; para código atual, prefira `gap`.**
