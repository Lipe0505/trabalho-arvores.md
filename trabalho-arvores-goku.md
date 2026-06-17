# Trabalho – Estruturas Avançadas de Árvores

**Disciplina:** Estruturas de Dados  
**Curso:** Análise e Desenvolvimento de Sistemas  

---

## Parte 1 – Tipos de Árvores

### AVL

**Conceito**

A árvore AVL (Adelson-Velsky e Landis) é uma Árvore Binária de Busca que se mantém balanceada automaticamente. Após cada inserção ou remoção, ela verifica o **fator de balanceamento** de cada nó, que é a diferença entre a altura da subárvore esquerda e a da direita. Se esse fator sair do intervalo `[-1, 0, 1]`, a árvore realiza rotações para se reequilibrar.

**Características**

- Cada nó armazena um fator de balanceamento calculado como: `altura(esquerda) - altura(direita)`
- O fator de balanceamento deve ser sempre -1, 0 ou +1
- Rotações simples ou duplas são feitas automaticamente para manter o balanceamento
- É uma árvore binária de busca: nós à esquerda são menores, à direita são maiores

**Vantagens**

- Garante complexidade O(log n) para busca, inserção e remoção
- Estrutura sempre equilibrada, sem degradação de desempenho
- Ideal para aplicações com muitas leituras

**Desvantagens**

- Inserção e remoção são mais lentas que em uma BST simples por causa das rotações
- Implementação mais complexa
- Armazena informação extra (fator de balanceamento) em cada nó

**Exemplo ilustrado**

Inserindo os valores `10, 20, 30` em uma BST simples (sem balanceamento), a árvore ficaria degenerada:

```
10
  \
   20
     \
      30
```

A AVL percebe que o fator de balanceamento do nó 10 ficou -2 e realiza uma **rotação à esquerda**, resultando em:

```
    20
   /  \
  10   30
```

---

### Rubro-Negra

**Conceito**

A árvore Rubro-Negra é uma BST balanceada onde cada nó possui uma cor: **vermelho** ou **preto**. Essas cores seguem regras específicas que garantem que o caminho mais longo da raiz até uma folha seja no máximo o dobro do caminho mais curto, mantendo o balanceamento aproximado.

**Regras de coloração**

1. Todo nó é vermelho ou preto
2. A raiz é sempre **preta**
3. Toda folha (`NIL`) é considerada **preta**
4. Se um nó é **vermelho**, seus dois filhos devem ser **pretos** (não há dois nós vermelhos consecutivos)
5. Todo caminho de um nó até suas folhas descendentes tem o mesmo número de nós **pretos** (altura negra)

**Vantagens**

- Balanceamento eficiente com menos rotações que a AVL
- Bom desempenho tanto para leitura quanto para escrita
- Amplamente utilizada em implementações de bibliotecas padrão (como `std::map` em C++ e `TreeMap` em Java)

**Desvantagens**

- Implementação mais complexa que a AVL
- Não é tão rigidamente balanceada quanto a AVL (pode ter caminhos mais longos)
- Difícil de depurar

**Exemplo ilustrado**

Inserindo os valores `10, 20, 30`:

```
Inserção de 10:
[10 Preto]

Inserção de 20:
[10 Preto]
    \
   [20 Vermelho]

Inserção de 30: viola regra (dois vermelhos consecutivos) → rotação + recoloração

    [20 Preto]
   /          \
[10 Vermelho] [30 Vermelho]
```

---

### N-ária

**Conceito**

Uma árvore N-ária (ou árvore de múltiplos filhos) é uma estrutura onde cada nó pode ter **N filhos**, ou seja, o número de filhos não está limitado a dois como nas árvores binárias. O valor de N pode ser fixo (como nas árvores B, onde N varia entre `t` e `2t`) ou variável.

**Diferenças em relação às árvores binárias**

| Característica | Árvore Binária | Árvore N-ária |
|---|---|---|
| Filhos por nó | Máximo 2 | Máximo N (pode ser variável) |
| Altura | Maior para o mesmo número de elementos | Menor, pois cada nó armazena mais dados |
| Uso de memória por nó | Menor | Maior (mais ponteiros) |
| Ideal para | Dados em memória principal | Dados em disco (ex: banco de dados) |

**Vantagens**

- Altura menor que árvores binárias para o mesmo número de elementos
- Melhor desempenho em memórias com acesso por blocos (disco, SSD)
- Representa hierarquias naturais com múltiplos filhos (pastas, menus)

**Desvantagens**

- Implementação mais complexa
- Maior uso de memória por nó
- A lógica de busca e balanceamento é mais elaborada

**Exemplo ilustrado**

Uma árvore de diretórios (N-ária):

```
         [/]
       /  |  \
    [home] [etc] [usr]
    /   \         |
[user1] [user2] [local]
           |
        [docs]
```

**Aplicações práticas**

- **Sistema de arquivos**: pastas com múltiplos subdiretórios e arquivos
- **Banco de dados (Árvore B e B+)**: índices de tabelas para acesso rápido em disco
- **Menus de aplicativos**: hierarquia de opções com submenus
- **XML/HTML DOM**: estrutura de documentos com nós pai e múltiplos filhos
- **Taxonomias**: classificações biológicas, categorias de produtos em e-commerce

---

## Parte 2 – Operações em Árvores

### Rotação Simples à Direita

**Objetivo**

Rebalancear uma árvore quando ela está "pesada" para a esquerda, ou seja, quando o fator de balanceamento de um nó é +2 e seu filho esquerdo tem fator +1 ou 0.

**Situação em que é utilizada**

Ocorre quando há dois nós à esquerda consecutivos (inserção à esquerda-esquerda).

**Exemplo antes e depois da rotação**

Antes:
```
      C (FB = +2)
     /
    B (FB = +1)
   /
  A
```

Depois da rotação à direita em C:
```
    B
   / \
  A   C
```

O nó B sobe, C desce para a direita de B, e o filho direito de B (se existir) vai para a esquerda de C.

---

### Rotação Simples à Esquerda

**Objetivo**

Rebalancear uma árvore quando ela está "pesada" para a direita, ou seja, quando o fator de balanceamento é -2 e o filho direito tem fator -1 ou 0.

**Situação em que é utilizada**

Ocorre quando há dois nós à direita consecutivos (inserção à direita-direita).

**Exemplo antes e depois da rotação**

Antes:
```
  A (FB = -2)
   \
    B (FB = -1)
     \
      C
```

Depois da rotação à esquerda em A:
```
    B
   / \
  A   C
```

O nó B sobe, A desce para a esquerda de B, e o filho esquerdo de B (se existir) vai para a direita de A.

---

### Rotação Dupla

#### Esquerda-Direita (LR)

Utilizada quando o fator de balanceamento é +2 no nó atual, mas o filho esquerdo tem fator -1 (ou seja, o desequilíbrio está no lado direito do filho esquerdo).

**Passos:**
1. Rotação simples à esquerda no filho esquerdo
2. Rotação simples à direita no nó atual

**Exemplo:**

Antes:
```
      C (FB = +2)
     /
    A (FB = -1)
     \
      B
```

Após rotação à esquerda em A:
```
      C
     /
    B
   /
  A
```

Após rotação à direita em C:
```
    B
   / \
  A   C
```

---

#### Direita-Esquerda (RL)

Utilizada quando o fator de balanceamento é -2 no nó atual, mas o filho direito tem fator +1 (desequilíbrio no lado esquerdo do filho direito).

**Passos:**
1. Rotação simples à direita no filho direito
2. Rotação simples à esquerda no nó atual

**Exemplo:**

Antes:
```
  A (FB = -2)
   \
    C (FB = +1)
   /
  B
```

Após rotação à direita em C:
```
  A
   \
    B
     \
      C
```

Após rotação à esquerda em A:
```
    B
   / \
  A   C
```

---

### Inversão (Espelhamento)

**Conceito**

A inversão (ou espelhamento) de uma árvore binária é uma operação que troca recursivamente os filhos esquerdo e direito de todos os nós da árvore, gerando a imagem espelhada da estrutura original.

**Aplicação**

- Algoritmos de processamento de imagens
- Resolução de puzzles e problemas de simetria
- Verificação se duas árvores são espelhos uma da outra
- Problema clássico de entrevistas técnicas (ex: "Invert Binary Tree")

**Exemplo antes e depois**

Antes:
```
       4
      / \
     2   7
    / \ / \
   1  3 6  9
```

Depois da inversão:
```
       4
      / \
     7   2
    / \ / \
   9  6 3  1
```

---

## Parte 3 – Aplicação Prática

### Sistema de Banco de Dados – Árvore B+

**Estrutura escolhida:** Árvore B+ (N-ária balanceada)

**Justificativa:**

Em sistemas de banco de dados relacionais (como MySQL, PostgreSQL e Oracle), os índices de tabelas são implementados com **Árvore B+**, uma variação da árvore N-ária. Essa escolha se justifica por vários fatores:

**Desempenho:**
- A altura da árvore é muito menor que em estruturas binárias para grandes volumes de dados. Com milhões de registros, uma árvore B+ com fator de ramificação 100 tem apenas 3 a 4 níveis de altura, enquanto uma BST poderia ter 20 ou mais.
- Isso reduz o número de operações de leitura em disco (I/O), que é o principal gargalo em bancos de dados.

**Organização dos dados:**
- Na Árvore B+, todos os dados ficam nas **folhas**, que são conectadas em lista encadeada. Isso permite percorrer os registros em ordem sequencial de forma eficiente, ideal para consultas com `BETWEEN`, `ORDER BY` e `RANGE SCAN`.
- Os nós internos armazenam apenas chaves de índice, maximizando o aproveitamento de cada bloco de disco lido.

**Operações realizadas:**
- Busca por chave primária: O(log n) com poucos acessos a disco
- Inserção e remoção com rebalanceamento automático
- Varredura sequencial eficiente para consultas por intervalo

Uma BST simples seria inadequada por poder ficar desbalanceada. Uma AVL seria melhor que a BST, mas ainda binária — implicaria muitos mais acessos a disco. A Árvore B+ foi projetada especificamente para minimizar I/O em memória secundária, sendo a escolha natural para índices de banco de dados.

---

## Parte 4 – Comparação entre Estruturas

| Estrutura | Nº Máximo de Filhos | Balanceamento | Complexidade de Busca | Complexidade de Inserção | Vantagem Principal | Desvantagem Principal | Exemplo de Aplicação |

|---|---|---|---|---|---|---|---|

| BST | 2 | Não possui balanceamento automático. Pode ficar desbalanceada conforme a ordem de inserção. | O(log n) no melhor caso e O(n) no pior caso | O(log n) no melhor caso e O(n) no pior caso | Estrutura simples de entender e implementar | Pode perder desempenho se ficar desbalanceada | Árvores de busca básicas |

| AVL | 2 | Sim. Mantém o fator de balanceamento com rotações após cada inserção/remoção | O(log n) | O(log n) | Busca previsível e eficiente sempre garantida | Inserção e remoção são mais custosas por causa das rotações | Índices e estruturas que exigem leitura frequente |

| Rubro-Negra | 2 | Sim. Usa coloração e rotações para manter o balanceamento aproximado | O(log n) | O(log n) | Balanceamento eficiente com menos rotações que a AVL | Implementação mais complexa que AVL | Estruturas internas de linguagens (C++ `std::map`, Java `TreeMap`) |

| N-ária | N (vários filhos) | Pode ou não possuir mecanismos de balanceamento, dependendo da variação (ex: Árvore B é balanceada) | O(log n) em estruturas balanceadas, depende da aplicação | O(log n) ou proporcional à estrutura | Representa hierarquias com muitos filhos, ótima para memória em bloco | Implementação mais complexa, maior uso de memória por nó | Sistema de arquivos, índices de banco de dados (Árvore B+) |

**Explicação da tabela:**

A BST é a mais simples das estruturas, mas pode se tornar uma lista encadeada no pior caso (inserção em ordem crescente), degradando para O(n). A AVL resolve esse problema forçando o balanceamento rígido após cada operação, garantindo sempre O(log n), mas ao custo de mais rotações durante escrita. A Rubro-Negra oferece um equilíbrio entre a rigidez da AVL e a simplicidade da BST: faz menos rotações na inserção/remoção e ainda garante O(log n), por isso é preferida em bibliotecas padrão de linguagens de programação que precisam de bom desempenho tanto em leitura quanto em escrita. Já a árvore N-ária escapa do paradigma binário: ao permitir múltiplos filhos por nó, ela reduz drasticamente a altura da árvore, sendo essencial quando os dados estão em disco e cada acesso é caro, como em sistemas de arquivos e bancos de dados.
