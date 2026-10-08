# CSS Grid — `grid-template`

## 1. O que é `grid-template`?

A propriedade:

```css
grid-template
```

é um **shorthand** que permite definir, em uma única declaração, aspectos relacionados à estrutura do Grid.

Ela pode reunir:

```text
grid-template-rows
grid-template-columns
grid-template-areas
```

A ideia é facilitar a configuração de um layout quando queremos definir essas características juntas.

### Mapa mental

```text
                 grid-template
                       │
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
      areas          rows          columns
        │              │              │
        ↓              ↓              ↓
      áreas          linhas         colunas
```

---

# 2. A sintaxe básica

Uma forma importante de entender o `grid-template` é:

```css
grid-template:
  rows / columns;
```

Ou seja:

```text
ANTES da /
→ linhas

DEPOIS da /
→ colunas
```

### Regra mental

```text
grid-template:
    LINHAS / COLUNAS;
```

---

# 3. Exemplo simples

Considere:

```css
.grid {
  display: grid;

  grid-template:
    100px 50px
    /
    1fr 1fr;
}
```

Isso significa:

```text
LINHAS
100px
50px

      /

COLUNAS
1fr
1fr
```

Visualmente:

```text
┌──────────┬──────────┐
│          │          │ 100px
├──────────┼──────────┤
│          │          │ 50px
└──────────┴──────────┘
    1fr          1fr
```

---

# 4. A barra `/` é fundamental

A barra:

```css
/
```

separa:

```text
linhas
   ↓
/
   ↓
colunas
```

Portanto:

```css
grid-template: 100px 50px / 1fr 1fr;
```

deve ser lido como:

```text
100px 50px
→ tamanho das linhas

/

1fr 1fr
→ tamanho das colunas
```

### Corte mental

```text
grid-template:
     ROWS
      ↓
      /
      ↓
   COLUMNS
```

---

# 5. Sem a barra, a declaração fica incorreta

Observe:

```css
grid-template: 100px 50px 1fr 1fr;
```

Aqui não existe uma separação clara entre:

```text
linhas
```

e:

```text
colunas
```

A estrutura correta utiliza:

```css
grid-template: 100px 50px / 1fr 1fr;
```

### Memorize:

> **`/` = separador entre linhas e colunas.**

---

# 6. `grid-template` x propriedades individuais

Podemos configurar o Grid de forma explícita:

```css
.grid {
  grid-template-rows:
    100px
    50px;

  grid-template-columns:
    1fr
    1fr;
}
```

Ou utilizar o shorthand:

```css
.grid {
  grid-template:
    100px 50px
    /
    1fr 1fr;
}
```

A ideia é reunir as definições em uma única propriedade.

---

# 7. Exemplo equivalente

### Forma separada

```css
.grid {
  grid-template-rows: 100px 50px;
  grid-template-columns: 1fr 1fr;
}
```

### Forma com shorthand

```css
.grid {
  grid-template: 100px 50px / 1fr 1fr;
}
```

Visualmente, a estrutura pretendida é a mesma:

```text
┌──────────┬──────────┐
│          │          │
├──────────┼──────────┤
│          │          │
└──────────┴──────────┘
```

---

# 8. Podemos utilizar `fr`

Assim como em `grid-template-columns` e `grid-template-rows`, podemos utilizar:

```css
fr
```

Exemplo:

```css
.grid {
  grid-template:
    1fr 2fr
    /
    1fr 1fr;
}
```

Temos:

```text
LINHAS
1fr
2fr

COLUNAS
1fr
1fr
```

Visualmente:

```text
┌──────────┬──────────┐
│          │          │
│   1fr    │   1fr    │
├──────────┼──────────┤
│          │          │
│   2fr    │   2fr    │
│          │          │
└──────────┴──────────┘
```

A divisão das linhas é proporcional ao espaço disponível no eixo vertical.

---

# 9. Valores diferentes para as linhas

Podemos utilizar diferentes tamanhos:

```css
.grid {
  grid-template:
    200px 150px 100px
    /
    1fr 1fr;
}
```

Temos:

```text
LINHAS
200px
150px
100px

COLUNAS
1fr
1fr
```

Resultado:

```text
┌──────────┬──────────┐
│          │          │ 200px
├──────────┼──────────┤
│          │          │ 150px
├──────────┼──────────┤
│          │          │ 100px
└──────────┴──────────┘
```

---

# 10. Podemos deixar algumas linhas automáticas

Também é possível utilizar `auto`.

Por exemplo:

```css
.grid {
  grid-template:
    200px auto 100px
    /
    1fr 1fr;
}
```

Aqui:

```text
Linha 1 → 200px
Linha 2 → auto
Linha 3 → 100px
```

A linha `auto` pode ser dimensionada de acordo com as necessidades do layout e do conteúdo.

### Mapa mental

```text
200px
 ↓
fixo

auto
 ↓
automático

100px
 ↓
fixo
```

---

# 11. Definindo as colunas depois da `/`

Tudo que vem depois da barra representa as colunas.

Exemplo:

```css
grid-template:
  100px 50px
  /
  1fr 1fr;
```

Temos:

```text
ANTES da /
→ 100px 50px
→ linhas

DEPOIS da /
→ 1fr 1fr
→ colunas
```

Podemos mudar para:

```css
grid-template:
  100px 50px
  /
  1fr 2fr;
```

Agora:

```text
Coluna 1 → 1fr
Coluna 2 → 2fr
```

Visualmente:

```text
┌──────────┬──────────────────┐
│          │                  │
│          │                  │
├──────────┼──────────────────┤
│          │                  │
└──────────┴──────────────────┘
    1fr           2fr
```

---

# 12. `repeat()` também pode ser utilizado

O `grid-template` aceita construções como:

```css
grid-template:
  100px 50px
  /
  repeat(3, 1fr);
```

Isso significa:

```text
LINHAS
100px
50px

COLUNAS
1fr
1fr
1fr
```

Visualmente:

```text
┌────────┬────────┬────────┐
│        │        │        │
├────────┼────────┼────────┤
│        │        │        │
└────────┴────────┴────────┘
```

---

# 13. `repeat()` facilita estruturas repetitivas

Sem `repeat()`:

```css
grid-template:
  100px 50px
  /
  1fr 1fr 1fr;
```

Com `repeat()`:

```css
grid-template:
  100px 50px
  /
  repeat(3, 1fr);
```

A segunda forma é mais compacta.

### Regra mental

```text
repeat(3, 1fr)
       ↓
"repita 1fr três vezes"
```

---

# 14. Podemos combinar valores diferentes

Também podemos misturar `repeat()` com outras colunas:

```css
grid-template:
  100px 50px
  /
  2fr repeat(2, 1fr);
```

Resultado:

```text
2fr | 1fr | 1fr
```

A primeira coluna possui o dobro da proporção de cada uma das outras.

---

# 15. Onde entra `grid-template-areas`?

O `grid-template` também está relacionado à definição de áreas.

Em termos conceituais, o shorthand pode representar:

```text
areas
+
rows
+
columns
```

Porém, a sintaxe para `grid-template-areas` possui regras específicas.

A ideia principal é:

```text
grid-template
   │
   ├── rows
   ├── columns
   └── areas
```

---

# 16. Diferença entre `grid-template` e `grid-template-areas`

Não devemos confundir:

```css
grid-template
```

com:

```css
grid-template-areas
```

`grid-template-areas` define especificamente o **mapa de áreas nomeadas**.

Exemplo:

```css
grid-template-areas:
  "header header"
  "nav content"
  "footer footer";
```

Enquanto o `grid-template` é um **shorthand** que pode reunir diferentes aspectos da definição do Grid.

---

# 17. Uma visão geral dos shorthands

Podemos visualizar:

```text
                 CSS GRID
                    │
                    ↓
             PROPRIEDADES
                    │
         ┌──────────┼──────────┐
         ↓          ↓          ↓
       rows      columns      areas
         │          │          │
         └──────────┼──────────┘
                    ↓
             grid-template
```

### Corte mental

```text
grid-template
→ "atalho para definir a estrutura principal"
```

---

# 18. Por que utilizar `grid-template`?

A principal vantagem é concentrar a configuração.

Em vez de:

```css
.grid {
  grid-template-rows: 100px 50px;
  grid-template-columns: 1fr 1fr;
}
```

podemos escrever:

```css
.grid {
  grid-template: 100px 50px / 1fr 1fr;
}
```

Isso pode deixar algumas configurações mais compactas.

Por outro lado, uma declaração muito complexa pode ser mais difícil de ler.

---

# 19. Legibilidade é importante

Considere:

```css
grid-template: 100px 50px 200px / 100px 1fr 50px;
```

Funciona, mas pode ser difícil interpretar rapidamente.

Podemos quebrar visualmente:

```css
grid-template:
  100px
  50px
  200px
  /
  100px
  1fr
  50px;
```

Essa forma continua representando:

```text
LINHAS
100px
50px
200px

COLUNAS
100px
1fr
50px
```

### Regra prática

> **Uma declaração pode ser compacta, mas deve continuar fácil de entender.**

---

# 20. Comparação direta

## Forma individual

```css
.grid {
  grid-template-rows:
    100px
    50px;

  grid-template-columns:
    1fr
    1fr;
}
```

### Leitura

```text
rows
 ↓
100px / 50px

columns
 ↓
1fr / 1fr
```

---

## Forma shorthand

```css
.grid {
  grid-template:
    100px 50px
    /
    1fr 1fr;
}
```

### Leitura

```text
antes da /
→ rows

depois da /
→ columns
```

---

# 21. Exemplo de layout completo

Considere:

```css
.layout {
  display: grid;

  grid-template:
    60px 1fr 50px
    /
    200px 1fr 100px;
}
```

Podemos interpretar:

```text
LINHAS
60px
1fr
50px

COLUNAS
200px
1fr
100px
```

Visualmente:

```text
┌────────────┬──────────────────┬──────────┐
│            │                  │          │ 60px
├────────────┼──────────────────┼──────────┤
│            │                  │          │
│            │                  │          │ 1fr
│  200px     │       1fr        │ 100px    │
│            │                  │          │
├────────────┼──────────────────┼──────────┤
│            │                  │          │ 50px
└────────────┴──────────────────┴──────────┘
```

---

# 22. Mapa mental — `grid-template`

```text
                    grid-template
                          │
               ┌──────────┴──────────┐
               ↓                     ↓
            ANTES DA /          DEPOIS DA /
               │                     │
               ↓                     ↓
             ROWS                 COLUMNS
               │                     │
               ↓                     ↓
          linhas/altura         colunas/largura
               │                     │
               ├── px                ├── px
               ├── %                 ├── %
               ├── fr                ├── fr
               ├── auto              ├── auto
               └── repeat()          └── repeat()
```

### Corte mental ①

```text
grid-template:
    [ROWS] / [COLUMNS]
```

Essa é a forma mais importante de memorizar a sintaxe.

---

# 23. Mapa mental — leitura da declaração

Considere:

```css
grid-template:
  100px 1fr auto
  /
  200px 1fr 50px;
```

Leia assim:

```text
ANTES DA /
100px
1fr
auto
 ↓
LINHAS
```

```text
DEPOIS DA /
200px
1fr
50px
 ↓
COLUNAS
```

### Corte mental ②

```text
           /
          / \
         /   \
      ROWS   COLUMNS
```

---

# 24. Mapa mental — shorthand

```text
             grid-template
                    │
                    ↓
                SHORTHAND
                    │
         ┌──────────┼──────────┐
         ↓          ↓          ↓
       areas       rows      columns
         │          │          │
         │          │          │
         └──────────┼──────────┘
                    ↓
             estrutura do Grid
```

---

# 25. Uma observação importante sobre `grid-template`

Embora o conceito geral seja:

```text
grid-template
→ shorthand relacionado a rows, columns e areas
```

a sintaxe completa do shorthand possui regras específicas.

Por isso, é importante distinguir entre:

```css
grid-template:
```

e as propriedades individuais:

```css
grid-template-rows:
grid-template-columns:
grid-template-areas:
```

Para o uso básico desta etapa, o ponto essencial é dominar:

```css
grid-template:
  rows
  /
  columns;
```

---

# 26. ⚠️ Não confundir com `grid`

Existe também outra propriedade shorthand:

```css
grid
```

Ela possui uma abrangência maior e uma sintaxe diferente.

Portanto, neste ponto:

```text
grid-template
```

não significa:

```text
grid
```

São propriedades diferentes.

A ideia desta documentação é focar no:

```css
grid-template
```

como shorthand relacionado à definição da estrutura de template do Grid.

---

# 27. Exemplo com `repeat()`

```css
.grid {
  display: grid;

  grid-template:
    100px 200px
    /
    repeat(3, 1fr);
}
```

Interpretação:

```text
LINHAS
100px
200px

COLUNAS
1fr
1fr
1fr
```

Visual:

```text
┌────────┬────────┬────────┐
│        │        │        │ 100px
├────────┼────────┼────────┤
│        │        │        │ 200px
└────────┴────────┴────────┘
```

---

# 28. Exemplo com `auto`

```css
.grid {
  display: grid;

  grid-template:
    auto 200px auto
    /
    100px 1fr 100px;
}
```

Temos:

```text
LINHAS
auto
200px
auto

COLUNAS
100px
1fr
100px
```

Isso permite misturar tamanhos fixos e automáticos.

---

# 29. Mapa mental para revisão rápida

```text
┌───────────────────────────────────────┐
│           GRID-TEMPLATE               │
├───────────────────────────────────────┤
│                                       │
│        grid-template:                 │
│                                       │
│          ROWS / COLUMNS               │
│                                       │
├───────────────────────────────────────┤
│ ROWS                                  │
│ → linhas                              │
│ → altura                              │
│ → antes da /                          │
├───────────────────────────────────────┤
│ COLUMNS                               │
│ → colunas                             │
│ → largura                             │
│ → depois da /                         │
└───────────────────────────────────────┘
```

---

# 30. 🧠 Regra para memorizar

Quando encontrar:

```css
grid-template:
```

primeiro procure:

```text
/
```

Depois:

```text
ANTES
↓
ROWS
```

e:

```text
DEPOIS
↓
COLUMNS
```

### Fórmula mental

```text
grid-template: ROWS / COLUMNS;
```

Ou simplesmente:

```text
             /
            / \
         LINHAS COLUNAS
```

---

# 31. Exemplo final

```css
.layout {
  display: grid;

  grid-template:
    60px 1fr 40px
    /
    150px 2fr 100px;
}
```

Leia assim:

```text
                 GRID
                  │
                  ↓
           grid-template
                  │
        ┌─────────┴─────────┐
        ↓                   ↓
      ROWS               COLUMNS
        │                   │
        ↓                   ↓
60px  1fr  40px       150px  2fr  100px
```

Estrutura:

```text
┌─────────┬────────────────────┬─────────┐
│         │                    │         │ 60px
├─────────┼────────────────────┼─────────┤
│         │                    │         │
│         │                    │         │ 1fr
├─────────┼────────────────────┼─────────┤
│         │                    │         │ 40px
└─────────┴────────────────────┴─────────┘
  150px          2fr              100px
```

---

# 32. 📌 Resumo final

```text
grid-template
      ↓
shorthand
      ↓
estrutura do Grid
```

A regra principal:

```css
grid-template:
  rows
  /
  columns;
```

Ou:

```text
ANTES DA /
→ linhas

DEPOIS DA /
→ colunas
```

Pode utilizar valores como:

```css
px
%
fr
auto
repeat()
```

Exemplo:

```css
.grid {
  display: grid;

  grid-template:
    100px 1fr 50px
    /
    200px repeat(2, 1fr);
}
```

Leitura:

```text
LINHAS
100px
1fr
50px

COLUNAS
200px
1fr
1fr
```

### 🧠 Para guardar

> **`grid-template` é um shorthand para definir a estrutura do Grid de forma compacta. Na sintaxe básica, tudo antes de `/` representa as linhas e tudo depois de `/` representa as colunas.**
