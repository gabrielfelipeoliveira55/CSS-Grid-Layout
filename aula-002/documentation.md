# CSS Grid — `grid-template-columns`, `minmax()`, `repeat()`, `auto-fit` e `auto-fill`

## 1. `grid-template-columns`

A propriedade:

```css
grid-template-columns
```

é responsável por definir a estrutura das **colunas do Grid**.

Exemplo:

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px 100px;
}
```

Nesse caso, temos quatro colunas de `100px`:

```text
┌────────┬────────┬────────┬────────┐
│ 100px  │ 100px  │ 100px  │ 100px  │
└────────┴────────┴────────┴────────┘
```

Uma regra importante:

> Cada valor informado em `grid-template-columns` representa uma coluna.

---

# 2. `px` define o tamanho da coluna

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

Esses valores definem o tamanho da **célula da grade**.

É importante não confundir a largura da coluna com o tamanho total do conteúdo de um item dentro dela.

---

# 3. Coluna e conteúdo do item são conceitos diferentes

Uma coluna pode ter:

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

Se o conteúdo não conseguir se ajustar dentro dos `100px`, ele poderá ultrapassar a largura da coluna.

Visualmente:

```text
┌──────────────┐
│ coluna 100px │
│ conteúdo     │───────────────>
└──────────────┘
```

Portanto:

> **A coluna continua com `100px`; o conteúdo é que pode ultrapassar seus limites.**

---

# 4. Conteúdo maior que a coluna

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

o navegador pode quebrar o texto quando existem oportunidades de quebra, como espaços.

```text
┌──────────────┐
│ Uma palavra  │
│ muito        │
│ grande       │
└──────────────┘
```

Mas uma palavra única e muito grande pode não possuir um ponto natural de quebra.

Nesse caso:

```text
┌──────────────┐
│ superpalavraque│────────>
└──────────────┘
```

O conteúdo pode ultrapassar a coluna.

---

# 5. `width` e a célula do Grid

Também é importante diferenciar:

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

Essas propriedades podem influenciar o comportamento visual do item sem alterar necessariamente a definição da coluna.

---

# 6. `min-width` pode fazer o item ultrapassar a coluna

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

Resultado:

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

# 7. Unidade `fr`

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

O espaço é dividido proporcionalmente.

```text
┌──────────┬────────────────────┐
│   1fr    │        2fr         │
└──────────┴────────────────────┘
```

A segunda coluna recebe o dobro da proporção da primeira.

---

# 8. `fr` e o conteúdo

Um ponto importante é que o comportamento de uma faixa `fr` pode levar em consideração o conteúdo mínimo dos itens.

Por exemplo:

```css
grid-template-columns: 1fr 2fr;
```

não deve ser mentalmente interpretado simplesmente como:

```text
33% | 66%
```

sem considerar outras restrições do layout.

Se existir um conteúdo muito grande em uma das colunas, esse conteúdo pode influenciar o tamanho mínimo necessário para aquela faixa.

Isso pode fazer com que a distribuição visual não seja exatamente a proporção que você imaginou inicialmente.

### Regra mental

> **`fr` representa uma proporção flexível, mas o conteúdo e as restrições mínimas dos itens também podem influenciar o resultado final.**

---

# 9. `minmax()`

A função:

```css
minmax()
```

permite definir:

```text
valor mínimo
+
valor máximo
```

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

# 10. Exemplo de `minmax()`

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

# 11. `minmax(100px, 1fr)`

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

A coluna poderá diminuir até:

```text
100px
```

mas não abaixo disso.

---

# 12. `minmax(50px, 100px)`

O máximo também pode ser um valor fixo:

```css
grid-template-columns:
  minmax(50px, 100px);
```

Agora a coluna possui:

```text
mínimo → 50px
máximo → 100px
```

Portanto:

```text
50px ≤ coluna ≤ 100px
```

Se houver espaço:

```text
coluna → 100px
```

Se o espaço diminuir:

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

# 13. `minmax()` resolve um problema importante

Imagine:

```css
grid-template-columns: 200px 1fr 1fr;
```

Quando a tela diminuir, a primeira coluna continuará com:

```text
200px
```

Isso pode consumir uma quantidade grande de espaço.

Com:

```css
grid-template-columns:
  minmax(100px, 1fr) 1fr 1fr;
```

essa coluna pode diminuir até `100px`.

Isso torna a estrutura mais flexível.

---

# 14. `minmax()` e conteúdo grande

Considere:

```css
grid-template-columns:
  minmax(200px, 1fr) 1fr 1fr;
```

Se o conteúdo da primeira coluna for muito grande, o Grid ainda precisa lidar com o tamanho mínimo necessário do conteúdo.

A função `minmax()` limita a faixa que foi definida, mas as necessidades mínimas do conteúdo e outras propriedades podem afetar o resultado.

Isso é especialmente importante quando trabalhamos com:

* palavras muito grandes;
* imagens;
* elementos com largura mínima;
* conteúdo que não pode ser reduzido.

---

# 15. `repeat()`

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

Os dois representam a mesma estrutura:

```text
1fr | 1fr | 1fr | 1fr
```

---

# 16. Sintaxe do `repeat()`

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

# 17. `repeat()` economiza código

Sem:

```css
grid-template-columns:
  1fr 1fr 1fr 1fr 1fr 1fr;
```

Com:

```css
grid-template-columns:
  repeat(6, 1fr);
```

A segunda forma é mais curta e facilita a manutenção.

---

# 18. `repeat()` aceita diferentes valores

Podemos repetir:

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

Também podemos combinar `repeat()` com funções como:

```css
minmax()
```

---

# 19. `repeat()` também pode ser combinado com outras colunas

Não precisamos usar `repeat()` para toda a declaração.

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

# 20. `auto-fit`

O `repeat()` possui palavras-chave especiais, entre elas:

```css
auto-fit
```

O objetivo é permitir que a quantidade de colunas se adapte ao espaço disponível.

Exemplo:

```css
grid-template-columns:
  repeat(auto-fit, 100px);
```

A ideia é:

> **Tente colocar a maior quantidade possível de colunas de `100px` dentro do container.**

---

# 21. Como `auto-fit` se comporta

Imagine um container com espaço suficiente para quatro colunas:

```text
┌─────┬─────┬─────┬─────┐
│ 100 │ 100 │ 100 │ 100 │
└─────┴─────┴─────┴─────┘
```

Se aumentarmos o container e houver espaço suficiente para mais uma:

```text
┌─────┬─────┬─────┬─────┬─────┐
│ 100 │ 100 │ 100 │ 100 │ 100 │
└─────┴─────┴─────┴─────┴─────┘
```

Se diminuirmos:

```text
┌─────┬─────┬─────┐
│ 100 │ 100 │ 100 │
└─────┴─────┴─────┘
```

O número de colunas se adapta.

---

# 22. `auto-fit` com `1fr`

Podemos fazer:

```css
grid-template-columns:
  repeat(auto-fit, 1fr);
```

Nesse caso, as colunas tentam se expandir para ocupar o espaço disponível.

A ideia é:

```text
auto-fit
→ quantas colunas cabem?

1fr
→ como distribuir o espaço entre elas?
```

Isso produz uma estrutura bastante flexível.

---

# 23. `auto-fit` com um tamanho fixo

Podemos utilizar:

```css
grid-template-columns:
  repeat(auto-fit, 200px);
```

Agora cada coluna possui uma largura de `200px`.

O `auto-fit` determina quantas delas podem caber.

```text
Espaço grande
→ várias colunas de 200px

Espaço menor
→ menos colunas de 200px
```

---

# 24. O padrão mais útil: `auto-fit` + `minmax()`

Uma combinação muito importante é:

```css
grid-template-columns:
  repeat(auto-fit, minmax(100px, 1fr));
```

Podemos interpretar assim:

```text
repeat()
→ repita

auto-fit
→ crie quantas colunas couberem

minmax(100px, 1fr)
→ cada coluna possui no mínimo 100px
→ e pode crescer até ocupar uma fração do espaço
```

Essa combinação permite criar grids responsivos com pouco código.

---

# 25. Como funciona `repeat(auto-fit, minmax())`

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

E assim por diante.

---

# 26. Por que `minmax()` ajuda o `auto-fit`?

Sem um limite mínimo, uma coluna poderia continuar ficando extremamente pequena.

Com:

```css
minmax(100px, 1fr)
```

estamos dizendo:

```text
"Cada coluna deve ter pelo menos 100px."
```

Quando não houver espaço suficiente para manter mais colunas:

```text
coluna 1 → 100px
coluna 2 → 100px
coluna 3 → 100px
```

e o Grid pode reorganizar os itens em novas linhas.

---

# 27. `auto` como valor máximo

Também é possível encontrar:

```css
minmax(100px, auto)
```

A ideia é permitir que o tamanho máximo seja determinado automaticamente.

Nesse caso:

```text
mínimo → 100px
máximo → auto
```

O comportamento de `auto` pode considerar o tamanho necessário do conteúdo.

Por exemplo, uma coluna pode ficar maior para acomodar seu conteúdo.

---

# 28. `auto-fit` x `auto-fill`

Além de:

```css
auto-fit
```

existe:

```css
auto-fill
```

Os dois estão relacionados à criação automática de colunas, mas possuem uma diferença importante quando existe espaço para mais colunas do que elementos.

---

# 29. `auto-fit`

Com:

```css
repeat(auto-fit, minmax(100px, 1fr))
```

o Grid tenta ajustar as colunas às necessidades dos itens existentes.

Depois que todos os itens foram acomodados, o espaço adicional tende a ser utilizado para expandir as colunas existentes.

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

# 30. `auto-fill`

Com:

```css
repeat(auto-fill, minmax(100px, 1fr))
```

o Grid pode criar tantas faixas quanto couberem, mesmo que algumas dessas faixas não tenham itens.

Por exemplo:

```text
┌────────┬────────┬────────┬────────┬────────┐
│   A    │   B    │   C    │        │        │
└────────┴────────┴────────┴────────┴────────┘
```

As últimas colunas podem estar vazias, mas continuam fazendo parte da grade implícita criada pela configuração.

---

# 31. Diferença mental entre `auto-fit` e `auto-fill`

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

# 32. O que acontece quando o container aumenta?

Considere:

```css
grid-template-columns:
  repeat(auto-fit, minmax(100px, 1fr));
```

Quando existe espaço suficiente para mais colunas:

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

O comportamento depende do número de itens e das restrições definidas.

---

# 33. Layout responsivo sem várias Media Queries

A combinação:

```css
grid-template-columns:
  repeat(auto-fit, minmax(100px, 1fr));
```

é especialmente útil para criar estruturas responsivas.

Em vez de definir manualmente:

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

podemos deixar o Grid adaptar a quantidade de colunas automaticamente.

Isso **não significa que Media Queries deixaram de ser necessárias**; significa apenas que determinados layouts podem precisar de menos regras explícitas.

---

# 34. Padrão de Grid responsivo

Um padrão muito importante para guardar é:

```css
.grid {
  display: grid;
  grid-template-columns:
    repeat(auto-fit, minmax(100px, 1fr));
  gap: 20px;
}
```

Interpretação:

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

# 35. Exemplo completo

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

Container maior:

```text
┌────────┬────────┬────────┬────────┐
│   1    │   2    │   3    │   4    │
├────────┼────────┼────────┼────────┤
│   5    │   6    │        │        │
└────────┴────────┴────────┴────────┘
```

Container menor:

```text
┌────────┬────────┬────────┐
│   1    │   2    │   3    │
├────────┼────────┼────────┤
│   4    │   5    │   6    │
└────────┴────────┴────────┘
```

Container ainda menor:

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

# 36. Combinações importantes

## Colunas fixas

```css
grid-template-columns: 100px 100px 100px;
```

Use quando quiser tamanhos específicos.

---

## Colunas proporcionais

```css
grid-template-columns: 1fr 1fr 1fr;
```

Use quando quiser dividir o espaço proporcionalmente.

---

## Repetição

```css
grid-template-columns: repeat(4, 1fr);
```

Use quando a mesma configuração se repete.

---

## Limite mínimo e máximo

```css
grid-template-columns:
  minmax(100px, 1fr) 1fr 1fr;
```

Use quando uma coluna precisa respeitar um tamanho mínimo ou máximo.

---

## Grid responsivo

```css
grid-template-columns:
  repeat(auto-fit, minmax(150px, 1fr));
```

Use quando quiser que o número de colunas se adapte ao espaço disponível.

---

# 37. Estrutura mental completa

```text
grid-template-columns
│
├── valores fixos
│   └── 100px 100px
│
├── valores proporcionais
│   └── 1fr 2fr
│
├── repeat()
│   └── repeat(4, 1fr)
│
├── minmax()
│   └── minmax(100px, 1fr)
│
└── auto-fit / auto-fill
    └── criação dinâmica de colunas
```

---

# 38. 🧠 Como memorizar

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

# 39. 📌 O padrão mais importante

Entre todas as combinações, uma das mais úteis para layouts responsivos é:

```css
.container {
  display: grid;
  grid-template-columns:
    repeat(auto-fit, minmax(150px, 1fr));
  gap: 20px;
}
```

A leitura dessa declaração é:

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

Esse padrão permite construir uma grade que se adapta ao espaço disponível sem precisar determinar manualmente uma quantidade fixa de colunas para cada largura.

---

# 40. Resumo final

```text
grid-template-columns
        ↓
define as colunas
        │
        ├── px
        │    → tamanho fixo
        │
        ├── %
        │    → proporção relativa ao container
        │
        ├── fr
        │    → fração proporcional do espaço
        │
        ├── minmax()
        │    → define mínimo e máximo
        │
        ├── repeat()
        │    → repete uma configuração
        │
        ├── auto-fit
        │    → ajusta as colunas aos itens/espaço
        │
        └── auto-fill
             → cria todas as faixas possíveis
```

### Regra mental principal

> **`grid-template-columns` define as colunas. `fr` define proporções. `minmax()` define limites. `repeat()` evita repetição de código. `auto-fit` e `auto-fill` tornam a quantidade de colunas dinâmica.**
