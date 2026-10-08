# CSS Grid — `grid-template-areas` e `grid-area`

## 1. O que é `grid-template-areas`?

A propriedade:

```css id="48nqj4"
grid-template-areas
```

permite **nomear áreas do Grid** e, a partir desses nomes, organizar visualmente os elementos da página.

Em vez de pensar apenas em:

```text
coluna 1 | coluna 2 | coluna 3
```

podemos pensar em:

```text
┌──────────┬──────────┬──────────┐
│   logo   │   nav    │   advert │
├──────────┼──────────┼──────────┤
│ side-nav │ content  │  advert  │
├──────────┼──────────┼──────────┤
│ side-nav │ footer   │  advert  │
└──────────┴──────────┴──────────┘
```

Cada região recebe um **nome**.

---

# 2. A ideia principal

Existem duas propriedades que trabalham juntas:

```css id="2zi7ax"
grid-template-areas
```

e:

```css id="5vpw5f"
grid-area
```

Podemos pensar assim:

```text id="4jzr2m"
Grid Container
      │
      ↓
grid-template-areas
      │
      ↓
define o mapa das áreas
      │
      ↓
Grid Items
      │
      ↓
grid-area
      │
      ↓
cada item escolhe sua área
```

### Regra mental

> **`grid-template-areas` cria o mapa. `grid-area` coloca cada item no mapa.**

---

# 3. Estrutura básica

Exemplo:

```css id="vslc5e"
.grid {
  display: grid;

  grid-template-areas:
    "logo nav advert"
    "side-nav content advert"
    "side-nav footer advert";
}
```

Esse código representa:

```text id="odkk1f"
┌──────────┬──────────┬──────────┐
│   logo   │   nav    │  advert  │
├──────────┼──────────┼──────────┤
│ side-nav │ content  │  advert  │
├──────────┼──────────┼──────────┤
│ side-nav │ footer   │  advert  │
└──────────┴──────────┴──────────┘
```

Agora o Grid possui um mapa visual da estrutura.

---

# 4. Cada string representa uma linha

A sintaxe utiliza strings:

```css id="ynk02b"
grid-template-areas:
  "logo nav advert"
  "side-nav content advert"
  "side-nav footer advert";
```

Cada linha entre aspas representa uma **linha do Grid**.

Podemos separar mentalmente:

```text id="s4o8s0"
"logo nav advert"
        ↓
      linha 1
```

```text id="sk4dzw"
"side-nav content advert"
        ↓
      linha 2
```

```text id="9ui6kr"
"side-nav footer advert"
        ↓
      linha 3
```

---

# 5. A quantidade de valores define as colunas

Observe:

```css id="k29h15"
"logo nav advert"
```

Temos três nomes:

```text id="s6hxir"
logo
nav
advert
```

Portanto, temos três colunas nessa linha.

```text id="1q4w5e"
logo | nav | advert
```

Se tivermos:

```css id="5y5ef9"
"logo nav"
```

teremos duas colunas:

```text id="o9o3pc"
logo | nav
```

### Regra mental

> **Cada nome dentro de uma linha representa uma célula/posição de coluna.**

---

# 6. As linhas precisam manter a mesma quantidade de colunas

Considere:

```css id="vrda3s"
grid-template-areas:
  "logo nav advert"
  "side-nav content advert"
  "side-nav footer";
```

A terceira linha possui apenas duas posições:

```text id="78elv6"
logo | nav | advert
side | content | advert
side | footer
```

Isso quebra a estrutura esperada do Grid.

Para um mapa válido, as linhas precisam formar uma grade consistente:

```css id="6ikj0w"
grid-template-areas:
  "logo nav advert"
  "side-nav content advert"
  "side-nav footer advert";
```

---

# 7. Uma mesma área pode ocupar várias células

Uma das maiores vantagens de `grid-template-areas` é que podemos repetir o mesmo nome.

Exemplo:

```css id="f0x7c5"
grid-template-areas:
  "logo nav advert"
  "side-nav content advert"
  "side-nav content advert";
```

Agora:

```text id="s1b8kn"
┌──────────┬──────────┬──────────┐
│   logo   │   nav    │  advert  │
├──────────┼──────────┼──────────┤
│ side-nav │ content  │  advert  │
├──────────┼──────────┼──────────┤
│ side-nav │ content  │  advert  │
└──────────┴──────────┴──────────┘
```

A área `side-nav` ocupa duas linhas:

```text id="81z4s7"
side-nav
   ↓
┌──────────┐
│          │
├──────────┤
│          │
└──────────┘
```

E `content` também:

```text id="cda8a5"
content
   ↓
┌──────────┐
│          │
├──────────┤
│          │
└──────────┘
```

---

# 8. Uma área pode ocupar várias linhas

Se repetirmos:

```css id="al5rj0"
side-nav
```

em duas linhas:

```css id="7d0u2v"
grid-template-areas:
  "logo nav advert"
  "side-nav content advert"
  "side-nav content advert";
```

a área será expandida verticalmente.

Visualmente:

```text id="u1v2gw"
┌──────────┐
│          │
│ side-nav │
│          │
├──────────┤
│          │
│ side-nav │
│          │
└──────────┘
```

---

# 9. Uma área pode ocupar várias colunas

Também podemos repetir o mesmo nome horizontalmente.

Exemplo:

```css id="xpp5pd"
grid-template-areas:
  "logo logo nav"
  "side content advert";
```

Agora:

```text id="1l1bzg"
┌────────────────────┬──────────┐
│        logo        │   nav    │
├──────────┬─────────┼──────────┤
│   side   │ content │  advert  │
└──────────┴─────────┴──────────┘
```

A área `logo` ocupa duas colunas:

```text id="pwzj6y"
┌────────────────────┐
│        logo        │
└────────────────────┘
```

---

# 10. Formas válidas das áreas

Uma área precisa formar uma região retangular.

Por exemplo:

```css id="cmf2td"
grid-template-areas:
  "logo logo"
  "logo nav";
```

A área `logo` forma um retângulo:

```text id="15dcdy"
┌────────┬────────┐
│  logo  │  logo  │
├────────┼────────┤
│  logo  │  nav   │
└────────┴────────┘
```

Isso funciona.

---

# 11. Formas inválidas

Uma área não pode formar uma estrutura em `L`.

Por exemplo:

```css id="aywcy3"
grid-template-areas:
  "logo logo"
  "logo nav"
  "content logo";
```

A área `logo` ficaria espalhada de uma maneira que não representa um único retângulo.

Visualmente:

```text id="teq8bj"
┌──────┬──────┐
│ logo │ logo │
├──────┼──────┤
│ logo │ nav  │
├──────┼──────┤
│ cont │ logo │
└──────┴──────┘
```

A área `logo` não forma um retângulo simples.

### Regra importante

> **Uma área nomeada deve ocupar um retângulo contínuo.**

Ela pode expandir:

```text id="63ukid"
←→ horizontalmente
↕ verticalmente
```

mas não pode fazer uma curva ou formar um `L`.

---

# 12. Exemplo de uma estrutura de site

Podemos imaginar:

```text id="0gy2nw"
┌──────────────────────────────────────────────┐
│                    LOGO                      │
├─────────────────┬────────────────────────────┤
│   SIDE NAV      │          CONTENT           │
├─────────────────┼────────────────────────────┤
│   SIDE NAV      │          CONTENT           │
├─────────────────┴────────────────────────────┤
│                   FOOTER                     │
└──────────────────────────────────────────────┘
```

Podemos transformar essa estrutura em:

```css id="lq9s8q"
grid-template-areas:
  "logo logo"
  "side-nav content"
  "side-nav content"
  "footer footer";
```

---

# 13. `grid-area`

Depois de definir o mapa, precisamos informar aos itens qual área eles devem ocupar.

Exemplo:

```css id="abkcsq"
.logo {
  grid-area: logo;
}

.nav {
  grid-area: nav;
}

.side-nav {
  grid-area: side-nav;
}

.content {
  grid-area: content;
}

.advert {
  grid-area: advert;
}

.footer {
  grid-area: footer;
}
```

Agora cada item possui uma área correspondente.

---

# 14. `grid-template-areas` + `grid-area`

Temos:

```css id="51v1cd"
.grid {
  display: grid;

  grid-template-areas:
    "logo nav advert"
    "side-nav content advert"
    "side-nav footer advert";
}
```

E:

```css id="zryxjb"
.logo {
  grid-area: logo;
}

.nav {
  grid-area: nav;
}

.side-nav {
  grid-area: side-nav;
}

.content {
  grid-area: content;
}

.advert {
  grid-area: advert;
}

.footer {
  grid-area: footer;
}
```

Resultado:

```text id="1ydj1w"
┌──────────┬──────────┬──────────┐
│   logo   │   nav    │  advert  │
├──────────┼──────────┼──────────┤
│ side-nav │ content  │  advert  │
├──────────┼──────────┼──────────┤
│ side-nav │ footer   │  advert  │
└──────────┴──────────┴──────────┘
```

---

# 15. O nome da área pode ser qualquer um

Os nomes:

```text id="8hf0gs"
logo
nav
content
footer
advert
```

não são palavras reservadas.

Podemos usar nomes diferentes:

```css id="7s6t2j"
grid-template-areas:
  "a b c"
  "d e c"
  "d f c";
```

Isso funciona.

Porém, usar nomes sem significado:

```text id="1y96fb"
a
b
c
d
e
f
```

torna o código difícil de entender.

É muito melhor utilizar nomes semânticos:

```text id="gf7p3j"
logo
nav
content
side-nav
advert
footer
```

### Boa prática

> **Nomeie as áreas de acordo com a função que elas possuem no layout.**

---

# 16. Nome da classe e nome da área são coisas diferentes

Considere:

```css id="ivk0tw"
.navigation {
  grid-area: nav;
}
```

Aqui temos:

```text id="mo88oz"
.navigation
     ↓
nome da classe CSS

nav
     ↓
nome da área do Grid
```

Eles não precisam ser iguais.

Mas mantê-los semelhantes pode facilitar a leitura:

```css id="ct4wrm"
.nav {
  grid-area: nav;
}
```

---

# 17. O Grid Area depende do mapa

Se temos:

```css id="nf4i2m"
grid-template-areas:
  "logo nav"
  "content footer";
```

e:

```css id="xym7y7"
.footer {
  grid-area: footer;
}
```

o item `.footer` irá para a região marcada como:

```text id="g0l5xm"
footer
```

Se mudarmos o mapa:

```css id="7d9lx6"
grid-template-areas:
  "logo footer"
  "content nav";
```

o `.footer` muda automaticamente de posição.

Isso é uma das maiores vantagens desse recurso.

---

# 18. Alterando o layout sem alterar os itens

Imagine:

```css id="rz2sv6"
.logo {
  grid-area: logo;
}

.nav {
  grid-area: nav;
}

.content {
  grid-area: content;
}

.footer {
  grid-area: footer;
}
```

Esses itens continuam com os mesmos nomes.

Podemos alterar somente:

```css id="iwsu4y"
grid-template-areas
```

Por exemplo:

```css id="ba7w5a"
grid-template-areas:
  "logo logo"
  "nav content"
  "footer footer";
```

Depois:

```css id="q94d1a"
grid-template-areas:
  "nav logo"
  "content content"
  "footer footer";
```

O layout muda sem precisarmos redefinir:

```css id="dkb3zg"
grid-area
```

de cada item.

---

# 19. `grid-template-areas` em Media Queries

Essa característica é excelente para layouts responsivos.

Podemos ter um layout desktop:

```css id="26rtp1"
.grid {
  display: grid;

  grid-template-areas:
    "logo nav advert"
    "side-nav content advert"
    "side-nav footer advert";
}
```

E alterar a organização em uma Media Query:

```css id="6qpgml"
@media (max-width: 500px) {
  .grid {
    grid-template-areas:
      "logo"
      "nav"
      "content"
      "advert"
      "footer";
  }
}
```

No desktop:

```text id="65xnsl"
┌────────┬────────┬────────┐
│  logo  │  nav   │ advert │
├────────┼────────┼────────┤
│  side  │content │ advert │
├────────┼────────┼────────┤
│  side  │ footer │ advert │
└────────┴────────┴────────┘
```

No mobile:

```text id="v1voin"
┌──────────┐
│   logo   │
├──────────┤
│   nav    │
├──────────┤
│ content  │
├──────────┤
│  advert  │
├──────────┤
│  footer  │
└──────────┘
```

Os elementos continuam sendo os mesmos.

O que mudou foi apenas o **mapa do Grid**.

---

# 20. Responsividade com duas colunas

Nem todo layout mobile precisa ter uma única coluna.

Podemos fazer:

```css id="dwf0w5"
@media (max-width: 600px) {
  .grid {
    grid-template-areas:
      "logo logo"
      "nav nav"
      "content advert"
      "footer footer";
  }
}
```

Resultado:

```text id="cqjcsj"
┌──────────┬──────────┐
│          logo       │
├──────────┴──────────┤
│          nav        │
├──────────┬──────────┤
│ content  │ advert   │
├──────────┴──────────┤
│        footer       │
└─────────────────────┘
```

Isso é útil quando determinados elementos continuam pequenos o suficiente para dividir a tela em dispositivos menores.

---

# 21. A ordem do HTML continua importante

`grid-template-areas` altera a **apresentação visual**, mas não deve ser usado para criar uma ordem de leitura incoerente.

Considere uma estrutura HTML:

```html id="fjzv4n"
<header>...</header>
<nav>...</nav>
<main>...</main>
<aside>...</aside>
<footer>...</footer>
```

A ordem faz sentido semanticamente:

```text id="y3suvi"
header
 ↓
nav
 ↓
main
 ↓
aside
 ↓
footer
```

Mesmo que visualmente desejemos posicioná-los de outra maneira:

```text id="b2z1pu"
┌───────────┬───────────┐
│   header  │   header  │
├───────────┼───────────┤
│   nav     │   main    │
├───────────┼───────────┤
│   aside   │   main    │
├───────────┴───────────┤
│        footer         │
└───────────────────────┘
```

A estrutura HTML continua sendo a referência para:

* leitura;
* acessibilidade;
* interpretação por tecnologias assistivas;
* interpretação do documento pelos mecanismos de busca.

### Regra importante

> **Use o Grid para alterar a apresentação visual, mas mantenha o HTML em uma ordem lógica e semântica.**

---

# 22. Visualização x estrutura

Podemos separar:

```text id="av2c9g"
HTML
 ↓
estrutura e significado
```

e:

```text id="6n6f8q"
CSS Grid
 ↓
organização visual
```

Isso permite que:

```text id="fjod6l"
estrutura semântica
        +
layout visual
```

sejam tratados separadamente.

---

# 23. O ponto `.`

Dentro de:

```css id="bcfev9"
grid-template-areas
```

podemos utilizar:

```text id="nk0a4r"
.
```

O ponto representa uma **célula vazia**.

Exemplo:

```css id="d1qj8w"
grid-template-areas:
  "logo nav ."
  "content content advert"
  "footer footer footer";
```

Visualmente:

```text id="7ssyby"
┌────────┬────────┬────────┐
│  logo  │  nav   │   .    │
├────────┼────────┼────────┤
│       content   │ advert │
├────────┴────────┼────────┤
│       footer             │
└──────────────────────────┘
```

A posição marcada com:

```text id="fi13a6"
.
```

fica vazia.

---

# 24. Vários pontos

Podemos utilizar vários pontos:

```css id="cwi9fi"
grid-template-areas:
  "logo . ."
  "nav content ."
  "footer footer advert";
```

Isso cria células vazias nas posições indicadas.

O ponto pode ser útil quando queremos criar um espaço proposital no layout.

---

# 25. Estrutura de um layout completo

Um exemplo:

```css id="x7u0yb"
.grid {
  display: grid;

  grid-template-areas:
    "logo logo advert"
    "nav content advert"
    "side-nav content ."
    "footer footer footer";
}
```

Mapa:

```text id="5pw0f1"
┌──────┬──────┬──────┐
│ logo │ logo │advert│
├──────┼──────┼──────┤
│ nav  │content│advert│
├──────┼──────┼──────┤
│ side │content│  .   │
├──────┴──────┴──────┤
│       footer        │
└─────────────────────┘
```

Isso é praticamente um desenho textual do layout.

---

# 26. Mapa mental — conceito principal

```text id="p2x6f0"
                 CSS GRID
                    │
                    ↓
       grid-template-areas
                    │
                    ↓
             CRIA UM MAPA
                    │
        ┌───────────┼───────────┐
        ↓           ↓           ↓
      linhas      colunas      áreas
        │           │           │
        └───────────┼───────────┘
                    ↓
              nomes das áreas
                    │
                    ↓
                grid-area
                    │
                    ↓
             posiciona o item
```

### Corte mental ①

```text id="r8b58n"
template-areas
      ↓
"DESENHE O MAPA"
```

```text id="ra3nhp"
grid-area
      ↓
"COLOQUE O ITEM NO MAPA"
```

---

# 27. Mapa mental — sintaxe

```text id="kse036"
grid-template-areas:
        │
        ├── "linha 1"
        │
        ├── "linha 2"
        │
        └── "linha 3"
```

Exemplo:

```css id="ihizyn"
grid-template-areas:
  "logo nav advert"
  "side content advert"
  "side footer advert";
```

Visualização:

```text id="7a0xum"
"logo nav advert"
      ↓
┌──────┬──────┬──────┐
│ logo │ nav  │advert│
└──────┴──────┴──────┘

"side content advert"
      ↓
┌──────┬──────┬──────┐
│ side │content│advert│
└──────┴──────┴──────┘

"side footer advert"
      ↓
┌──────┬──────┬──────┐
│ side │footer│advert│
└──────┴──────┴──────┘
```

---

# 28. Mapa mental — áreas repetidas

```text id="8z5br6"
MESMO NOME
     │
     ↓
REPRESENTA A MESMA ÁREA
     │
     ├── horizontal
     │      ↓
     │   ocupa várias colunas
     │
     └── vertical
            ↓
         ocupa várias linhas
```

Exemplo:

```css id="um0l47"
"logo logo nav"
```

```text id="o7q0mv"
┌──────────────┬──────┐
│     logo     │ nav  │
└──────────────┴──────┘
```

Outro exemplo:

```css id="d78e7y"
"side content"
"side content"
```

```text id="w1sd5e"
┌──────┬─────────┐
│ side │ content │
├──────┼─────────┤
│ side │ content │
└──────┴─────────┘
```

---

# 29. Mapa mental — regra do retângulo

```text id="hfcq3x"
ÁREA
 │
 ├── pode ocupar 1 célula
 │
 ├── pode ocupar várias colunas
 │
 └── pode ocupar várias linhas
```

Mas:

```text id="z0e8eq"
ÁREA EM "L"
      ↓
     ❌
```

A região precisa continuar sendo um retângulo.

---

# 30. Mapa mental — responsividade

```text id="wvebht"
              GRID
                │
                ↓
     grid-template-areas
                │
       ┌────────┴────────┐
       ↓                 ↓
    DESKTOP             MOBILE
       │                 │
       ↓                 ↓
"logo nav content"   "logo"
"side content..."    "nav"
"side footer..."     "content"
                     "footer"
```

A ideia:

```text id="dtk9qp"
mesmos elementos
      +
novo mapa
      ↓
novo layout
```

---

# 31. Mapa mental — HTML + CSS Grid

```text id="f0lhw6"
                   WEB PAGE
                      │
         ┌────────────┴────────────┐
         │                         │
        HTML                       CSS
         │                         │
         ↓                         ↓
   estrutura lógica          estrutura visual
         │                         │
         │                  grid-template-areas
         │                         │
         │                         ↓
         │                    layout visual
         │                         │
         └────────────┬────────────┘
                      ↓
                  página final
```

### Corte mental ②

```text id="au6mf0"
HTML
→ "Qual é a ordem e o significado?"
```

```text id="u5n3gp"
Grid
→ "Como isso será organizado visualmente?"
```

---

# 32. Exemplo completo

### HTML

```html id="3s5k8j"
<div class="layout">
  <header class="logo">Logo</header>

  <nav class="nav">Navegação</nav>

  <aside class="side-nav">Menu lateral</aside>

  <main class="content">Conteúdo</main>

  <aside class="advert">Publicidade</aside>

  <footer class="footer">Rodapé</footer>
</div>
```

### CSS

```css id="dwu5j4"
.layout {
  display: grid;

  grid-template-areas:
    "logo nav advert"
    "side-nav content advert"
    "side-nav content advert"
    "footer footer footer";
}

.logo {
  grid-area: logo;
}

.nav {
  grid-area: nav;
}

.side-nav {
  grid-area: side-nav;
}

.content {
  grid-area: content;
}

.advert {
  grid-area: advert;
}

.footer {
  grid-area: footer;
}
```

Resultado:

```text id="l2gq2h"
┌──────────┬──────────┬──────────┐
│   LOGO   │   NAV    │  ADVERT  │
├──────────┼──────────┼──────────┤
│          │          │          │
│ SIDE NAV │ CONTENT  │  ADVERT  │
│          │          │          │
├──────────┼──────────┼──────────┤
│ SIDE NAV │ CONTENT  │  ADVERT  │
│          │          │          │
├──────────┴──────────┴──────────┤
│             FOOTER             │
└────────────────────────────────┘
```

---

# 33. Responsividade do exemplo

Podemos reorganizar o mesmo layout:

```css id="6j1q5r"
@media (max-width: 500px) {
  .layout {
    grid-template-areas:
      "logo"
      "nav"
      "content"
      "advert"
      "footer";
  }
}
```

Agora:

```text id="3eay1u"
┌──────────────┐
│     LOGO     │
├──────────────┤
│     NAV      │
├──────────────┤
│   CONTENT    │
├──────────────┤
│   ADVERT     │
├──────────────┤
│    FOOTER    │
└──────────────┘
```

Os itens continuam usando:

```css id="ps2ksv"
grid-area: logo;
grid-area: nav;
grid-area: content;
grid-area: advert;
grid-area: footer;
```

Apenas o mapa mudou.

---

# 34. Um segundo layout mobile

Podemos também utilizar duas colunas:

```css id="j6i5hm"
@media (max-width: 600px) {
  .layout {
    grid-template-areas:
      "logo logo"
      "nav nav"
      "content advert"
      "footer footer";
  }
}
```

Resultado:

```text id="f8vg6x"
┌───────────────┬───────────────┐
│             LOGO              │
├───────────────┴───────────────┤
│              NAV              │
├───────────────┬───────────────┤
│    CONTENT    │    ADVERT     │
├───────────────┴───────────────┤
│             FOOTER            │
└───────────────────────────────┘
```

---

# 35. `grid-template-areas` + `grid-template-columns`

Podemos utilizar as áreas para definir a estrutura e, ao mesmo tempo, determinar o tamanho das colunas.

Exemplo:

```css id="5rx5hv"
.layout {
  display: grid;

  grid-template-columns:
    100px
    1fr
    50px;

  grid-template-areas:
    "logo nav advert"
    "side-nav content advert"
    "side-nav footer advert";
}
```

Nesse caso:

```text id="b9to0k"
Coluna 1 → 100px
Coluna 2 → 1fr
Coluna 3 → 50px
```

E o mapa:

```text id="umukra"
logo       nav       advert
side-nav   content   advert
side-nav   footer    advert
```

Os dois trabalham juntos.

---

# 36. `grid-template-areas` + `grid-template-rows`

Também podemos definir alturas:

```css id="m0yc68"
.layout {
  display: grid;

  grid-template-columns:
    100px
    1fr
    50px;

  grid-template-rows:
    50px
    200px
    50px;

  grid-template-areas:
    "logo nav advert"
    "side-nav content advert"
    "side-nav footer advert";
}
```

Agora temos:

```text id="6c0cx0"
COLUNAS
100px | 1fr | 50px

LINHAS
50px
200px
50px
```

Isso permite controlar tanto:

* a estrutura;
* os tamanhos;
* a posição visual.

---

# 37. ⚠️ Manutenção do layout

`grid-template-areas` é excelente para definir a estrutura **macro** de uma página.

Por exemplo:

```text id="5rr8oy"
header
sidebar
content
advert
footer
```

Porém, criar centenas de áreas para cada pequeno componente pode deixar o código difícil de manter.

Uma estratégia mais organizada é usar Grid Areas para a estrutura principal:

```text id="s8meu7"
PÁGINA
├── Header
├── Navigation
├── Main
├── Sidebar
└── Footer
```

e deixar componentes internos utilizarem seu próprio sistema de layout quando necessário.

---

# 38. 🧠 Resumo mental definitivo

```text id="7kjwkm"
              GRID TEMPLATE AREAS
                       │
                       ↓
                CRIA UM MAPA
                       │
                       ↓
        ┌──────────────┼──────────────┐
        │              │              │
      NOME          REPETIÇÃO         .
        │              │              │
        ↓              ↓              ↓
    identifica     expande uma     cria área
      a área          área          vazia
        │
        ↓
    grid-area
        │
        ↓
   coloca o item
   naquela área
```

### Corte mental ③

```text id="0g7f5y"
TEMPLATE AREAS
↓
"MAPA"

GRID AREA
↓
"POSIÇÃO"
```

---

# 39. Checklist mental para escrever o código

```text id="sfyr6d"
1. Ative o Grid
   ↓
display: grid;

2. Desenhe o mapa
   ↓
grid-template-areas

3. Dê nomes semânticos
   ↓
logo / nav / content / footer

4. Vincule os itens
   ↓
grid-area

5. Defina tamanhos
   ↓
grid-template-columns
grid-template-rows

6. Torne responsivo
   ↓
@media + novo grid-template-areas

7. Mantenha a ordem do HTML lógica
```

---

# 40. 📌 Regra final para memorizar

> **`grid-template-areas` transforma o Grid em um mapa visual nomeado.**

```css id="on41pw"
grid-template-areas:
  "header header"
  "nav content"
  "footer footer";
```

Pode ser lido como:

```text id="0xg4tr"
HEADER | HEADER
NAV    | CONTENT
FOOTER | FOOTER
```

Depois:

```css id="hd3g93"
.header {
  grid-area: header;
}

.nav {
  grid-area: nav;
}

.content {
  grid-area: content;
}

.footer {
  grid-area: footer;
}
```

A relação fica:

```text id="2ik3da"
grid-template-areas
        ↓
      MAPA
        ↓
   ┌─────────────┐
   │ header      │
   │ nav content │
   │ footer      │
   └─────────────┘
        ↓
grid-area
        ↓
   POSICIONA OS
      ITENS
```

### 🧠 Três frases para guardar

```text id="6f7gvc"
grid-template-areas
→ "desenha o layout"

grid-area
→ "liga o elemento à área"

"."
→ "deixa a célula vazia"
```

E a regra estrutural mais importante:

> **As áreas nomeadas devem formar regiões retangulares; o Grid não permite que uma mesma área forme uma estrutura em `L`.**
