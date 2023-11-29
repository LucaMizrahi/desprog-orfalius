Algoritmo Boyer-Moore de Busca em Texto
======

Importância da Busca em Texto
---------

A busca em texto é um problema clássico da computação. A ideia é encontrar um padrão de caracteres em uma sequência de caracteres maior. Por exemplo, encontrar a palavra "algoritmo" em um texto. A busca em texto é um problema importante, pois é a base de muitas aplicações, como *editores de texto*, *mecanismos de busca* como o google, *editores de código*, *compiladores*, *o famoso control + f*, etc.

------------------------------------------------------------------------------

Tipo Classico de Busca em Texto
---------
Mas antes de se aprofundar no Boyer-Moore que é um algoritmo mais complexo, vamos antes entender um algoritmo mais simples de busca em texto. 

Um exemplo de algoritmo de busca em texto mais simples é o [Naive String Search](https://www.geeksforgeeks.org/naive-algorithm-for-pattern-searching/). Esse algoritmo é conhecido por ser {red}(simples mas ineficiente). A base dele é de seguir de letra por letra no texto, comparando o padrão com o texto, até que o padrão apareça no texto (caso apareça), e é justamente isso que o torna ineficiente (já que todos os caracteres são lidos, mesmo que grande parte deles não corresponda a palavra que se deseja encontrar).

Segue uma simulação do funcionamento do algoritmo **Naive String Search**, considerando um texto $T$ e um padrão $P$, que se deseja encontrar no texto:

:naive_matching_algorithm

??? Checkpoint
Considerando o a explicação do algoritmo ingênuo e o exemplo acima, qual seria a complexidade desse algoritmo (*No Pior Caso*)? 

(Lembre-se que a complexidade é dada em função de $n$ sendo o tamanho do texto e $m$ o tamanho do padrão)

::: Gabarito
De forma intuitiva, a complexidade do algoritmo é $O(nm)$, pois para cada caractere do texto, é necessário comparar com todos os caracteres do padrão. O que faz com que, no pior caso, todos os caracteres do texto sejam comparados com todos os caracteres do padrão.
???

Assim, podemos entender o motivo dele ser ineficiente, é preciso percorrer um caractere por vez para encontrar o que é desejado. Mas será que existe uma forma de pular caracteres que com certeza não faz parte do padrão?

Observe o exemplo a seguir: 

:exemplo_naive

Observando a sequência de passos acima é intuítivo perceber que é impossível que a palavra "word" esteja dentro da palavra "there", já que essa palavra nem sequer possui um caractere **w**. Fazendo com que a comparação entre todos os caracteres seja {red}(desnecessária).

E é justamente pensando em uma maneira de evitar essas comparações desnecessárias, durante a busca de palavras em um texto, que o algoritmo Boyer-Moore foi desenvolvido.

E então a questão que fica é:

*Como podemos **pular caracteres** que com certeza {red}(não) fazem parte da palavra que está sendo buscada?*

Algoritmo Boyer-Moore de Busca em Texto
---------

O algoritmo Boyer-Moore é um algoritmo eficiente de busca em texto e hoje é um dos algoritmos mais utilizados para esse problema. Ele foi desenvolvido por Robert S. Boyer e J Strother Moore em 1977.

A eficiência do algoritmo Boyer-Moore se destaca pelo uso de uma heurística para pular caracteres que com certeza não fazem parte da palavra que está sendo buscada (*'padrão'*). Essa heurística é baseada em duas regras:

1. *The bad-character rule*;

2. *The good-suffix rule*;

Primeiramente vamos entender como funciona a heurística do *bad-character rule*.

Observe o a simulação a seguir de uma busca em texto: 

:bad_character

Voltando a pergunta que foi feita anteriormente:

*Como podemos **pular caracteres** que com certeza {red}(não) fazem parte da palavra que está sendo buscada?*

??? Checkpoint
Considere a seguinte comparação:

$T$: "They"

$P$: "word" 

Considerando o que já foi falado em relação ao algoritmo ingênuo, faz sentido comparar o todos os caracteres de "word" com todos os caracteres de "they"?

::: Gabarito
**Não**, pois é impossível que a palavra "word" esteja dentro da palavra "they", já que essa palavra não possui caracteres em comum com a palavra "word".
???

??? Checkoint 
Considerando a mesma comparação anterior:

$T$: "They"

$P$: "word"

Agora, **com base na simulação da heurística do *bad-character rule***, seria uma maneira inteligente comparar o último caractere de "word" com o último caractere de "they" e caso eles não sejam iguais tentar encontrar esse primeiro caractere **y** de "they" dentro de "word"?

::: Gabarito
**Sim**, pois a única possibilidade de encontrar a palavra "word" dentro de they seria se houvesse um caractere *y* dentro de "word".
???

??? Checkpoint
Considerando a mesma comparação anterior:

$T$: "They"

$P$: "word"

Por fim, se o caractere *y* que foi comparado com o último caractere de "word", não existe dentro da palavra "word", seria necessário comparar os outros caracteres de "word" com o último caractere de "they"?

::: Gabarito
**Não**, pois se o último caractere de "they" não existe dentro de "word", é impossível que a palavra "word" esteja dentro de "they", logo não é necessário comparar os outros caracteres de "word" com o último caractere de "they".
???

Considerando essas três perguntas que foram feitas, o entendimento da heurística do *bad-character rule* fica mais claro.

Segue a definição formal dessa heurística:

Vemos na simulação que a questão que foi levantada de ser impossível que a palavra "word" esteja dentro da palavra "they", já que o *padrão* não possui um caractere "**y**". O que é justamente o princípio da heurística do *bad-character rule*. 

Sempre que houver um mismatch, o caractere de $T$ que diferiu do caractere de $P$ é comparado com os demais caracteres a esquerda, se houver algum igual significa que a palavra buscada ainda pode estar na região que foi comparada, caso contrário, o padrão é shiftado para depois do caractere que não corresponde a nenhum caractere do padrão, já que é impossível que a palavra buscada esteja na região que foi comparada. 

!!! Aviso
É muito importante enteder que tanto a regra do *bad-character*, quanto a do *good-sufix* só funcionam efetivamente, pois o padrão é comparado com o texto da {red}(direita para a esquerda) (**como indicado pela seta pontilhada na simulação**), diferentemente do algoritmo de busca em texto convencional apresentado no começo do handout.
!!!

??? Exercício
Seguindo uma versão mais curta do exemplo do tipo classico de busca em texto, descubra em quantos passos será completada a mesma busca, agora utilizando o *bad character rule*.

$T$: "There would word"

$P$: "word" 

::: Gabarito
```
iteração 1:
There would word
word

iteração 2:
There would word
 word

iteração 3:
There would word
     word

iteração 4:
There would word
         word

iteração 5:
There would word
            word
```
Serão realizadas **5** iterações para encontrar o padrão no texto.
???

Observe que, utilizando a heurística do *bad-character rule*, é possível pular diversos alinhamentos, facilitando encontrar o padrão no texto, de forma mais eficiente.

!!! Aviso
Por curiosidade, o código em Python/C da heurística do *bad-character rule* está no **final** do handout. 
!!!

Mas isso não é tudo, ainda existe uma outra heurística para evitar comparações desnecessárias e tornar o algoritmo ainda mais eficiente, a *good-suffix rule*.

Observe agora uma outra simulação de busca em texto:

:good_sufix

Na mesma lógica de entender como podemos pular caracteres, a heurística do good-sufix nada mais é do que novo jeito de pular comparações desnecessárias de caracteres.



A heurística do *good-sufix rule* consiste em comparar $P$ e $T$ {red}(da direita para esquerda), e verificar se existem caracteres em comum. Se houver, esse trecho similar é chamado de sufixo **t**. Caso haja uma reocorrência de **t** em $P$, $P$ é shiftado até essa recorrência do sufixo em $P$ dar "match" com o sufixo **t** que foi estabelecido. Existem alguns casos onde essa regra pode ficar um pouco mais confusa, como por exemplo, o caso onde é estabelecido o sufixo **t** em $T$, e apenas uma parte desse sufixo aparece em $P$. Nesse caso, $P$ será shiftado até que essa parte semelhante dê "match" em $P$ e $T$ (*mais claro no exemplo - passo 8*). 

Um último caso seria quando não existe nenhum sufixo de $P$ em $T$, nesse caso, similiar ao *bad-character rule*, toda a palavra $P$ será shiftada para depois de onde foi encontrado o sufixo **t** de $T$ ou do primeiro caractere comparado.

Caso não seja definido nenhum sufixo **t** em $T$, a heurística do *good-sufix rule* não é aplicada (pula apenas 1 caractere, similar ao algoritmo ingênuo de busca em texto).

*Um pouco mais complexo que a outra heurística né?* 

??? Checkpoint
Com base no seu entendimento dessa nova heurística, simule o funcionamento do *good-sufix rule* para a seguinte situação:

$T$: AACABABACBAAB

$P$: CBAAB

::: Gabarito
```
iteração 1:
AACABABACBAAB
CBAAB

iteração 2:
AACABABACBAAB
     CBAAB

iteração 3:
AACABABACBAAB
        CBAAB
```
???

Assim como o *bad-character rule*, o *good-sufix rule* também melhora muito a quantidade de iterações necessárias para encontrar o padrão no texto.

!!! Aviso
Por curiosidade, o código em Python da heurística do *good-sufix rule* está no **final** do handout
!!!

No entanto é importante perceber as diferenças entre as duas heurísticas, e para isso vamos avaliar algumas situações considerando as duas heurísticas estudadas anteriormente.

??? Exercício

Considerando que a maior eficiência está diretamente relacionada a *quantidade de comparações desnecessárias que são evitadas*, avalie a seguinte situação e determine qual das duas heurísticas é mais eficiente para a primeira comparação da busca em texto a seguir:

$T$: GTTATAGCTGATCGCGGCGTAGCGGCGAA

$P$: GTAGCGGCG

::: Gabarito
```
bad-character rule:
iteração 1:
GTTATAGCTGATCGCGGCGTAGCGGCGAA
GTAGCGGCG

iteração 2:
GTTATAGCTGATCGCGGCGTAGCGGCGAA  - 6 alinhamentos pulados (7 caracteres)
       GTAGCGGCG


good-sufix rule:
iteração 1:
GTTATAGCTGATCGCGGCGTAGCGGCGAA - 
GTAGCGGCG

iteração 2:
GTTATAGCTGATCGCGGCGTAGCGGCGAA - 0 alinhamentos pulados (1 caractere)
 GTAGCGGCG

```
Logo, para essa comparação a heurística do *bad-character rule* é mais eficiente.
:::
???

??? Exercício

Na mesma lógica do exercício anterior, avalie a seguinte situação e determine qual das duas heurísticas é mais eficiente para a primeira comparação da busca em texto a seguir:

$T$: CTGATCGCGGCGTAGCGGCGAA

$P$: GTAGCGGCG

::: Gabarito
```
bad-character rule:
iteração 1:
CTGATCGCGGCGTAGCGGCGAA
GTAGCGGCG

iteração 2:
CTGATCGCGGCGTAGCGGCGAA
 GTAGCGGCG - 0 alinhamentos pulados (1 caractere)


good-sufix rule:
iteração 1:
CTGATCGCGGCGTAGCGGCGAA
GTAGCGGCG

iteração 2:
CTGATCGCGGCGTAGCGGCGAA
   GTAGCGGCG - 2 alinhamentos pulados (3 caracteres)
```
Para essa nova comparação, o *good-sufix rule* é mais eficiente.
:::
???

Assim, agora que entedemos as duas heurísticas do algoritmo Boyer-Moore e percebemos que para diferentes comparações uma heurística pode ser mais eficiente que a outra, podemos realmente entender de forma intuitíva o funcionamento do algoritmo Boyer-Moore.

Esse funcionamento se resume basicamente em:

1. Comparar $P$ e $T$ {red}(da direita para esquerda);
2. Avaliar quantos alinhamentos podem ser pulados utilizando **cada uma das heurísticas**;
3. Shiftar $P$ para a direita de acordo com a heurística que for mais eficiente;
4. Repetir os passos 1, 2 e 3 para encontrar todas as ocorrências do padrão $P$ em $T$ (caso existam), até que acabe o texto. 

??? Exercício De Fixação

Agora que já entedemos o funcionamento do algoritmo Boyer-Moore, simule o funcionamento do algoritmo completo para a situação a seguir, explicitando qual heurística foi utilizada em cada comparação e quantos alinhamentos foram pulados em cada comparação.

$T$: JULIETTHOTELTANGOFOXTROT

$P$: FOXTROT

::: Gabarito
```
Comparação 1: (Ambas as heurísticas podem ser utilizadas)
JULIETTHOTELTANGOFOXTROT
FOXTROT

bad-character rule:
JULIETTHOTELTANGOFOXTROT
   FOXTROT - 2 alinhamentos pulados (3 caracteres)

good-sufix rule:
JULIETTHOTELTANGOFOXTROT
   FOXTROT - 2 alinhamentos pulados (3 caracteres)

----------------------------------------------------------------------------

Comparação 2: (A heurística do good-sufix rule deve ser utilizada)
JULIETTHOTELTANGOFOXTROT
   FOXTROT

bad-character rule:
JULIETTHOTELTANGOFOXTROT
        FOXTROT - 3 alinhamentos pulados (4 caracteres)

good-sufix rule:
JULIETTHOTELTANGOFOXTROT
          FOXTROT - 5 alinhamentos pulados (6 caractere)
        
----------------------------------------------------------------------------

Comparação 3: (Ambas as heurísticas podem ser utilizadas)
JULIETTHOTELTANGOFOXTROT
          FOXTROT

bad-character rule:
JULIETTHOTELTANGOFOXTROT
           FOXTROT - 0 alinhamentos pulados (1 caractere)

good-sufix rule:
JULIETTHOTELTANGOFOXTROT
           FOXTROT - 0 alinhamentos pulados (1 caractere)

----------------------------------------------------------------------------

Comparação 4: (A heurística do bad-character rule deve ser utilizada)
JULIETTHOTELTANGOFOXTROT
           FOXTROT

bad-character rule:
JULIETTHOTELTANGOFOXTROT
                 FOXTROT - 4 alinhamentos pulados (5 caracteres)

good-sufix rule:
JULIETTHOTELTANGOFOXTROT
            FOXTROT - 0 alinhamentos pulados (1 caractere)
```
Padrão encontrado no índice **17** do texto!
:::
???

!!! Aviso
Caso tenha curiosidade, o código completo do algoritmo Boyer-Moore em Python ou C está disponível nos links ao final do handout.
!!!

Por fim, o ultimo tópico que falta ser abordado é a **complexidade** do algoritmo Boyer-Moore.

Primeiramente, voltando as convenções utilizadas:

* O texto em que o padrão será buscado é representado por $T$;	
* O padrão que será buscado no texto é representado por $P$;
* O tamanho do texto é representado por $n$;
* O tamanho do padrão é representado por $m$;

Agora observe a seguinte simulação de busca em texto, utilizando o algoritmo Boyer-Moore:

:Simulacao_PiorCaso

??? Exercício
Com base na simulação anterior, determine a complexidade do algoritmo Boyer-Moore quando o padrão é encontrado no texto, porém o algoritmo não consegue otimizar a busca por meio de suas heurísticas.

**DICA** : Relembre o algoritmo ingênuo de busca em texto apresentado no começo do handout.
::: Gabarito
A complexidade do algoritmo Boyer-Moore quando o padrão é encontrado no texto, porém o algoritmo não consegue otimizar a busca por meio de suas heurísticas é $O(nm)$, já que de forma similar ao algoritmo ingênuo, todos os caracteres do texto são comparados com todos os caracteres do padrão.
:::
???

Para finalizar, observe essa última simulação de busca em texto, utilizando o algoritmo Boyer-Moore:

:Simulacao_MelhorCaso

??? Exercício
Com base na simulação anterior, determine a complexidade do algoritmo Boyer-Moore quando o padrão é encontrado no texto e o algoritmo consegue otimizar a busca por meio de suas heurísticas.

**DICA** : Relembre as iterações do algortimo Boyer-Moore por meio de suas heurísticas.
::: Gabarito
A complexidade do algoritmo Boyer-Moore quando o padrão é encontrado no texto e o algoritmo consegue otimizar a busca por meio de suas heurísticas é $O(n/m)$, já que o algoritmo consegue pular $m$ alinhamentos a cada iteração.
:::
???

Ao final desse handout fica claro que o algoritmo ingênuo de busca em texto é muito pouco eficiente, principalmente quando pensamos em situações de texto grandes e que possuem muitos caracteres a serem percorridos e comparados.

E é justamente por isso que algoritmos como o Boyer-Moore são tão importantes, pois eles conseguem otimizar a busca em texto de forma muito mais eficiente, tornando ferramentas como o "control + f" possíveis de serem implementadas de forma satisfatória.

------------------------------------------------------------------------------
Códigos
-------

Por curiosidade, segue o código em Python desse algoritmo básico de busca em texto:
!!! Aviso
( Optamos por colocar uma linguagem de alto nível para facilitar o entendimento, o código em C está disponível no link - [Naive String Search](https://www.geeksforgeeks.org/naive-algorithm-for-pattern-searching/) )
!!!

```python
def Naive_Search(pat, txt):
    M = len(pat)
    N = len(txt)
 
    # A loop to slide pat[] one by one */
    for i in range(N - M + 1):
        j = 0
 
        # For current index i, check
        # for pattern match */
        while(j < M):
            if (txt[i + j] != pat[j]):
                break
            j += 1
 
        if (j == M):
            print("Pattern found at index ", i)
```

Segue o código em Python da heurística do *bad-character rule*:

```python
# Python3 Program for Bad Character Heuristic
# of Boyer Moore String Matching Algorithm 
 
NO_OF_CHARS = 256

def badCharHeuristic(string, size):
    '''
    The preprocessing function for
    Boyer Moore's bad character heuristic
    '''
 
    # Initialize all occurrence as -1
    badChar = [-1]*NO_OF_CHARS
 
    # Fill the actual value of last occurrence
    for i in range(size):
        badChar[ord(string[i])] = i
 
    # return initialized list
    return badChar
```	

Segue o código em Python da heurística do *good-sufix rule*:

```python
# Python3 program for Boyer Moore Algorithm with 
# Good Suffix heuristic to find pattern in 
# given text string
 
# preprocessing for strong good suffix rule
def preprocess_strong_suffix(shift, bpos, pat, m):
 
    # m is the length of pattern
    i = m
    j = m + 1
    bpos[i] = j
 
    while i > 0:
         
        '''if character at position i-1 is 
        not equivalent to character at j-1, 
        then continue searching to right 
        of the pattern for border '''
        while j <= m and pat[i - 1] != pat[j - 1]:
             
            ''' the character preceding the occurrence 
            of t in pattern P is different than the 
            mismatching character in P, we stop skipping
            the occurrences and shift the pattern 
            from i to j '''
            if shift[j] == 0:
                shift[j] = j - i
 
            # Update the position of next border
            j = bpos[j]
             
        ''' p[i-1] matched with p[j-1], border is found. 
        store the beginning position of border '''
        i -= 1
        j -= 1
        bpos[i] = j
 
# Preprocessing for case 2
def preprocess_case2(shift, bpos, pat, m):
    j = bpos[0]
    for i in range(m + 1):
         
        ''' set the border position of the first character 
        of the pattern to all indices in array shift
        having shift[i] = 0 '''
        if shift[i] == 0:
            shift[i] = j
             
        ''' suffix becomes shorter than bpos[0], 
        use the position of next widest border
        as value of j '''
        if i == j:
            j = bpos[j]
```



-----------------------------------------------------
Links Relacionados a Este Algoritmo
---------

* [Wikipedia](https://en.wikipedia.org/wiki/Boyer%E2%80%93Moore_string-search_algorithm)

* [GeeksForGeeks](https://www.geeksforgeeks.org/boyer-moore-algorithm-for-pattern-searching/)

------------------------------------------------------------------------------
//FIM DO NOSSO HANDOUT
![](fim.png)