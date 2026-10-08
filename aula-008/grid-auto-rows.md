# CSS Grid — `grid-auto-rows`

## 1. O que é `grid-auto-rows`?

A propriedade:

```css
grid-auto-rows
```

define o tamanho das **linhas implícitas** do Grid.

Linhas implícitas são linhas que o Grid cria automaticamente quando precisamos de mais linhas do que aquelas definidas explicitamente.

Podemos pensar em:

```text
Grid explícito
→ aquilo que definimos

Grid implícito
→ aquilo que o Grid cria automaticamente
```

A lógica é semelhante a:

```css
grid-auto-columns
```

A diferença é:

```text
grid-auto-columns
→ colunas implícitas

grid-auto-rows
→ linhas implícitas
```

---

# 2. Linhas explícitas x linhas implícitas

Considere:

```css
.grid {
  display: grid;

  grid-template-rows:
    100px
    100px;
}
```

Temos duas linhas explícitas:

```text
Linha 1 → 100px
Linha 2 → 100px
```

Se novos itens exigirem mais linhas:

```text
Linha 1 → explícita
Linha 2 → explícita
Linha 3 → implícita
Linha 4 → implícita
...
```

É justamente nessas linhas implícitas que:

```css
grid-auto-rows
```

atua.

---

# 3. Exemplo básico

Considere:

```css
.grid {
  display: grid;

  grid-template-rows:
    1fr
    1fr;

  grid-auto-rows: 100px;
}
```

Temos:

```text
Linhas explícitas:
1fr
1fr
```

E quando forem necessárias novas linhas:

```text
Linhas implícitas:
100px
100px
100px
...
```

Visualmente:

```text
┌──────────┬──────────┐
│          │          │ 1fr
├──────────┼──────────┤
│          │          │ 1fr
├──────────┼──────────┤
│          │          │ 100px
├──────────┼──────────┤
│          │          │ 100px
└──────────┴──────────┘
```

---

# 4. Removendo `grid-template-rows`

Se retirarmos:

```css
grid-template-rows
```

todas as linhas poderão ser implícitas.

Por exemplo:

```css
.grid {
  display: grid;
  grid-auto-rows: 100px;
}
```

Agora todas as linhas criadas automaticamente terão:

```text
100px
```

Visualmente:

```text
┌──────────┬──────────┐
│          │          │ 100px
├──────────┼──────────┤
│          │          │ 100px
├──────────┼──────────┤
│          │          │ 100px
├──────────┼──────────┤
│          │          │ 100px
└──────────┴──────────┘
```

### Regra mental

> **`grid-auto-rows` define o tamanho das linhas criadas automaticamente.**

---

# 5. `grid-auto-rows` x `grid-template-rows`

Essa comparação é fundamental.

```text
grid-template-rows
→ linhas explícitas
→ "eu defini"
```

```text
grid-auto-rows
→ linhas implícitas
→ "o Grid criou"
```

### Corte mental ①

```text
TEMPLATE
→ estrutura planejada

AUTO
→ estrutura criada automaticamente
```

---

# 6. Exemplo completo

```css
.grid {
  display: grid;

  grid-template-rows:
    1fr
    1fr;

  grid-auto-rows:
    100px;
}
```

Podemos visualizar:

```text
              GRID
               │
       ┌───────┴───────┐
       ↓               ↓
   EXPLÍCITO         IMPLÍCITO
       │               │
       ↓               ↓
     1fr              100px
     1fr              100px
                      100px
                      ...
```

---

# 7. O valor `auto`

Se não definirmos:

```css
grid-auto-rows
```

o comportamento padrão utiliza um dimensionamento automático.

Podemos representar isso como:

```css
grid-auto-rows: auto;
```

Nesse caso, o tamanho da linha pode ser determinado conforme as necessidades do conteúdo e do layout.

Por exemplo:

```text
┌──────────────┐
│ Texto        │
└──────────────┘

┌──────────────┐
│ Texto maior  │
│ em duas      │
│ linhas       │
└──────────────┘
```

A altura das linhas pode variar de acordo com o conteúdo.

---

# 8. `grid-auto-rows: 100px`

Podemos definir:

```css
.grid {
  grid-auto-rows: 100px;
}
```

Agora todas as linhas implícitas criadas por essa configuração terão:

```text
100px
```

Exemplo:

```text
Linha 1 → 100px
Linha 2 → 100px
Linha 3 → 100px
Linha 4 → 100px
...
```

---

# 9. `grid-auto-rows: 1fr`

Também podemos utilizar:

```css
.grid {
  grid-auto-rows: 1fr;
}
```

Agora cada linha implícita utilizará uma unidade fracional.

```text
1fr
1fr
1fr
...
```

A distribuição dependerá do espaço disponível no eixo vertical e das demais características do Grid.

---

# 10. Vários valores em `grid-auto-rows`

Assim como em:

```css
grid-auto-columns: 50px 75px;
```

podemos utilizar vários valores:

```css
.grid {
  grid-auto-rows:
    50px
    100px;
}
```

Isso cria um padrão:

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
┌──────────┬──────────┐
│          │          │ 50px
├──────────┼──────────┤
│          │          │ 100px
├──────────┼──────────┤
│          │          │ 50px
├──────────┼──────────┤
│          │          │ 100px
└──────────┴──────────┘
```

### Regra mental

> **Vários valores em `grid-auto-rows` formam um padrão que se repete nas novas linhas implícitas.**

---

# 11. Exemplo com `50px 100px`

```css
.grid {
  grid-auto-rows: 50px 100px;
}
```

As linhas implícitas serão:

```text
Linha 1 → 50px
Linha 2 → 100px
Linha 3 → 50px
Linha 4 → 100px
Linha 5 → 50px
Linha 6 → 100px
...
```

Podemos representar:

```text
50 → 100 → 50 → 100 → 50 → 100 → ...
```

---

# 12. Se utilizarmos apenas `100px`

```css
.grid {
  grid-auto-rows: 100px;
}
```

não existe um padrão alternado.

Todas as linhas terão o mesmo tamanho:

```text
100 → 100 → 100 → 100 → 100 → ...
```

---

# 13. `grid-auto-rows` e um layout completo

Podemos ter um Grid com uma estrutura inicial:

```css
.grid {
  display: grid;

  grid-template-columns:
    100px
    1fr
    50px;

  grid-template-rows:
    50px
    150px
    100px;

  grid-auto-rows:
    50px 100px;
}
```

Temos:

### Colunas explícitas

```text
100px | 1fr | 50px
```

### Linhas explícitas

```text
50px
150px
100px
```

### Linhas implícitas

```text
50px
100px
50px
100px
...
```

---

# 14. Exemplo de estrutura de página

Podemos imaginar:

```text
┌─────────┬──────────────────────┬────────┐
│  Logo   │        Nav           │        │
├─────────┼──────────────────────┼────────┤
│ Sidebar │       Content        │ Advert │
├─────────┼──────────────────────┼────────┤
│ Sidebar │       Content        │ Advert │
├─────────┴──────────────────────┴────────┤
│                  Footer                  │
└──────────────────────────────────────────┘
```

Podemos definir as áreas principais com:

```css
grid-template-areas:
  "logo nav nav"
  "side content advert"
  "side content advert"
  "footer footer footer";
```

E definir os tamanhos principais:

```css
grid-template-columns:
  100px
  1fr
  50px;

grid-template-rows:
  50px
  150px
  100px;
```

Se novos elementos forem adicionados e novas linhas forem necessárias, podemos controlar essas linhas adicionais com:

```css
grid-auto-rows:
  50px 100px;
```

---

# 15. O que acontece quando novos itens aparecem?

Imagine que o Grid tenha uma estrutura já definida:

```text
3 colunas
```

e que novos elementos sejam adicionados além das linhas existentes.

O Grid poderá criar novas linhas automaticamente:

```text
Linha 1
Linha 2
Linha 3
Linha 4 ← implícita
Linha 5 ← implícita
Linha 6 ← implícita
```

Se tivermos:

```css
grid-auto-rows: 50px 100px;
```

essas novas linhas seguem o padrão:

```text
Linha 4 → 50px
Linha 5 → 100px
Linha 6 → 50px
...
```

---

# 16. `grid-auto-rows` funciona como um padrão

Podemos pensar em:

```css
grid-auto-rows: 50px 100px;
```

como:

```text
        PADRÃO
          │
     ┌────┴────┐
     ↓         ↓
   50px       100px
     │         │
     └────┬────┘
          ↓
       repete
          ↓
50 → 100 → 50 → 100 → ...
```

### Corte mental ②

> **`grid-auto-rows` pode definir não apenas um tamanho, mas uma sequência de tamanhos para as linhas implícitas.**

---

# 17. Comparação com `grid-auto-columns`

A lógica é praticamente a mesma.

## `grid-auto-columns`

```css
grid-auto-columns: 50px 100px;
```

Controla:

```text
COLUNAS
50 → 100 → 50 → 100 → ...
```

## `grid-auto-rows`

```css
grid-auto-rows: 50px 100px;
```

Controla:

```text
LINHAS
50 → 100 → 50 → 100 → ...
```

### Corte mental ③

```text
AUTO-COLUMNS
→ colunas implícitas

AUTO-ROWS
→ linhas implícitas
```

---

# 18. Mapa mental — `grid-auto-rows`

```text
                 GRID AUTO-ROWS
                       │
                       ↓
               LINHAS IMPLÍCITAS
                       │
            ┌──────────┼──────────┐
            ↓          ↓          ↓
          auto        100px      1fr
            │           │          │
            ↓           ↓          ↓
       automático      fixo    proporcional
```

---

# 19. Mapa mental — explícito x implícito

```text
                     GRID
                      │
          ┌───────────┴───────────┐
          ↓                       ↓
      EXPLÍCITO                IMPLÍCITO
          │                       │
          ↓                       ↓
grid-template-rows         grid-auto-rows
          │                       │
          ↓                       ↓
    linhas definidas        linhas criadas
       por você              pelo Grid
```

### Corte mental ④

```text
template
→ "eu defini"

auto
→ "o Grid criou"
```

---

# 20. Mapa mental — fluxo da criação

```text
          GRID CONTAINER
                │
                ↓
      grid-template-rows
                │
                ↓
       linhas explícitas
                │
                ↓
      novas linhas são
          necessárias?
                │
              SIM
                ↓
       linhas implícitas
                │
                ↓
         grid-auto-rows
                │
        ┌───────┼────────┐
        ↓       ↓        ↓
       auto    50px     1fr
```

---

# 21. Mapa mental — padrão

```text
grid-auto-rows:
50px 100px;
        │
        ↓
     sequência
        │
        ↓
50 → 100
        │
        ↓
50 → 100
        │
        ↓
50 → 100
```

Isso facilita imaginar o comportamento quando o Grid precisar criar muitas linhas automaticamente.

---

# 22. `repeat()` também pode ser usado

Podemos escrever:

```css
grid-auto-rows:
  repeat(2, 50px);
```

Isso significa:

```text
50px
50px
```

e o padrão pode continuar conforme novas linhas implícitas forem necessárias:

```text
50 → 50 → 50 → 50 → ...
```

Também podemos utilizar outras formas de dimensionamento disponíveis para faixas do Grid.

---

# 23. `minmax()` também pode ser utilizado

A mesma ideia de flexibilidade estudada anteriormente pode ser aplicada:

```css
grid-auto-rows:
  minmax(100px, 1fr);
```

Isso significa:

```text
mínimo → 100px
máximo → 1fr
```

A linha não deve ficar menor que o limite mínimo definido, podendo crescer de forma flexível quando houver espaço suficiente.

---

# 24. Exemplo completo

```css
.grid {
  display: grid;

  grid-template-columns:
    100px
    1fr
    50px;

  grid-template-rows:
    50px
    150px
    100px;

  grid-auto-rows:
    50px
    100px;
}
```

Interpretação:

```text
             GRID
               │
      ┌────────┴────────┐
      ↓                 ↓
  EXPLÍCITO          IMPLÍCITO
      │                 │
      ↓                 ↓
   ROWS               AUTO-ROWS
      │                 │
      ↓                 ↓
50px                 50px
150px               100px
100px                50px
                    100px
                    ...
```

---

# 25. Diferença em uma frase

```text
grid-template-rows
→ "Quais linhas eu quero definir?"

grid-auto-rows
→ "Como devem ser as linhas que o Grid criar sozinho?"
```

Essa é a melhor forma de separar as duas propriedades.

---

# 26. Exemplo lado a lado

```css
.grid {
  grid-template-rows:
    50px
    150px;

  grid-auto-rows:
    50px 100px;
}
```

Temos:

```text
LINHAS EXPLÍCITAS

50px
150px
```

Depois, se forem necessárias novas linhas:

```text
LINHAS IMPLÍCITAS

50px
100px
50px
100px
...
```

Visualmente:

```text
┌──────────┐
│          │ 50px       ← explícita
├──────────┤
│          │ 150px      ← explícita
├──────────┤
│          │ 50px       ← implícita
├──────────┤
│          │ 100px      ← implícita
├──────────┤
│          │ 50px       ← implícita
└──────────┘
```

---

# 27. ⚠️ Não confundir com `grid-template-rows`

Se você escrever:

```css
grid-template-rows: 50px 100px;
```

você está definindo especificamente as linhas explícitas.

Se escrever:

```css
grid-auto-rows: 50px 100px;
```

está definindo como as linhas implícitas devem ser dimensionadas.

São responsabilidades diferentes.

---

# 28. Relação com `grid-auto-columns`

Podemos montar uma tabela mental:

| Propriedade             | Controla           |
| ----------------------- | ------------------ |
| `grid-template-columns` | Colunas explícitas |
| `grid-template-rows`    | Linhas explícitas  |
| `grid-auto-columns`     | Colunas implícitas |
| `grid-auto-rows`        | Linhas implícitas  |

Uma maneira simples de memorizar:

```text
             GRID
              │
      ┌───────┴───────┐
      ↓               ↓
   COLUMNS           ROWS
      │               │
  ┌───┴───┐       ┌───┴───┐
  ↓       ↓       ↓       ↓
template auto  template auto
  ↓       ↓       ↓       ↓
explícito implícito explícito implícito
```

---

# 29. 📌 Resumo final

```text
grid-auto-rows
      ↓
controla linhas implícitas
      ↓
linhas criadas automaticamente
      ↓
quando o Grid precisa de mais linhas
```

Exemplo:

```css
grid-auto-rows: 100px;
```

```text
100 → 100 → 100 → 100 → ...
```

Com dois valores:

```css
grid-auto-rows: 50px 100px;
```

```text
50 → 100 → 50 → 100 → ...
```

Com `auto`:

```css
grid-auto-rows: auto;
```

```text
tamanho automático
conforme as necessidades do conteúdo/layout
```

---

# 30. 🧠 Regras para memorizar

> **`grid-template-rows` define as linhas explícitas.**

> **`grid-auto-rows` define as linhas implícitas.**

> **Uma linha implícita é criada quando o Grid precisa de uma linha que não foi definida explicitamente.**

> **Um único valor em `grid-auto-rows` é aplicado às linhas implícitas criadas.**

> **Vários valores formam um padrão que é repetido.**

### Fórmula mental definitiva

```text
grid-template-rows
        ↓
"EU DEFINO AS LINHAS"

grid-auto-rows
        ↓
"EU DEFINO COMO O GRID
DEVE CRIAR AS NOVAS LINHAS"
```

E a relação completa:

```text
grid-template-columns → colunas explícitas
grid-auto-columns     → colunas implícitas

grid-template-rows    → linhas explícitas
grid-auto-rows        → linhas implícitas
```
