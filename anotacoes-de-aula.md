# Anotações - Análise de Algoritmos

## Aula 1 - Introdução

### Insertion Sort

```
1. for j <- 2 to length[A]
2.     do key <- A[j]
3.         # Insert A[j] into the sorted sequence A[1...j-1]
4.         i <- j - 1
5.         while i > 0 and A[i] > key
6.             do A[i+1] <- A[i]
7.                 i <- i - 1
8.         A[i+1] <- key
```

* Ordenação in-place
* Invariante de laço: no início de cada iteração do laço for, o subvetor A[1...j-1] consiste dos elemtnos originalmente em A[i...j-1] mas em sequência ordenada.

### Invariantes

* **Inicialização**: a invariante é verdadeira antes da primeira iteração do laço;
* **Manutenção**: se a invariante é verdadeira antes de uma iteração do laço, ela se mantém verdadeira antes da próxima iteração;
* **Término**: Quando o laço termina, a invariante nos oferece uma propriedade útil para mostrar que o algoritmo é correto.

### Análise

* **Tamanho da entrada**: O tamanho da entrada depende do problema a ser estudado. Em alguns problemas, pode ser o número de itens da entrada (e.g. a quantidade de números num vetor), mas eventualmente pode ser o número total de bits necessários para representar a entrada, ou então o número de vértices e de arestas em um grafo, etc.
* **Tempo de execução**: o tempo de execução de uma entrada particular é o número de operações primitivas (ou "passos") executadas. É conveniente definir a noção de passo de forma que seja independente da máquina: Uma quantidade constante de tempo é necessária para executar cada linha do pseudocódigo; uma linha pode tomar uma quantidade de tempo diferente do que outra, mas assumiremos que cada execução da linha i tomará $c_i$ tempo, onde $c_i$ é constante.
* **Ordem de crescimento**: Usar apenas o termo de maior ordem, já que, para uma entrada grande, os termos de ordem menor são relativamente significantes. Também ignoramos a constante que multiplica o termo de maior odem, já que fatores constantes são menos significantes do que a ordem de crescimento ao determinar a eficiência para entradas grandes. 

| Linha | Custo | # de vezes | 
| ---- | ----- | ------ |
| 1 | $c_1$ | n |
| 2 | $c_2$ | n - 1 |
| 3 | 0 | n - 1 |
| 4 | $c_4$ | n - 1 |
| 5* | $c_5$ | $\sum{j=2}^{n} t_j$ |
| 6 | $c_6$ | $\sum{j=2}}^{n} (t_j - 1)$ |
| 7 | $c_7$ | $\sum{j=2}^{n} (t_j - 1)$ |
| 8 | $c_8$ | n - 1|

(*) Note que no while, temos uma atribuição a mais do que o número de iterações, para que a condição deixe de ser satisfeita e não entre mais no laço.

O tempo de execução do algoritmo é a soma do número de execução para cada linha do código; uma linha que toma $c_i$ passos e é executada n vezes irá contribuir $c_in$ para o tempo de execução total. Denotamos o tempo de execução por T(n).

$T(n) = c_1n + c_2(n - 1) + c_4(n - 1) + c_5\sum{j=2}^{n} t_j + c_6\sum{j=2}^{n} (t_j - 1) + c_7\sum{j=2}^{n} (t_j - 1) + c_8(n-1)$.


### O grande

Sejam $T(n)$ e $f(n)$ funções dos inteiros nos reais. Dizemos que $T(n)$ é $O(f(n))$ se existem constantes positivas $c$ e $n_0$ tais que $T(n) \leq cf(n) \forall n \geq n_0$.

#### Exemplos

1. $10n^2$ é $O(n^3)$.
Provamos pela definição:
Seja $n \geq 0$. Então $0 \leq 10n^2 \leq 10n^3$.
Alternativamente, seja $n \geq 10$. Então $0 \leq 10n^2 \leq n \times n^2 = 1n^3$.

2. $20n^3 + 10n \log n + 5$ é $O(n^3)$
Seja $n \geq 1$. Então $20n^3 + 10n \log n + 5 \leq 20 n^3 + 10 n^3 + 5 n^3 = 35 n^3$.

#### Uso da notação

$O(f(n))$ é um conjunto! $O(f(n)) = {T(n) : \exists c \land n_0 $t.q.$ T(n) \leq cf(n) \forall n \geq n_0}$. 

Logo, "$T(n)$ é $O(f(n))$" deve ser entendido como $T(n)$ \in $O(f(n))$".

* $T(n) \leq O(f(n))$ é feio.
* $T(n) \geq O(f(n))$ não faz sentido.
* "$T(n)$ é $g(n) + O(f(n))$" significa que existem $c$ e $n_0$ positivas tais que $T(n)$ \leq $g(n) + c f(n)$ para todo $n \geq n_0$.