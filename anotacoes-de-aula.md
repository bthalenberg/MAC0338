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
| 6 | $c_6$ | $\sum{j=2}^{n} (t_j - 1)$ |
| 7 | $c_7$ | $\sum{j=2}^{n} (t_j - 1)$ |
| 8 | $c_8$ | n - 1|

(* ) Note que no while, temos uma atribuição a mais do que o número de iterações, para que a condição deixe de ser satisfeita e não entre mais no laço.

O tempo de execução do algoritmo é a soma do número de execução para cada linha do código; uma linha que toma $c_i$ passos e é executada n vezes irá contribuir $c_in$ para o tempo de execução total. Denotamos o tempo de execução por T(n).

$T(n) = c_1n + c_2(n - 1) + c_4(n - 1) + c_5\sum{j=2}^{n} t_j + c_6\sum{j=2}^{n} (t_j - 1) + c_7\sum{j=2}^{n} (t_j - 1) + c_8(n-1)$.


### O grande

Sejam $T(n)$ e $f(n)$ funções dos inteiros nos reais. Dizemos que $T(n)$ é $O(f(n))$ se existem constantes positivas $c$ e $n_0$ tais que $T(n) \leq cf(n) \forall n \geq n_0$. Dizemos, então, que temos um **limitante superior assintótico**: trata-se do limitante no pior caso.

#### Exemplos

1. $10n^2$ é $O(n^3)$.
Provamos pela definição:
Seja $n \geq 0$. Então $0 \leq 10n^2 \leq 10n^3$.
Alternativamente, seja $n \geq 10$. Então $0 \leq 10n^2 \leq n \times n^2 = 1n^3$.

2. $20n^3 + 10n \log n + 5$ é $O(n^3)$
Seja $n \geq 1$. Então $20n^3 + 10n \log n + 5 \leq 20 n^3 + 10 n^3 + 5 n^3 = 35 n^3$.

#### Uso da notação

$O(f(n))$ é um conjunto! $O(f(n)) = \{T(n) : \exists c \land n_0$ t.q. $T(n) \leq cf(n) \forall n \geq n_0\}$.

Logo, "$T(n)$ é $O(f(n))$" deve ser entendido como $T(n)$ \in $O(f(n))$".

* $T(n) \leq O(f(n))$ é feio.
* $T(n) \geq O(f(n))$ não faz sentido.
* "$T(n)$ é $g(n) + O(f(n))$" significa que existem $c$ e $n_0$ positivas tais que $T(n)$ \leq $g(n) + c f(n)$ para todo $n \geq n_0$.


--------------------------------

## Aula 2

### Notação Omega

Dizemos que $T(n)$ é $\Omega(f(n))$ se existem constantes positivas $c$ e $n_0$ tais que $cf(n) \leq T(n) \forall n \geq n_0$.

Ou seja, temos um **limitante inferior assintótico**: trata-se do desempenho no melhor caso.

#### Exemplos

1. Se $T(n) \geq 0.001n^2 \forall n \geq 8$, então $T(n)$ é $\Omega(n^2)$.
Prova: Aplique a definição com $c = 0.001$ e $n_0 = 8$.

2. O consumo de tempo da ordenação por inserção é $\Omega(n)$.

| Linha | # mínimo de vezes |
| ---- | ------ |
| 1 | n |
| 2 | n - 1 |
| 3 | n - 1 |
| 4 | n - 1 |
| 5* | 0 |
| 6 | 0 |
| 7 | 0 |
| 8 | n - 1|

Total: $\geq 4n - 3 = \Omega(n)$.

### Notação Theta

Sejam $T(n)$ e $f(n)$ funções dos inteiros nos reais. Dizemos que $T(n)$ é $\Theta(f(n))$ se $T(n)$ é $O(f(n))$ e $T(n)$ é $\Omega(f(n)).$.

Alternativamente, dizemos que $T(n)$ é $\Theta(f(n))$ se existem constantes positivas $c_1$, $c_2$, e $n_0$ tais que $0 \leq c_1f(n) \leq T(n) \leq c_2 f(n)$. Neste caso, dizemos que f(n) é um **limite assintoticamente estreito** para T(n).

### Comparação assintótica

* Assintoticamente = no limite.

| comparação | comparação assintótica |
| ----- | ----- |
| $T(n) \leq f(n)$ | $T(n)$ é $O(f(n))$|
| $T(n) \geq g(n)$ | $T(n)$ é $\Omega(f(n))$|
| $T(n) = f(n)$ | $T(n)$ é $\Theta(f(n))$|

Do ponto de vista da análise de algoritmos, eficiente = polinomial.

## Análise do caso médio

Consideremos que a entrada do algoritmo é uma permutação de 1 a n escolhida com probabilidade uniforme. Qual o valor esperado do número de execuções de cada linha?

Seja $n\geq 1, j\in [n], t\in [j]$. Queremos provar que o número de permutações de $A[1...n]$ de $1$ a $n$ com essa propriedade é $n!$.
Hipótese: $A[i...j]$ tem $t-1$ elementos maiores que $A[j]$.

Para construir qualquer permutação desse tipo, podemos:

1. Escolher $S \subseteq [n]$ de tamnho $j$ para formar $A[1...j]$ "sem ordem." $\rightarrow {n \choose j}$.
2. Ordenar $S$ num vetor $B[1...j]$ e tome $A[j] := B[j - t + 1] = k$. $\rightarrow 1$.
3. Permutar $S\setminus{k}$ com $A[1...j-1]$. $\rightarrow (j-1)!$.
4. Permutar $[n]\setminus S$ em $A[j+1...n]$. $\rightarrow (n-j)!$.

-----------------------------

## Aula 3

### Divisão e conquista

1. **Divisão**: dividir a instância do problema em instâncias menores do problema;
2. **Conquista**: resolver o problema nas instâncias menores recursivamente;
3. **Combinação**: combinar as soluções das instâncias menores para gerar uma solução da instância original.

Exemplo: Mergesort

```
   mergesort(A, p, r):
1. se p < r
2.     então q <- piso((p+r)/2)
3.         mergesort(A, p, q)
4.         mergesort(A, q+1, r)
5.         intercala(A, p, q, r)

intercala(A, p, q, r)
0. #B[p...r] é um vetor auxiliar
1. para i <- p até q faça
2.     B[i] <- A[i]
3. para j <- q + 1 até r faça
4.     B[r + q + 1 - j] <- A[j]
5. i <- p
6. j <- r
7. para k <- p até r faça
8.     se i <= q e B[i] <= B[j]
9.         A[k] <- B[i];
10.        i <- i + 1;
11.    senão
12.        A[k] <- B[j]; j <- j - 1;
```

Note que a cópia da segunda metade invertida em intercala evita o uso de um sentinela.

#### Consumo de tempo

Quanto tempo consome *intercala* em função de $n := r - p + 1$?

| linha | consumo de todas as execuções |
| ----- | ----- |
| 1 | $O(n)$ |
| 2 | $O(n)$ |
| 3 | $O(n)$ |
| 4 | $O(n)$ |
| 5-6 | $\Theta(1)$ |
| 7 | $\Theta(n)$ |
| 8 | $\Theta(n)$ |
| 9-12 | $\Theta(n)$ |

Total: $\Theta(7n+1) = \Theta(n)$.

E *mergesort*?

| linha | consumo máximo na linha |
| ----- | ----- |
| 1 | $\Theta(1)$ |
| 2 | $\Theta(1)$ |
| 3 | $T(\lceil n/2\rceil)$ |
| 4 | $T(\lfloor n/2\rfloor)$ |
| 5 | $\Theta(n)$ |

$T(n) = T(\lceil n/2\rceil) + T(\lfloor n/2\rfloor) + \Theta(n)$.

### Resolvendo recorrências

#### Receita

1. Substitua a notação assintótica por função da classe. (Ex.: $\Theta(1)$ vira 1).
2. Restrinja-se a $n$ potência de 2 (para facilitar contas).
3. Estipule que na base o valor é 1.
4. Use expansão ou árvore de recorrência para determinar um chute de solução.
5. Confira se o chute está correto.

#### Exemplo

Para n potência de 2 e $k = \lg n$ temos que:

$T(n) = 2T(n/2) + n$

$= 2(2T(n/2^2) + n/2) + n = 2^2T(n/2^2) + 2n$

$= 2^2(2T(n/2^3) + n/2^2) + 2n = 2^3T(n/2^3) + 3n$

$\ldots = 2^kT(n/2^k) + kn$

$= n + n\lg n = \Theta(n\lg n)$.

#### Conferência

Para n potência de 2 e $k = \lg n$, $T(n) = 2T(n/2) + n$ (e, deixamos implícito, $T(1) = 1$).

**Afirmação**: $T(n) = n + n\lg n$.

Prova por indução em k, onde $k = \lg n$.

Afirmação reescrita em termos de $k$: $T(2^k) = 2^k + k2^k$.

Para $k = 0$, temos que T(2^0) = T(1) = 1 = 2^0 + 0.2^0$.

Para $k \geq 1$, suponha que $T(2^{k-1} = 2^{k-1} + (k-1)2^{k-1})$.

Então, $T(2^k) = 2T(2^{k-1}) + 2^k$ pela recorrência.

Logo, $T(2^k) = 2(2^{k-1} + (k-1)2^{k-1}) + 2^k = 2^k + (k-1)2^k + 2^k = 2^k + k2^k$.

#### Mais exemplos

**$T(n) = T(\lfloor n/2 \rfloor) + \Theta(1)$.**

Exemplo de código que tem esse consumo de tempo: busca binária.

Versão simplicada da recorrência acima:
$T(1) = 1$ e $T(n) = T(n/2) + 1$ para $n \geq 2$ potência de 2.

Por expansão:

$T(n) = T(n/2) + 1 = T(n/2^2 + 1) + 1 = T(n/2^2) + 2$

$= \ldots = T(n/2^k) + k$ para $k = \lg n$

$= T(1) + \lg n = 1 + \lg n = \Theta(\lg n)$.

------
**$T(n) = T(n-1) + \Theta(n)$**

Exemplo de código que tem esse consumo de tempo: ordenação por seleção.

Versão simplicada da recorrência acima:
$T(1) = 1$ e $T(n) = T(n-1) + n$ para $n \geq 2$.

Por expansão:

$T(n) = T(n-1) + n = T(n-2) + (n-1) + n$

$= \ldots = T(1) + 2 + 3 + \ldots + (n - 1) + n$

$= 1 + 2 + \ldots + (n-1) + n = \frac{(n+1)n}{2} = \Theta(n^2)$.

Note que não temos restrição no $n$ neste caso.

**Só faça a conta quando os termos que saem da recorrência são o mesmo (como o $n$ na recorrência do mergesort e o 1 na anterior).**
