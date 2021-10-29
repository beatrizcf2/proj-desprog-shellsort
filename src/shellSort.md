Shell Sort - algoritimo de ordenação
======

Primeira ideia
---------

O shell sort trata-se de um refinamento do método de inserção que vimos em aula. Sua ideia é primeiro ordenar os elementos mais distantes uns dos outros e ir diminuindo o intervalo até que todos os elementos estejam adequadamente ordenados, que era uma das limitações do insrtion sort. Os intervalos sao diminuidos a partir da sequencia usada, que ira influenciar diretamente na complexidade do algoritmo. 

!!! OBS
 Neste handout vamos implementar a sequencia original do shell sort, a qual o intervalo é dividido por dois a cada iteracao. Lembre-se que a escolha da sequência irá variar de acordo com as necessidades de aplicacao desse algoritmo. 
!!!

Abaixo tem uma lista das possíveis sequencias utilizadas nesse algoritmo:

* Sequencia original do shell sort - N/2 ... N/4 ... N/2^x
* 1, 4, 13, …, (3k – 1) / 2
* 1, 8, 23, 77, 281, 1073, 4193, 16577...4j+1+ 3·2j+ 1
* 1, 3, 7, 15, 31, 63, 127, 255, 511…
* 1, 3, 5, 9, 17, 33, 65,...
* 1, 2, 3, 4, 6, 9, 8, 12, 18, 27, 16, 24, 36, 54, 81....

A base do seu desenvolvimento se dá da seguinte maneira: em um vetor de n elementos determina-se um intervalo *h* que inicialmente começa em n/2. Esse intervalo é utilizado para comparar dois valores do vetor (ex: v[0] com v[h], v[1] com v[h+1] ... v[h-1] com v[h + n/2 -1]). Nessa comparação, se o elemento de menor índice do vetor, for maior que o elemnto de maior índice, inverte-se as posições.

Após percorrido todo o vetor, divide-se o *h* em 2, e assim, as cxomparações serão em intervalos cada vez menores, até que o *h* seja 1, e a comparação de cada elememento do vetor, seja em relação com o elemento seguinte. 


Em outras palavras, a ideia geral é a seguinte:

``` txt
intervalo = n/2
para cada intervalo em v, enquanto h>0:
    ordene os elementos separados pelo intervalo
```

Mas você não falou que ele aplica o método de inserção? Cade o insertion sort nessa ideia? 
Calma, o insertion sort é aplicado a cada comparação entre dois elementos separados por um intervalo. Assim, podemos dizer que:

``` txt
intervalo = n/2
para cada intervalo em v, enquanto h>0:
    aplique o insertion sort entre os elementos separados pelo intervalo
```
Se você não se lembra muito bem como funciona o método de insercao, vale a pena fazer uma pequena revisao.

::: Revisão: Insertion sort
O insertion sort é um método de ordenação que funciona de maneira similar a como ordenamos cartas de baralho. Um índice percorre toda a lista e aloca os elementos em dois grupos: dos ordenados e dos não-ordenados. 

Para selecionar quais elementos farão parte de cada uma das listas, comparamos o primeiro elemento do grupo dos não-ordenados com os elementos do grupo dos ordenados (que inicia-se com o primeiro elemento da lista). Caso o elemento sendo comparado seja menor do que o último elemento da lista dos ordenados, ele será comparado com o penúltimo elemento dos ordenados, e assim por diante. Até que ele é maior do que um deles ou a lista acaba e ele é encaixado ali, deslocando sempre o resto da lista para a direita conforme o elemento a ser alocado é deslocado para a esquerda.

Com isso, os elementos são ordenados e alocados automaticamente.


![](insertion-sort1.png)
:::

Como mencionado, a ideia é que o vetor seja percorrido a cada iteração, e a cada iteração o valor do intervalo h seja diminuído, de acordo com a sequencia escolhida, neste caso, dividido por 2 e arredondado para cima no caso em que a divisão não é inteira.

![](shell-sort1.png)


Para percorrer o vetor a cada iteração, iremos inicialmente comparar os elementos v[j] e v[i], sendo i=h e j=i-h, sendo eles distantes h um do outro. Assim iremos percorrer o vetor ate que i seja igual a n-1.

``` txt
intervalo = n/2
para cada intervalo maior que 0:
    para cada intervalo em v enquanto i<n:
        aplique o insertion sort entre os elementos v[j] e v[i] 
        i+=1
        j=i-h
        intervalo/=2
```

A ideia do insertion sort sera comparar os elementos v[j] e v[i], se v[j]>v[i] inverte-se a posicao desses elementos. Para isso, é necessério armazenar v[i] em uma variável temporária, copiar v[i] para v[i - h] e colocar a variável temporária em v[i - h] para que a troca seja feita.


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