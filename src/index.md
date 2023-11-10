Algoritmo Boyer-Moore de Busca em Texto
======

Links Relacionados a Este Algoritmo
---------

* [Wikipedia](https://en.wikipedia.org/wiki/Boyer%E2%80%93Moore_string-search_algorithm)

* [GeeksForGeeks](https://www.geeksforgeeks.org/boyer-moore-algorithm-for-pattern-searching/)

------------------------------------------------------------------------------

Importância da Busca em Texto
---------

A busca em texto é um problema clássico da computação. A ideia é encontrar um padrão de caracteres em uma sequência de caracteres maior. Por exemplo, encontrar a palavra "algoritmo" em um texto. A busca em texto é um problema importante, pois é a base de muitas aplicações, como *editores de texto*, *mecanismos de busca* como o google, *editores de código*, *compiladores*, *o famoso control + f*, etc.

------------------------------------------------------------------------------

Tipo Classico de Busca em Texto
---------
Mas antes de se aprofundar no Boyer-Moore que é um algoritmo mais complexo, vamos antes entender um algoritmo mais simples de busca em texto. 

Um exemplo de algoritmo de busca em texto mais simples é o [Naive String Search](https://www.geeksforgeeks.org/naive-algorithm-for-pattern-searching/). Esse algoritmo é conhecido por ser {red}(simples mas ineficiente). A base dele é de seguir de letra por letra no texto, comparando o padrão com o texto, até que o padrão apareça no texto (caso apareça), e é justamente isso que o torna ineficiente (já que todos os caracteres são lidos, mesmo que grande parte deles não corresponda a palavra que se deseja encontrar). Dessa forma, de forma intuitiva, a complexidade do algoritmo é $O(nm)$, onde o $n$ é *tamanho do texto* e $m$ é o *tamanho do padrão* (palavra desejada).

Segue uma simulação do funcionamento do algoritmo **Naive String Search**, considerando um texto $T$ e um padrão $P$, que se deseja encontrar no texto:

:naive_matching_algorithm

Por curiosidade, segue o código em C desse algoritmo básico de busca em texto:

```c
void Naive_String_Search(char* pat, char* txt)
{
    int M = strlen(pat);
    int N = strlen(txt);
 
    /* A loop to slide pat[] one by one */
    for (int i = 0; i <= N - M; i++) {
        int j;
 
        /* For current index i, check for pattern match */
        for (j = 0; j < M; j++)
            if (txt[i + j] != pat[j])
                break;
 
        if (j == M) // if pat[0...M-1] = txt[i, i+1, ...i+M-1]
            printf("Pattern found at index %d \n", i);
    }
}
```

??? Checkpoint

Você consegue pensar em alguma forma de deixar a busca em texto mais rápida e eficiente?

::: Gabarito
Uma possível maneira de otimizar a busca seria, ao invés de passar de caractere por caractere do texto, encontrar uma maneira de **pular caracteres** que com certeza {red}(não) fazem parte da palavra que está sendo buscada.
:::
???

E então a questão que fica é:

*Como podemos pular caracteres que com certeza não fazem parte da palavra que está sendo buscada?*



Algoritmo Boyer-Moore de Busca em Texto
---------

O algoritmo Boyer-Moore é um algoritmo eficiente de busca em texto e hoje é um dos algoritmos mais utilizados para esse problema. Ele foi desenvolvido por Robert S. Boyer e J Strother Moore em 1977.

A eficiência do algoritmo Boyer-Moore se destaca pelo uso de uma heurística para pular caracteres que com certeza não fazem parte da palavra que está sendo buscada (*'padrão'*). Essa heurística é baseada em duas regras:

1. *The bad-character rule*;

2. *The good-suffix rule*;

Primeiramente vamos entender como funciona a heurística do *bad-character rule*. Considerando um texto $T$ e um padrão $P$, a heurística do *bad-character rule* consiste em, sempre que há um "mismatch" entre $P$ e $T$ (ou seja, quando um caractere do padrão não corresponde ao caractere do texto), o padrão é deslocado para a direita de forma que o caractere do texto que não corresponde ao caractere do padrão seja alinhado com a próxima ocorrência desse caractere no padrão. Caso não haja nenhuma ocorrência, todo o padrão $P$ é shiftado para depois do caractere do texto que não corresponde a nenhum caractere do padrão.

Segue uma curta simulação para ficar mais claro o funcionamento do **bad-character rule**:

!!! Aviso
É muito importante enteder que tanto a regra do *bad-character*, quanto a do *good-sufix* só funcionam efetivamente, pois o padrão é comparado com o texto da {red}(direita para a esquerda) (**como indicado pela seta pontilhada na simulação**), diferentemente do algoritmo de busca em texto convencional apresentado no começo do handout.
!!!


:bad_character

Por curiosidade, segue o código em C da heurística do *bad-character rule*:

```c
# define NO_OF_CHARS 256 

void badCharHeuristic( string str, int size, int badchar[NO_OF_CHARS])
{
	int i;
	// Initializing all occurrences as -1
	for (i = 0; i < NO_OF_CHARS; i++)
		badchar[i] = -1;

	// Fill the actual value of last occurrence
	// of a character
	for (i = 0; i < size; i++)
		badchar[(int) str[i]] = i;
}
```	

??? Checkpoint

Utilizando somente a heurística do *bad-character rule*, simule o funcionamento do algoritmo Boyer-Moore para encontrar a palavra $P$ no texto $T$:

$P$: "algoritmo" 

$T$ "Esse algoritmo parece interessante"

::: Gabarito
```
```
:::
???

Em relação a segunda heurística utilizada pelo algoritmo Boyer-Moore, a *good-suffix rule*, ela é utilizada para shiftar o padrão $P$ para a direita, caso haja um "mismatch" entre $P$ e $T$. Sendo assim, a heurística consiste em comparar $P$ e $T$ {red}(da direita para esquerda), e verificar se existem caracteres em comum. Se houver, esse trecho similar é chamado de sufixo **t** .

É então verificado se esse sufixo **t** aparece em $P$. Caso haja, $P$ é shiftado até essa recorrência do sufixo em $P$ dar "match" com o sufixo **t** %T% que foi estabelecido. Existem alguns casos onde essa regra pode ficar um pouco mais confusa, como por exemplo, o caso onde é estabelecido o sufixo **t** em $T$, e apenas uma parte desse sufixo aparece em $P$. Nesse caso, $P$ será shiftado até que essa parte semelhança 
dê "match" em $P$ e $T$ (*mais claro no exemplo*).

Um último caso seria quando não existe nenhum sufixo de $P$ em $T$, nesse caso, similiar ao *bad-character rule*, toda a palavra $P$ será shiftada para depois de onde foi encontrado o sufixo **t** de $T$.

*Bem mais complexo que a outra heurística né?* Isso pode estar um pouco confuso agora, mas analizando a simulação, ficará mais fácil de entender.

Segue uma curta simulação para ficar mais claro o funcionamento do **good-sufix rule**:

:good_sufix

Por curiosidade, segue o código em C da heurística do *good-sufix rule*:

```c
// preprocessing for strong good suffix rule
void preprocess_strong_suffix(int *shift, int *bpos,
                                char *pat, int m)
{
    // m is the length of pattern 
    int i=m, j=m+1;
    bpos[i]=j;
 
    while(i>0)
    {
        /*if character at position i-1 is not equivalent to
          character at j-1, then continue searching to right
          of the pattern for border */
        while(j<=m && pat[i-1] != pat[j-1])
        {
            /* the character preceding the occurrence of t in 
               pattern P is different than the mismatching character in P, 
               we stop skipping the occurrences and shift the pattern
               from i to j */
            if (shift[j]==0)
                shift[j] = j-i;
 
            //Update the position of next border 
            j = bpos[j];
        }
        /* p[i-1] matched with p[j-1], border is found.
           store the  beginning position of border */
        i--;j--;
        bpos[i] = j; 
    }
}
 
//Preprocessing for case 2
void preprocess_case2(int *shift, int *bpos,
                      char *pat, int m)
{
    int i, j;
    j = bpos[0];
    for(i=0; i<=m; i++)
    {
        /* set the border position of the first character of the pattern
           to all indices in array shift having shift[i] = 0 */
        if(shift[i]==0)
            shift[i] = j;
 
        /* suffix becomes shorter than bpos[0], use the position of 
           next widest border as value of j */
        if (i==j)
            j = bpos[j];
    }
}
 
/*Search for a pattern in given text using
  Boyer Moore algorithm with Good suffix rule */
void search(char *text, char *pat)
{
    // s is shift of the pattern with respect to text
    int s=0, j;
    int m = strlen(pat);
    int n = strlen(text);
 
    int bpos[m+1], shift[m+1];
 
    //initialize all occurrence of shift to 0
    for(int i=0;i<m+1;i++) shift[i]=0;
 
    //do preprocessing
    preprocess_strong_suffix(shift, bpos, pat, m);
    preprocess_case2(shift, bpos, pat, m);
 
    while(s <= n-m)
    {
 
        j = m-1;
 
        /* Keep reducing index j of pattern while characters of
             pattern and text are matching at this shift s*/
        while(j >= 0 && pat[j] == text[s+j])
            j--;
 
        /* If the pattern is present at the current shift, then index j
             will become -1 after the above loop */
        if (j<0)
        {
            printf("pattern occurs at shift = %d\n", s);
            s += shift[0];
        }
        else
            /*pat[i] != pat[s+j] so shift the pattern
              shift[j+1] times  */
            s += shift[j+1];
    }
 
}
```


-----------------------------------------------------------------------------
//FIM DO NOSSO HANDOUT
![](fim.png)


assim como

* listas;

* não-ordenadas

e imagens. Lembre que todas as imagens devem estar em uma subpasta *img*.



Para tabelas, usa-se a [notação do
MultiMarkdown](https://fletcher.github.io/MultiMarkdown-6/syntax/tables.html),
que é muito flexível. Vale a pena abrir esse link para saber todas as
possibilidades.

| coluna a | coluna b |
|----------|----------|
| 1        | 2        |

Ao longo de um texto, você pode usar *itálico*, **negrito**, {red}(vermelho) e
[[tecla]]. Também pode usar uma equação LaTeX: $f(n) \leq g(n)$. Se for muito
grande, você pode isolá-la em um parágrafo.

$$\lim_{n \rightarrow \infty} \frac{f(n)}{g(n)} \leq 1$$

Para inserir uma animação, use `md :` seguido do nome de uma pasta onde as
imagens estão. Essa pasta também deve estar em *img*.

:bubble

Você também pode inserir código, inclusive especificando a linguagem.

``` py
def f():
    print('hello world')
```

``` c
void f() {
    printf("hello world\n");
}
```

Se não especificar nenhuma, o código fica com colorização de terminal.

```
hello world
```


!!! Aviso
Este é um exemplo de aviso, entre `md !!!`.
!!!


??? Exercício

Este é um exemplo de exercício, entre `md ???`.

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???


??? Checkpoint

Este é um exemplo de Checkpoint, entre `md ???`.

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::
???