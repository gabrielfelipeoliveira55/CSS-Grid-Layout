# CSS Grid — `grid-auto-columns`

## 1. O que é `grid-auto-columns`?

A propriedade:

```css
grid-auto-columns
```

define o tamanho das **colunas implícitas** do Grid.

Para entender isso, precisamos diferenciar:

```text
colunas explícitas
```

de:

```text
colunas implícitas
```

---

# 2. Colunas explícitas

São as colunas que definimos diretamente através de:

```css
grid-template-columns
```

Por exemplo:

```css
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
}
```

Aqui estamos dizendo explicitamente:

```text
Coluna 1 → 1fr
Coluna 2 → 1fr
```

Visualmente:

```text
┌──────────────┬──────────────┐
│   1fr        │     1fr      │
└──────────────┴──────────────┘
```

Essas são **colunas explícitas** porque foram definidas diretamente no código.

---

# 3. Colunas implícitas

As colunas **implícitas** são criadas automaticamente quando o Grid precisa de mais colunas do que aquelas que foram definidas explicitamente.

Imagine:

```css
grid-template-columns: 1fr 1fr;
```

Temos apenas:

```text
1 | 2
```

Mas podemos determinar que um item deve ficar na coluna `3`.

```css
.item {
  grid-column: 3;
}
```

Agora o Grid precisa criar uma nova coluna:

```text
1        2        3
↓        ↓        ↓
┌────────┬────────┬────────┐
│        │        │        │
└────────┴────────┴────────┘
```

A terceira coluna é **implícita**, porque não foi declarada em:

```css
grid-template-columns
```

---

# 4. Explícito x implícito

Podemos memorizar assim:

```text
grid-template-columns
        ↓
DEFINE
        ↓
COLUNAS EXPLÍCITAS
```

Enquanto:

```text
necessidade do Grid
        ↓
CRIA
        ↓
COLUNAS IMPLÍCITAS
```

### Corte mental ①

```text
EXPLÍCITA
→ "Eu defini."

IMPLÍCITA
→ "O Grid criou porque precisou."
```

---

# 5. Exemplo simples

Considere:

```css
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
}
```

Temos:

```text
┌──────────┬──────────┐
│ Coluna 1 │ Coluna 2 │
└──────────┴──────────┘
```

Agora imagine que um item precise ocupar:

```css
.item {
  grid-column: 3;
}
```

Como a terceira coluna não existia explicitamente:

```text
Coluna 1 → explícita
Coluna 2 → explícita
Coluna 3 → implícita
```

---

# 6. Por que precisamos de `grid-auto-columns`?

Agora surge a pergunta:

> **Qual será o tamanho dessa nova coluna implícita?**

É exatamente isso que:

```css
grid-auto-columns
```

permite controlar.

Exemplo:

```css
.grid {
  display: grid;

  grid-template-columns: 1fr 1fr;

  grid-auto-columns: 100px;
}
```

Agora:

```text
Coluna 1 → 1fr
Coluna 2 → 1fr
Coluna 3 → 100px
```

A terceira coluna foi criada automaticamente e recebeu:

```text
100px
```

---

# 7. Sem `grid-auto-columns`

Se não definirmos:

```css
grid-auto-columns
```

as colunas implícitas utilizarão seu comportamento automático.

Na situação apresentada, isso pode ser entendido como:

```css
grid-auto-columns: auto;
```

Ou seja:

> O tamanho da coluna será determinado automaticamente conforme as necessidades do conteúdo e do layout.

---

# 8. `auto` em `grid-auto-columns`

Podemos escrever explicitamente:

```css
.grid {
  grid-auto-columns: auto;
}
```

Isso significa:

```text
tamanho automático
```

Por exemplo, se o conteúdo for pequeno:

```text
┌──────┐
│ A    │
└──────┘
```

a coluna pode ser pequena.

Se o conteúdo for maior:

```text
┌────────────────────┐
│ Conteúdo maior     │
└────────────────────┘
```

a coluna pode crescer para acomodá-lo.

### Regra mental

```text
auto
↓
"deixe o tamanho ser determinado automaticamente"
```

---

# 9. `grid-auto-columns: 100px`

Podemos definir:

```css
.grid {
  grid-auto-columns: 100px;
}
```

Agora todas as colunas implícitas criadas por essa regra terão:

```text
100px
```

Exemplo:

```text
Colunas explícitas:
1fr | 1fr

Colunas implícitas:
100px | 100px | 100px | ...
```

---

# 10. `grid-auto-columns: 1fr`

Também podemos utilizar:

```css
.grid {
  grid-auto-columns: 1fr;
}
```

Agora cada coluna implícita utilizará uma unidade fracional.

Exemplo:

```text
┌────────┬────────┬────────┬────────┐
│  1fr   │  1fr   │  1fr   │  1fr   │
└────────┴────────┴────────┴────────┘
```

As colunas automáticas participam da divisão proporcional do espaço.

---

# 11. Exemplo completo

```css
.grid {
  display: grid;

  grid-template-columns: 1fr 1fr;

  grid-auto-columns: 100px;
}
```

Se todos os itens couberem nas duas colunas:

```text
┌──────────┬──────────┐
│ Item 1   │ Item 2   │
├──────────┼──────────┤
│ Item 3   │ Item 4   │
├──────────┼──────────┤
│ Item 5   │ Item 6   │
└──────────┴──────────┘
```

Nenhuma coluna implícita precisa ser criada.

Portanto:

```text
grid-auto-columns
→ ainda não teve efeito visível
```

---

# 12. Quando as colunas implícitas aparecem?

Elas aparecem quando alguma regra do Grid exige mais colunas.

Por exemplo:

```css
.item-6 {
  grid-column: 3;
}
```

Agora:

```text
1 | 2 | 3
```

A terceira coluna precisa existir.

```text
1 → explícita
2 → explícita
3 → implícita
```

E:

```css
grid-auto-columns: 100px;
```

define o tamanho dessa terceira coluna.

---

# 13. Forçando uma posição ainda mais distante

Imagine:

```css
.item-6 {
  grid-column: 6;
}
```

Se o Grid possui:

```text
1fr | 1fr
```

mas estamos exigindo:

```text
coluna 6
```

o Grid precisa criar:

```text
1 | 2 | 3 | 4 | 5 | 6
```

Temos:

```text
1 → explícita
2 → explícita
3 → implícita
4 → implícita
5 → implícita
6 → implícita
```

Agora `grid-auto-columns` será aplicada às colunas criadas automaticamente.

---

# 14. Visualizando as colunas implícitas

```text
grid-template-columns
        ↓
┌────────┬────────┐
│   1    │   2    │
└────────┴────────┘
   1fr      1fr

        ↓ item precisa da coluna 6

┌────────┬────────┬───────┬───────┬───────┬───────┐
│   1    │   2    │   3   │   4   │   5   │   6   │
└────────┴────────┴───────┴───────┴───────┴───────┘
   1fr      1fr     auto    auto    auto    auto
```

Se tivermos:

```css
grid-auto-columns: 100px;
```

as colunas implícitas serão:

```text
100px | 100px | 100px | 100px
```

---

# 15. O padrão das colunas implícitas pode se repetir

Podemos definir mais de um tamanho:

```css
grid-auto-columns: 50px 100px;
```

Agora temos um padrão:

```text
50px
100px
50px
100px
50px
100px
...
```

Visualmente:

```text
┌──────┬──────────┬──────┬──────────┬──────┬──────────┐
│ 50px │  100px   │ 50px │  100px   │ 50px │  100px   │
└──────┴──────────┴──────┴──────────┴──────┴──────────┘
```

### Regra mental

> **Quando fornecemos vários valores, o padrão é repetido conforme novas colunas implícitas são necessárias.**

---

# 16. Exemplo com `50px 75px`

```css
.grid {
  grid-auto-columns: 50px 75px;
}
```

As colunas implícitas serão:

```text
Coluna implícita 1 → 50px
Coluna implícita 2 → 75px
Coluna implícita 3 → 50px
Coluna implícita 4 → 75px
Coluna implícita 5 → 50px
Coluna implícita 6 → 75px
```

Ou:

```text
50 | 75 | 50 | 75 | 50 | 75 | ...
```

---

# 17. `grid-auto-columns` não altera as colunas explícitas

Considere:

```css
.grid {
  grid-template-columns: 100px 100px;
  grid-auto-columns: 50px;
}
```

Temos:

```text
Coluna 1 → 100px
Coluna 2 → 100px
Coluna 3 → 50px
Coluna 4 → 50px
Coluna 5 → 50px
...
```

O `grid-auto-columns` não substitui:

```css
grid-template-columns
```

Ele controla as colunas **criadas automaticamente**.

### Corte mental ②

```text
grid-template-columns
→ colunas que EU DEFINI

grid-auto-columns
→ colunas que o GRID CRIOU
```

---

# 18. Mapa mental — explícitas x implícitas

```text
                         GRID
                          │
             ┌────────────┴────────────┐
             ↓                         ↓
        EXPLÍCITAS                 IMPLÍCITAS
             │                         │
             ↓                         ↓
grid-template-columns         grid-auto-columns
             │                         │
             ↓                         ↓
       eu defini                 Grid criou
       diretamente               automaticamente
```

### Regra mental

```text
template
→ "eu desenho"

auto
→ "o Grid completa"
```

---

# 19. `grid-auto-columns` e `grid-column`

Uma forma simples de demonstrar a propriedade é usar:

```css
.item {
  grid-column: 6;
}
```

O item está dizendo:

> **"Quero ficar na coluna 6."**

Se essa coluna não existe explicitamente, o Grid cria as colunas necessárias.

Então:

```text
grid-column: 6
        ↓
o Grid precisa criar até a coluna 6
        ↓
colunas implícitas
        ↓
grid-auto-columns
```

---

# 20. Exemplo

```html
<div class="grid">
  <div>1</div>
  <div>2</div>
  <div>3</div>
  <div>4</div>
  <div>5</div>
  <div class="item-6">6</div>
</div>
```

```css
.grid {
  display: grid;

  grid-template-columns:
    1fr 1fr;

  grid-auto-columns:
    100px;
}

.item-6 {
  grid-column: 6;
}
```

Resultado conceitual:

```text
┌──────┬──────┬─────┬─────┬─────┬─────┐
│  1fr │ 1fr  │100px│100px│100px│100px│
└──────┴──────┴─────┴─────┴─────┴─────┘
```

---

# 21. O Grid cria tantas colunas quanto necessário

Se temos:

```css
.item {
  grid-column: 6;
}
```

o Grid precisa possuir pelo menos:

```text
1 2 3 4 5 6
```

Não importa se definimos explicitamente apenas:

```text
1 2
```

O Grid terá que criar as restantes para atender ao posicionamento.

---

# 22. E se o item for para a coluna 9?

Considere:

```css
.item {
  grid-column: 9;
}
```

Agora o Grid precisa criar:

```text
1 2 3 4 5 6 7 8 9
```

Portanto:

```text
2 explícitas
+
7 implícitas
```

As colunas implícitas serão dimensionadas de acordo com:

```css
grid-auto-columns
```

---

# 23. Adicionando novos itens

Depois que criamos uma estrutura com várias colunas implícitas, novos itens podem continuar sendo distribuídos pelo Grid de acordo com o fluxo estabelecido.

Por exemplo:

```text
1 | 2 | 3 | 4 | 5 | 6
```

Quando adicionamos elementos:

```text
Item 1
Item 2
Item 3
Item 4
...
```

o Grid utiliza as colunas disponíveis e cria novas linhas conforme necessário.

---

# 24. Quando as colunas implícitas não são necessárias

Considere:

```css
grid-template-columns: 1fr 1fr;
```

e seis itens:

```text
1
2
3
4
5
6
```

Se o Grid puder distribuir os itens normalmente:

```text
┌──────┬──────┐
│  1   │  2   │
├──────┼──────┤
│  3   │  4   │
├──────┼──────┤
│  5   │  6   │
└──────┴──────┘
```

nenhuma coluna implícita precisa ser criada.

Isso é importante:

> **Ter muitos itens não significa automaticamente ter muitas colunas.**

As novas colunas surgem quando a estrutura do Grid exige isso.

---

# 25. `grid-auto-columns` só controla as colunas automáticas

Imagine:

```css
grid-template-columns: 100px 200px;
grid-auto-columns: 50px;
```

Temos:

```text
100px | 200px | 50px | 50px | 50px | ...
```

Portanto:

```text
grid-template-columns
→ primeiras colunas

grid-auto-columns
→ próximas colunas implícitas
```

---

# 26. Valor padrão

Quando não especificamos:

```css
grid-auto-columns
```

o comportamento padrão é:

```css
grid-auto-columns: auto;
```

De forma simplificada:

```text
auto
↓
tamanho determinado automaticamente
```

O conteúdo pode influenciar esse tamanho.

---

# 27. `auto` x `100px`

### `auto`

```css
grid-auto-columns: auto;
```

A largura pode acompanhar as necessidades do conteúdo.

```text
[A]
[Conteúdo maior]
```

As colunas podem ter tamanhos diferentes.

### `100px`

```css
grid-auto-columns: 100px;
```

As colunas implícitas possuem uma faixa definida de `100px`.

```text
100 | 100 | 100 | 100
```

---

# 28. `auto` x `1fr`

### `auto`

```css
grid-auto-columns: auto;
```

```text
conteúdo
   ↓
tamanho automático
```

### `1fr`

```css
grid-auto-columns: 1fr;
```

```text
espaço disponível
   ↓
distribuição proporcional
```

As duas propostas são diferentes.

---

# 29. `grid-auto-columns` + `grid-template-columns`

Um padrão comum de entender é:

```css
.grid {
  grid-template-columns: 100px 100px;
  grid-auto-columns: 1fr;
}
```

Visualmente:

```text
┌───────┬───────┬────────┬────────┬────────┐
│ 100px │ 100px │  1fr   │  1fr   │  1fr   │
└───────┴───────┴────────┴────────┴────────┘
```

As duas primeiras foram definidas explicitamente.

As seguintes são automáticas.

---

# 30. Mapa mental — fluxo completo

```text
              ITEM PRECISA DE
             UMA NOVA COLUNA
                     │
                     ↓
          GRID NÃO POSSUI A COLUNA
                     │
                     ↓
           CRIA COLUNA IMPLÍCITA
                     │
                     ↓
            grid-auto-columns
                     │
            ┌────────┼────────┐
            ↓        ↓        ↓
           auto     100px     1fr
            │        │        │
            ↓        ↓        ↓
       automático   fixo    proporcional
```

---

# 31. Mapa mental — `grid-column`

```text
              grid-column
                   │
                   ↓
          "Quero este item
           nesta coluna"
                   │
             ┌─────┴─────┐
             ↓           ↓
       coluna existe   coluna não existe
             │           │
             ↓           ↓
        usa a coluna   Grid cria
                       implícitas
                          │
                          ↓
                  grid-auto-columns
```

---

# 32. Mapa mental — padrão repetitivo

```text
grid-auto-columns:
50px 75px;
        │
        ↓
padrão
50 → 75
        │
        ↓
repete
        │
        ↓
50 → 75 → 50 → 75 → 50 → 75
```

### Corte mental ③

> **Quando existem vários valores em `grid-auto-columns`, eles formam um padrão que é repetido conforme novas colunas implícitas aparecem.**

---

# 33. Exemplo com `repeat()`

Também podemos utilizar:

```css
grid-auto-columns: repeat(2, 100px);
```

Isso significa:

```text
100px | 100px
```

como padrão das colunas automáticas.

Conforme novas colunas surgem:

```text
100px | 100px | 100px | 100px | ...
```

---

# 34. `minmax()` também pode ser utilizado

Podemos definir:

```css
grid-auto-columns: minmax(100px, 1fr);
```

Isso significa:

```text
mínimo → 100px
máximo → 1fr
```

Portanto, as colunas implícitas terão uma estrutura mais flexível.

---

# 35. Exemplos de valores

```css
grid-auto-columns: auto;
```

```css
grid-auto-columns: 100px;
```

```css
grid-auto-columns: 1fr;
```

```css
grid-auto-columns: 50px 75px;
```

```css
grid-auto-columns: minmax(100px, 1fr);
```

A propriedade pode utilizar diferentes formas de dimensionamento.

---

# 36. `grid-auto-columns` x `grid-template-columns`

Essa é provavelmente a distinção mais importante desta etapa.

```text
grid-template-columns
        ↓
colunas EXPLÍCITAS
        ↓
"Eu defini essas colunas."
```

```text
grid-auto-columns
        ↓
colunas IMPLÍCITAS
        ↓
"O Grid precisou criar essas colunas."
```

### Corte mental ④

```text
TEMPLATE
→ planejado

AUTO
→ criado conforme necessário
```

---

# 37. Exemplo visual definitivo

```css
.grid {
  display: grid;

  grid-template-columns:
    100px 100px;

  grid-auto-columns:
    50px 75px;
}
```

Se o Grid precisar de seis colunas:

```text
┌──────┬──────┬─────┬──────┬─────┬──────┐
│100px │100px │50px │ 75px │50px │ 75px │
└──────┴──────┴─────┴──────┴─────┴──────┘
   ↑      ↑      ↑      ↑      ↑      ↑
   │      │      └──────┴──────┴──────┘
   │      │             implícitas
   └──────┘
    explícitas
```

---

# 38. ⚠️ Não confundir com `grid-template-columns`

Se você escrever:

```css
grid-template-columns: 1fr 1fr;
```

você está dizendo:

> "Crie explicitamente duas colunas."

Se escrever:

```css
grid-auto-columns: 100px;
```

você está dizendo:

> "Quando surgir uma coluna implícita, ela deve ter 100px."

São responsabilidades diferentes.

---

# 39. Exemplo completo

### HTML

```html
<div class="grid">
  <div>1</div>
  <div>2</div>
  <div>3</div>
  <div>4</div>
  <div>5</div>
  <div class="item-6">6</div>
</div>
```

### CSS

```css
.grid {
  display: grid;

  grid-template-columns:
    1fr 1fr;

  grid-auto-columns:
    100px;
}

.item-6 {
  grid-column: 6;
}
```

Resultado conceitual:

```text
┌──────┬──────┬──────┬──────┬──────┬──────┐
│ 1fr  │ 1fr  │100px │100px │100px │100px │
└──────┴──────┴──────┴──────┴──────┴──────┘
```

---

# 40. 📌 Resumo final

```text
                    GRID
                     │
          ┌──────────┴──────────┐
          ↓                     ↓
      EXPLÍCITO              IMPLÍCITO
          │                     │
          ↓                     ↓
grid-template-columns    grid-auto-columns
          │                     │
          ↓                     ↓
    você define          Grid cria quando
                         necessário
```

### `grid-template-columns`

```css
grid-template-columns: 1fr 1fr;
```

```text
→ define colunas explicitamente
```

### `grid-auto-columns`

```css
grid-auto-columns: 100px;
```

```text
→ define o tamanho das colunas implícitas
```

### Sem `grid-auto-columns`

O comportamento padrão é:

```css
grid-auto-columns: auto;
```

### Com vários valores

```css
grid-auto-columns: 50px 75px;
```

```text
50 → 75 → 50 → 75 → 50 → 75 → ...
```

---

# 41. 🧠 Regras para memorizar

> **`grid-template-columns` define as colunas explícitas.**

> **`grid-auto-columns` define o tamanho das colunas implícitas.**

> **Colunas implícitas aparecem quando o Grid precisa de colunas que não foram definidas explicitamente.**

> **`auto` deixa o tamanho ser determinado automaticamente.**

> **Vários valores em `grid-auto-columns` formam um padrão que é repetido.**

### Fórmula mental definitiva

```text
grid-template-columns
        ↓
"EU DEFINO"

grid-auto-columns
        ↓
"O GRID CRIA"
```

E quando encontrar:

```css
grid-column: 6;
```

pense:

```text
"Preciso da coluna 6."
        ↓
Se ela não existir
        ↓
o Grid cria colunas implícitas
        ↓
grid-auto-columns define seus tamanhos.
```
