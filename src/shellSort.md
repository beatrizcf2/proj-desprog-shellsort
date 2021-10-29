Shell Sort - algoritimo de ordenação
======

Primeira ideia
---------

O shell sort trata-se de um refinamento do método de inserção que vimos em aula. Sua ideia é primeiro ordenar os elementos mais distantes uns dos outros e ir diminuindo o intervalo até que todos os elementos estejam adequadamente ordenados, que era uma das limitações do insertion sort. Os intervalos sao diminuidos a partir da sequencia usada, que ira influenciar diretamente na complexidade do algoritmo. 

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

A base do seu desenvolvimento se dá da seguinte maneira: em um vetor de n elementos determina-se um intervalo *h* que inicialmente começa em n/2. Esse intervalo é utilizado para comparar dois valores do vetor (ex: v[0] com v[h], v[1] com v[h+1] ... v[h-1] com v[h + n/2 -1]). Essa comparacao deve ser feita 
Nessa comparação, se o elemento de menor índice do vetor, for maior que o elemnto de maior índice, inverte-se as posições. 

Após percorrido todo o vetor, divide-se o *h* em 2, e assim, as comparações serão em intervalos cada vez menores, até que o *h* seja 1, e a comparação de cada elememento do vetor, seja em relação com o elemento seguinte. 


Em outras palavras, a ideia geral é a seguinte:

``` txt
intervalo = n/2
para cada intervalo maior que 0:
    para cada intervalo em v, enquanto h>0:
        ordene os elementos separados pelo intervalo
    intervalo/=2
```

Mas você não falou que ele ele é um refinamento do método de inserção? Cade o insertion sort nessa ideia? 

Se você não se lembra muito bem como funciona o método de insercao, vale a pena fazer uma pequena revisao.

::: Revisão: Insertion sort
A ideia do insertion sort é bem similar a como ordenamos cartas de baralho. Um índice percorre todo o vetor e aloca os elementos em dois grupos: dos ordenados e dos não-ordenados. 

Primeiramente, assumimos que o primeiro elemento ja esta ordenado, e todo os outros elementos faram parte do grupo dos nao ordenados. E entao,  para cada elemento do grupo dos nao-ordenados iremos retira-lo e  compara-lo com cada elemento do grupo dos elementos ordenados, ate que se encontre um elemento maior do que ele, podendo assim, inseri-lo no grupo dos elementos ordenados de modo a manter a ordem.

Note que no insertion sort cada elemento é comparado com o elemento consecutivo a ele.

![](insertion-sort1.png)
:::

??? Exercicio
Tente analisar em que situação o shell sort será igual ao insertion sort.

***Dica:*** *está relacionado ao valor do intervalo.*
::: Gabarito
 A partir do momento em que o intervalo é igual a 1, iremos comparar e mover elementos vizinhos um do outro, como acontece no caso do insertion sort.
:::
???

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

A ideia do insertion sort sera comparar os elementos v[j] e v[i], se v[j]>v[i] inverte-se a posicao desses elementos. Para isso, é necessério armazenar v[i] em uma variável temporária, e se v[j]>v[i], copiar v[i] para v[i - h] e colocar a variável temporária em v[i - h] para que a troca seja feita.

``` txt
intervalo = n/2
para cada intervalo maior que 0:
    i = intervalo
    j = i-h
    para cada intervalo em v enquanto i<n:
        temp = v[i]
        se v[j]>v[i]:
            v[i] = v[j]
        v[j] = temp 
        i+=1
        j=i-h
    intervalo/=2
```



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

;shell


!!! Dica
Se você quiser, tem um site bem legal que dá pra visualizar linha a linha a execução através desse [link](https://pythontutor.com/visualize.html#code=//%20Shell%20Sort%20in%20C%20programming%0A//%20fonte%3A%20https%3A//www.programiz.com/dsa/shell-sort%0A%0A%23include%20%3Cstdio.h%3E%0A%0A//%20Shell%20sort%0Avoid%20shellSort%28int%20array%5B%5D,%20int%20n%29%20%7B%0A%20%20//%20Rearrange%20elements%20at%20each%20n/2,%20n/4,%20n/8,%20...%20intervals%0A%20%20for%20%28int%20interval%20%3D%20n%20/%202%3B%20interval%20%3E%200%3B%20interval%20/%3D%202%29%20%7B%0A%20%20%20%20for%20%28int%20i%20%3D%20interval%3B%20i%20%3C%20n%3B%20i%20%2B%3D%201%29%20%7B%0A%20%20%20%20%20%20int%20temp%20%3D%20array%5Bi%5D%3B%0A%20%20%20%20%20%20int%20j%3B%0A%20%20%20%20%20%20for%20%28j%20%3D%20i%3B%20j%20%3E%3D%20interval%20%26%26%20array%5Bj%20-%20interval%5D%20%3E%20temp%3B%20j%20-%3D%20interval%29%20%7B%0A%20%20%20%20%20%20%20%20array%5Bj%5D%20%3D%20array%5Bj%20-%20interval%5D%3B%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20array%5Bj%5D%20%3D%20temp%3B%0A%20%20%20%20%7D%0A%20%20%20%20printf%28%22Final%20loop%20externo%20com%20h%3D%25d%20%3A%20%7B%22,interval%29%3B%0A%20%20%20%20printArray%28array,%20n%29%3B%0A%20%20%20%20printf%28%22%7D%5Cn%22%29%3B%0A%20%20%7D%0A%7D%0A%0A//%20Print%20an%20array%0Avoid%20printArray%28int%20array%5B%5D,%20int%20size%29%20%7B%0A%20%20for%20%28int%20i%20%3D%200%3B%20i%20%3C%20size%3B%20%2B%2Bi%29%20%7B%0A%20%20%20%20printf%28%22%25d%20%20%22,%20array%5Bi%5D%29%3B%0A%20%20%7D%0A%7D%0A%0A//%20Driver%20code%0Aint%20main%28%29%20%7B%0A%20%20int%20data%5B%5D%20%3D%20%7B9,%200,%207,%202,%205,%204,%206,%203,%208,%201%7D%3B%0A%20%20int%20size%20%3D%20sizeof%28data%29%20/%20sizeof%28data%5B0%5D%29%3B%0A%20%20shellSort%28data,%20size%29%3B%0A%20%20printf%28%22Sorted%20array%3A%20%22%29%3B%0A%20%20printArray%28data,%20size%29%3B%0A%7D&cumulative=false&curInstr=0&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=c_gcc9.3.0&rawInputLstJSON=%5B%5D&textReferences=false)
!!!

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



|            **recomendacao de tempo na pratica**         ||
|:---------------------:|:--------------------------------:|
| crescente ou quase    |                                  |
| sem ordem aparente    |                                  |
| decrescente ou quase  |                                  |
|           **complexidade de memoria adicional**         ||
|                           O(1)                          ||
|                     **estabilidade**                    ||
|                           O(1)                          ||

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