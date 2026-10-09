# CSS Grid Layout — `grid-template-areas` e `grid-area`

## Índice

1. [O que é `grid-template-areas`?](#1-o-que-é-grid-template-areas)
2. [A ideia principal](#2-a-ideia-principal)
3. [Estrutura básica](#3-estrutura-básica)
4. [Cada string representa uma linha](#4-cada-string-representa-uma-linha)
5. [A quantidade de valores define as colunas](#5-a-quantidade-de-valores-define-as-colunas)
6. [As linhas precisam manter a mesma quantidade de colunas](#6-as-linhas-precisam-manter-a-mesma-quantidade-de-colunas)
7. [Uma mesma área pode ocupar várias células](#7-uma-mesma-área-pode-ocupar-várias-células)
8. [Uma área pode ocupar várias linhas](#8-uma-área-pode-ocupar-várias-linhas)
9. [Uma área pode ocupar várias colunas](#9-uma-área-pode-ocupar-várias-colunas)
10. [Formas válidas das áreas](#10-formas-válidas-das-áreas)
11. [Formas inválidas](#11-formas-inválidas)
12. [Exemplo de uma estrutura de site](#12-exemplo-de-uma-estrutura-de-site)
13. [`grid-area`](#13-grid-area)
14. [`grid-template-areas` + `grid-area`](#14-grid-template-areas--grid-area)
15. [O nome da área pode ser qualquer um](#15-o-nome-da-área-pode-ser-qualquer-um)
16. [Nome da classe e nome da área são coisas diferentes](#16-nome-da-classe-e-nome-da-área-são-coisas-diferentes)
17. [O Grid Area depende do mapa](#17-o-grid-area-depende-do-mapa)
18. [Alterando o layout sem alterar os itens](#18-alterando-o-layout-sem-alterar-os-itens)
19. [`grid-template-areas` em Media Queries](#19-grid-template-areas-em-media-queries)
20. [Responsividade com duas colunas](#20-responsividade-com-duas-colunas)
21. [A ordem do HTML continua importante](#21-a-ordem-do-html-continua-importante)
22. [Visualização x estrutura](#22-visualização-x-estrutura)
23. [O ponto `.`](#23-o-ponto-)
24. [Vários pontos](#24-vários-pontos)
25. [Estrutura de um layout completo](#25-estrutura-de-um-layout-completo)
26. [Mapa mental — conceito principal](#26-mapa-mental--conceito-principal)
27. [Mapa mental — sintaxe](#27-mapa-mental--sintaxe)
28. [Mapa mental — áreas repetidas](#28-mapa-mental--áreas-repetidas)
29. [Mapa mental — regra do retângulo](#29-mapa-mental--regra-do-retângulo)
30. [Mapa mental — responsividade](#30-mapa-mental--responsividade)
31. [Mapa mental — HTML + CSS Grid](#31-mapa-mental--html--css-grid)
32. [Exemplo completo](#32-exemplo-completo)
33. [Responsividade do exemplo](#33-responsividade-do-exemplo)
34. [Um segundo layout mobile](#34-um-segundo-layout-mobile)
35. [`grid-template-areas` + `grid-template-columns`](#35-grid-template-areas--grid-template-columns)
36. [`grid-template-areas` + `grid-template-rows`](#36-grid-template-areas--grid-template-rows)
37. [⚠️ Manutenção do layout](#37-️-manutenção-do-layout)
38. [🧠 Resumo mental definitivo](#38-🧠-resumo-mental-definitivo)
39. [Checklist mental para escrever o código](#39-checklist-mental-para-escrever-o-código)
40. [📌 Regra final para memorizar](#40-📌-regra-final-para-memorizar)

---

## 1. O que é `grid-template-areas`?

A propriedade:

```css
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
│   logo   │   nav    │  advert  │
├──────────┼──────────┼──────────┤
│ side-nav │ content  │  advert  │
├──────────┼──────────┼──────────┤
│ side-nav │ footer   │  advert  │
└──────────┴──────────┴──────────┘
```

Cada região recebe um **nome**.

---

## 2. A ideia principal

Existem duas propriedades que trabalham juntas:

```css
grid-template-areas
```

e:

```css
grid-area
```

Podemos pensar assim:

```text
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

## 3. Estrutura básica

Exemplo:

```css
.grid {
  display: grid;

  grid-template-areas:
    "logo nav advert"
    "side-nav content advert"
    "side-nav footer advert";
}
```

Esse código representa:

```text
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

## 4. Cada string representa uma linha

A sintaxe utiliza strings:

```css
grid-template-areas:
  "logo nav advert"
  "side-nav content advert"
  "side-nav footer advert";
```

Cada linha entre aspas representa uma **linha do Grid**.

Podemos separar mentalmente:

```text
"logo nav advert"

      ↓

    linha 1
```

```text
"side-nav content advert"

      ↓

    linha 2
```

```text
"side-nav footer advert"

      ↓

    linha 3
```

---

## 5. A quantidade de valores define as colunas

Observe:

```css
"logo nav advert"
```

Temos três nomes:

```text
logo

nav

advert
```

Portanto, temos três colunas nessa linha.

```text
logo | nav | advert
```

Se tivermos:

```css
"logo nav"
```

teremos duas colunas:

```text
logo | nav
```

### Regra mental

> **Cada nome dentro de uma linha representa uma célula/posição de coluna.**

---

## 6. As linhas precisam manter a mesma quantidade de colunas

Considere:

```css
grid-template-areas:
  "logo nav advert"
  "side-nav content advert"
  "side-nav footer";
```

A terceira linha possui apenas duas posições:

```text
logo | nav | advert
side | content | advert
side | footer
```

Isso quebra a estrutura esperada do Grid.

Para um mapa válido, as linhas precisam formar uma grade consistente:

```css
grid-template-areas:
  "logo nav advert"
  "side-nav content advert"
  "side-nav footer advert";
```

---

## 7. Uma mesma área pode ocupar várias células

Uma das maiores vantagens de `grid-template-areas` é que podemos repetir o mesmo nome.

Exemplo:

```css
grid-template-areas:
  "logo nav advert"
  "side-nav content advert"
  "side-nav content advert";
```

Agora:

```text
┌──────────┬──────────┬──────────┐
│   logo   │   nav    │  advert  │
├──────────┼──────────┼──────────┤
│ side-nav │ content  │  advert  │
├──────────┼──────────┼──────────┤
│ side-nav │ content  │  advert  │
└──────────┴──────────┴──────────┘
```

A área `side-nav` ocupa duas linhas:

```text
side-nav

   ↓

┌──────────┐
│          │
├──────────┤
│          │
└──────────┘
```

E `content` também:

```text
content

   ↓

┌──────────┐
│          │
├──────────┤
│          │
└──────────┘
```

---

## 8. Uma área pode ocupar várias linhas

Se repetirmos:

```css
side-nav
```

em duas linhas:

```css
grid-template-areas:
  "logo nav advert"
  "side-nav content advert"
  "side-nav content advert";
```

a área será expandida verticalmente.

Visualmente:

```text
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

## 9. Uma área pode ocupar várias colunas

Também podemos repetir o mesmo nome horizontalmente.

Exemplo:

```css
grid-template-areas:
  "logo logo nav"
  "side content advert";
```

Agora:

```text
┌────────────────────┬──────────┐
│        logo        │   nav    │
├──────────┬─────────┼──────────┤
│   side   │ content │  advert  │
└──────────┴─────────┴──────────┘
```

A área `logo` ocupa duas colunas:

```text
┌────────────────────┐
│        logo        │
└────────────────────┘
```

---

## 10. Formas válidas das áreas

Uma área precisa formar uma região retangular.

Por exemplo:

```css
grid-template-areas:
  "logo logo"
  "logo nav";
```

A área `logo` forma um retângulo:

```text
┌────────┬────────┐
│  logo  │  logo  │
├────────┼────────┤
│  logo  │  nav   │
└────────┴────────┘
```

Isso funciona.

---

## 11. Formas inválidas

Uma área não pode formar uma estrutura em `L`.

Por exemplo:

```css
grid-template-areas:
  "logo logo"
  "logo nav"
  "content logo";
```

A área `logo` ficaria espalhada de uma maneira que não representa um único retângulo.

Visualmente:

```text
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

```text
←→ horizontalmente

↕ verticalmente
```

mas não pode fazer uma curva ou formar um `L`.

---

## 12. Exemplo de uma estrutura de site

Podemos imaginar:

```text
┌──────────────────────────────────────────────┐
│                    LOGO                      │
├─────────────────┬────────────────────────────┤
│   SIDE NAV      │          CONTENT           │
├─────────────────┼────────────────────────────┤
│   SIDE NAV      │          CONTENT           │
├─────────────────┴────────────────────────────┤
│                   FOOTER                      │
└──────────────────────────────────────────────┘
```

Podemos transformar essa estrutura em:

```css
grid-template-areas:
  "logo logo"
  "side-nav content"
  "side-nav content"
  "footer footer";
```

---

## 13. `grid-area`

Depois de definir o mapa, precisamos informar aos itens qual área eles devem ocupar.

Exemplo:

```css
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

## 14. `grid-template-areas` + `grid-area`

Temos:

```css
.grid {
  display: grid;

  grid-template-areas:
    "logo nav advert"
    "side-nav content advert"
    "side-nav footer advert";
}
```

E:

```css
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

```text
┌──────────┬──────────┬──────────┐
│   logo   │   nav    │  advert  │
├──────────┼──────────┼──────────┤
│ side-nav │ content  │  advert  │
├──────────┼──────────┼──────────┤
│ side-nav │ footer   │  advert  │
└──────────┴──────────┴──────────┘
```

---

## 15. O nome da área pode ser qualquer um

Os nomes:

```text
logo
nav
content
footer
advert
```

não são palavras reservadas.

Podemos usar nomes diferentes:

```css
grid-template-areas:
  "a b c"
  "d e c"
  "d f c";
```

Isso funciona.

Porém, usar nomes sem significado:

```text
a

b

c

d

e

f
```

torna o código difícil de entender.

É muito melhor utilizar nomes semânticos:

```text
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

## 16. Nome da classe e nome da área são coisas diferentes

Considere:

```css
.navigation {
  grid-area: nav;
}
```

Aqui temos:

```text
.navigation

     ↓

nome da classe CSS
```

e:

```text
nav

 ↓

nome da área do Grid
```

Eles não precisam ser iguais.

Mas mantê-los semelhantes pode facilitar a leitura:

```css
.nav {
  grid-area: nav;
}
```

---

## 17. O Grid Area depende do mapa

Se temos:

```css
grid-template-areas:
  "logo nav"
  "content footer";
```

e:

```css
.footer {
  grid-area: footer;
}
```

o item `.footer` irá para a região marcada como:

```text
footer
```

Se mudarmos o mapa:

```css
grid-template-areas:
  "logo footer"
  "content nav";
```

o `.footer` muda automaticamente de posição.

Isso é uma das maiores vantagens desse recurso.

---

## 18. Alterando o layout sem alterar os itens

Imagine:

```css
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

```css
grid-template-areas
```

Por exemplo:

```css
grid-template-areas:
  "logo logo"
  "nav content"
  "footer footer";
```

Depois:

```css
grid-template-areas:
  "nav logo"
  "content content"
  "footer footer";
```

O layout muda sem precisarmos redefinir:

```css
grid-area
```

de cada item.

---

## 19. `grid-template-areas` em Media Queries

Essa característica é excelente para layouts responsivos.

Podemos ter um layout desktop:

```css
.grid {
  display: grid;

  grid-template-areas:
    "logo nav advert"
    "side-nav content advert"
    "side-nav footer advert";
}
```

E alterar a organização em uma Media Query:

```css
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

```text
┌────────┬────────┬────────┐
│  logo  │  nav   │ advert │
├────────┼────────┼────────┤
│  side  │content │ advert │
├────────┼────────┼────────┤
│  side  │ footer │ advert │
└────────┴────────┴────────┘
```

No mobile:

```text
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

## 20. Responsividade com duas colunas

Nem todo layout mobile precisa ter uma única coluna.

Podemos fazer:

```css
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

```text
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

## 21. A ordem do HTML continua importante

`grid-template-areas` altera a **apresentação visual**, mas não deve ser usado para criar uma ordem de leitura incoerente.

Considere uma estrutura HTML:

```html
<header>...</header>

<nav>...</nav>

<main>...</main>

<aside>...</aside>

<footer>...</footer>
```

A ordem faz sentido semanticamente:

```text
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

```text
┌───────────┬───────────┐
│   header  │   header  │
├───────────┼───────────┤
│   nav     │   main    │
├───────────┼───────────┤
│   aside   │   main    │
├───────────┴───────────┤
│         footer        │
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

## 22. Visualização x estrutura

Podemos separar:

```text
HTML

↓

estrutura e significado
```

e:

```text
CSS Grid

↓

organização visual
```

Isso permite que:

```text
estrutura semântica

        +

layout visual
```

sejam tratados separadamente.

---

## 23. O ponto `.`

Dentro de:

```css
grid-template-areas
```

podemos utilizar:

```text
.
```

O ponto representa uma **célula vazia**.

Exemplo:

```css
grid-template-areas:
  "logo nav ."
  "content content advert"
  "footer footer footer";
```

Visualmente:

```text
┌────────┬────────┬────────┐
│  logo  │  nav   │   .    │
├────────┼────────┼────────┤
│       content   │ advert │
├────────┴────────┼────────┤
│       footer             │
└──────────────────────────┘
```

A posição marcada com:

```text
.
```

fica vazia.

---

## 24. Vários pontos

Podemos utilizar vários pontos:

```css
grid-template-areas:
  "logo . ."
  "nav content ."
  "footer footer advert";
```

Isso cria células vazias nas posições indicadas.

O ponto pode ser útil quando queremos criar um espaço proposital no layout.

---

## 25. Estrutura de um layout completo

Um exemplo:

```css
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

```text
┌──────┬────────┬──────┐
│ logo │  logo  │advert│
├──────┼────────┼──────┤
│ nav  │ content│advert│
├──────┼────────┼──────┤
│ side │ content│  .   │
├──────┴────────┴──────┤
│       footer         │
└──────────────────────┘
```

Isso é praticamente um desenho textual do layout.

---

## 26. Mapa mental — conceito principal

```text
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
      linhas       colunas      áreas
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

```text
template-areas

      ↓

"DESENHE O MAPA"
```

```text
grid-area

      ↓

"COLOQUE O ITEM NO MAPA"
```

---

## 27. Mapa mental — sintaxe

```text
grid-template-areas:

        │

        ├── "linha 1"

        │

        ├── "linha 2"

        │

        └── "linha 3"
```

Exemplo:

```css
grid-template-areas:
  "logo nav advert"
  "side content advert"
  "side footer advert";
```

Visualização:

```text
"logo nav advert"

      ↓

┌──────┬──────┬──────┐
│ logo │ nav  │advert│
└──────┴──────┴──────┘
```

```text
"side content advert"

      ↓

┌──────┬──────┬──────┐
│ side │content│advert│
└──────┴──────┴──────┘
```

```text
"side footer advert"

      ↓

┌──────┬──────┬──────┐
│ side │footer│advert│
└──────┴──────┴──────┘
```

---

## 28. Mapa mental — áreas repetidas

```text
MESMO NOME

     │

     ↓

REPRESENTA A MESMA ÁREA

     │

     ├── horizontal
     │      ↓
     │  ocupa várias colunas
     │
     └── vertical
            ↓
        ocupa várias linhas
```

Exemplo:

```css
"logo logo nav"
```

```text
┌──────────────┬──────┐
│     logo     │ nav  │
└──────────────┴──────┘
```

Outro exemplo:

```css
"side content"
"side content"
```

```text
┌──────┬─────────┐
│ side │ content │
├──────┼─────────┤
│ side │ content │
└──────┴─────────┘
```

---

## 29. Mapa mental — regra do retângulo

```text
ÁREA

 │

 ├── pode ocupar 1 célula
 │
 ├── pode ocupar várias colunas
 │
 └── pode ocupar várias linhas
```

Mas:

```text
ÁREA EM "L"

      ↓

    ❌
```

A região precisa continuar sendo um retângulo.

---

## 30. Mapa mental — responsividade

```text
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

```text
mesmos elementos

      +

novo mapa

      ↓

novo layout
```

---

## 31. Mapa mental — HTML + CSS Grid

```text
                  WEB PAGE
                     │
          ┌──────────┴──────────┐
          │                     │
         HTML                  CSS
          │                     │
          ↓                     ↓
   estrutura lógica      estrutura visual
                                │
                                ↓
                     grid-template-areas
                                │
                                ↓
                         layout visual
                                │
          ┌─────────────────────┘
          ↓
      página final
```

### Corte mental ②

```text
HTML

→ "Qual é a ordem e o significado?"
```

```text
Grid

→ "Como isso será organizado visualmente?"
```

---

## 32. Exemplo completo

### HTML

```html
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

```css
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

```text
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

## 33. Responsividade do exemplo

Podemos reorganizar o mesmo layout:

```css
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

```text
┌──────────────┐
│     LOGO     │
├──────────────┤
│     NAV      │
├──────────────┤
│   CONTENT    │
├──────────────┤
│    ADVERT    │
├──────────────┤
│    FOOTER    │
└──────────────┘
```

Os itens continuam usando:

```css
grid-area: logo;
grid-area: nav;
grid-area: content;
grid-area: advert;
grid-area: footer;
```

Apenas o mapa mudou.

---

## 34. Um segundo layout mobile

Podemos também utilizar duas colunas:

```css
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

```text
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

## 35. `grid-template-areas` + `grid-template-columns`

Podemos utilizar as áreas para definir a estrutura e, ao mesmo tempo, determinar o tamanho das colunas.

Exemplo:

```css
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

```text
Coluna 1 → 100px

Coluna 2 → 1fr

Coluna 3 → 50px
```

E o mapa:

```text
logo       nav       advert
side-nav   content   advert
side-nav   footer    advert
```

Os dois trabalham juntos.

---

## 36. `grid-template-areas` + `grid-template-rows`

Também podemos definir alturas:

```css
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

```text
COLUNAS

100px | 1fr | 50px
```

```text
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

## 37. ⚠️ Manutenção do layout

`grid-template-areas` é excelente para definir a estrutura **macro** de uma página.

Por exemplo:

```text
header

sidebar

content

advert

footer
```

Porém, criar centenas de áreas para cada pequeno componente pode deixar o código difícil de manter.

Uma estratégia mais organizada é usar Grid Areas para a estrutura principal:

```text
PÁGINA
├── Header
├── Navigation
├── Main
├── Sidebar
└── Footer
```

e deixar componentes internos utilizarem seu próprio sistema de layout quando necessário.

---

## 38. 🧠 Resumo mental definitivo

```text
             GRID TEMPLATE AREAS

                       │

                       ↓

                CRIA UM MAPA

                       │

        ┌──────────────┼──────────────┐
        │              │              │
      NOME          REPETIÇÃO         .
        │              │              │
        ↓              ↓              ↓
    identifica     expande uma      cria área
      a área          área           vazia
        │
        ↓
    grid-area
        │
        ↓
   coloca o item
   naquela área
```

### Corte mental ③

```text
TEMPLATE AREAS

↓

"MAPA"
```

```text
GRID AREA

↓

"POSIÇÃO"
```

---

## 39. Checklist mental para escrever o código

```text
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

## 40. 📌 Regra final para memorizar

> **`grid-template-areas` transforma o Grid em um mapa visual nomeado.**

```css
grid-template-areas:
  "header header"
  "nav content"
  "footer footer";
```

Pode ser lido como:

```text
HEADER | HEADER

NAV    | CONTENT

FOOTER | FOOTER
```

Depois:

```css
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

```text
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

```text
grid-template-areas

→ "desenha o layout"

grid-area

→ "liga o elemento à área"

"."

→ "deixa a célula vazia"
```

E a regra estrutural mais importante:

> **As áreas nomeadas devem formar regiões retangulares; o Grid não permite que uma mesma área forme uma estrutura em `L`.**
