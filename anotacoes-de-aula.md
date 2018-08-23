# Anotações - Análise de Algoritmos

## Aula 1 - Introdução

**Cormen 1.1, 1.2, 2.1, 2.2**.

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

## Aula 2 -- Introdução parte 2

**Cormen 3.1 e 3.2**

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

## Aula 3 -- Mergesort e recorrências

**Cormen 2.3, 4.1, 4.2**

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

-----------------------------

## Aula 4 -- Quicksort

**Cormen 7.1 e 7.2**

O quicksort também aplica o paradigma da divisão e conquista.

1. **Divisão**: particiona o vetor A[p...r] entre dois subvetores (possivelmente vazios) A[p...q-1] e A[q+1...r] tais que todos os elementos em A[p...q-1] sejam menores ou iguais a A[q] e em A[q+1...r] sejam maiores ou iguais a A[q]. 
2. **Conquista**: ordena os dois subvetores por meio de chamadas recursivas.
3. **Combinação**: como os subvetores já estão ordenados, o vetor está ordenado.

```
   quicksort(A, p, r):
1. if p < r
2.     q = particione(A, p, r)
3.     quicksort(A, p, q-1)
4.     quicksort(A, q+1, r)
```

### Partição

O problema da partição consiste em rearranjar um dado vetor A[p...r] tal que $p \leq q \leq r$ e $A[p...q-1] \leq A[q] \leq A[q+1...r]$.


```
   particione(A, p, r):
1. x = A[r]
2. i = p - 1
3. for j = p to r-1:
4.     if A[k] <= x
5.         i++
6.         swap(A[i], A[j])
7. swap(A[i + 1], A[r])
8. return i + 1
```

#### Invariantes

No começo de cada iteração de 3-6, temos:
* $A[p..i] \leq x$
* $A[i+1... j-1] > x$
* $A[r] = x$

1. **Inicialização**: antes da primeira iteração, i = p - 1 e j = p. Como não há valores entre p e i e nem entre i + 1 e j - 1, as duas primeiras invariantes são trivialmente satisfeitas. A atribuição da linha 1 satisfaz a terceira condição.
2. **Manutenção**: Temos dois casos, a depender do teste da linha 4. Quando A[j] > x, incrementa-se j. A condição 2 passa a ser satisfeita para A[j-1], e as outras entradas permanecem inalteradas. Quando A[j] <= x, incrementa-se i, A[i] e A[j] são trocados e incrementa-se j. Por conta da troca, A[i] <= x e a condição 1 é satisfeita. De forma similar, A[j-1] > x já que o item que foi colocado em A[j-1] é, por conta da invariante, maior que x.
3. **Término**: Quando o programa termina, j = r. Assim, toda entrada no vetor está em um dos três conjuntos descritos pelas invariantes e a partição obteve sucesso.

#### Consumo de tempo do particione

Em função de $n := r - p + 1$.

O consumo de tempo do quicksort depende do balanceamento da partição, que por sua vez depende dos elementos usados para particionar. 

| linha | consumo de todas as execuções |
| ----- | ----- |
| 1-2 | $2\Theta(1)$ |
| 3 | $\Theta(n)$ |
| 4 | $\Theta(n)$ |
| 5-6 | $2O(n)$ |
| 7-8 | $2\Theta(1)$ |
| total | $\Theta(2n + 4) + O(2n)$ |

Conclusão: o algoritmo **particione** consome tempo $\Theta(n)$.

### Consumo de tempo do quicksort

$T(n) :=$ consumo de tempo no pior caso, sendo que $n := r - p + 1$.

| linha | consumo de todas as execuções |
| ----- | ----- |
| 1 | $\Theta(1)$ |
| 2 | $\Theta(n)$ |
| 3 | $T(k)$ |
| 4 | $T(n - k - 1)$ |

Sendo que $0 \leq k := q - p \leq n - 1$.

#### Pior caso

$T(n) :=$ consumo de tempo máximo quando $n = r - p + 1$.

O comportamento do quicksort no pior caso ocorre quando o particionamento produz um subproblema com n - 1 elementos e outro com 0 elementos. O particionamento custa $\Theta(n)$, e como a chamada recursiva em vetores de 0 elementos apenas retorna, T(0) = \Theta(1). Assim, a recorrência de tempo será 

\[
T(n) = T(n - 1) + T(0) + \Theta(n) = T(n-1) + \Theta(n)
\]

Intuitivamente, somando o custo em cada nível, teremos uma progressão aritmética que será avaliada em $\Theta(n^2)$.

* **Recorrência cuidadosa**: $T(n)  = max {T(k) + T(n-k-1)} + \Theta(n)$; $0 \leq k \leq n-1.
    - $T(0) = 1$
    - $T(1) = 1$
    - T(n) = max {T(k) + T(n-k-1)} + n$; $0 \leq k \leq n-1$ para $n = 2, 3, 4 \ldots$.

Vamos mostrar que $T(n) \leq n^2 + 1$ para $n \geq 0$.

Prova: Trivial para $n \leq 1$. Se $n \geq 2$, então:

\[
T(n) = max {T(k) + T(n-k-1)} + n 
(H.I.) \leq max {k^2 + 1 + (n - k - 1)^2 + 1} + n
= n^2 - n + 3 \leq n^2 + 1.
\]

Algumas conclusões:
* $T(n)$ é $\Theta(n^2)$
* O consumo de tempo do quicksort no pior caso é $O(n^2)$
* O consumo de tempo do quicksort é $O(n^2)$.
* O pior caso ocorre quando o vetor já está ordenado ou está em ordem reversa.

#### Melhor caso

Na melhor divisão possível, cada um dos subvetores tem tamanho $k \leq \frac{n}{2}$. Na verdade, um terá tamanho $\lfloor n/2 \rfloor$ e outro $\lceil n/2 \rceil - 1$.

Nesse caso, a recorrência será 

\[
T(n) = 2T(n/2) + \a(n) = \Theta(n\lg n),
\]

tolerando a remoção do piso, do teto e da subtração de 1. Alternativamente, tomamos $M(n) :=$ consumo de tempo mínimo quando $n = r - p + 1$.

\[
M(n) = min{M(k) + M(n - k - 1)} + \Theta(n); 0 \leq k \leq n-1
\]

A prova de que $M(n) \geq (n+1)\lg (n+1)$ é análoga à prova anterior. Isso implica que no melhor caso o quicksort é $\Omega(n\lg n)$


Algumas conclusões:
* $M(n)$ é $\Theta(n\lg n)$
* O consumo de tempo do quicksort no melhor caso é $\Omega(n \log n)$
* O consumo de tempo do quicksort no melhor caso é $\Theta(n \log n)$.

Note que, se o quicksort fizer uma boa partição a cada, digamos, 5 níveis da recursão, o efeito geral é o mesmo, assintoticamente, que ter feito uma boa partição em todos os níveis. Suponha, por exemplo, que tenhamos uma partição que divide numa razão de 9 para 1. Obteremos a recorrência $T(n) = T(9n/10) + T(n/10) + cn$. Note que cada nível terá custo $cn$ aé que a recursão atinge uma parada no nível de profundidade $\log{10}n = \Omega(\lg n)$, obtendo um custo total de $O(n \lg n)$.

-----------------

## Aula 5 - Análise probabilística

**Cormen 5.1, 5.2, C.1 a C.3**

Chamamos um algoritmo de **aleatorizado** se seu comportamento não é apenas determinado pela sua entrada mas também pelos valores produzidos por um gerador de números aleatórios. Distinguimos esses algoritmos daqueles cuja _entrada é aleatorizada_ referindo-nos ao seu tempo de execução como **tempo de execução esperado**. No geral, discutimos o caso médio do tempo de execução quando a distribuição de probabilidade refere-se à entrada do algortimo, e discutimos o tempo esperado de execução quando o próprio algoritmo faz escolhas aleatorizadas.

### Um pouco de probabilidade

* $(S, Pr)$ espaço de probabilidade.
* $S = $ conjunto finito (eventos elementares).
* $Pr\{\} =$ função distribuição de probabilidades, de $S$ em $[0, 1]$ tal que:
    - $Pr\{s\} \geq 0$
    - $Pr\{s\} = 1$, onde $Pr\{U\}$ é abreviação de \sum_{u\in U} Pr\{u\}$.
* Uma variável aleatória é uma função numérica definida sobre os eventos elementares.
    - $X = k$ é uma abreviação de $\{s\in S : X(s) = k\}$
* Esperança: $E[x]$ = \sum X(s) Pr\{s\}$.

### Problema do máximo

Encontrar o elemento máximo de um vetor A[1...n] de números inteiros positivos distintos.

```
   MAX(A, n):
1. max <- 0
2. for i <- 1 to n do:
3.     if A[i] > max:
4.         max <- A[i]
5. return max
```

Quantas vezes a linha 4 é executada?

Iremos supor que A[1...n] é uma permutação aleatória uniforme de 1, $\ldots$, n. Cada permutação tem probabilidade $\frac{1}{n!}$.

* $X =$ número total de execuções da linha 4 = \sum X_i$.
* $X_i = 1$ se a linha 4 é executada; $X_i$ é uma variável aleatória indicadora.
* E[X_i] =$ probabilidade de que A[i] seja máximo em A[i...i] $= 1/i$.

\[
E[x] = E[X_n + \ldots + X_n] = E[X_1] + \ldots E[X_n]
= 1/1 + 1/2 + \ldots + 1/n < 1 + \ln n
= \Theta(\lg n)
\]

### Análise de caso médio do Quicksort

Considere que A[1...n] é uma permutação escolhida uniformemente dentre todas as permutações de 1 a n. Seja X(A) o número de vezes que a linha 4 do *particione* é executada para uma chamada de quicksort. Observe que X é uma variável aleatória. Uma maneira de estimarmos o consumo de tempo médio do quicksort é calcularmos $E[X]$. Podemos escrever X como soma de variáveis aleatórias binárias, cuja esperança é mais fácil de calcular.

Para $a$ e $b$ em $\{1, \ldots, n\}$, seja $X_{ab}$ o número de comparações entre $a$ e $b$ na linha 4 de *particione*.

\[
X = \sum{a = 1}^{n-1}\sum{b = a+1}^{n} X_{ab}
\]

Supondo $a < b$, $X_{ab} = 1$ se o primeiro pivô em $\{a, \ldots, b\}$ é $a$ ou $b$. Qual a probabilidade de $X_{ab}$ valer 1?

\[
E[X_{ab}] = Pr\{X_{ab} = 1\} = \frac{1}{b - a + 1} + \frac{1}{b - a + 1}
\]

\[
E[X] = \sum{a = 1}^{n-1}\sum{b = a+1}^{n} E[X_{ab}]
= \sum{a = 1}^{n-1}\sum{b = a+1}^{n} Pr{X_{ab} = 1}
= \sum{a = 1}^{n-1}\sum{b = a+1}^{n} \frac{2}{b - a + 1}
= \sum{a = 1}^{n-1}\sum{k = 1}^{n-a} \frac{2}{k+1}
< \sum{a = 1}^{n-1} 2\lparen \frac{1}{1} + \frac{1}{2} + \ldots + \frac{1}{n}\rparen
< 2n\lparen 1(\ln n\rparen
\]

Logo, o consumo de tempo esperado do quicksort, quando sua entrada é uma permutação de $1, \ldots, n$ escolhida uniformemente é $O(n \log n)$.

----------------

## Aula 6 - Algoritmos aleatorizados

**Cormen 7.3, 7.4, 9.2**

```
   randomParticione(A, p, r):
1. i <- random(p, r)
2. swap(A[i], A[r])
3. return particione(A, p, r)

   randomQuicksort(A, p, r):
1. if p < r:
2.     q <- randomParticione(A, p, r)
3.     randomQuicksort(A, p, q-1)
4.     randomQuicksort(a, q+1, r)
```

Para obter o consumo de tempo esperado, devemos contar o número esperado de comparações na linha 4 do particione. Ver aula anterior. O consumo de tempo esperado é $\Theta(n \log n)$

### Problema do k-ésimo menor elemento

Encontrar o k-ésimo menor elemento de A[1...n], supondo que o vetor não tem elementos repetidos.

A mediana é o \lfloor \frac{n+1}{2}\rfloor ou o \lceil\frac{n+1}{2}\rceil-ésimo menor elemento.

```
   select-ord(A, n, k):
1. ordene(A, n)
2. return A[k]
```

O consumo de tempo do _select-ord_ é $\Theta(n \lg n)$. Conseguimos fazer melhor para o menor elemento (percorrendo o vetor e guardando o valor do menor, analogamente ao algoritmo _MAX_). Para o segundo, fazemos um algoritmo semelhante, guardando o valor dos dois menores, e assim por diante. 

Podemos usar o _particione_ do quicksort para selecionar a mediana ou o k-ésimo menor elemento em tempo linear.

```
   randomSelect(A, p, r, k):
1. if (p = r) return A[p]
2. q <= randomParticione(A, p, r)
3. if k = q - p + 1:
4.     return A[q]
5. if k < q - p + 1:
6.     return randomSelect(A, p, q-1, k)
7. else return randomSelect(A, q+1, r, k-(q - p + 1))
```

#### Consumo de tempo esperado

* $X_{ab} = 1$ se o primeiro pivô em ${a, \ldots, n}$ é $a$ ou $b$.

\[
Pr\{X_{ab} = 1 =\} = \frac{1}{n - a + 1} + \frac{1}{n - a + 1} = E[X_{ab}]$.
\]

\[
E[X] = \sum{a = 1}^{n-1}\sum{b = a+1}^{n} E[X_{ab}] = \sum{a = 1}^{n-1}\sum{b = a+1}^{n} \frac{2}{n - a + 1}
= \sum{a = 1}^{n-1} \frac{2(n - a)}{n - a + 1} < \sum{a = 1}^{n-1} 2 < 2n.
\]

Logo, o consumo de tempo esperado do algoritmo _randomSelect_ é $O(n)$.

--------------

## Aula 7 - Ordenação em tempo linear

**Cormen 8**

Não é possível ter consumo de tempo menor do que $O(n \lg n)$ com um algoritmo de ordenação baseado em comparações. Isso porque eles são baseados em árvores de decisão. 

No pior caso, o número de comparações feito será igual à altura h da árvore. Todas as $n!$ permutações de $1, \ldots, n$ devem ser folhas, e toda árvore binária de altura h tem no máximo $2^h$ folhas (provado por indução em h). Assim, devemos ter $2^h \geq n! \rarrow h \geq \lg(n!)$.

\[
(n!)^2 = \prod{i=0}^{n-1} (n-i)(i+1) \geq \prod{i=1}{n} n = n^n
\]

Portanto, $h \geq \lg(n!) \geq \frac{1}{2} n\lg n. Assim concluímos que todo algoritmo de ordenação baseado em comparações faz $\Omega(n\lg n)$ comparações no pior caso.

### Counting sort

Recebe inteiros n e k e um vetor A[1...n] onde cada elemento é um inteiro entre 1 e k. Devolve um vetor B[1...n] com os elementos de A[1...n] em ordem crescente.

```
   countingSort(A, n):
1. for i <- to k do:
2.     C[i] <- 0
3. for j <- 1 to n do:
4.     C[A[j]]++
5. for i <- 2 to k do:
6.     C[i] += C[i-1]
7. for j <- to n decreasing 1 do:
8.     B[C[A[j]]] <- A[j]
9.     C[A[j]]--
10. return B
```

#### Consumo de tempo

| linha | consumo na linha |
| ----  | ----             |
| 1 | $\Theta(k)$ |
| 2 | $O(k)$ |
| 3 | $\Theta(n)$ |
| 4 | $O(n)$ |
| 5 | $\Theta(k)$ |
| 6 | $O(k)$ |
| 7 | $\Theta(n)$ |
| 8 | $O(n)$ |
| 9 | $O(n)$ |
| 10 | $\Theta(1) |
| total | $\Theta(k + n)$ |

Logo, se $k = O(n)$, o consumo de tempo é $\Theta(n)$.

### Radix sort

Algoritmo usado para ordenar, por exemplo, inteiros não-negativos com d dígitos, ou strings com d caracteres.

```
   radixSort(A, n, d):
1. for i <- 1 to d do:
2.     ordene(A, n, i)
```

_ordene(A, n, i)_ ordena A[1...n] pelo i-ésimo dígito dos números em A por meio de um algoritmo estável. A ordenação estável só é relevante quando temos informação satélite.

O consumo de tempo do Radixsort depende do algoritmo _ordene_. Se cada dígito é um inteiro de 1 a k (ou um caractere), podemos usar o _countingSort_. Neste caso, o consumo de tempo é $\Theta(d(k+n))$. Se d é limitado por uma constante e $k = O(n)$, então o consumo de tempo é $\Theta(n)$.

### Bucket sort

Recebe um inteiro n e um vetor A[1...n] onde cada elemento é um número no intervalo $[0, 1)$. Devolve um vetor $C[1...n]$ com os elementos de $A[1...n]$ em ordem crescente.

```
   bucketSort(A, n):
1. for i <- 0 to n - 1 do:
2.     B[i] <- NULL
3. for i <- 1 to n do:
4.     insert(B[ceiling(floor(nA[i]))], A[i])
5. for i <- 0 to n - 1 do:
6.     orderList(B[i])
7. C <- concatenate(B, n)
8. return C
```

* _insert_(p, x): insere x na lista apontada por p
* _orderList_(p): ordena a lista apontada por p
* _concatenate_(B, n): devolve a lista obtida da concatenação das listas apontadas por $B[0], \ldots, B[n-1]$.

#### Consumo de tempo

Suponha que os números em A[1...n] são uniformemente distribuídos no intervalo $[0, 1)$. Suponha que o _orderList_ seja o _insertionSort_. 

Seja $X_i$ o número de elementos na lista B[i].

Seja $X_{ij} = 1$ se o j-ésimo elemento foi para a lista B[i]. Observe que $X_i = \sum{j} X_{ij}.

Definimos $Y_i$ como o número de comparações para ordenar a lista B[i].

Observe que $Y_i \leq X_i^2$; logo, $E[Y_i] \leq E[X_i^2]$ = E[(\sum{j} X_{ij})^2]. Além disso, $E[Y_i] \leq \sum{j} E[X_{ij}^2] + \sum{j}\sum{k\neq j} E[X_{ij}X_{ik}]$.

Note que $X_{ij}^2$ é uma variável aleatória binária, e $E[X_{ij}^2] = Pr[X_{ij}^2 = 1] = \frac{1}{n}.

Para calcular $E[X_{ij}X_{ik}], j \neq k$, primeiro note que se tratam de variáveis aleatórias independentes. Portanto, $E[X_{ij}X_{ik}] = $E[X_{ij}] E[X_{ik}]$. Ademais, $E[X_{ij}] = \frac{1}{n}. Logo,

\[
E[Y_i] \leq \sum{j}\frac{1}{n} + \sum{j}\sum{k \neq j}\frac{1}{n^2} = \frac{n}{n} + n(n-1)\frac{1}{n^2}
= 1 + (n-1)\frac{1}{n} = 2 - \frac{1}{n}
\]

Agora, seja $Y = \sum{i} Y_i$. Notque que $Y$ é o número de comparações realizadas pelo _bucketSort_ no total. Assim, $E[Y]$ é o número esperado de comparações realizadas pelo algoritmo, e tal numero determina o consumo assintótico de tempo do _bucketSort_. 

\[
E[Y] = \sum{i} E[Y_i] \leq 2n - 1 = O(n)
\]