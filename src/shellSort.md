Shell Sort - algoritimo de ordenação
======

Primeira ideia
---------

O shell sort trata-se de um refinamento do método de inserção que vimos em aula. Sua ideia é primeiro ordenar os elementos mais distantes uns dos outros e ir diminuindo o intervalo até que todos os elementos estejam adequadamente ordenados. Os intervalos sao diminuidos a partir da sequencia usada, que ira influenciar diretamente na complexidade do algoritmo. 

!!! OBS
 Neste handout vamos implementar a sequencia original do shell sort, a qual o intervalo é dividido por dois a cada iteracao. Lembre-se que a escolha da sequencia irá variar de acordo com as necessidades de aplicacao desse algoritmo. 
!!!

Abaixo tem uma lista das possíveis sequencias utilizadas nesse algoritmo:

* Sequencia original do shell sort - N/2 ... N/4 ... N/2^x
* 1, 4, 13, …, (3k – 1) / 2
* 1, 8, 23, 77, 281, 1073, 4193, 16577...4j+1+ 3·2j+ 1
* 1, 3, 7, 15, 31, 63, 127, 255, 511…
* 1, 3, 5, 9, 17, 33, 65,...
* 1, 2, 3, 4, 6, 9, 8, 12, 18, 27, 16, 24, 36, 54, 81....

Em outras palavras, a ideia geral é a seguinte:

``` txt
intervalo = n/2
para cada intervalo em v:
    ordene os elementos separados pelo intervalo
```

Mas você nao falou que ele aplica o método de inserção? Cade o insertion sort nessa ideia? 
Calma, o insertion sort é aplicado a cada comparacao entre dois elementos separados por um intervalo. Assim, podemos dizer que:

``` txt
intervalo = n/2
para cada intervalo em v:
    aplique o insertion sort entre os elementos separados pelo intervalo
```
Se você não se lembra muito bem como funciona o método de insercao, vale a pena fazer uma pequena revisao.

::: Revisão: Insertion sort
O insertion sort é um método de ordenação que funciona de maneira similar a como ordenamos cartas de baralho. Um índice percorre toda a lista e aloca os elementos em dois grupos: dos ordenados e dos não-ordenados. 

Para selecionar quais elementos farão parte de cada uma das listas, comparamos o primeiro elemento do grupo dos não-ordenados com os elementos do grupo dos ordenados (que inicia-se com o primeiro elemento da lista). Caso o elemento sendo comparado seja menor do que o último elemento da lista dos ordenados, ele será comparado com o penúltimo elemento dos ordenados, e assim por diante. Até que ele é maior do que um deles ou a lista acaba e ele é encaixado ali, deslocando sempre o resto da lista para a direita conforme o elemento a ser alocado é deslocado para a esquerda.

Com isso, os elementos são ordenados e alocados automaticamente.


![](insertion-sort1.png)
:::

![](shell-sort1.png)

A ideia é que o vetor seja percorrido a cada iteracao, e que a cada iteracao o valor do intervalo seja diminuido de acordo com a sequencia escolhida até que seja menor ou igual a 1. Como precisamos comparar dois elementos distantes h um do outro, podemos considerar que o elemento v[i] e v[i-h], sendo i inicialmente igual a h e j igual a Assim iremos percorrer o vetor ate que i seja igual a n-1, ou seja, quando chegamos ao final dele.



``` txt
intervalo = n/2
para cada intervalo maior que 0:
    para cada intervalo em v:
        aplique o insertion sort entre os elementos separados pelo intervalo
        intervalo/=2
```






??? Exercício

Tente simular o algoritmo para o vetor abaixo. Como esta o vetor ao final de cada iteracao do loop externo? E o tamanho do intervalo h?
``` txt
    v = {3 5 8 1} 
    n = 4
```
::: Gabarito
```
fim da iteracao 1: v = {3 1 8 5}
                   h = 2

fim da iteracao 2: v = {1 3 5 8}
                   h = 1




``` 
:::

???



??? Exercício

Implemente o shell sort em C. Suponha que os elementos são inteiros.

::: Gabarito
``` c
void shellSort(int v[], int n) {
    for (int h = n / 2; h > 0; h /= 2) {
        for (int i = h; i < n; i += 1) {
            int temp = v[i];
            for (int j = i; j >= h && v[j - h] > temp; j -= h) {
                v[j] = v[j - h];
            }
            v[j] = temp;
        }
    }
}
```
:::

???

Estimando a complexidade do shell sort
--------- 

??? Exercício

Estime a complexidade do algoritmo.

!!! Aviso
Se precisar de ajuda, consulte o material da [aula 7](https://ensino.hashi.pro.br/desprog/aula/7/).
!!!

::: Gabarito

:::

???



Aplicações e recomendacoes do shell sort 
--------- 

??? Exercício

Com base no que foi visto até agora, o que você consideraria vantagens desse algoritmo de ordenacao? e desvantagens?

::: Gabarito
Vantagens:
* codigo simples

Desvantagens:
* A complexidade, bem como o tempo de execução irão variar de acordo com o vetor inicial. O shellsort sera mais eficiente naqueles casos em que o vetor ja esta pre oprdenado 

:::

???

O shell sort 



Referências  
--------- 
* https://www.programiz.com/dsa/shell-sort