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
* Outras

Em outras palavras, a ideia geral é a seguinte:

``` txt
intervalo = n/2
para cada intervalo em v:
    ordene os elementos separados pelo intervalo
```

Mas você nao falou que ele aplica o método de inserção? Cade o insertion sort nessa ideia? 
Calma, o insertion sort é aplicado a cada comparacao entre dois elementos separados por um intervalo. Assim, podemos dizer:

``` txt
intervalo = n/2
para cada intervalo em v:
    aplique o insertion sort entre os elementos separados pelo intervalo
```
Se você não se lembra muito bem como funciona o método de insercao, vale a pena fazer uma pequena revisao.
::: Revisão: Insertion sort

:::



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

??? Exercício

Estime a complexidade do algoritmo.

!!! Aviso
Se precisar de ajuda, consulte o material da [aula 7](https://ensino.hashi.pro.br/desprog/aula/7/).
!!!

::: Gabarito

:::

???

??? Exercício

Com base no que foi visto até agora, o que você consideraria vantagens desse algoritmo de ordenacao? e desvantagens?

::: Gabarito
Vantagens:
* codigo simples

Desvantagens:
* A complexidade, bem como o tempo de execução irão variar de acordo com o vetor inicial. O shellsort sera mais eficiente naqueles casos em que o vetor ja esta pre oprdenado 

:::

???
