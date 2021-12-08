Shell Sort - algoritimo de ordenação
======

Imagine que um hospital precise organizar as pessoas em seu sistema, de modo que elas fiquem em ordem de chegada, a fim de identificar os pacientes que necessitam de mais cuidados médicos (partindo do princípio que, quanto mais tempo no hospital, mais grave é a situação), ou que uma escola decidiu fazer uma premiação para os alunos com melhores rendimentos, e fazer um plano de ação para os alunos com piores rendimentos. Para esses, e outras infinitas possibilidaades, é necessário utilizar um algorítimo de ordenação. Esse Handout irá explicar todos os detalhes de um dos inúmeros algoritimo de ordenação existentes, o Shell Sort, uma melhoria do métodos que estudamos na aula 8. 


Shell Sort e Insertion Sort 
---------

O shell sort tem uma ideia muito similar ao insertion sort. Na verdade, podemos dizer que se trata de um refinamento do método de inserção. Se você não se lembra muito bem como funciona o método de inserção, vale a pena fazer uma pequena revisão:

::: Revisão: Insertion sort
A ideia do insertion sort é bem similar a como ordenamos cartas de baralho. Um índice percorre todo o vetor e aloca os elementos em dois grupos: dos ordenados e dos não-ordenados. 

Primeiramente, assumimos que o primeiro elemento já está ordenado, e todo os outros elementos fazem parte do grupo dos não ordenados. E então, para cada elemento do grupo dos não-ordenados iremos retirá-lo e compará-lo com cada elemento do grupo dos elementos ordenados, até que se encontre um elemento maior do que ele, podendo assim, inserí-lo no grupo dos elementos ordenados de modo a manter a ordem. 
Note que no insertion sort cada elemento é comparado com o elemento **consecutivo** a ele. 

![](insertion-sort1.png)
:::



??? Exercicio
Com o Insertion Sort fresco em mente, pense em algumas desvantagens em utilizar esse método de ordenação,  a partir do vetor abaixo.

![](vetor-desordenado.png)

::: Desvantagens
Imagine o custo de levar o número 9 (maior número do vetor) para a última posição, ou de levar o número 1 (segundo menor número de vetor) para a segunda posição, por meio do uso do Insertion Sort, movendo os elementos apenas uma posição à frente. Em caso como esses, é mais interessante, por exemplo, comparar o número 9 com um elemento bem mais a frente do que seu vizinho, e desta forma, ele estará cada vez mais próximo de ficar na sua posição final. 
:::
???

Foi tentando mitigar essa desvantagem no uso do Insertion Sort que o **Shell Sort** foi pensado. Sua ideia é primeiro ordenar os elementos mais distantes uns dos outros e ir diminuindo o intervalo até que ele seja 1, finalizando a ordenação. Em outras palavras, o que acontece é que a cada iteração uma versão do insertion sort é calcula sobre um subvetor. 

!!!
OBS: ressaltar que os intervalos são diminuídos a partir da sequência usada, que é definida pelo usuário com base em suas necessidades.
!!!

??? Exercicio
Tente analisar em que situação o shell sort será igual ao insertion sort.

***Dica:*** *está relacionado ao valor do intervalo.*
::: Gabarito
 A partir do momento em que o intervalo é igual a 1, iremos comparar e mover elementos vizinhos um do outro, como acontece no caso do insertion sort.
:::
???

??? Insertion Sort X Shell Sort
Com essas diferenças em mente, pense em um comparativo entre esses dois algoritimos de ordenação:

* Na ordenação pelo Insertion Sort, movemos os elementos apenas uma posição à frente. Esse fator em muitos momentos pode ser uma desvantagem, especialmente quando um elemento precisa ser movido muito à frente, e assim, muitos movimentos serão mais precisos. A ideia do Shell Sort é permitir a troca de itens distantes, possibilitando uma organização mais rápida. 

* O Shell Sort organiza os elementos analisando sublistas separadas por um intervalo que o usuário escolhe o padrão, até que ele seja 1 (quando o algoritimo passa a ser um Insertion Sort). Escolhendo bem esse padrão, fazendo um estudo e adequando para a sua respectiva necessidade, é possível fazer com que o algoritmo seja muito mais rápido e eficiente. 

???


Primeira ideia
---------

A base do desenvolvimento do Shell Sort se dá da seguinte maneira: em um vetor de n elementos determina-se um intervalo *h* que iremos adotar como n/2. Esse intervalo é utilizado para comparar dois valores do vetor separados por um intervalo de distância. 

!!! OBS
 Neste handout, por simplicidade, vamos implementar a sequência original do shell sort, a qual o intervalo é dividido por dois a cada iteração. Lembre-se que a escolha da sequência irá variar de acordo com as necessidades de aplicação desse algoritmo.

Abaixo tem uma lista das possíveis sequências utilizadas nesse algoritmo:

* Sequencia original do shell sort - N/2 ... N/4 ... N/2^x
* Hibbard’s - 0, 1, 3, 7, 15, 31 ... 2x-1
* Papernov & Stasevich’s - 1, 3, 5, 9, 17, 33 ... 2x+1
* Knuth’s - 1, 4, 13,... (3k – 1) / 2

O uso dessas diferentes intervalos infuenciará na complexidade final do algorítimo.
!!!

Essa ideia pode ser observada no exemplo abaixo, na qual o primeiro elemento é comparado com o 5 elemento, o segundo é comparado ao 6, e por assim em diante, até chegar ao final do vetor.


![](comparacoes2.png)


Nessa comparação, se o elemento de menor índice do vetor, for maior que o elemento de maior índice, **invertem-se as posições**. É desta forma que é possível ordenar os elementos distantes um do outro de forma mais rápida do que a do insertion sort.

Assim após percorrido todo o vetor, divide-se o *h* em 2, e assim, as comparações serão em intervalos cada vez menores, até que o *h* seja 1, e a comparação de cada elemento do vetor, seja em relação com o elemento seguinte. 

Em outras palavras, a ideia geral é a seguinte:

``` txt
intervalo = n/2
enquanto intervalo maior que 0:
    para cada intervalo em v, enquanto h>0:
        ordene os elementos separados pelo intervalo
    intervalo/=2
```

Como mencionado, a ideia é que o vetor seja percorrido a cada iteração, e a cada iteração o valor do intervalo h seja diminuído, de acordo com a sequência escolhida, neste caso, dividido por 2 e arredondado para cima no caso em que a divisão não é inteira.


![](shell-sort1.png)

Para percorrer o vetor a cada iteração, a ideia é que i comece em $n/2$ e j comece valendo 0. Desta maneira, se $v[j]$ for maior do que $v[i]$, inverte-se a posição desses elementos. Assim iremos incrementando o valor de i e j, comparando os elementos dessas posicoes até que se tenha chegado ao final do vetor e i seja igual a $n-1$.

``` txt
intervalo = n/2
enquanto intervalo maior que 0:
    i = intervalo
    j = i-intervalo
    para cada intervalo em v enquanto i<n:
        se v[j]>v[i]:
            troca v[i] e v[j]
        i+=1
        j=i-intervalo
    intervalo/=2
```

??? Exercício

Tente simular o algoritmo para o vetor abaixo. Como esta o vetor ao final de cada iteração do loop externo? E o tamanho do intervalo h?
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

Note que para realizar a troca, é necessário armazenar $v[i]$ em uma variável temporária, assim como fizemos para o insertion sort, e se essa condição for satisfeita, copiar $v[j]$ para $v[i]$ e colocar a variável temporária em $v[j]$ para que a troca seja feita.

``` txt
intervalo = n/2
enquanto intervalo maior que 0:
    i = intervalo
    j = i-intervalo
    para cada intervalo em v enquanto i<n:
        temp = v[i]
        se v[j]>v[i]:
            v[i] = v[j]
        v[j] = temp 
        i+=1
        j=i-intervalo
    intervalo/=2
```

Na verdade, está faltando algo - precisamos comparar TODOS os elementos distantes h um do outro a esquerda de i. Além disso observe que precisamos considerar apenas intervalos positivos, uma vez que utilizar índices negativos não faz sentido para essa implementação.

``` txt
intervalo = n/2
enquanto intervalo maior que 0:
    i = intervalo
    j = i
    para cada intervalo em v enquanto i<n:
        temp = v[i]
        enquanto v[j-h]>v[i] e j>=h:
            v[j] = v[j-h]
            j = j-h
        v[j] = temp 
        i+=1
        
    intervalo/=2
```

??? DESAFIO

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

Complexidade
---------

Usando a receita da [Aula 7](https://ensino.hashi.pro.br/desprog/aula/7/) para estimar a complexidade do shellsort para o pior caso.

Lembrando do código:

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

Para determinar a complexidade, vamos considerar que x é a quantidade de iterações do segundo loop y do loop mais interno em função de i.

??? Exercicio
Determine a quantidade de iterações do primeiro loop interno (x), e do segundo interno y. Em seguida, tente enumerar os valores de i para as iterações do primeiro loop interno.

::: Gabarito
Contador começa de h e aumenta em 1 enquanto for menor que n.

Depois de x-1 iterações, a condição ainda vale.
$$h + 1(x-1) < n$$

$$h + 1x - 1 < n$$

$$x < n - h + 1$$

Depois de x iterações, a condição não vale.
$$h + 1x >= n$$

$$x >= n - h$$

$$(n-h) <= x < (n-h+1)$$

No pior caso, o contador começa de i e diminui em h enquanto for maior igual que h.

Depois de y-1 iterações, a condição ainda vale.

$$i - h(y-1) >= h$$

$$i - hy + h >= h$$

$$hy >= -i$$

$$y <= i/h$$

Valores de i ao longo do primeiro loop interno:

$$i = h, h+1, h+2, ..., h + (x-1)$$
:::
???

??? Exercicio
Determine o limitante para as iterações de todas as execuções do segundo loop interno:

::: Gabarito
$$((h / h) + ((h + 1) / h) + ((h + 2) / h) + ... + (h + (x-1)) / h)$$

$$= 1 + (h + 1) / h + (h + 2)/ h + ... + (h + x + 1) / h$$

Isso é uma soma de PA com:
- primeiro elemento 1;
- último elemento $(h + x + 1) / h$;
- número de elementos x.

$$= ((1 + (h + x + 1) / h) * x) / 2$$

$$= (h + h + x + 1) * x / 2$$

$$= 2hx + x^2 + x/ 2$$
:::
???

??? Exercicio
A partir dessa soma, e dos limitantes, determine a complexidade final do pior caso. 

::: Gabarito
Como:
$$(n-h) <= x < (n-h+1)$$

Podemos concluir que a complexidade é menor que:

$$2h(n-h+1) + (n-h+1)^2 + (n-h+1)/ 2$$

$$= (-h^2 - h + n^2 + 3n + 2)/2$$

$$= O(n^2 - h^2)$$

Portanto é a complexidade é:
$$O(n^2)$$
:::
???

O melhor caso será quando o vetor já estiver quase ou completamente ordenado, uma vez que serão feitas menos comparações. Por isso, o if interno nunca vai ser verdadeiro, fazendo o loop interno ser constante.

Analogamente, o pior caso será aquele em que o vetor estiver quase ou completamente desordenado. Sendo i o valor do intervalo, agora o loop será percorrido $<= n/i$ vezes . No loop do meio, será percorrido $<= n^2/i$ vezes. Já o loop mais externo vai entrar um número equivalente à progressão aritmética de $n^2/2^x$ , com x sendo inicialmente 0 e aumentando de 1 em 1 vez. Com isso, resulta em uma complexidade de $O(n^2)$ .

Complexidade de Memória Adicional e Estabilidade
---------

??? Exercicio

Considerando o que vimos até agora sobre o shell sort, o que se pode afirmar sobre a instabilidade e memória adicional:

::: Gabarito
O shell Sort é considerado instável, dado que ele não mantém a ordem original dos elementos iguais. Vale ressaltar também quem ele não demanda memória adicional, dado que ele não demanda o uso de outro vetor para ordenar, e raliza todas as trocas utilizando o mesmo array.
:::
???

Resumo Shell Sort
---------

|            **recomendacao de tempo na pratica**         ||
|:---------------------:|:--------------------------------:|
| pior caso             |       $$O(n^2))$$                |
| melhor caso           |       $$O(n\log (n))$$           |
| caso médio            |       $$O(n\log (n))$$           |
|           **complexidade de memoria adicional**         ||
|                          $$O(1)$$                       ||
|                     **estabilidade**                    ||
|                          $$ O(1)$$                      ||

Algumas aplicações reais desse algoritmo são:
* **uClib** - mais utilizado em sistemas embarcados
* **compressor bzip2** - usado a fim de evitar problemas de exceder a profundidade de recursão
* **kernel do Linux** - uma vez que não utiliza chamada em pilha, que pode gerar sobrecarga

??? Exercício

Com base no que foi visto até agora, o que você consideraria vantagens desse algoritmo de ordenação? E desvantagens?

::: Gabarito
Vantagens:
* codigo simples
* Não utiliza memória extra
* Interessante ser usado quando os elementos a serem ordenados já estão parcialmente ordenados
* Pode ser otimizado alterando o incremento utilizado

Desvantagens:
* A complexidade, bem como o tempo de execução irão variar de acordo com o vetor inicial. O shellsort será mais eficiente naqueles casos em que o vetor ja esta pré ordenado 

:::

???

Abaixo é possível observar uma comparação entre outros tipos de ordenação.

![](comparacao.gif)


Referências  
--------- 
* https://www.programiz.com/dsa/shell-sort
* https://www.geeksforgeeks.org/shellsort/
* https://pt.wikipedia.org/wiki/Shell_sort
* https://dcm.ffclrp.usp.br/~augusto/teaching/icii/Ordenacao-em-Vetores-Metodos-Avancados-Apresentacao.pdf
* https://www.educative.io/edpresso/what-is-a-shell-sort
* https://www.codingeek.com/algorithms/shell-sort-algorithm-explanation-implementation-and-complexity/
* https://stackoverflow.com/questions/12767588/time-complexity-for-shell-sort