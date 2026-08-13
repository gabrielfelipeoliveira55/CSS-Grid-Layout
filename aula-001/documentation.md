# CSS Grid Layout — Introdução e `display: grid`

## 1. O que é CSS Grid?

O **CSS Grid Layout** é um sistema de layout baseado na organização de elementos em **linhas e colunas**.

Assim como no Flexbox, existe a ideia de um **container** e de **itens**.

No Grid temos:

```text
Grid Container
      │
      ├── Grid Item
      ├── Grid Item
      ├── Grid Item
      └── Grid Item
```

---

# 2. Grid Container

O **Grid Container** é o elemento que recebe:

```css
.container {
  display: grid;
}
```

A partir desse momento, ele passa a ser um **container Grid**.

Exemplo:

```html
<section class="grid">
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>
</section>
```

```css
.grid {
  display: grid;
}
```

Nesse caso:

```text
section.grid
     ↓
Grid Container
     │
     ├── div → Grid Item
     ├── div → Grid Item
     └── div → Grid Item
```

---

# 3. Quem são os Grid Items?

Os **Grid Items** são os **filhos diretos** do Grid Container.

Por exemplo:

```html
<div class="grid">
  <div>Item 1</div>
  <div>Item 2</div>

  <section>
    <div>Item 3</div>
  </section>
</div>
```

Temos:

```text
.grid
 │
 ├── div
 │    └── Grid Item
 │
 ├── div
 │    └── Grid Item
 │
 └── section
      └── Grid Item
           │
           └── div → NÃO é Grid Item do .grid
```

O `div` que está dentro do `section` é um **filho do filho**, portanto não é um Grid Item direto do `.grid`.

### Regra importante

> **Somente os filhos diretos do Grid Container participam diretamente do Grid.**

---

# 4. `display: grid`

A propriedade principal para ativar o CSS Grid é:

```css
.container {
  display: grid;
}
```

Ela transforma o elemento em um **Grid Container**.

Sem essa declaração:

```css
.container {
  /* sem display: grid */
}
```

o elemento continua seguindo o comportamento definido pelo seu `display` normal.

---

# 5. `display: inline-grid`

Também existe:

```css
.container {
  display: inline-grid;
}
```

A diferença principal é que o elemento terá características de Grid, mas também se comportará como um elemento **inline-level** em relação ao seu posicionamento externo.

Por exemplo, dois elementos `inline-grid` podem aparecer lado a lado:

```text
[ Grid ] [ Grid ]
```

Enquanto:

```css
display: grid;
```

cria um elemento Grid com comportamento de bloco no fluxo externo.

### Uso

Na prática, o mais comum será:

```css
display: grid;
```

---

# 6. `subgrid`

Também existe o valor:

```css
display: subgrid;
```

A ideia apresentada na aula é utilizar um **Grid dentro de outro Grid**, fazendo com que um elemento filho possa participar da estrutura do Grid do elemento pai.

A ideia pode ser representada assim:

```text
Grid principal
│
├── Item
├── Item
└── Item
     │
     └── Subgrid
         ├── Item
         ├── Item
         └── Item
```

O ponto importante da aula é que `subgrid` foi apresentado como um recurso relacionado a grids aninhados, mas a demonstração tratou seu suporte como limitado naquele contexto.

> **Para os estudos atuais, é importante separar a ideia de `subgrid` da simples criação de um Grid dentro de outro.** Um elemento pode ser simultaneamente um Grid Item do pai e um Grid Container dos seus próprios filhos usando `display: grid`.

---

# 7. `display: grid` sozinho não define as colunas

Um detalhe muito importante:

```css
.container {
  display: grid;
}
```

não significa automaticamente:

```text
"Crie várias colunas."
```

Ao usar somente:

```css
display: grid;
```

sem definir uma estrutura de colunas ou outras regras de posicionamento, os elementos podem continuar aparecendo um embaixo do outro.

Exemplo:

```text
[Item 1]
[Item 2]
[Item 3]
[Item 4]
```

Isso pode parecer semelhante ao comportamento padrão de elementos de bloco.

---

# 8. Diferença entre `display: flex` e `display: grid`

Essa é uma diferença importante em relação ao Flexbox.

Quando fazemos:

```css
.container {
  display: flex;
}
```

o Flexbox já altera imediatamente a disposição dos itens.

Por padrão, temos uma direção principal:

```text
[Item 1] [Item 2] [Item 3]
```

No Grid:

```css
.container {
  display: grid;
}
```

ainda precisamos definir **como a grade será estruturada**.

Por exemplo:

```css
grid-template-columns: 200px 200px;
```

---

# 9. `grid-template-columns`

A propriedade:

```css
grid-template-columns
```

é utilizada para definir as **colunas do Grid Container**.

Exemplo:

```css
.container {
  display: grid;
  grid-template-columns: 200px 200px;
}
```

Isso cria:

```text
┌──────────┬──────────┐
│  200px   │  200px   │
├──────────┼──────────┤
│  Item    │  Item    │
└──────────┴──────────┘
```

Temos duas colunas:

```text
Coluna 1 → 200px
Coluna 2 → 200px
```

---

# 10. Removendo `grid-template-columns`

Se removermos:

```css
grid-template-columns: 200px 200px;
```

não teremos mais duas colunas explícitas.

Os itens podem voltar a ser distribuídos em uma única coluna:

```text
[Item 1]
[Item 2]
[Item 3]
[Item 4]
```

Isso reforça a ideia:

> **`display: grid` ativa o sistema Grid, mas `grid-template-columns` define a estrutura das colunas.**

---

# 11. Criando três colunas

Podemos escrever:

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
}
```

Agora teremos três colunas:

```text
┌────────┬────────┬────────┐
│ 100px  │ 100px  │ 100px  │
├────────┼────────┼────────┤
│ Item 1 │ Item 2 │ Item 3 │
└────────┴────────┴────────┘
```

Se o container possuir uma largura maior que `300px`, poderá sobrar espaço:

```text
┌────────┬────────┬────────┬───────────────┐
│ 100px  │ 100px  │ 100px  │ espaço vazio  │
└────────┴────────┴────────┴───────────────┘
```

---

# 12. `grid-template-columns` aceita diferentes unidades

As colunas não precisam ser definidas apenas em pixels.

Podemos utilizar:

```css
grid-template-columns: 25% 25%;
```

ou:

```css
grid-template-columns: 50% 50%;
```

ou ainda:

```css
grid-template-columns: 150px 200px;
```

Cada valor representa o tamanho da respectiva coluna.

---

# 13. Usando porcentagem

Imagine:

```css
.container {
  display: grid;
  grid-template-columns: 25% 25%;
}
```

Temos:

```text
25% + 25% = 50%
```

Portanto, duas colunas ocuparão metade da largura disponível:

```text
┌──────────────┬──────────────┬──────────────────────┐
│     25%      │      25%     │   espaço restante    │
└──────────────┴──────────────┴──────────────────────┘
```

Se fizermos:

```css
grid-template-columns: 50% 50%;
```

teremos:

```text
┌──────────────────────┬──────────────────────┐
│         50%          │         50%          │
└──────────────────────┴──────────────────────┘
```

---

# 14. Valores diferentes para as colunas

Também podemos misturar tamanhos:

```css
.container {
  display: grid;
  grid-template-columns: 150px 200px;
}
```

A primeira coluna terá:

```text
150px
```

e a segunda:

```text
200px
```

O Grid respeita os tamanhos definidos.

Se a soma dos tamanhos ultrapassar a largura do container, o conteúdo pode ultrapassar os limites disponíveis.

---

# 15. Uma dica sobre os nomes das propriedades

Muitas propriedades do Grid possuem `grid` no nome.

Por exemplo:

```css
grid-template-columns
grid-template-rows
grid-template-areas
```

Isso ajuda a identificar que estamos trabalhando com propriedades relacionadas ao Grid.

---

# 16. `columns` no plural

Observe:

```css
grid-template-columns
```

A palavra está no plural:

```text
columns
   ↑
   S
```

Isso acontece porque estamos falando de **colunas do container**.

Não devemos escrever:

```css
grid-template-column
```

mas:

```css
grid-template-columns
```

### ⚠️ Atenção

```css
grid-template-columns: 200px 200px;
```

✅ Correto.

```css
grid-template-column: 200px 200px;
```

❌ Incorreto.

---

# 17. Grid Container x Grid Item

Essa distinção é muito importante.

Imagine:

```html
<section class="grid">
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>
</section>
```

```css
.grid {
  display: grid;
}
```

Temos:

```text
<section class="grid">
        ↓
Grid Container

     ┌───────────────┐
     │      div      │ → Grid Item
     ├───────────────┤
     │      div      │ → Grid Item
     ├───────────────┤
     │      div      │ → Grid Item
     └───────────────┘
```

Portanto:

```text
Container
→ controla a estrutura do Grid

Items
→ participam dessa estrutura
```

---

# 18. `fr` — unidade fracional

Uma das principais unidades do Grid é:

```css
fr
```

`fr` representa uma **fração do espaço disponível**.

Por exemplo:

```css
grid-template-columns: 1fr 1fr 1fr;
```

significa:

```text
1 parte
+
1 parte
+
1 parte
```

Como as três partes são iguais, temos aproximadamente:

```text
33,33% + 33,33% + 33,33%
```

Visualmente:

```text
┌──────────┬──────────┬──────────┐
│   1fr    │   1fr    │   1fr    │
└──────────┴──────────┴──────────┘
```

---

# 19. `1fr 1fr 1fr`

Podemos pensar em:

```css
grid-template-columns: 1fr 1fr 1fr;
```

como:

```text
1 parte | 1 parte | 1 parte
```

Todas as colunas terão o mesmo tamanho proporcional.

Não precisamos calcular:

```text
100 ÷ 3 = 33,333...
```

Podemos simplesmente escrever:

```css
1fr 1fr 1fr
```

---

# 20. `2fr 2fr 2fr`

Agora imagine:

```css
grid-template-columns: 2fr 2fr 2fr;
```

Temos:

```text
2 partes | 2 partes | 2 partes
```

Como todas possuem a mesma proporção, continuam tendo o mesmo tamanho.

```text
┌──────────┬──────────┬──────────┐
│   2fr    │   2fr    │   2fr    │
└──────────┴──────────┴──────────┘
```

O número absoluto não importa tanto quanto a proporção entre eles.

---

# 21. `1fr 2fr 1fr`

Agora temos:

```css
grid-template-columns: 1fr 2fr 1fr;
```

Isso significa:

```text
1 parte
+
2 partes
+
1 parte
```

Total:

```text
1 + 2 + 1 = 4 partes
```

Logo:

```text
Coluna 1 → 1/4
Coluna 2 → 2/4
Coluna 3 → 1/4
```

Visualmente:

```text
┌────────┬────────────────┬────────┐
│  1fr   │      2fr       │  1fr   │
└────────┴────────────────┴────────┘
```

A coluna do meio será aproximadamente **duas vezes maior** que as outras.

---

# 22. `3fr`

Se quisermos que uma coluna seja três vezes maior que outra:

```css
grid-template-columns: 3fr 1fr 1fr;
```

Temos:

```text
3 partes | 1 parte | 1 parte
```

Total:

```text
5 partes
```

Portanto:

```text
┌───────────────────┬───────┬───────┐
│        3fr        │  1fr  │  1fr  │
└───────────────────┴───────┴───────┘
```

A primeira coluna recebe uma fração três vezes maior que cada uma das outras.

---

# 23. Por que usar `fr`?

A grande vantagem é trabalhar com **proporções**, sem precisar calcular manualmente porcentagens.

Em vez de:

```css
grid-template-columns: 33.33% 33.33% 33.33%;
```

podemos utilizar:

```css
grid-template-columns: 1fr 1fr 1fr;
```

Muito mais simples:

```text
1fr → 1 parte
1fr → 1 parte
1fr → 1 parte
```

---

# 24. Regra mental da unidade `fr`

Sempre que encontrar:

```css
fr
```

pense:

> **"Uma fração do espaço disponível."**

Exemplos:

```css
1fr 1fr
```

```text
1 parte | 1 parte
```

```css
1fr 2fr
```

```text
1 parte | 2 partes
```

```css
1fr 3fr
```

```text
1 parte | 3 partes
```

---

# 25. `grid-gap`

Também podemos definir o espaçamento entre os itens usando:

```css
grid-gap
```

Por exemplo:

```css
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 20px;
}
```

Isso cria um espaço entre as células do Grid.

Sem espaçamento:

```text
┌────────┬────────┐
│ Item 1 │ Item 2 │
├────────┼────────┤
│ Item 3 │ Item 4 │
└────────┴────────┘
```

Com `grid-gap: 20px`:

```text
┌────────┐  20px  ┌────────┐
│ Item 1 │        │ Item 2 │
└────────┘        └────────┘

┌────────┐  20px  ┌────────┐
│ Item 3 │        │ Item 4 │
└────────┘        └────────┘
```

> `gap` é a forma moderna e genérica de definir espaçamentos entre linhas e colunas, mas a aula apresenta `grid-gap` como a propriedade utilizada nesse exemplo.

---

# 26. Grid dentro de Grid

Um elemento que é um **Grid Item** também pode se tornar um **Grid Container**.

Por exemplo:

```html
<section class="grid">
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>

  <div class="sub-grid">
    <div>Item A</div>
    <div>Item B</div>
    <div>Item C</div>
  </div>
</section>
```

Podemos fazer:

```css
.grid {
  display: grid;
}

.sub-grid {
  display: grid;
}
```

Nesse caso, `.sub-grid` possui dois papéis:

```text
Grid Item do Grid pai
        +
Grid Container para seus próprios filhos
```

Visualmente:

```text
GRID PAI
│
├── Item 1
├── Item 2
├── Item 3
└── .sub-grid
     │
     ├── Item A
     ├── Item B
     └── Item C
```

Isso é perfeitamente possível.

---

# 27. Exemplo de Grid aninhado

Imagine:

```css
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}

.sub-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}
```

O Grid principal possui:

```text
1fr | 1fr | 1fr
```

E o `.sub-grid` possui:

```text
1fr | 1fr
```

Portanto, temos uma estrutura de Grid dentro de outra:

```text
GRID PRINCIPAL

┌──────┬──────┬──────────────────┐
│Item 1│Item 2│    SUB-GRID      │
│      │      ├────────┬─────────┤
│      │      │ Item A │ Item B  │
│      │      └────────┴─────────┘
└──────┴──────┴──────────────────┘
```

---

# 28. `subgrid` não é simplesmente "Grid dentro de Grid"

É importante não confundir:

```css
display: grid;
```

em um elemento filho com:

```css
display: subgrid;
```

São conceitos diferentes.

### Grid aninhado

```css
.child {
  display: grid;
}
```

O filho cria sua **própria estrutura de Grid**.

### Subgrid

```css
.child {
  display: grid;
  /* exemplo de uso de subgrid em propriedades do grid */
}
```

O `subgrid` permite que determinadas estruturas de linhas ou colunas sejam herdadas do Grid pai.

A demonstração da aula apresenta `subgrid` como uma funcionalidade específica para trabalhar com essa relação entre grids.

---


# 29. Exemplo completo

HTML:

```html
<section class="grid">
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>
  <div>Item 4</div>
</section>
```

CSS:

```css
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 20px;
}
```

Temos:

```text
┌──────────┬──────────┬──────────┐
│  Item 1  │  Item 2  │  Item 3  │
├──────────┼──────────┼──────────┤
│  Item 4  │          │          │
└──────────┴──────────┴──────────┘
```

Como existem três colunas:

```text
1fr | 1fr | 1fr
```

os itens vão sendo colocados nessas colunas conforme o fluxo do Grid.

---

# 30. Estrutura mental do CSS Grid

Podemos resumir o funcionamento desta aula assim:

```text
GRID
│
├── display: grid
│      ↓
│   ativa o Grid
│
├── Grid Container
│      ↓
│   elemento pai
│
├── Grid Items
│      ↓
│   filhos diretos
│
├── grid-template-columns
│      ↓
│   define as colunas
│
├── fr
│      ↓
│   divide o espaço proporcionalmente
│
├── gap
│      ↓
│   cria espaçamento
│
└── Grid aninhado
       ↓
    Grid dentro de Grid
```

---

# 31. Resumo das propriedades estudadas

| Propriedade             | Função                                     |
| ----------------------- | ------------------------------------------ |
| `display: grid`         | Transforma o elemento em Grid Container    |
| `display: inline-grid`  | Cria um Grid com comportamento inline      |
| `grid-template-columns` | Define as colunas                          |
| `grid-gap`              | Define o espaçamento entre itens do Grid   |
| `gap`                   | Forma moderna de definir espaçamento       |
| `fr`                    | Representa uma fração do espaço disponível |

---

# 32. Resumo de `grid-template-columns`

### Duas colunas fixas

```css
grid-template-columns: 200px 200px;
```

```text
200px | 200px
```

### Três colunas fixas

```css
grid-template-columns: 100px 100px 100px;
```

```text
100px | 100px | 100px
```

### Duas colunas iguais

```css
grid-template-columns: 1fr 1fr;
```

```text
1 parte | 1 parte
```

### Três colunas iguais

```css
grid-template-columns: 1fr 1fr 1fr;
```

```text
1 parte | 1 parte | 1 parte
```

### Uma coluna maior

```css
grid-template-columns: 3fr 1fr 1fr;
```

```text
3 partes | 1 parte | 1 parte
```

---

# 33. 🧠 Regra mental para revisão

Quando encontrar:

```css
display: grid;
```

pense:

> **"Este elemento virou um Grid Container."**

Quando encontrar:

```css
grid-template-columns
```

pense:

> **"Estou definindo as colunas do Grid."**

Quando encontrar:

```css
1fr
```

pense:

> **"Uma fração do espaço disponível."**

Quando encontrar:

```css
2fr 1fr
```

pense:

> **"A primeira coluna terá o dobro da proporção da segunda."**

Quando encontrar:

```css
gap: 20px;
```

pense:

> **"Estou criando espaço entre as células do Grid."**

E quando encontrar um elemento com:

```css
display: grid;
```

dentro de outro Grid:

> **"Esse elemento pode ser um Grid Item do pai e, ao mesmo tempo, um Grid Container para seus próprios filhos."**

---

# 34. 📌 O que realmente precisa ficar na memória

```text
1. display: grid
   → ativa o CSS Grid.

2. Grid Container
   → é o elemento pai que recebeu display: grid.

3. Grid Items
   → são os filhos diretos do Grid Container.

4. grid-template-columns
   → define as colunas.

5. fr
   → divide o espaço proporcionalmente.

6. gap
   → cria espaçamento entre os itens.

7. Grid pode existir dentro de outro Grid
   → um elemento pode ser Grid Item e Grid Container ao mesmo tempo.
```

### Conceito central da aula

> **O `display: grid` ativa o Grid, mas as propriedades como `grid-template-columns` definem como a estrutura da grade será organizada.**

A partir daí, podemos começar a construir layouts utilizando **colunas, proporções e espaçamento**.
