# CSS Grid Layout — `align-self`

> [!NOTE]
> Esta documentação foi construída a partir da aula fornecida e complementada com a documentação oficial do CSS para esclarecer o funcionamento técnico da propriedade `align-self`, especialmente no CSS Grid.

---

# 1. O que é `align-self`?

A propriedade:

```css
align-self
````

define **como um único Grid Item será alinhado dentro da Grid Area que ele ocupa**, no **eixo block**.

Em um layout horizontal tradicional:

```text
EIXO BLOCK
    ↑
    │
    │
    ↓

EIXO INLINE
←────────────────────→
```

Portanto, podemos criar inicialmente este modelo mental:

```text
justify-self
    ↓
eixo inline
    ↓
normalmente horizontal


align-self
    ↓
eixo block
    ↓
normalmente vertical
```

A documentação oficial do MDN define `align-self` como a propriedade que alinha um item dentro da sua Grid Area no eixo block quando estamos utilizando Grid.

---

# 2. A ideia central

Imagine uma Grid Area grande:

```text
┌───────────────────────────────┐
│                               │
│                               │
│                               │
│           ITEM                │
│                               │
│                               │
│                               │
└───────────────────────────────┘
```

O item ocupa apenas o espaço necessário para seu conteúdo.

O `align-self` permite decidir onde esse item ficará **verticalmente dentro dessa área**, em um layout tradicional.

Por exemplo:

```css
.item {
    align-self: start;
}
```

O item vai para o início do eixo block:

```text
┌───────────────────────────────┐
│           ITEM                │
│                               │
│                               │
│                               │
│                               │
└───────────────────────────────┘
```

Com:

```css
.item {
    align-self: end;
}
```

ele vai para o final:

```text
┌───────────────────────────────┐
│                               │
│                               │
│                               │
│                               │
│           ITEM                │
└───────────────────────────────┘
```

Com:

```css
.item {
    align-self: center;
}
```

ele vai para o centro:

```text
┌───────────────────────────────┐
│                               │
│                               │
│           ITEM                │
│                               │
│                               │
└───────────────────────────────┘
```

---

# 3. O significado de `self`

O nome ajuda a entender a propriedade:

```text
align
↓
alinhamento

self
↓
o próprio elemento
```

Portanto:

```css
align-self
```

significa, conceitualmente:

> "Como este próprio item deve ser alinhado?"

Isso diferencia `align-self` de `align-items`.

---

# 4. `align-items` × `align-self`

Essa é uma das distinções mais importantes do CSS Grid.

## `align-items`

É aplicada no **Grid Container**:

```css
.container {
    display: grid;
    align-items: center;
}
```

Ela define o alinhamento dos Grid Items como grupo.

## `align-self`

É aplicada no **Grid Item**:

```css
.item {
    align-self: end;
}
```

Ela permite modificar o alinhamento de **um item específico**.

O MDN define `align-items` como a propriedade que estabelece o valor de `align-self` para os filhos como um grupo.

Podemos representar isso assim:

```text
                 GRID CONTAINER
                       │
                       │
                align-items
                       │
         ┌─────────────┼─────────────┐
         ↓             ↓             ↓
      ITEM 1         ITEM 2        ITEM 3
         │             │             │
         └─────────────┼─────────────┘
                       │
                regra padrão
```

Mas um item pode sobrescrever essa regra:

```text
                 GRID CONTAINER
                       │
                align-items: center
                       │
              ┌────────┼────────┐
              ↓        ↓        ↓
            ITEM 1   ITEM 2   ITEM 3
              │        │        │
              │        │        │
              │        └── align-self: end
              │
              └── segue o padrão
```

Resultado:

```text
ITEM 1 → center
ITEM 2 → end
ITEM 3 → center
```

Portanto:

```text
align-items
    ↓
regra geral

align-self
    ↓
regra individual
```

---

# 5. Onde `align-self` atua?

O ponto mais importante é:

```text
align-self
      ↓
alinhamento individual
      ↓
dentro da Grid Area
      ↓
no eixo block
```

Visualmente:

```text
┌────────────────────────────────────┐
│             GRID AREA              │
│                                    │
│                                    │
│               ┌───────┐            │
│               │ ITEM  │            │
│               └───────┘            │
│                                    │
│                                    │
└────────────────────────────────────┘
```

A Grid Area continua no mesmo lugar.

O que muda é a posição do item **dentro dela**.

---

# 6. Não confunda Grid Area com Item

Considere:

```css
.item {
    grid-row: 1 / 3;
}
```

O item pode ocupar duas linhas:

```text
┌───────────────┐
│               │ ← linha 1
│               │
│     ITEM      │
│               │
│               │ ← linha 2
└───────────────┘
```

Toda essa região é a área disponível para o item.

Agora usamos:

```css
.item {
    align-self: start;
}
```

O item é alinhado no início dessa área.

```text
┌───────────────┐
│    ┌──────┐   │
│    │ ITEM │   │
│    └──────┘   │
│               │
│               │
└───────────────┘
```

A propriedade não alterou:

```css
grid-row
```

Ela apenas alterou o alinhamento do item dentro da área já determinada.

---

# 7. Relação com `grid-row`

Uma maneira excelente de diferenciar as propriedades é pensar em duas perguntas:

### `grid-row`

> "Em quais linhas o item ficará?"

### `align-self`

> "Como o item ficará dentro dessa área?"

Visualmente:

```text
grid-row
   ↓
define a área

┌───────────────┐
│               │
│               │
│    GRID AREA  │
│               │
│               │
└───────────────┘

align-self
      ↓
posiciona o item

┌───────────────┐
│    ┌──────┐   │
│    │ ITEM │   │
│    └──────┘   │
│               │
└───────────────┘
```

Essa distinção evita uma confusão muito comum em CSS Grid.

---

# 8. Sintaxe

A sintaxe básica é:

```css
.item {
    align-self: valor;
}
```

Na aula, os valores principais apresentados são:

```css
align-self: start;
align-self: end;
align-self: center;
align-self: stretch;
```

Esses quatro valores são suficientes para compreender o conceito central mostrado na aula.

A propriedade também aceita outros valores definidos pelo CSS Box Alignment, como `auto`, `normal`, `self-start`, `self-end`, `baseline` e combinações com `safe` e `unsafe`.

---

# 9. `align-self: start`

```css
.item {
    align-self: start;
}
```

Coloca o item no **início do eixo block** da sua Grid Area.

Em um layout horizontal tradicional, isso significa colocá-lo na parte superior.

```text
┌───────────────────────────────┐
│       ┌──────────┐            │
│       │   ITEM   │            │
│       └──────────┘            │
│                               │
│                               │
│                               │
└───────────────────────────────┘
```

Mentalmente:

```text
start = início
```

---

# 10. `align-self: end`

```css
.item {
    align-self: end;
}
```

Coloca o item no **final do eixo block**.

Em um layout horizontal tradicional:

```text
┌───────────────────────────────┐
│                               │
│                               │
│                               │
│       ┌──────────┐            │
│       │   ITEM   │            │
│       └──────────┘            │
└───────────────────────────────┘
```

Mentalmente:

```text
end = final
```

---

# 11. `align-self: center`

```css
.item {
    align-self: center;
}
```

Centraliza o item dentro da Grid Area no eixo block.

```text
┌───────────────────────────────┐
│                               │
│                               │
│       ┌──────────┐            │
│       │   ITEM   │            │
│       └──────────┘            │
│                               │
│                               │
└───────────────────────────────┘
```

Mentalmente:

```text
center = meio
```

---

# 12. `align-self: stretch`

Esse valor possui uma característica diferente.

```css
.item {
    align-self: stretch;
}
```

Em vez de simplesmente colocar o item no início, centro ou fim, `stretch` permite que um item dimensionado de forma compatível **se estique para ocupar o espaço disponível no eixo de alinhamento**.

```text
┌───────────────────────────────┐
│ ┌───────────────────────────┐ │
│ │            ITEM           │ │
│ └───────────────────────────┘ │
└───────────────────────────────┘
```

Em uma Grid, o comportamento de `stretch` precisa ser entendido em conjunto com o dimensionamento do item.

Restrições de tamanho, como:

```css
min-height
max-height
height
```

podem influenciar o resultado.

A documentação oficial descreve `stretch` como o comportamento que estica itens com tamanho compatível para preencher o espaço disponível.

---

# 13. A comparação dos quatro valores

Podemos visualizar:

```text
START

┌───────────────────────────────┐
│       ┌──────────┐            │
│       │   ITEM   │            │
│       └──────────┘            │
│                               │
│                               │
└───────────────────────────────┘
```

```text
CENTER

┌───────────────────────────────┐
│                               │
│       ┌──────────┐            │
│       │   ITEM   │            │
│       └──────────┘            │
│                               │
└───────────────────────────────┘
```

```text
END

┌───────────────────────────────┐
│                               │
│                               │
│       ┌──────────┐            │
│       │   ITEM   │            │
│       └──────────┘            │
└───────────────────────────────┘
```

```text
STRETCH

┌───────────────────────────────┐
│ ┌───────────────────────────┐ │
│ │            ITEM           │ │
│ └───────────────────────────┘ │
└───────────────────────────────┘
```

A grande pergunta é:

```text
O item vai:

start   → para o começo?
center  → para o meio?
end     → para o final?
stretch → ocupar o espaço disponível?
```

---

# 14. Exemplo prático

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
    grid-auto-rows: 120px;
}

.item-1 {
    align-self: start;
}

.item-2 {
    align-self: end;
}

.item-3 {
    align-self: center;
}

.item-4 {
    align-self: stretch;
}
```

Cada item está em sua própria área, mas cada um possui um comportamento diferente no eixo block.

Mentalmente:

```text
┌───────────────────┬───────────────────┐
│ ┌───────┐         │                   │
│ │   1   │         │                   │
│ └───────┘         │                   │
├───────────────────┼───────────────────┤
│                   │                   │
│             ┌─────┐                   │
│             │  3  │                   │
│             └─────┘                   │
├───────────────────┼───────────────────┤
│ ┌───────────────┐ │                   │
│ │       4       │ │          2        │
│ └───────────────┘ │                   │
└───────────────────┴───────────────────┘

1 → start
2 → end
3 → center
4 → stretch
```

---

# 15. Relação com a aula

A aula demonstra exatamente essa lógica:

```text
align-self
    │
    ├── start
    │      ↓
    │   início
    │
    ├── end
    │      ↓
    │   final
    │
    ├── center
    │      ↓
    │   centro
    │
    └── stretch
           ↓
      ocupa o espaço disponível
```

A ideia apresentada na aula é que o item pode ocupar uma área que se estende por mais de uma linha e, depois, ser alinhado dentro dessa área.

O conceito essencial é:

```text
GRID AREA
┌─────────────────────────────┐
│                             │
│                             │
│          espaço             │
│                             │
│                             │
└─────────────────────────────┘

          ↓

align-self decide onde
o ITEM fica dentro dela.
```

---

# 16. `align-self` não altera as linhas

Considere:

```css
.item {
    grid-row: 1 / 3;
}
```

O Grid continua com:

```text
linha 1
──────────────
      │
      │
linha 2
──────────────
```

A área do item permanece a mesma.

Se adicionarmos:

```css
.item {
    align-self: end;
}
```

não estamos dizendo:

> "mova o item para a linha 3."

Estamos dizendo:

> "alinhe o item no final da área que ele já ocupa."

Esse detalhe é fundamental.

---

# 17. `align-items` como regra geral

Considere:

```css
.container {
    display: grid;
    align-items: center;
}
```

Todos os itens serão alinhados ao centro de suas respectivas Grid Areas.

```text
┌───────────────┬───────────────┐
│               │               │
│    ITEM 1     │    ITEM 2     │
│               │               │
├───────────────┼───────────────┤
│               │               │
│    ITEM 3     │    ITEM 4     │
│               │               │
└───────────────┴───────────────┘
```

Mas podemos sobrescrever:

```css
.item-2 {
    align-self: end;
}
```

Agora:

```text
ITEM 1 → center
ITEM 2 → end
ITEM 3 → center
ITEM 4 → center
```

Esse é um dos usos mais importantes de `align-self`.

---

# 18. Modelo "regra geral + exceção"

Uma maneira muito poderosa de pensar em:

```css
align-items
```

e:

```css
align-self
```

é:

```text
align-items
     ↓
regra geral
     ↓
todos os itens


align-self
     ↓
exceção
     ↓
um item específico
```

Exemplo:

```css
.container {
    align-items: center;
}

.item-destaque {
    align-self: end;
}
```

Resultado:

```text
TODOS
   ↓
center

EXCETO
   ↓
.item-destaque
   ↓
end
```

---

# 19. `align-self` × `justify-self`

Essa é a relação mais importante entre as propriedades estudadas.

```text
              GRID ITEM
                  │
          ┌───────┴───────┐
          │               │
          ▼               ▼
   justify-self       align-self
          │               │
          ▼               ▼
    eixo inline      eixo block
          │               │
          ▼               ▼
   geralmente         geralmente
   horizontal          vertical
```

Em um layout horizontal tradicional:

```text
┌─────────────────────────────────┐
│                                 │
│             ↑                   │
│             │                   │
│      align-self                 │
│             │                   │
│             ↓                   │
│         ┌────────┐              │
│ ←────── │  ITEM  │ ──────→      │
│         └────────┘              │
│         ↑              ↑        │
│         └ justify-self ┘        │
│                                 │
└─────────────────────────────────┘
```

Podemos combinar:

```css
.item {
    justify-self: center;
    align-self: center;
}
```

Resultado:

```text
┌─────────────────────────────────┐
│                                 │
│                                 │
│            ITEM                 │
│                                 │
│                                 │
└─────────────────────────────────┘
```

O item fica centralizado nos dois eixos.

---

# 20. Por que tecnicamente falamos em eixo e não em "horizontal/vertical"?

É muito comum aprender:

```text
justify = horizontal
align   = vertical
```

Essa simplificação funciona em muitos layouts tradicionais.

Mas tecnicamente o CSS utiliza os conceitos de:

```text
inline axis
block axis
```

O eixo inline acompanha a direção em que o texto flui no `writing-mode`.

O eixo block é o eixo em que os blocos são organizados.

Por isso, a forma tecnicamente correta é:

```text
justify-self
    ↓
eixo inline

align-self
    ↓
eixo block
```

Em idiomas e modos de escrita tradicionais, esses eixos normalmente aparecem como horizontal e vertical, mas isso não é uma regra universal.

---

# 21. `align-self: auto`

Embora não tenha sido explorado na aula, existe:

```css
align-self: auto;
```

Esse é o valor inicial de `align-self`.

No contexto de Grid, `auto` utiliza o valor de `align-items` do elemento pai.

Assim:

```text
GRID CONTAINER
       │
       │ align-items: center
       ↓
GRID ITEM
       │
       │ align-self: auto
       ↓
usa o alinhamento do pai
       ↓
center
```

Isso explica por que `align-items` pode funcionar como uma configuração geral e `align-self` pode ser utilizado para sobrescrever apenas alguns itens.

---

# 22. O comportamento padrão

No CSS Grid, o valor inicial de:

```css
align-items
```

é `normal`.

Para Grid, `normal` resolve para um comportamento equivalente a `stretch` na situação padrão.

Enquanto:

```css
align-self
```

possui `auto` como valor inicial e, por meio dessa relação com `align-items`, o item normalmente acaba se estendendo pela área disponível.

Existe uma exceção importante para elementos com **aspect ratio** ou tamanho intrínseco, como imagens, para evitar distorções.

Modelo simplificado:

```text
align-items
    ↓
normal
    ↓
Grid
    ↓
stretch
    ↓
itens ocupam a área disponível
```

---

# 23. Exceção importante: imagens e proporção

Imagine:

```html
<img src="foto.jpg" alt="Foto">
```

A imagem possui uma proporção natural.

Por exemplo:

```text
1920 × 1080

16 : 9
```

Esticar livremente a imagem nos dois eixos poderia distorcê-la.

Por isso, o comportamento padrão de alinhamento possui tratamento especial para itens com proporção intrínseca. O MDN destaca essa exceção para elementos como imagens.

Modelo mental:

```text
ITEM NORMAL
    ↓
stretch
    ↓
ocupa a área


IMAGEM COM PROPORÇÃO
    ↓
evita distorção
    ↓
comportamento semelhante a start
```

---

# 24. `align-self` e `height`

Não devemos pensar em:

```css
align-self: stretch;
```

como sendo simplesmente:

```css
height: 100%;
```

São mecanismos diferentes.

`align-self` participa do sistema de **Box Alignment**.

O resultado também depende de como o item foi dimensionado e das restrições aplicadas.

Por exemplo:

```css
.item {
    align-self: stretch;
    max-height: 100px;
}
```

A propriedade:

```css
max-height
```

pode limitar o quanto o item consegue se expandir.

Portanto, `stretch` significa utilizar o espaço disponível **dentro das regras de dimensionamento existentes**.

---

# 25. `place-self`

Existe também uma propriedade shorthand:

```css
place-self
```

Ela combina:

```text
align-self
+
justify-self
```

Por exemplo:

```css
.item {
    place-self: center;
}
```

é equivalente a:

```css
.item {
    align-self: center;
    justify-self: center;
}
```

Também podemos definir valores diferentes para cada eixo:

```css
.item {
    place-self: end center;
}
```

Mentalmente:

```text
place-self
    │
    ├── primeiro valor → align-self
    │
    └── segundo valor → justify-self
```

O MDN define `place-self` como shorthand dessas duas propriedades.

---

# 26. Valores adicionais

Além dos quatro valores trabalhados na aula:

```css
start
end
center
stretch
```

a propriedade também suporta outros valores, dependendo do contexto:

```css
align-self: auto;
align-self: normal;

align-self: self-start;
align-self: self-end;

align-self: baseline;
align-self: first baseline;
align-self: last baseline;

align-self: safe center;
align-self: unsafe center;
```

Esses valores fazem parte do sistema CSS Box Alignment e são úteis em situações mais avançadas.

Para o estudo inicial de Grid, entretanto, o modelo essencial da aula pode ser reduzido a:

```text
start
center
end
stretch
```

---

# 27. Mapa mental

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
                            ▼
                         ALINHAR
                            │
              ┌─────────────┴─────────────┐
              │                           │
              ▼                           ▼
       EIXO INLINE                  EIXO BLOCK
              │                           │
              ▼                           ▼
       justify-self                  align-self
              │                           │
       ┌──────┼──────┐            ┌───────┼───────┐
       │      │      │            │       │       │
       ▼      ▼      ▼            ▼       ▼       ▼
     start center   end         start   center    end
                         \          /
                          \        /
                           stretch
```

---

# 28. Regra de ouro

> **`align-self` controla o alinhamento de um único Grid Item dentro da sua Grid Area, no eixo block.**

Em um layout horizontal tradicional:

```text
align-self
    ↓
vertical
    ↓
dentro da Grid Area
    ↓
item individual
```

E a comparação fundamental fica:

```text
justify-self
    ↓
eixo inline
    ↓
normalmente horizontal


align-self
    ↓
eixo block
    ↓
normalmente vertical
```

---

# 29. Como raciocinar diante de um problema

Quando encontrar:

```css
.item {
    align-self: center;
}
```

siga este raciocínio:

```text
1. Este elemento é um Grid Item?
        ↓
       SIM

2. Qual é a Grid Area dele?
        ↓
   descubra as linhas/
   colunas que ele ocupa

3. Em qual eixo estamos trabalhando?
        ↓
   eixo block

4. Qual valor foi utilizado?
        ↓
      center

5. Resultado:
        ↓
   o item é centralizado
   dentro da Grid Area
```

Esse processo é muito mais útil do que decorar:

```text
align = vertical
```

porque você passa a entender **por que** o resultado acontece.

---

# 30. Tabela de fixação

| Valor     | Comportamento                                                                                                    | Mentalidade            |
| --------- | ---------------------------------------------------------------------------------------------------------------- | ---------------------- |
| `start`   | alinha o item no início do eixo block                                                                            | "vai para o começo"    |
| `center`  | centraliza o item dentro da área                                                                                 | "vai para o meio"      |
| `end`     | alinha o item no final do eixo block                                                                             | "vai para o final"     |
| `stretch` | estica o item para utilizar o espaço disponível, respeitando suas restrições de tamanho                          | "preenche o espaço"    |
| `auto`    | utiliza o alinhamento definido por `align-items` no pai                                                          | "segue a regra geral"  |
| `normal`  | comportamento definido pelo modelo de layout; em Grid, normalmente conduz a comportamento semelhante a `stretch` | "comportamento normal" |

---

# 31. Tabela de comparação

| Propriedade       | Aplicada em    | Controla                          | Eixo   |
| ----------------- | -------------- | --------------------------------- | ------ |
| `align-content`   | Grid Container | conjunto de tracks do Grid        | block  |
| `align-items`     | Grid Container | alinhamento padrão dos itens      | block  |
| `align-self`      | Grid Item      | alinhamento de um item específico | block  |
| `justify-content` | Grid Container | conjunto de tracks do Grid        | inline |
| `justify-items`   | Grid Container | alinhamento padrão dos itens      | inline |
| `justify-self`    | Grid Item      | alinhamento de um item específico | inline |

---

# 32. A diferença em uma única imagem mental

```text
                        GRID CONTAINER
┌─────────────────────────────────────────────────┐
│                                                 │
│          Grid Area                              │
│      ┌─────────────────────┐                    │
│      │                     │                    │
│      │        ITEM         │ ← align-self       │
│      │                     │                    │
│      └─────────────────────┘                    │
│                                                 │
└─────────────────────────────────────────────────┘
                     ↑
                     │
               Grid Area definida
               por Grid Placement
```

Depois:

```text
GRID AREA
┌───────────────────────────────┐
│ start                         │
│                               │
│            center             │
│                               │
│                         end   │
└───────────────────────────────┘
```

O `align-self` escolhe como o item será alinhado nessa dimensão.

---

# 33. Checklist de compreensão

Antes de seguir para outro assunto, verifique se consegue explicar:

```text
[ ] O que significa "self" em align-self?

[ ] Em qual elemento coloco align-self?

[ ] Qual é a diferença entre align-items e align-self?

[ ] Em qual eixo align-self trabalha?

[ ] O que acontece com align-self: start?

[ ] O que acontece com align-self: center?

[ ] O que acontece com align-self: end?

[ ] O que acontece com align-self: stretch?

[ ] align-self muda a Grid Area?

[ ] align-self muda as linhas do Grid?

[ ] Qual é a relação entre align-self e grid-row?

[ ] Qual é a relação entre align-self e justify-self?

[ ] Qual é a diferença entre align-self e text-align?

[ ] Qual é a função de place-self?
```

---

# 34. Resumo final

```text
align-self
│
├── aplicado ao Grid Item
│
├── controla o alinhamento individual
│
├── atua dentro da Grid Area
│
├── trabalha no eixo block
│
├── em layouts horizontais tradicionais
│   └── geralmente corresponde ao eixo vertical
│
├── valores fundamentais
│   ├── start
│   ├── center
│   ├── end
│   └── stretch
│
├── pode sobrescrever align-items
│
└── não define onde a Grid Area está
    └── define como o item fica dentro dela
```

---

# 35. Regra definitiva para memorizar

```text
grid-row / grid-column / grid-area
        ↓
"ONDE O ITEM ESTÁ?"

align-self
        ↓
"COMO O ITEM FICA DENTRO DESSE ESPAÇO?"

justify-self
        ↓
"COMO O ITEM FICA NO OUTRO EIXO?"
```

Ou ainda:

```text
             GRID AREA
                 │
        ┌────────┴────────┐
        │                 │
        ▼                 ▼
  justify-self       align-self
        │                 │
        ▼                 ▼
   eixo inline       eixo block
        │                 │
        ▼                 ▼
   normalmente        normalmente
   horizontal          vertical
```

---

# Referências oficiais

* MDN — `align-self`: definição, sintaxe, valores e comportamento em Grid.
* MDN — Box alignment in grid layout: eixos inline/block, `align-items`, `align-self` e comportamento em Grid.
* MDN — `align-items`: relação entre `align-items` e `align-self`.
* MDN — `place-self`: shorthand para `align-self` e `justify-self`.
* CSS Box Alignment / `<self-position>`: valores de posicionamento utilizados pelo sistema de alinhamento CSS.

---

# GitHub

<a href="https://github.com/gabrielfelipeoliveira55" target="_blank" rel="noopener noreferrer">Gabriel Felipe de Oliveira Rateiro</a>
