````markdown
# CSS Grid Layout — `justify-self`

> [!NOTE]
> Esta documentação apresenta o funcionamento da propriedade `justify-self`, com foco principal no **CSS Grid Layout**, relacionando o conceito com `justify-items`, `justify-content`, `align-self` e `place-self`.

---

## 1. Objetivo

A propriedade `justify-self` controla **como um único item é alinhado dentro do espaço que foi reservado para ele**.

No CSS Grid, esse espaço normalmente é a **grid area** ocupada pelo item.

A ideia fundamental é:

```text
GRID CONTAINER
└── GRID AREA DO ITEM
    └── ITEM
````

O `justify-self` decide **onde o item ficará dentro dessa área no eixo inline**.

Em uma escrita horizontal comum, como a utilizada normalmente em português:

```text
Eixo inline
←────────────────────────────→

┌──────────────────────────────┐
│          GRID AREA           │
│                              │
│      ┌──────────────┐        │
│      │     ITEM     │        │
│      └──────────────┘        │
│                              │
└──────────────────────────────┘
```

Portanto:

> `justify-self` não move a célula do Grid.
> Ele posiciona o **item dentro da área que esse item já ocupa**.

A especificação de CSS Box Alignment define `justify-self` como uma propriedade de **self-alignment**, isto é, alinhamento da própria caixa dentro de seu container de alinhamento.

---

# 2. Antes de entender `justify-self`

Para compreender completamente essa propriedade, precisamos separar quatro conceitos.

```text
┌─────────────────────────────────────────────┐
│              GRID CONTAINER                 │
│                                             │
│   ┌─────────────────────────────────────┐   │
│   │             GRID AREA               │   │
│   │                                     │   │
│   │           ┌─────────┐               │   │
│   │           │  ITEM   │               │   │
│   │           └─────────┘               │   │
│   │                                     │   │
│   └─────────────────────────────────────┘   │
│                                             │
└─────────────────────────────────────────────┘
```

### Grid Container

É o elemento que possui:

```css
display: grid;
```

Exemplo:

```css
.container {
    display: grid;
}
```

Ele cria o contexto de Grid.

---

### Grid Track

É uma coluna ou uma linha criada pelo Grid.

```text
COLUNAS

      coluna 1        coluna 2
         ↓               ↓

      ┌───────┬───────────┐
linha │       │           │
 1    │       │           │
      ├───────┼───────────┤
linha │       │           │
 2    │       │           │
      └───────┴───────────┘
```

Uma coluna é um **column track**.

Uma linha é um **row track**.

---

### Grid Area

É a região formada por uma ou mais linhas e colunas ocupada por um item.

```css
.item {
    grid-column: 1 / 3;
}
```

Nesse caso, o item ocupa duas colunas.

```text
┌───────────────┬───────────────┐
│               │               │
│      ITEM ----------------→   │
│               │               │
└───────────────┴───────────────┘
```

A área disponível para o item é maior do que o próprio conteúdo necessariamente precisa.

É justamente nesse espaço que `justify-self` atua.

---

### Grid Item

É o elemento filho direto do Grid Container.

```html
<div class="container">
    <div class="item"></div>
</div>
```

Como `.item` é filho direto do elemento com `display: grid`, ele é um grid item.

---

# 3. O que significa `self`?

O nome da propriedade ajuda bastante:

```css
justify-self
```

Podemos separar assim:

```text
justify → alinhar no eixo inline
self    → o próprio item
```

Ou seja:

```text
justify-items → define uma regra para os itens

justify-self  → define a regra para um item específico
```

Essa diferença é essencial.

---

# 4. `justify-items` × `justify-self`

## `justify-items`

É definido no **Grid Container**.

Ele estabelece a regra padrão de alinhamento para os itens.

```css
.container {
    display: grid;
    justify-items: center;
}
```

Todos os itens passam a utilizar esse alinhamento como padrão.

---

## `justify-self`

É definido diretamente no **Grid Item**.

```css
.item {
    justify-self: end;
}
```

Agora somente esse item possui uma regra própria.

A documentação do MDN descreve `justify-items` como a propriedade que define o comportamento padrão de `justify-self` para os itens, enquanto `justify-self` permite alterar o alinhamento de um item individualmente.

Visualmente:

```text
┌────────────────────────────────────┐
│           GRID CONTAINER           │
│                                    │
│  ┌────────────┐  ┌────────────┐    │
│  │   ITEM 1   │  │   ITEM 2   │    │
│  │   center   │  │   center   │    │
│  └────────────┘  └────────────┘    │
│                                    │
└────────────────────────────────────┘
```

Se o segundo item receber:

```css
.item-2 {
    justify-self: end;
}
```

Temos:

```text
┌────────────────────────────────────┐
│           GRID CONTAINER           │
│                                    │
│  ┌────────────┐                    │
│  │   ITEM 1   │                    │
│  │   center   │                    │
│  └────────────┘                    │
│                          ┌───────┐ │
│                          │ITEM 2 │ │
│                          │  end  │ │
│                          └───────┘ │
└────────────────────────────────────┘
```

Portanto:

> `justify-items` define o padrão para todos.
> `justify-self` permite que um item tenha seu próprio alinhamento.

---

# 5. Onde `justify-self` atua?

No Grid, `justify-self` atua no **eixo inline**.

Em um documento com `writing-mode` horizontal tradicional:

```text
EIXO INLINE
←──────────────────────────────→

EIXO BLOCK
↑
│
│
↓
```

Por isso, em um layout tradicional:

```text
justify-self → direção horizontal
align-self   → direção vertical
```

Mas existe uma observação técnica importante:

`justify-self` é definido em termos do **eixo inline**, e não simplesmente como "horizontal".

Isso significa que o comportamento depende do **writing mode**.

Por exemplo, em diferentes modos de escrita, o eixo inline pode ter outra orientação.

Portanto, a forma tecnicamente correta de pensar é:

```text
justify-self → eixo inline
align-self   → eixo block
```

O MDN e a especificação de CSS Box Alignment descrevem `justify-self` dessa forma.

---

# 6. O que exatamente é alinhado?

Essa é uma das partes mais importantes da propriedade.

`justify-self` não está alinhando o texto.

Ele está alinhando a **caixa do elemento** dentro do seu espaço de alinhamento.

Imagine:

```text
GRID AREA

┌──────────────────────────────────┐
│                                  │
│        ┌──────────────┐          │
│        │     ITEM     │          │
│        └──────────────┘          │
│                                  │
└──────────────────────────────────┘
```

O Grid possui uma área disponível.

O elemento possui sua própria caixa.

`justify-self` decide onde essa caixa ficará.

Isso é diferente de:

```css
text-align: center;
```

### `justify-self`

Move/alinha a **caixa do elemento**.

### `text-align`

Alinha o **conteúdo textual dentro da caixa**.

Exemplo:

```css
.item {
    justify-self: center;
    text-align: center;
}
```

As duas propriedades estão fazendo coisas diferentes.

```text
GRID AREA
┌─────────────────────────────────────┐
│                                     │
│             ┌───────────┐           │
│             │   ITEM    │           │
│             │   texto   │           │
│             └───────────┘           │
│                                     │
└─────────────────────────────────────┘

justify-self
       ↓
move a caixa do ITEM

text-align
       ↓
alinha o texto dentro da caixa
```

---

# 7. Sintaxe

A forma básica é:

```css
.item {
    justify-self: valor;
}
```

Exemplo:

```css
.item {
    justify-self: center;
}
```

A propriedade possui diversos tipos de valores.

Os principais para aprender primeiro são:

```css
justify-self: auto;
justify-self: normal;
justify-self: stretch;

justify-self: start;
justify-self: center;
justify-self: end;
```

Também existem valores como:

```css
justify-self: self-start;
justify-self: self-end;
justify-self: left;
justify-self: right;
justify-self: baseline;
justify-self: first baseline;
justify-self: last baseline;
justify-self: safe center;
justify-self: unsafe center;
```

A sintaxe atual da especificação também inclui `anchor-center`, relacionado a posicionamento por âncora. Nem todos os valores mais recentes possuem o mesmo nível de suporte em todos os navegadores.

---

# 8. `justify-self: start`

```css
.item {
    justify-self: start;
}
```

Coloca o item junto ao início do eixo inline.

Em uma escrita horizontal tradicional:

```text
INÍCIO →                                     FIM

┌──────────────────────────────────────────────┐
│ ┌──────────┐                                 │
│ │   ITEM   │                                 │
│ └──────────┘                                 │
└──────────────────────────────────────────────┘
```

Mentalmente:

```text
start = começo
```

O item vai para o início da área de alinhamento.

---

# 9. `justify-self: center`

```css
.item {
    justify-self: center;
}
```

Centraliza o item dentro da sua área de alinhamento.

```text
┌──────────────────────────────────────────────┐
│                                              │
│              ┌──────────┐                    │
│              │   ITEM   │                    │
│              └──────────┘                    │
│                                              │
└──────────────────────────────────────────────┘
```

Mentalmente:

```text
center = meio
```

---

# 10. `justify-self: end`

```css
.item {
    justify-self: end;
}
```

Coloca o item junto ao final do eixo inline.

Em uma escrita horizontal tradicional:

```text
INÍCIO                                     FIM
                                            ↓

┌──────────────────────────────────────────────┐
│                                 ┌──────────┐ │
│                                 │   ITEM   │ │
│                                 └──────────┘ │
└──────────────────────────────────────────────┘
```

Mentalmente:

```text
end = final
```

---

# 11. Visualizando os três principais valores

Podemos imaginar a mesma Grid Area três vezes:

```text
start

┌──────────────────────────────────┐
│ ┌───────┐                        │
│ │ ITEM  │                        │
│ └───────┘                        │
└──────────────────────────────────┘
```

```text
center

┌──────────────────────────────────┐
│            ┌───────┐             │
│            │ ITEM  │             │
│            └───────┘             │
└──────────────────────────────────┘
```

```text
end

┌──────────────────────────────────┐
│                        ┌───────┐ │
│                        │ ITEM  │ │
│                        └───────┘ │
└──────────────────────────────────┘
```

A área não mudou.

O item não mudou de célula.

Apenas sua posição **dentro da área** mudou.

---

# 12. `justify-self: stretch`

Esse valor é extremamente importante porque possui uma diferença fundamental em relação aos valores de posicionamento.

```css
.item {
    justify-self: stretch;
}
```

O item pode ocupar o espaço disponível no eixo de alinhamento.

Visualmente:

```text
┌─────────────────────────────────────┐
│                                     │
│ ┌─────────────────────────────────┐ │
│ │              ITEM               │ │
│ └─────────────────────────────────┘ │
│                                     │
└─────────────────────────────────────┘
```

Compare:

```text
start

┌─────────────────────────────────────┐
│ ┌─────────┐                         │
│ │  ITEM   │                         │
│ └─────────┘                         │
└─────────────────────────────────────┘
```

```text
center

┌─────────────────────────────────────┐
│          ┌─────────┐                │
│          │  ITEM   │                │
│          └─────────┘                │
└─────────────────────────────────────┘
```

```text
end

┌─────────────────────────────────────┐
│                         ┌─────────┐ │
│                         │  ITEM   │ │
│                         └─────────┘ │
└─────────────────────────────────────┘
```

```text
stretch

┌─────────────────────────────────────┐
│ ┌─────────────────────────────────┐ │
│ │              ITEM               │ │
│ └─────────────────────────────────┘ │
└─────────────────────────────────────┘
```

A especificação explica `stretch` como o comportamento que aumenta o tamanho de itens dimensionados como `auto` para preencher o espaço disponível, respeitando restrições como `min-width`, `max-width` e equivalentes.

---

# 13. Por que `stretch` pode parecer o padrão?

No Grid:

```css
justify-items: stretch;
```

é o valor inicial de `justify-items`.

Ao mesmo tempo:

```css
justify-self: auto;
```

é o valor inicial de `justify-self`.

O `auto` pode utilizar o valor de `justify-items` do elemento pai.

Assim:

```text
GRID CONTAINER
    │
    │ justify-items: stretch
    ↓
GRID ITEM
    │
    │ justify-self: auto
    ↓
usa o padrão do pai
    ↓
stretch
```

Por isso é muito comum observar os itens ocupando todo o espaço disponível na célula.

O MDN descreve exatamente essa relação entre `justify-items`, `justify-self` e os valores iniciais dessas propriedades.

---

# 14. `justify-self: auto`

```css
.item {
    justify-self: auto;
}
```

`auto` não significa simplesmente "centralizar", "esticar" ou "não fazer nada".

Ele significa, essencialmente:

> Use a configuração de `justify-items` fornecida pelo elemento pai, quando aplicável.

Exemplo:

```css
.container {
    display: grid;
    justify-items: center;
}
```

E:

```css
.item {
    justify-self: auto;
}
```

O item utiliza o alinhamento definido pelo pai:

```text
justify-items: center
          ↓
      ITEM 1 → center
      ITEM 2 → center
      ITEM 3 → center
```

Agora imagine:

```css
.item-2 {
    justify-self: end;
}
```

Resultado:

```text
ITEM 1 → center
ITEM 2 → end
ITEM 3 → center
```

Temos, portanto:

```text
justify-items = regra padrão
justify-self  = regra específica
```

---

# 15. `justify-self: normal`

Também existe:

```css
.item {
    justify-self: normal;
}
```

O significado de `normal` depende do modelo de layout.

No Grid, seu comportamento é semelhante ao `stretch`, com uma exceção importante para caixas com **aspect ratio** (proporção intrínseca) ou tamanho intrínseco, nas quais o comportamento tende a ser `start`.

Isso é especialmente relevante para elementos como imagens.

Exemplo:

```html
<div class="container">
    <img src="imagem.jpg" alt="">
</div>
```

Uma imagem pode possuir uma proporção natural, como:

```text
largura : altura
   16   :   9
```

Forçar o elemento a preencher o espaço sem respeitar essa proporção poderia produzir distorção.

Por isso, o comportamento relacionado a itens com proporção intrínseca merece atenção.

---

# 16. `start` e `self-start`

À primeira vista:

```css
justify-self: start;
```

e:

```css
justify-self: self-start;
```

parecem iguais.

Mas existe uma diferença conceitual.

### `start`

Refere-se ao início do **container de alinhamento** no eixo correspondente.

### `self-start`

Refere-se ao lado do container correspondente ao **início lógico do próprio item**.

Visualmente, em situações simples:

```text
start
↓
┌────────────────────────────┐
│ ITEM                       │
└────────────────────────────┘
```

e:

```text
self-start
↓
┌────────────────────────────┐
│ ITEM                       │
└────────────────────────────┘
```

podem produzir o mesmo resultado.

A diferença aparece principalmente quando existem diferentes **writing modes** ou direções de escrita.

Isso acontece porque `start` e `end` são valores lógicos relacionados ao fluxo de escrita, enquanto `self-start` e `self-end` levam em consideração o lado inicial e final do próprio item.

---

# 17. `self-end`

Seguindo a mesma lógica:

```css
.item {
    justify-self: self-end;
}
```

alinha o item ao lado do container correspondente ao lado final do próprio item.

Em layouts simples:

```text
┌──────────────────────────────────┐
│                         ┌──────┐ │
│                         │ITEM  │ │
│                         └──────┘ │
└──────────────────────────────────┘
```

Mas seu valor fica mais interessante quando lidamos com diferentes sistemas de escrita.

---

# 18. `left` e `right`

Também existem:

```css
justify-self: left;
justify-self: right;
```

Esses valores são físicos.

Ou seja:

```text
left  → esquerda física
right → direita física
```

Isso é diferente de:

```css
start
end
```

que são valores lógicos.

Exemplo:

```text
left
↓
┌────────────────────────────────┐
│ ITEM                           │
└────────────────────────────────┘
```

```text
right
↓
┌────────────────────────────────┐
│                           ITEM │
└────────────────────────────────┘
```

Em interfaces modernas, `start` e `end` normalmente são mais adequados quando queremos que o layout respeite diferentes direções e modos de escrita.

O CSS Box Alignment define `left` e `right` como valores físicos, enquanto `start`, `end`, `self-start` e `self-end` são valores lógicos.

---

# 19. `baseline`

Também é possível utilizar alinhamento por linha de base:

```css
justify-self: baseline;
```

Existem ainda:

```css
justify-self: first baseline;
justify-self: last baseline;
```

Esse tipo de alinhamento é mais avançado.

Em vez de simplesmente colocar a caixa no início, meio ou fim, o navegador considera a **baseline**, ou linha de base tipográfica utilizada para alinhar conteúdo.

Um modelo mental simplificado:

```text
ITEM 1          ITEM 2

Texto grande    Texto pequeno
      ────────────────
          baseline
```

A baseline é muito importante quando diferentes conteúdos precisam compartilhar um alinhamento tipográfico consistente.

A especificação de CSS Box Alignment define `baseline`, `first baseline` e `last baseline` como formas de alinhamento por linha de base.

---

# 20. `safe` e `unsafe`

Também podemos combinar alguns valores posicionais com:

```css
safe
```

ou:

```css
unsafe
```

Exemplo:

```css
.item {
    justify-self: safe center;
}
```

### `safe`

Se a posição escolhida causar overflow do item em relação ao container de alinhamento, o navegador poderá optar por um alinhamento equivalente a `start`.

### `unsafe`

O alinhamento solicitado é mantido mesmo que possa ocorrer overflow.

Visualmente:

```text
SAFE

┌──────────────────────────────┐
│ ITEM                         │
└──────────────────────────────┘

evita deixar o item
em uma posição problemática
```

Enquanto:

```text
UNSAFE

┌──────────────────────────────┐
│        ITEM██████████→       │
└──────────────────────────────┘

mantém o alinhamento solicitado
mesmo que ultrapasse o espaço
```

Esses valores existem para lidar com situações em que a posição desejada pode gerar conteúdo fora da área disponível.

---

# 21. Exemplo completo

HTML:

```html
<div class="container">
    <div class="item item-1">1</div>
    <div class="item item-2">2</div>
    <div class="item item-3">3</div>
    <div class="item item-4">4</div>
</div>
```

CSS:

```css
.container {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;

    justify-items: stretch;
}

.item {
    padding: 20px;
}

.item-1 {
    justify-self: start;
}

.item-2 {
    justify-self: center;
}

.item-3 {
    justify-self: end;
}

.item-4 {
    justify-self: stretch;
}
```

Mentalmente:

```text
┌─────────────────────┬─────────────────────┐
│ ┌──────┐            │      ┌──────┐       │
│ │  1   │            │      │  2   │       │
│ └──────┘            │      └──────┘       │
├─────────────────────┼─────────────────────┤
│            ┌──────┐ │ ┌──────────────────┐│
│            │  3   │ │ │        4         ││
│            └──────┘ │ └──────────────────┘│
└─────────────────────┴─────────────────────┘
   start                center

end                    stretch
```

Cada item continua em sua própria célula.

O que muda é a posição ou o tamanho da caixa do item dentro da respectiva área.

---

# 22. `justify-self` não altera o posicionamento do Grid

É importante não confundir:

```css
justify-self
```

com:

```css
grid-column
grid-row
grid-area
```

Essas propriedades têm funções diferentes.

### `grid-column`

Determina **onde o item será colocado em relação às colunas e linhas do Grid**.

### `grid-row`

Determina **onde o item será colocado em relação às linhas**.

### `grid-area`

Pode determinar a região ocupada pelo item.

### `justify-self`

Determina **como o item será alinhado dentro da área que ele ocupa**.

Mental model:

```text
1. Primeiro descubra ONDE o item está.

grid-column
grid-row
grid-area
       ↓
┌───────────────────────┐
│      GRID AREA        │
└───────────────────────┘

2. Depois descubra COMO o item ficará dentro dela.

justify-self
       ↓
┌───────────────────────┐
│    ┌────────────┐     │
│    │    ITEM    │     │
│    └────────────┘     │
└───────────────────────┘
```

Essa separação é extremamente importante para entender CSS Grid.

---

# 23. `justify-self` × `justify-items` × `justify-content`

Essas três propriedades possuem nomes parecidos, mas trabalham em níveis diferentes.

| Propriedade       | Definida em    | Atua sobre                             | Ideia principal                         |
| ----------------- | -------------- | -------------------------------------- | --------------------------------------- |
| `justify-content` | Grid Container | o conjunto dos tracks/conteúdo do Grid | distribui o Grid dentro do container    |
| `justify-items`   | Grid Container | todos os Grid Items                    | define o alinhamento padrão dos itens   |
| `justify-self`    | Grid Item      | um único Grid Item                     | define o alinhamento individual do item |

Visualização:

```text
GRID CONTAINER
│
├── justify-content
│       ↓
│   posiciona/distribui
│   o conjunto do Grid
│
├── justify-items
│       ↓
│   define o padrão
│   para os itens
│
└── GRID ITEM
        │
        └── justify-self
                ↓
           altera somente
           este item
```

---

# 24. Exemplo para diferenciar `justify-content`

Considere:

```css
.container {
    display: grid;
    grid-template-columns: 100px 100px;
    width: 400px;
}
```

O Grid possui:

```text
100px + 100px = 200px
```

mas o container possui:

```text
400px
```

Existe espaço sobrando.

`justify-content` pode decidir onde o **conjunto das colunas** ficará.

Já:

```css
justify-self
```

trabalha com o item dentro da área que já foi criada.

Portanto:

```text
justify-content

┌─────────────────────────────────────────┐
│      [ COLUNA ][ COLUNA ]               │
└─────────────────────────────────────────┘
         ↑
      conjunto
      do Grid
```

Enquanto:

```text
justify-self

┌──────────────────────┐
│   ┌──────────────┐   │
│   │     ITEM     │   │
│   └──────────────┘   │
└──────────────────────┘
          ↑
       item individual
```

---

# 25. `justify-self` × `align-self`

As duas propriedades utilizam o conceito de **self-alignment**.

```css
justify-self
align-self
```

A diferença está no eixo.

Em uma escrita horizontal tradicional:

```text
justify-self
      ↓
eixo inline
      ↓
horizontal

align-self
      ↓
eixo block
      ↓
vertical
```

Exemplo:

```css
.item {
    justify-self: center;
    align-self: center;
}
```

Isso posiciona o item no centro da sua área nos dois eixos.

Visualmente:

```text
┌──────────────────────────────────┐
│                                  │
│                                  │
│            ┌────────┐            │
│            │  ITEM  │            │
│            └────────┘            │
│                                  │
│                                  │
└──────────────────────────────────┘
```

O MDN descreve `align-self` como o equivalente para o outro eixo de alinhamento no Grid.

---

# 26. `place-self`

Existe ainda uma forma abreviada de controlar:

```css
align-self
justify-self
```

Essa propriedade é:

```css
place-self
```

Exemplo:

```css
.item {
    place-self: center;
}
```

É equivalente, nesse caso, a:

```css
.item {
    align-self: center;
    justify-self: center;
}
```

Também podemos escrever:

```css
.item {
    place-self: start end;
}
```

O primeiro valor representa:

```text
align-self
```

e o segundo:

```text
justify-self
```

Portanto:

```css
place-self: <align-self> <justify-self>;
```

A documentação do MDN define `place-self` como shorthand dessas duas propriedades.

---

# 27. Um detalhe importante sobre `stretch`

`stretch` não significa simplesmente:

> "faça o elemento ficar com `width: 100%`".

O comportamento depende do dimensionamento do item.

A ideia é que o espaço livre no eixo de alinhamento seja utilizado quando o tamanho do item permitir esse comportamento.

Por isso:

```css
justify-self: stretch;
```

não deve ser confundido automaticamente com:

```css
width: 100%;
```

São mecanismos diferentes.

O `stretch` atua dentro do sistema de alinhamento, respeitando as restrições de dimensionamento do item.

---

# 28. Quando `stretch` não funciona como esperado?

Imagine:

```css
.item {
    width: 100px;
    justify-self: stretch;
}
```

Nesse caso, o dimensionamento explícito do elemento pode impedir que ele simplesmente seja expandido como um item cujo tamanho permite o comportamento de `stretch`.

Da mesma forma, restrições como:

```css
max-width
min-width
```

podem limitar o resultado.

Exemplo:

```css
.item {
    max-width: 200px;
    justify-self: stretch;
}
```

O item não poderá simplesmente ultrapassar:

```css
max-width: 200px;
```

A própria definição de `stretch` considera essas restrições.

---

# 29. Exemplo prático: botão dentro de uma célula

Imagine:

```html
<div class="card">
    <h2>Título</h2>
    <p>Descrição</p>
    <button>Comprar</button>
</div>
```

E o container:

```css
.card {
    display: grid;
}
```

Podemos fazer:

```css
button {
    justify-self: end;
}
```

Resultado:

```text
┌────────────────────────────────┐
│ Título                         │
│                                │
│ Descrição                      │
│                                │
│                     ┌────────┐ │
│                     │Comprar │ │
│                     └────────┘ │
└────────────────────────────────┘
```

O botão continua ocupando a mesma área do Grid.

Mas sua caixa é alinhada ao final do eixo inline.

---

# 30. Exemplo com imagem

Imagine uma imagem dentro de uma célula maior:

```css
img {
    justify-self: center;
}
```

Resultado:

```text
┌─────────────────────────────────────┐
│                                     │
│           ┌────────────┐            │
│           │    IMG     │            │
│           └────────────┘            │
│                                     │
└─────────────────────────────────────┘
```

Isso permite posicionar a imagem dentro da área sem necessariamente modificar a estrutura das colunas.

Para imagens, é importante também considerar suas dimensões intrínsecas e proporção para evitar distorções.

---

# 31. Relação com `grid-area`

Considere:

```css
.item {
    grid-area: 1 / 1 / 3 / 3;
}
```

Aqui:

```text
grid-area
    ↓
define a área ocupada
```

Depois:

```css
.item {
    justify-self: center;
}
```

Agora:

```text
grid-area
    ↓
┌───────────────────────────────┐
│                               │
│           GRID AREA           │
│                               │
│        ┌──────────┐           │
│        │   ITEM   │           │
│        └──────────┘           │
│                               │
└───────────────────────────────┘
             ↑
       justify-self
```

Essa combinação é extremamente poderosa.

Primeiro:

> **Onde o item está?**

Depois:

> **Como ele fica dentro daquele espaço?**

---

# 32. Relação com `span`

O mesmo conceito vale quando utilizamos:

```css
grid-column: span 2;
```

O item ocupa uma área maior:

```text
┌───────────────┬───────────────┐
│                               │
│             ITEM              │
│                               │
└───────────────┴───────────────┘
```

Agora:

```css
.item {
    justify-self: center;
}
```

O item é centralizado dentro dessa área maior.

Portanto:

```text
span
 ↓
aumenta a área ocupada

justify-self
 ↓
alinha o item dentro dessa área
```

---

# 33. Erro comum: pensar que `justify-self` move a coluna

Considere:

```css
.item {
    justify-self: end;
}
```

Isso NÃO significa:

```text
"mova a coluna para a direita"
```

Significa:

```text
"alinhe este item no final do espaço que ele possui"
```

A coluna continua exatamente onde estava.

```text
Antes:

┌───────────────┐
│    ITEM       │
└───────────────┘

Depois:

┌───────────────┐
│          ITEM │
└───────────────┘

mesma área
item em posição diferente
```

---

# 34. Erro comum: confundir com `text-align`

Isso:

```css
.item {
    justify-self: center;
}
```

não significa:

```text
"centralizar o texto"
```

Significa:

```text
"centralizar a caixa do item"
```

Para o texto:

```css
.item {
    text-align: center;
}
```

Podemos inclusive utilizar os dois:

```css
.item {
    justify-self: center;
    text-align: center;
}
```

Agora:

```text
justify-self
→ posição da caixa

text-align
→ posição do texto dentro da caixa
```

---

# 35. Erro comum: confundir `justify-items` com `justify-self`

### `justify-items`

```css
.container {
    justify-items: center;
}
```

Afeta o padrão de todos os itens.

### `justify-self`

```css
.item {
    justify-self: end;
}
```

Afeta apenas aquele item.

Mental model:

```text
justify-items
      ↓
"Como os itens devem se alinhar?"

justify-self
      ↓
"E este item especificamente?"
```

---

# 36. Erro comum: pensar somente em "horizontal"

É muito comum decorar:

```text
justify = horizontal
align   = vertical
```

Isso funciona como uma simplificação inicial em layouts horizontais tradicionais.

Mas a definição técnica é:

```text
justify-self → eixo inline
align-self   → eixo block
```

Essa forma de pensar é mais correta porque respeita diferentes `writing-mode` e direções de escrita.

---

# 37. Fluxo mental definitivo

Quando encontrar:

```css
justify-self: center;
```

não pense apenas:

> "centraliza."

Pense nesta sequência:

```text
1. Tenho um Grid Container
            ↓
2. Tenho um Grid Item
            ↓
3. O item recebeu uma Grid Area
            ↓
4. Existe espaço disponível dentro dessa área
            ↓
5. justify-self controla o alinhamento da caixa
            ↓
6. O alinhamento acontece no eixo inline
            ↓
7. center coloca o item no centro desse espaço
```

Esse processo é muito mais útil do que decorar palavras isoladas.

---

# 38. Mapa mental

```text
                         CSS GRID
                            │
                            ▼
                     GRID CONTAINER
                            │
                            ▼
                       GRID ITEM
                            │
                            ▼
                       GRID AREA
                            │
             ┌──────────────┴──────────────┐
             │                             │
             ▼                             ▼
        EIXO INLINE                    EIXO BLOCK
             │                             │
             ▼                             ▼
       justify-self                    align-self
             │                             │
      ┌──────┼────────┐             ┌──────┼──────┐
      │      │        │             │      │      │
      ▼      ▼        ▼          ▼      ▼      ▼
    start  center    end          start  center   end
      │      │        │
      └──────┴────────┘
             │
             ▼
         posiciona
         o ITEM
         dentro da
         GRID AREA
```

---

# 39. Regra de ouro

> **`justify-self` não decide onde a Grid Area fica. Ele decide onde o item fica dentro da Grid Area, no eixo inline.**

Essa frase resume o conceito.

```text
grid-column
grid-row
grid-area
      ↓
ONDE O ITEM ESTÁ

justify-self
      ↓
COMO O ITEM FICA DENTRO DESSE ESPAÇO
```

---

# 40. Tabela de fixação

| Valor           | O que faz                                                                           | Mentalidade                 |
| --------------- | ----------------------------------------------------------------------------------- | --------------------------- |
| `auto`          | utiliza a configuração apropriada do pai, normalmente relacionada a `justify-items` | "siga o padrão"             |
| `normal`        | comportamento padrão dependente do modelo de layout                                 | "comportamento normal"      |
| `start`         | alinha no início do eixo                                                            | "começo"                    |
| `center`        | centraliza o item                                                                   | "meio"                      |
| `end`           | alinha no final do eixo                                                             | "fim"                       |
| `stretch`       | usa o espaço disponível quando o dimensionamento permite                            | "preencher"                 |
| `self-start`    | alinha no início lógico do próprio item                                             | "início do próprio item"    |
| `self-end`      | alinha no final lógico do próprio item                                              | "fim do próprio item"       |
| `left`          | alinha à esquerda física                                                            | "esquerda"                  |
| `right`         | alinha à direita física                                                             | "direita"                   |
| `baseline`      | utiliza alinhamento por linha de base                                               | "alinhar pela baseline"     |
| `safe center`   | centraliza, evitando overflow problemático quando aplicável                         | "centralizar com segurança" |
| `unsafe center` | mantém o alinhamento mesmo podendo ocorrer overflow                                 | "forçar o alinhamento"      |

---

# 41. Tabela de comparação das propriedades

| Propriedade       | Elemento onde é aplicada | Principal função                                        |
| ----------------- | ------------------------ | ------------------------------------------------------- |
| `justify-content` | Grid Container           | posicionar/distribuir o conteúdo do Grid no eixo inline |
| `align-content`   | Grid Container           | posicionar/distribuir o conteúdo do Grid no eixo block  |
| `justify-items`   | Grid Container           | definir o alinhamento padrão dos itens                  |
| `align-items`     | Grid Container           | definir o alinhamento padrão no outro eixo              |
| `justify-self`    | Grid Item                | alinhar um item individual no eixo inline               |
| `align-self`      | Grid Item                | alinhar um item individual no eixo block                |
| `place-self`      | Grid Item                | shorthand de `align-self` + `justify-self`              |

---

# 42. Código de referência

```css
.container {
    display: grid;

    grid-template-columns: repeat(2, 1fr);

    /* Regra padrão para os itens */
    justify-items: stretch;
}

.item-start {
    justify-self: start;
}

.item-center {
    justify-self: center;
}

.item-end {
    justify-self: end;
}

.item-stretch {
    justify-self: stretch;
}

.item-auto {
    justify-self: auto;
}
```

---

# 43. Perguntas para verificar a compreensão

Antes de avançar para outra propriedade, tente responder:

1. O que significa `self` em `justify-self`?

2. `justify-self` é aplicado ao Grid Container ou ao Grid Item?

3. Qual é a diferença entre `justify-items` e `justify-self`?

4. `justify-self` posiciona o item em relação a quê?

5. Qual eixo é controlado por `justify-self`?

6. Qual a diferença entre `justify-self` e `text-align`?

7. O que acontece com:

```css
justify-self: start;
```

8. O que acontece com:

```css
justify-self: center;
```

9. O que acontece com:

```css
justify-self: end;
```

10. O que diferencia `stretch` dos outros valores posicionais?

11. O que significa `justify-self: auto`?

12. Qual é a diferença conceitual entre:

```css
grid-area
```

e:

```css
justify-self
```

Se essas perguntas estiverem claras, o conceito central da propriedade já está consolidado.

---

# 44. Resumo técnico

```text
justify-self
│
├── é uma propriedade de alinhamento individual
│
├── aplicada ao Grid Item
│
├── trabalha no eixo inline
│
├── alinha a caixa do item
│
├── não altera diretamente a posição da Grid Area
│
├── não é equivalente a text-align
│
├── pode sobrescrever o padrão de justify-items
│
└── principais valores
      ├── auto
      ├── normal
      ├── start
      ├── center
      ├── end
      └── stretch
```

---

# 45. Referências oficiais

* **MDN — `justify-self`**: definição, sintaxe, valores, exemplos e compatibilidade.
* **MDN — `justify-items`**: relação entre `justify-items` e `justify-self`.
* **MDN — Box Alignment in Grid Layout**: diferença entre `*-items` e `*-self` no CSS Grid.
* **MDN — `align-self`**: propriedade correspondente para o outro eixo de alinhamento.
* **MDN — `place-self`**: shorthand de `align-self` e `justify-self`.
* **CSS Working Group — CSS Box Alignment Module Level 3**: especificação oficial do sistema de alinhamento CSS.

---

# GitHub

**Gabriel Felipe de Oliveira Rateiro**

Repositório de estudos e documentação de desenvolvimento web.

**Próximo ponto importante do estudo:**

```text
justify-content
        │
        ├── conteúdo/conjunto do Grid
        │
        ▼
justify-items
        │
        ├── padrão para todos os itens
        │
        ▼
justify-self
        │
        └── item individual
```

> Essa sequência ajuda a diferenciar **conteúdo do Grid → conjunto de itens → item individual**.

```
```
