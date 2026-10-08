# CSS Grid — `justify-content`

## 1. O que é `justify-content`?

A propriedade:

```css
justify-content
```

controla o alinhamento horizontal do conjunto de colunas do Grid dentro do espaço disponível do container.

Ela não reorganiza o HTML e não muda a ordem dos itens. O que ela faz é decidir **como a estrutura do Grid se posiciona no eixo horizontal** quando sobra espaço dentro do container.

No contexto deste exemplo, o foco está em entender:

- como o Grid se desloca para a esquerda, direita ou centro;
- como o espaço pode ser distribuído entre as colunas;
- por que `stretch` depende do tipo de tamanho definido nas tracks.

---

## 2. Onde `justify-content` atua?

Primeiro existe o Grid Container:

```css
.grid {
  display: grid;
}
```

Depois existe a estrutura das colunas:

```css
grid-template: repeat(3, 8.75rem) / repeat(3, 8.75rem);
```

Essa declaração significa:

```text
repeat(3, 8.75rem)
→ 3 linhas com 8.75rem

/

repeat(3, 8.75rem)
→ 3 colunas com 8.75rem
```

Ou seja, o Grid deste exemplo cria uma grade com tamanho fixo:

```text
3 colunas fixas
3 linhas fixas
```

Visualmente:

```text
┌──────────┬──────────┬──────────┐
│    1     │    2     │    3     │
├──────────┼──────────┼──────────┤
│    4     │    5     │    6     │
├──────────┼──────────┼──────────┤
│          │          │          │
└──────────┴──────────┴──────────┘
```

O `justify-content` atua sobre esse bloco de colunas.

### Regra mental

> **`justify-content` responde à pergunta: "onde a grade deve ficar no eixo horizontal?"**

---

## 3. Estrutura usada no exemplo

O HTML repete a mesma grade com valores diferentes de `justify-content`:

```html
<div class="grid start">...</div>
<div class="grid end">...</div>
<div class="grid center">...</div>
<div class="grid stretch">...</div>
<div class="grid space-around">...</div>
<div class="grid space-between">...</div>
<div class="grid space-evenly">...</div>
```

Cada classe altera apenas a forma como o Grid se alinha horizontalmente.

---

## 4. Sintaxe

Exemplo geral:

```css
justify-content: start;
```

Decompondo:

```text
justify-content
→ propriedade que controla o alinhamento horizontal da grade

start
→ encosta a grade no início do eixo horizontal
```

Outros valores usados neste material:

```css
justify-content: end;
justify-content: center;
justify-content: stretch;
justify-content: space-around;
justify-content: space-between;
justify-content: space-evenly;
```

---

## 5. Valores demonstrados

### 5.1 `start`

```css
justify-content: start;
```

Posiciona a grade no início do eixo horizontal.

```text
┌──────────────────────────────────────────────┐
│┌──────┬──────┬──────┐                        │
││ GRID │ GRID │ GRID │                        │
│└──────┴──────┴──────┘                        │
└──────────────────────────────────────────────┘
```

---

### 5.2 `end`

```css
justify-content: end;
```

Posiciona a grade no final do eixo horizontal.

```text
┌──────────────────────────────────────────────┐
│                        ┌──────┬──────┬──────┐│
│                        │ GRID │ GRID │ GRID ││
│                        └──────┴──────┴──────┘│
└──────────────────────────────────────────────┘
```

---

### 5.3 `center`

```css
justify-content: center;
```

Centraliza a grade horizontalmente.

```text
┌──────────────────────────────────────────────┐
│        ┌──────┬──────┬──────┐                │
│        │ GRID │ GRID │ GRID │                │
│        └──────┴──────┴──────┘                │
└──────────────────────────────────────────────┘
```

---

### 5.4 `space-around`

```css
justify-content: space-around;
```

Distribui espaço ao redor da grade no eixo horizontal.

O espaço das extremidades tende a ficar menor do que o espaço entre os blocos internos.

---

### 5.5 `space-between`

```css
justify-content: space-between;
```

Distribui o espaço horizontal entre os extremos.

Em termos mentais:

```text
primeira coluna
→ encosta no início

última coluna
→ encosta no fim

espaço livre
→ fica entre as colunas
```

---

### 5.6 `space-evenly`

```css
justify-content: space-evenly;
```

Distribui o espaço horizontal de forma mais uniforme.

### 🧠 Corte mental

```text
space-around
→ espaço ao redor

space-between
→ espaço entre os extremos

space-evenly
→ espaço uniforme
```

---

## 6. O caso mais importante: `stretch`

### 6.1 O que `stretch` tenta fazer?

```css
justify-content: stretch;
```

O objetivo do `stretch` é fazer as tracks ocuparem o espaço horizontal disponível.

Em outras palavras:

```text
há espaço sobrando no container
        ↓
as colunas podem crescer?
        ↓
se puderem crescer, o Grid pode esticar
```

---

### 6.2 Por que `stretch` não funcionou como esperado?

No exemplo original, a grade foi definida assim:

```css
.grid {
  grid-template: repeat(3, 8.75rem) / repeat(3, 8.75rem);
}
```

Isso cria colunas com largura fixa:

```text
8.75rem
8.75rem
8.75rem
```

Quando as colunas já têm tamanho fixo, o `stretch` não tem liberdade para expandi-las.

### 🧠 Corte mental

```text
coluna fixa
→ não cresce

stretch
→ precisa de espaço livre + track com possibilidade de crescer
```

Então o problema não está na sintaxe do `justify-content`.

O problema está na relação entre:

```text
justify-content: stretch
        +
grid-template-columns com tamanho fixo
```

Essa combinação impede o efeito visual esperado.

---

## 7. Relação entre `justify-content` e `grid-template-columns`

Esse é o ponto central desta documentação:

```text
justify-content
→ alinha a grade no espaço disponível

grid-template-columns
→ define como as colunas são construídas
```

Se as colunas forem fixas:

```css
grid-template-columns: repeat(3, 8.75rem);
```

o alinhamento horizontal pode mover a grade, mas não consegue transformar colunas fixas em colunas elásticas.

Se as colunas forem flexíveis:

```css
grid-template-columns: repeat(3, 1fr);
```

ou:

```css
grid-template-columns: repeat(3, minmax(0, 1fr));
```

ou ainda:

```css
grid-template-columns: auto auto auto;
```

passa a existir margem para expansão horizontal das tracks.

### Mapa mental

```text
                 JUSTIFY-CONTENT
                        │
                        ↓
          "Como a grade se comporta no eixo horizontal?"
                        │
        ┌───────────────┴───────────────┐
        ↓                               ↓
   tracks fixas                    tracks flexíveis
        │                               │
        ↓                               ↓
 stretch quase não                stretch pode
 mostra efeito                   expandir tracks
```

---

## 8. Ajuste aplicado no exemplo

Na classe `.stretch`, foi usado:

```css
.stretch {
  grid-template-columns: auto;
  justify-content: stretch;
}
```

A intenção desse ajuste é remover a rigidez anterior das colunas para permitir que o `stretch` tenha efeito visível.

### Observação importante

```css
grid-template-columns: auto;
```

altera a estrutura horizontal da grade para uma configuração diferente da base original.

Isso resolve a limitação do exemplo anterior, mas muda a construção das colunas daquela demonstração específica.

> **Importante:** o ponto principal não é decorar esse ajuste isolado, mas entender que `stretch` depende do tipo de dimensionamento usado nas tracks.

---

## 9. Comparação direta

| Situação | Resultado esperado com `stretch` |
| --- | --- |
| `grid-template-columns: repeat(3, 8.75rem);` | pouco ou nenhum efeito visual de expansão |
| `grid-template-columns: auto auto auto;` | pode haver expansão conforme o espaço disponível |
| `grid-template-columns: repeat(3, 1fr);` | as colunas já ocupam o espaço de forma flexível |
| `grid-template-columns: repeat(3, minmax(0, 1fr));` | expansão flexível com controle melhor do tamanho |

---

## 10. Fluxo de raciocínio

```text
1. display: grid
        ↓
2. o elemento vira Grid Container
        ↓
3. grid-template-columns define as colunas
        ↓
4. justify-content tenta alinhar a grade no eixo horizontal
        ↓
5. se as tracks forem fixas, stretch quase não atua
        ↓
6. se as tracks puderem crescer, stretch passa a ter efeito
```

---

## 11. Regras mentais para memorizar

> **Regra mental:** `justify-content` não corrige uma estrutura rígida. Ele trabalha sobre a estrutura que já foi criada.

> **Regra mental:** colunas fixas favorecem diferenças visuais entre `start`, `end` e `center`, mas limitam o efeito de `stretch`.

> **Regra mental:** se a ideia é expandir colunas, olhe primeiro para `grid-template-columns`, não apenas para `justify-content`.

---

## 12. ⚠️ Erros e confusões comuns

Não confunda:

```text
justify-content
→ alinhamento horizontal da grade

grid-template-columns
→ definição estrutural das colunas
```

Também não é uma boa leitura pensar:

```text
"stretch não funciona"
```

O comportamento mais correto é:

```text
"stretch depende de tracks que possam crescer"
```

> **Atenção:** quando a grade é construída com medidas fixas, muitos efeitos de alinhamento aparecem apenas como deslocamento horizontal, não como expansão.

---

## 13. Mapa mental final

```text
                    CSS GRID
                       │
                       ↓
                justify-content
                       │
       ┌───────────────┼────────────────┐
       ↓               ↓                ↓
     posição       distribuição      expansão
       │               │                │
       ↓               ↓                ↓
 start/end/center  space-*          stretch
                                         │
                                         ↓
                         depende do tamanho das tracks
                                         │
                      ┌──────────────────┴──────────────────┐
                      ↓                                     ↓
                 tracks fixas                         tracks flexíveis
                      │                                     │
                      ↓                                     ↓
                 pouco efeito                         efeito visível
```

---

## 14. 📌 Resumo final

```text
CONCEITO
→ `justify-content` alinha a grade no eixo horizontal.

PROPRIEDADE
→ define como o conjunto de colunas se comporta dentro do espaço disponível.

VALORES
→ `start`, `end`, `center`, `stretch`, `space-around`, `space-between`, `space-evenly`.

RELAÇÃO
→ o resultado visual depende diretamente de como as colunas foram definidas em `grid-template-columns`.
```

No exemplo estudado:

```text
`start`, `end` e `center`
→ deslocam visualmente a grade

`space-*`
→ distribuem o espaço horizontal

`stretch`
→ só mostra o efeito esperado quando as tracks podem crescer
```

---

## 15. 🧠 Regra mental definitiva

```text
justify-content
→ "como a grade se alinha no eixo horizontal?"

grid-template-columns
→ "essas colunas são rígidas ou flexíveis?"

stretch
→ "só estica o que pode crescer"
```
