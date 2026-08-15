CSS Grid — grid-template-rows
1. O que é grid-template-rows?

A propriedade:

grid-template-rows

é utilizada para definir o tamanho e a estrutura das linhas do Grid.

Ela funciona de forma semelhante a:

grid-template-columns

A diferença é a direção em que estamos trabalhando:

grid-template-columns
        ↓
      COLUNAS
        →
grid-template-rows
        ↓
       LINHAS
        ↓

A ideia principal:

grid-template-columns controla as colunas e grid-template-rows controla as linhas.

2. As linhas podem existir sem grid-template-rows

O Grid consegue criar linhas automaticamente.

Por exemplo:

.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
}

Mesmo sem:

grid-template-rows

o Grid continuará criando linhas para acomodar os itens.

Visualmente:

┌──────────┬──────────┐
│ Item 1   │ Item 2   │ ← linha automática
├──────────┼──────────┤
│ Item 3   │ Item 4   │ ← linha automática
├──────────┼──────────┤
│ Item 5   │ Item 6   │ ← linha automática
└──────────┴──────────┘

Portanto:

grid-template-rows não é necessário para que o Grid crie linhas. Ele é utilizado quando queremos controlar explicitamente o tamanho dessas linhas.

3. Definindo linhas explicitamente

Podemos escrever:

.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;

  grid-template-rows:
    50px
    100px
    50px
    200px;
}

Isso cria uma estrutura com quatro linhas:

Linha 1 → 50px
Linha 2 → 100px
Linha 3 → 50px
Linha 4 → 200px

Visualmente:

┌──────────┬──────────┐
│          │          │ 50px
├──────────┼──────────┤
│          │          │ 100px
├──────────┼──────────┤
│          │          │ 50px
├──────────┼──────────┤
│          │          │ 200px
└──────────┴──────────┘
4. O que acontece quando definimos apenas algumas linhas?

Podemos definir somente as primeiras linhas:

.grid {
  display: grid;
  grid-template-rows: 100px 100px;
}

As duas primeiras linhas terão:

Linha 1 → 100px
Linha 2 → 100px

As linhas seguintes serão criadas automaticamente.

Visualmente:

┌──────────┬──────────┐
│          │          │ 100px
├──────────┼──────────┤
│          │          │ 100px
├──────────┼──────────┤
│          │          │ automática
├──────────┼──────────┤
│          │          │ automática
└──────────┴──────────┘

O tamanho das linhas automáticas pode ser influenciado pelo conteúdo.

5. Linhas automáticas e conteúdo

Imagine:

grid-template-rows: 100px 100px;

Se uma terceira linha surgir sem ter sido definida, o Grid poderá dimensioná-la de acordo com o conteúdo.

Por exemplo:

┌──────────┬──────────┐
│ Item 1   │ Item 2   │ 100px
├──────────┼──────────┤
│ Item 3   │ Item 4   │ 100px
├──────────┼──────────┤
│ texto    │ texto    │ ← altura automática
│ grande   │ grande   │
└──────────┴──────────┘

Se o conteúdo ocupar mais linhas, a linha automática poderá aumentar.

6. Conteúdo maior que a altura da linha

Uma linha também pode receber uma altura fixa menor que o conteúdo necessário.

Por exemplo:

.grid {
  display: grid;
  grid-template-rows: 50px;
}

Se o conteúdo precisar de mais espaço, a linha continuará com a dimensão especificada, enquanto o conteúdo poderá ultrapassar seus limites.

Visualmente:

┌─────────────────────┐
│ Conteúdo muito      │
│ grande que precisa  │
│ de mais espaço      │
└─────────────────────┘
     ↑
  linha = 50px

A ideia central é:

Definir o tamanho da linha não significa que o conteúdo será necessariamente reduzido para caber nela.

7. O Grid mantém sua estrutura

Um conceito importante do Grid é a existência de uma estrutura formada por:

LINHAS
───────
LINHAS
───────
LINHAS

e:

COLUNAS
│
│
│

Isso cria uma grade:

      Colunas
   ↓      ↓      ↓

┌──────┬──────┬──────┐
│      │      │      │ ← Linha
├──────┼──────┼──────┤
│      │      │      │ ← Linha
├──────┼──────┼──────┤
│      │      │      │ ← Linha
└──────┴──────┴──────┘

O conteúdo pode mudar de tamanho, mas a estrutura do Grid continua sendo formada por linhas e colunas.

8. fr nas linhas

Assim como podemos utilizar:

grid-template-columns: 1fr 2fr;

também podemos utilizar fr em:

grid-template-rows

Por exemplo:

grid-template-rows: 1fr 2fr;

A ideia é:

Linha 1 → 1 parte
Linha 2 → 2 partes

Porém, existe uma diferença importante:

O comportamento de fr nas linhas depende do espaço disponível no eixo vertical e das dimensões mínimas necessárias pelo conteúdo.

Por isso, não devemos pensar em:

1fr 2fr

simplesmente como uma divisão fixa de:

33% / 66%

em qualquer situação.

9. 1fr 2fr em grid-template-rows

Exemplo:

.grid {
  display: grid;
  grid-template-rows: 1fr 2fr;
}

A proporção desejada é:

1 : 2

Podemos visualizar:

┌──────────────┐
│              │
│     1fr      │
│              │
├──────────────┤
│              │
│              │
│     2fr      │
│              │
└──────────────┘

Mas o tamanho real dependerá do espaço vertical disponível e das demais restrições do Grid.

10. grid-template-columns + grid-template-rows

As duas propriedades podem ser utilizadas juntas para construir layouts completos.

Exemplo:

.grid {
  display: grid;

  grid-template-columns:
    100px
    1fr
    50px;

  grid-template-rows:
    50px
    200px
    50px;
}

Agora temos controle sobre:

COLUNAS
100px | 1fr | 50px

e:

LINHAS
50px
200px
50px

Visualmente:

┌────────┬────────────────────┬───────┐
│        │                    │       │
│        │       HEADER       │       │ 50px
├────────┼────────────────────┼───────┤
│ 100px  │      CONTENT       │ 50px  │ 200px
│        │                    │       │
├────────┼────────────────────┼───────┤
│        │       FOOTER       │       │ 50px
└────────┴────────────────────┴───────┘
11. Exemplo de layout real

Uma estrutura comum pode ser imaginada como:

┌─────────────┬────────────────────────┬────────────┐
│             │                        │            │
│ Navigation  │        Header          │            │
│             │                        │            │
├─────────────┼────────────────────────┼────────────┤
│             │                        │            │
│ Navigation  │        Content         │ Ads        │
│             │                        │            │
├─────────────┼────────────────────────┼────────────┤
│             │                        │            │
│             │        Footer          │            │
└─────────────┴────────────────────────┴────────────┘

Podemos representar as colunas como:

grid-template-columns:
  100px
  1fr
  50px;

E as linhas:

grid-template-rows:
  50px
  200px
  50px;

Assim temos:

COLUNAS
↓
100px | 1fr | 50px

LINHAS
↓
50px
200px
50px
12. Células vazias

Uma combinação de linhas e colunas cria várias células.

Por exemplo:

grid-template-columns: 100px 1fr 50px;

grid-template-rows:
  50px
  200px
  50px;

Temos:

3 colunas × 3 linhas = 9 células

Visualmente:

┌───────┬──────────────┬──────┐
│  C1   │      C2      │ C3   │
├───────┼──────────────┼──────┤
│  C4   │      C5      │ C6   │
├───────┼──────────────┼──────┤
│  C7   │      C8      │ C9   │
└───────┴──────────────┴──────┘

Nem toda célula precisa necessariamente conter um elemento.

Podemos ter:

┌───────┬──────────────┬──────┐
│ Item  │ Item         │ Item │
├───────┼──────────────┼──────┤
│ Item  │              │ Item │
├───────┼──────────────┼──────┤
│ Item  │ Item         │      │
└───────┴──────────────┴──────┘
13. O que acontece quando novos itens são adicionados?

Imagine:

grid-template-columns:
  100px
  1fr
  50px;

Temos três colunas.

Se adicionarmos mais itens, o Grid continuará respeitando essa estrutura de três colunas.

Exemplo:

Item 1 | Item 2 | Item 3
Item 4 | Item 5 | Item 6
Item 7 | Item 8 | Item 9

Os itens seguintes serão distribuídos nas próximas células disponíveis.

Regra mental

O número de colunas definido em grid-template-columns continua servindo como estrutura para os próximos itens.

14. Fluxo dos itens no Grid

Se temos:

grid-template-columns:
  100px
  1fr
  50px;

o Grid possui:

3 colunas

Os itens podem ser distribuídos assim:

┌────────┬────────┬────────┐
│ Item 1 │ Item 2 │ Item 3 │
├────────┼────────┼────────┤
│ Item 4 │ Item 5 │ Item 6 │
├────────┼────────┼────────┤
│ Item 7 │ Item 8 │ Item 9 │
└────────┴────────┴────────┘

Quando não existem itens suficientes:

┌────────┬────────┬────────┐
│ Item 1 │ Item 2 │ Item 3 │
├────────┼────────┼────────┤
│ Item 4 │ Item 5 │         │
└────────┴────────┴────────┘

A estrutura continua existindo mesmo que algumas células estejam vazias.

15. Linhas do Grid

As linhas são importantes não apenas visualmente.

Elas também podem ser utilizadas posteriormente para posicionar elementos.

Considere:

        coluna 1   coluna 2   coluna 3

linha 1 ┌─────────┬─────────┬─────────┐
        │         │         │         │
linha 2 ├─────────┼─────────┼─────────┤
        │         │         │         │
linha 3 ├─────────┼─────────┼─────────┤
        │         │         │         │
linha 4 └─────────┴─────────┴─────────┘

Observe que as linhas numeradas ficam nas divisões da grade:

Linha 1
   ↓
┌───────────────┐

Linha 2
   ↓
├───────────────┤

Linha 3
   ↓
├───────────────┤

Linha 4
   ↓
└───────────────┘

Isso será importante para propriedades de posicionamento de Grid.

16. Linhas não são a mesma coisa que áreas

É importante separar:

Grid Line
↓
linha estrutural da grade

de:

Grid Row
↓
faixa entre duas linhas

Por exemplo:

Linha 1
  ↓
┌─────────────┐
│   Row 1     │
├─────────────┤
│   Row 2     │
├─────────────┤
│   Row 3     │
└─────────────┘
  ↑
Linha 4

A primeira linha de conteúdo está entre:

linha 1
    e
linha 2
17. repeat() em grid-template-rows

Assim como em grid-template-columns, podemos utilizar:

repeat()

Exemplo:

grid-template-rows:
  repeat(3, 50px);

Isso equivale a:

grid-template-rows:
  50px
  50px
  50px;

Visualmente:

┌──────────────┐
│              │ 50px
├──────────────┤
│              │ 50px
├──────────────┤
│              │ 50px
└──────────────┘
18. repeat() com apenas algumas linhas

Podemos definir:

grid-template-rows:
  repeat(2, 50px);

Isso significa:

Linha 1 → 50px
Linha 2 → 50px

As linhas seguintes podem continuar sendo criadas automaticamente.

┌──────────────┐
│              │ 50px
├──────────────┤
│              │ 50px
├──────────────┤
│ conteúdo     │ automática
├──────────────┤
│ conteúdo     │ automática
└──────────────┘
19. minmax() em grid-template-rows

A função:

minmax()

também pode ser utilizada nas linhas.

Exemplo:

grid-template-rows:
  minmax(100px, 1fr);

Isso significa:

mínimo → 100px
máximo → 1fr

A linha pode crescer de forma flexível, mas possui um limite mínimo.

20. Valores que podem ser utilizados

Assim como em grid-template-columns, podemos utilizar diferentes tipos de valores em:

grid-template-rows

Por exemplo:

grid-template-rows: 50px 100px 200px;
grid-template-rows: 1fr 2fr;
grid-template-rows: 20% 40% 40%;
grid-template-rows:
  repeat(3, 50px);
grid-template-rows:
  minmax(100px, 1fr) 200px;

A ideia geral é a mesma:

COLUNAS
grid-template-columns
       ↓
define colunas

LINHAS
grid-template-rows
       ↓
define linhas
21. Mapa mental — grid-template-rows
                    GRID
                     │
             ┌───────┴───────┐
             │               │
         COLUNAS           LINHAS
             │               │
             ↓               ↓
 grid-template-columns  grid-template-rows
                             │
                             ↓
                       define as linhas
                             │
             ┌───────────────┼───────────────┐
             ↓               ↓               ↓
            px              fr             %
             │               │               │
        tamanho fixo    proporção       tamanho relativo
                             │
             ┌───────────────┴───────────────┐
             ↓                               ↓
          repeat()                         minmax()
             │                               │
       repete linhas                 mínimo + máximo
22. Mapa mental — columns x rows
                     CSS GRID
                        │
             ┌──────────┴──────────┐
             │                     │
         COLUMNS                 ROWS
             │                     │
             ↓                     ↓
grid-template-columns     grid-template-rows
             │                     │
             ↓                     ↓
       controla a largura     controla a altura
       das colunas            das linhas
Corte mental ①
COLUMN
→ esquerda ↔ direita
→ largura
ROW
→ cima ↕ baixo
→ altura
23. Mapa mental — construção do Grid
GRID CONTAINER
      │
      ↓
display: grid
      │
      ├───────────────────┐
      ↓                   ↓
  COLUNAS               LINHAS
      │                   │
      ↓                   ↓
grid-template-columns grid-template-rows
      │                   │
      ↓                   ↓
larguras              alturas
      │                   │
      └─────────┬─────────┘
                ↓
             CÉLULAS
                │
                ↓
             GRID ITEMS
Corte mental ②
display: grid
      ↓
"Ative o Grid"

Depois:

grid-template-columns
      ↓
"Como serão as colunas?"

E:

grid-template-rows
      ↓
"Como serão as linhas?"
24. Mapa mental — repeat()
repeat()
   │
   ├── quantidade
   │      ↓
   │   quantas vezes?
   │
   └── valor
          ↓
      o que repetir?

Exemplo:

grid-template-rows:
  repeat(3, 50px);

Leitura:

3 vezes
   +
50px
   ↓
50px
50px
50px
Corte mental ③

repeat(quantidade, valor) = "repita X vezes este valor".

25. Mapa mental — tamanho das linhas
grid-template-rows
        │
        ├── 50px
        │    ↓
        │  tamanho fixo
        │
        ├── 1fr
        │    ↓
        │  espaço proporcional
        │
        ├── 50%
        │    ↓
        │  porcentagem
        │
        ├── repeat()
        │    ↓
        │  repetição
        │
        └── minmax()
             ↓
        mínimo + máximo
26. Exemplo completo
.grid {
  display: grid;

  grid-template-columns:
    100px
    1fr
    50px;

  grid-template-rows:
    50px
    200px
    50px;

  gap: 20px;
}

Podemos traduzir:

GRID
 │
 ├── 3 COLUNAS
 │      │
 │      ├── 100px
 │      ├── 1fr
 │      └── 50px
 │
 └── 3 LINHAS
        │
        ├── 50px
        ├── 200px
        └── 50px

Visualmente:

             COLUNAS
       100px   1fr   50px
          ↓     ↓      ↓
      ┌──────┬───────┬─────┐
50px  │      │       │     │
      ├──────┼───────┼─────┤
200px │      │       │     │
      ├──────┼───────┼─────┤
50px  │      │       │     │
      └──────┴───────┴─────┘
          ↑
         ROWS
27. Pontos que precisam ficar na memória
┌─────────────────────────────────────────┐
│        GRID-TEMPLATE-ROWS               │
├─────────────────────────────────────────┤
│ Controla as linhas do Grid.             │
│                                         │
│ Não é obrigatório para criar linhas.   │
│ O Grid pode criá-las automaticamente.   │
│                                         │
│ Permite definir tamanhos explícitos.    │
│                                         │
│ Aceita px, %, fr, repeat(), minmax()... │
└─────────────────────────────────────────┘
Corte mental ④
grid-template-columns
→ COMO SÃO AS COLUNAS?

grid-template-rows
→ COMO SÃO AS LINHAS?
28. ⚠️ Atenção ao conteúdo

Definir:

grid-template-rows: 50px;

não significa necessariamente:

"Faça o conteúdo caber em 50px."

Significa:

"Essa faixa do Grid possui 50px."

Se o conteúdo precisar de mais espaço, ele poderá ultrapassar a área disponível conforme as demais regras do layout.

O mesmo raciocínio vale para colunas:

grid-template-columns: 100px;

não garante que todo conteúdo ficará visualmente limitado a 100px.

29. Resumo final
                    CSS GRID
                       │
          ┌────────────┴────────────┐
          │                         │
       COLUMNS                    ROWS
          │                         │
          ↓                         ↓
grid-template-columns      grid-template-rows
          │                         │
          │                         │
      largura                     altura
          │                         │
          └────────────┬────────────┘
                       ↓
                   GRID CELLS
                       │
                       ↓
                  GRID ITEMS
Fórmula mental principal
COLUMN
→ largura da coluna

ROW
→ altura da linha
grid-template-columns
→ controla colunas
grid-template-rows
→ controla linhas

E as duas propriedades podem trabalhar juntas:

.grid {
  display: grid;

  grid-template-columns:
    100px 1fr 50px;

  grid-template-rows:
    50px 200px 50px;
}
🧠 Regra final

grid-template-columns organiza o Grid horizontalmente através das colunas; grid-template-rows organiza o Grid verticalmente através das linhas.

As duas aceitam conceitos como px, %, fr, repeat() e minmax(), permitindo construir desde grades, simples e até layouts mais complexos e flexíveis.