# CSS Grid — `grid-auto-flow`

## 1. O que é `grid-auto-flow`?

A propriedade:

```css
grid-auto-flow
```

define **como o Grid deve posicionar automaticamente os itens** quando eles precisam ser distribuídos pela grade.

Por padrão, o Grid trabalha no sentido de **linhas**:

```text
Item 1 → Item 2 → Item 3
             ↓
        próxima linha
             ↓
Item 4 → Item 5 → Item 6
```

Mas podemos alterar esse comportamento para fazer o preenchimento acontecer por **colunas**.

---

# 2. O comportamento padrão

Por padrão, o `grid-auto-flow` possui o comportamento:

```css
grid-auto-flow: row;
```

Ou seja, os itens são colocados seguindo as linhas.

Imagine:

```css
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
}
```

Com vários itens:

```text
┌──────────┬──────────┐
│ Item 1   │ Item 2   │
├──────────┼──────────┤
│ Item 3   │ Item 4   │
├──────────┼──────────┤
│ Item 5   │ Item 6   │
└──────────┴──────────┘
```

O preenchimento acontece assim:

```text
1 → 2
3 → 4
5 → 6
```

### Regra mental

> **`row` preenche as linhas antes de criar novas linhas.**

---

# 3. `grid-auto-flow: row`

Podemos declarar explicitamente:

```css
.grid {
  grid-auto-flow: row;
}
```

Esse é o comportamento padrão.

Visualmente:

```text
1 → 2 → 3
↓
4 → 5 → 6
↓
7 → 8 → 9
```

O Grid continua adicionando novas linhas quando os itens não cabem na estrutura atual.

---

# 4. `grid-auto-flow: column`

Podemos alterar o fluxo:

```css
.grid {
  grid-auto-flow: column;
}
```

Agora o Grid tenta preencher **colunas** em vez de adicionar novas linhas.

Exemplo:

```text
1 ↓
2 ↓
3 ↓
4
```

Em uma estrutura visual:

```text
┌──────────┬──────────┬──────────┐
│ Item 1   │ Item 4   │ Item 7   │
├──────────┼──────────┼──────────┤
│ Item 2   │ Item 5   │ Item 8   │
├──────────┼──────────┼──────────┤
│ Item 3   │ Item 6   │ Item 9   │
└──────────┴──────────┴──────────┘
```

A direção mudou.

---

# 5. `row` x `column`

A diferença principal:

```text
grid-auto-flow: row;
↓
preenche por linhas
```

```text
grid-auto-flow: column;
↓
preenche por colunas
```

### Mapa mental

```text
                grid-auto-flow
                       │
              ┌────────┴────────┐
              ↓                 ↓
            row              column
              │                 │
              ↓                 ↓
        novas linhas        novas colunas
```

### Corte mental ①

```text
row
→ "continue para a próxima linha"

column
→ "continue para a próxima coluna"
```

---

# 6. `grid-auto-flow: column` precisa de uma estrutura de linhas

Esse ponto é importante.

Se utilizarmos:

```css
.grid {
  display: grid;
  grid-auto-flow: column;
}
```

o Grid precisa saber quantas linhas utilizar para organizar o fluxo.

Podemos definir:

```css
grid-template-rows: 100px 100px;
```

Agora temos duas linhas:

```text
Linha 1 → 100px
Linha 2 → 100px
```

E os itens podem ser distribuídos verticalmente:

```text
┌──────────┬──────────┬──────────┐
│ Item 1   │ Item 3   │ Item 5   │
├──────────┼──────────┼──────────┤
│ Item 2   │ Item 4   │ Item 6   │
└──────────┴──────────┴──────────┘
```

O fluxo ocorre por colunas.

---

# 7. Por que novas colunas podem aparecer?

Imagine:

```css
.grid {
  display: grid;

  grid-template-rows:
    100px
    100px;

  grid-auto-flow: column;
}
```

Temos apenas duas linhas.

Se existem seis itens:

```text
1
2
3
4
5
6
```

o Grid distribui:

```text
Coluna 1:
1
2

Coluna 2:
3
4

Coluna 3:
5
6
```

Visualmente:

```text
┌─────────┬─────────┬─────────┐
│    1    │    3    │    5    │
├─────────┼─────────┼─────────┤
│    2    │    4    │    6    │
└─────────┴─────────┴─────────┘
```

As colunas adicionais são criadas conforme necessário.

---

# 8. `grid-auto-flow: column` + `grid-auto-columns`

Podemos combinar:

```css
.grid {
  display: grid;

  grid-template-rows:
    100px
    100px;

  grid-auto-flow: column;

  grid-auto-columns: 100px;
}
```

Agora:

```text
linhas:
100px
100px

colunas implícitas:
100px
100px
100px
...
```

Resultado:

```text
┌─────────┬─────────┬─────────┐
│    1    │    3    │    5    │
├─────────┼─────────┼─────────┤
│    2    │    4    │    6    │
└─────────┴─────────┴─────────┘
```

---

# 9. Relação com `grid-auto-columns`

Isso conecta diretamente os conceitos estudados anteriormente.

```text
grid-auto-flow: column
        ↓
precisa criar novas colunas
        ↓
essas colunas são implícitas
        ↓
grid-auto-columns
        ↓
define o tamanho delas
```

### Mapa mental

```text
grid-auto-flow
      │
      ↓
"Como vou preencher?"
      │
      ├── row
      │    ↓
      │  novas linhas
      │
      └── column
           ↓
        novas colunas
             │
             ↓
     grid-auto-columns
```

---

# 10. Exemplo com `grid-template-columns`

Podemos combinar o fluxo por colunas com colunas explícitas:

```css
.grid {
  display: grid;

  grid-template-columns:
    100px
    200px
    100px;

  grid-auto-flow:
    column;
}
```

As colunas definidas continuam existindo.

O `grid-auto-flow` apenas determina como os novos itens serão distribuídos.

---

# 11. O fluxo não altera a definição das colunas

Considere:

```css
grid-template-columns:
  100px
  200px
  100px;
```

Temos:

```text
Coluna 1 → 100px
Coluna 2 → 200px
Coluna 3 → 100px
```

Se utilizarmos:

```css
grid-auto-flow: column;
```

o Grid continuará respeitando essa estrutura.

Se precisar criar novas colunas implícitas, elas serão adicionadas seguindo as regras:

```css
grid-auto-columns
```

---

# 12. `grid-auto-flow` não define tamanho

É importante não confundir:

```css
grid-auto-flow
```

com:

```css
grid-auto-columns
```

ou:

```css
grid-auto-rows
```

Cada propriedade possui uma responsabilidade diferente.

```text
grid-auto-flow
→ define A DIREÇÃO do preenchimento
```

```text
grid-auto-columns
→ define O TAMANHO das colunas implícitas
```

```text
grid-auto-rows
→ define O TAMANHO das linhas implícitas
```

### Corte mental ②

```text
FLOW
→ "para onde vai?"

AUTO-COLUMNS
→ "qual tamanho terá a coluna?"

AUTO-ROWS
→ "qual tamanho terá a linha?"
```

---

# 13. Exemplo comparativo

## Fluxo por linhas

```css
grid-auto-flow: row;
```

```text
1 → 2 → 3
4 → 5 → 6
7 → 8 → 9
```

## Fluxo por colunas

```css
grid-auto-flow: column;
```

```text
1 ↓ 4 ↓ 7
2 ↓ 5 ↓ 8
3 ↓ 6 ↓ 9
```

Visualmente:

```text
ROW

┌───┬───┬───┐
│ 1 │ 2 │ 3 │
├───┼───┼───┤
│ 4 │ 5 │ 6 │
├───┼───┼───┤
│ 7 │ 8 │ 9 │
└───┴───┴───┘
```

```text
COLUMN

┌───┬───┬───┐
│ 1 │ 4 │ 7 │
├───┼───┼───┤
│ 2 │ 5 │ 8 │
├───┼───┼───┤
│ 3 │ 6 │ 9 │
└───┴───┴───┘
```

---

# 14. `grid-auto-flow: dense`

Existe também:

```css
grid-auto-flow: dense;
```

O objetivo do `dense` é fazer o algoritmo tentar **preencher espaços vazios anteriormente disponíveis** na grade.

Isso pode ser útil quando alguns elementos ocupam várias células e deixam espaços que outros elementos menores poderiam ocupar.

---

# 15. O problema que `dense` tenta resolver

Imagine três colunas:

```css
grid-template-columns:
  1fr 1fr 1fr;
```

Temos:

```text
┌──────┬──────┬──────┐
│  1   │  2   │  3   │
├──────┼──────┼──────┤
│  4   │  5   │  6   │
└──────┴──────┴──────┘
```

Agora imagine que o item `2` precise ocupar três colunas:

```text
┌──────┬──────┬──────┐
│  1   │      │      │
├──────┼──────┼──────┤
│  2 → → → → →       │
└──────┴──────┴──────┘
```

Como o item 2 não cabe na primeira linha restante, ele pode ser colocado na linha seguinte.

Isso pode deixar espaços vazios.

---

# 16. Espaços vazios

Imagine:

```text
┌──────┬──────┬──────┐
│  1   │      │      │
├──────┼──────┼──────┤
│  2   │  2   │  2   │
├──────┼──────┼──────┤
│  4   │  5   │  6   │
└──────┴──────┴──────┘
```

Os espaços vazios poderiam ser ocupados por itens menores.

Sem `dense`, o Grid normalmente preserva a ordem automática dos itens.

---

# 17. O que `dense` tenta fazer?

Com:

```css
grid-auto-flow: dense;
```

o Grid tenta encontrar espaços vazios anteriores para colocar itens que caibam.

A ideia é:

```text
espaço vazio
     ↓
existem itens depois?
     ↓
algum cabe aqui?
     ↓
SIM
     ↓
tente preencher
```

Visualmente:

```text
ANTES

┌──────┬──────┬──────┐
│  1   │      │      │
├──────┼──────┼──────┤
│  2   │  2   │  2   │
├──────┼──────┼──────┤
│  4   │  5   │  6   │
└──────┴──────┴──────┘
```

Com `dense`, itens menores podem ocupar os espaços disponíveis.

```text
DEPOIS

┌──────┬──────┬──────┐
│  1   │  4   │  5   │
├──────┼──────┼──────┤
│  2   │  2   │  2   │
├──────┼──────┼──────┤
│  6   │  7   │  8   │
└──────┴──────┴──────┘
```

A disposição exata depende das dimensões e posições dos itens.

---

# 18. `dense` pode alterar a ordem visual

Esse é um ponto muito importante.

Sem `dense`, os itens tendem a manter a ordem natural do fluxo automático:

```text
1
2
3
4
5
6
```

Com `dense`, um item posterior pode ocupar um espaço anterior disponível.

Assim:

```text
ordem no HTML
↓
1, 2, 3, 4, 5, 6
```

pode resultar em uma ordem visual diferente da sequência original.

### Regra mental

> **`dense` prioriza o preenchimento dos espaços disponíveis, não a manutenção perfeita da ordem visual.**

---

# 19. Quando `dense` é útil?

É especialmente interessante quando a ordem visual dos itens **não é tão importante**.

Um exemplo típico:

```text
galeria de imagens
```

Imagine:

```text
┌───────┬───────┬───────┐
│       │       │       │
│ Foto  │ Foto  │ Foto  │
├───────┼───────┼───────┤
│ Foto grande         │
└───────────────────────┘
```

Uma galeria pode priorizar o aproveitamento visual do espaço.

Nesse caso:

```css
grid-auto-flow: dense;
```

pode ser útil.

---

# 20. Quando evitar `dense`?

Se a ordem visual dos elementos for importante, devemos ter cuidado.

Por exemplo:

```text
documento
texto
artigo
lista
conteúdo sequencial
```

Imagine:

```text
Item 1
Item 2
Item 3
```

Se o `dense` mover visualmente o `Item 3` para antes do `Item 2`, a leitura pode ficar confusa.

### Regra prática

```text
ordem importa
→ evite dense

ordem visual não importa muito
→ dense pode ser útil
```

---

# 21. `dense` é diferente de `column`

Não confunda:

```css
grid-auto-flow: column;
```

com:

```css
grid-auto-flow: dense;
```

`column` define:

> **A direção do fluxo.**

`dense` define:

> **Uma estratégia para tentar preencher espaços vazios.**

São conceitos diferentes.

---

# 22. Combinações possíveis

Podemos combinar:

```css
grid-auto-flow: row dense;
```

ou:

```css
grid-auto-flow: column dense;
```

A ideia é:

```text
row dense
→ fluxo por linhas + preenchimento denso
```

```text
column dense
→ fluxo por colunas + preenchimento denso
```

---

# 23. Mapa mental — `grid-auto-flow`

```text
                    grid-auto-flow
                          │
             ┌────────────┼────────────┐
             ↓            ↓            ↓
            row         column        dense
             │            │            │
             ↓            ↓            ↓
       novas linhas  novas colunas  preenche
                                     espaços
                                     vazios
```

### Corte mental ①

```text
row
→ "desce"

column
→ "vai para o lado"

dense
→ "procura espaços vazios"
```

---

# 24. Mapa mental — relação com outras propriedades

```text
                     GRID
                      │
              ┌───────┴───────┐
              ↓               ↓
          AUTO FLOW        AUTO SIZE
              │               │
              ↓               ↓
         direção          tamanho
              │               │
      ┌───────┴───────┐   ┌───┴────────┐
      ↓               ↓   ↓            ↓
     row            column auto-rows auto-columns
```

A distinção:

```text
grid-auto-flow
→ PARA ONDE os itens vão

grid-auto-rows
→ QUANTO mede a linha

grid-auto-columns
→ QUANTO mede a coluna
```

---

# 25. Exemplo completo — fluxo por linhas

```css
.grid {
  display: grid;

  grid-template-columns:
    repeat(3, 1fr);

  grid-auto-flow:
    row;
}
```

Fluxo:

```text
1 → 2 → 3
4 → 5 → 6
7 → 8 → 9
```

---

# 26. Exemplo completo — fluxo por colunas

```css
.grid {
  display: grid;

  grid-template-rows:
    repeat(3, 100px);

  grid-auto-flow:
    column;
}
```

Fluxo:

```text
1 ↓
2 ↓
3

4 ↓
5 ↓
6

7 ↓
8 ↓
9
```

Visualmente:

```text
┌───┬───┬───┐
│ 1 │ 4 │ 7 │
├───┼───┼───┤
│ 2 │ 5 │ 8 │
├───┼───┼───┤
│ 3 │ 6 │ 9 │
└───┴───┴───┘
```

---

# 27. Exemplo completo — `dense`

```css
.grid {
  display: grid;

  grid-template-columns:
    repeat(3, 1fr);

  grid-auto-flow:
    dense;
}
```

Agora o algoritmo tenta aproveitar melhor espaços vazios deixados por elementos que ocupam mais de uma célula.

---

# 28. Relação com `grid-column`

Uma situação comum para entender `dense` é quando um item ocupa várias colunas.

Por exemplo:

```css
.item-2 {
  grid-column: span 3;
}
```

Isso significa:

> O item 2 deve ocupar três colunas.

Se ele não couber na posição atual, o Grid poderá colocá-lo na próxima posição adequada, deixando espaços disponíveis.

Com:

```css
grid-auto-flow: dense;
```

outros itens podem tentar preencher esses espaços.

---

# 29. `dense` é uma estratégia de preenchimento

Podemos pensar em:

```text
grid-auto-flow: row;
```

como:

```text
"Continue na ordem."
```

Enquanto:

```text
grid-auto-flow: dense;
```

é:

```text
"Continue na ordem, mas tente aproveitar espaços disponíveis."
```

### Corte mental ②

```text
normal
→ respeita mais diretamente o fluxo

dense
→ prioriza o preenchimento da grade
```

---

# 30. ⚠️ Cuidado com a ordem

Imagine:

```text
HTML

1
2
3
4
5
6
```

O usuário espera:

```text
1
2
3
4
5
6
```

Mas com:

```css
grid-auto-flow: dense;
```

o posicionamento visual pode rearranjar itens para preencher espaços.

Portanto:

```text
ordem semântica importante
→ cuidado com dense
```

Para uma galeria:

```text
ordem menos importante
→ dense pode ser útil
```

---

# 31. `grid-auto-flow` e responsividade

Podemos utilizar o fluxo automático em layouts que precisam acomodar quantidades diferentes de itens.

Por exemplo:

```css
.gallery {
  display: grid;

  grid-template-columns:
    repeat(3, 1fr);

  grid-auto-flow:
    row;
}
```

Os itens adicionais continuam criando novas linhas.

Podemos combinar isso com outras propriedades do Grid para criar layouts flexíveis.

---

# 32. Mapa mental — escolha rápida

```text
                     PRECISO CONTROLAR
                       O FLUXO DOS ITENS?
                              │
                    ┌─────────┴─────────┐
                    ↓                   ↓
                   SIM                 NÃO
                    │
                    ↓
              grid-auto-flow
                    │
        ┌───────────┼───────────┐
        ↓           ↓           ↓
       row        column      dense
        │           │           │
        ↓           ↓           ↓
     linhas      colunas     espaços
                               vazios
```

---

# 33. Regra mental definitiva

```text
grid-auto-flow
       ↓
"Qual é a direção do preenchimento?"
```

### `row`

```text
→ → →
↓
→ → →
```

### `column`

```text
↓
↓
↓
→
↓
↓
↓
```

### `dense`

```text
"Existe um espaço vazio que outro item pode ocupar?"
        ↓
      tente preencher
```

---

# 34. Resumo das propriedades relacionadas

| Propriedade             | Função                                                  |
| ----------------------- | ------------------------------------------------------- |
| `grid-auto-flow`        | Define a direção/estratégia do preenchimento automático |
| `grid-auto-rows`        | Define o tamanho das linhas implícitas                  |
| `grid-auto-columns`     | Define o tamanho das colunas implícitas                 |
| `grid-template-rows`    | Define linhas explícitas                                |
| `grid-template-columns` | Define colunas explícitas                               |

Podemos memorizar:

```text
TEMPLATE
→ estrutura que você define

AUTO
→ estrutura criada automaticamente

FLOW
→ direção/estratégia usada para preencher
```

---

# 35. 📌 Resumo final

```text
                    GRID AUTO FLOW
                          │
           ┌──────────────┼──────────────┐
           ↓              ↓              ↓
          row           column          dense
           │              │              │
           ↓              ↓              ↓
     cria novas       cria novas      aproveita
       linhas          colunas        espaços
```

### `row`

```css
grid-auto-flow: row;
```

> Preenche os itens seguindo as linhas.

### `column`

```css
grid-auto-flow: column;
```

> Preenche os itens seguindo as colunas.

### `dense`

```css
grid-auto-flow: dense;
```

> Tenta preencher espaços vazios deixados por outros itens.

### Combinação

```css
grid-auto-flow: row dense;
```

ou:

```css
grid-auto-flow: column dense;
```

> Combina uma direção de fluxo com o preenchimento denso.

---

# 36. 🧠 Três frases para guardar

> **`grid-auto-flow: row` → os itens fluem por linhas.**

> **`grid-auto-flow: column` → os itens fluem por colunas.**

> **`dense` → o Grid tenta aproveitar espaços vazios mesmo que isso possa alterar a ordem visual.**

A relação geral fica:

```text
grid-auto-flow
→ COMO preencher?

grid-auto-rows
→ QUAL TAMANHO têm as linhas automáticas?

grid-auto-columns
→ QUAL TAMANHO têm as colunas automáticas?
```

### Corte mental final

```text
FLOW
→ direção

AUTO-ROWS
→ tamanho das linhas

AUTO-COLUMNS
→ tamanho das colunas
```
