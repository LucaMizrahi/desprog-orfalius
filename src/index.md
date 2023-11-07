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

Um exemplo de algoritmo de busca em texto é o [Naive String Search](https://www.geeksforgeeks.org/naive-algorithm-for-pattern-searching/). Esse algoritmo é conhecido por ser simples mas ineficiente. A base dele é de seguir de letra por letra no texto, por isso faz ele ser ineficiente e ter uma complexidade de $O(nm)$, onde o $n$ é *tamanho do texto* e $m$ é o *tamanho do padrão* (palavra desejada).

Por curiosidade, o código em C desse algoritmo é:

```c
void search(char* pat, char* txt)
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
 
        if (j
            == M) // if pat[0...M-1] = txt[i, i+1, ...i+M-1]
            printf("Pattern found at index %d \n", i);
    }
}
```

Você também pode criar

1. listas;

2. ordenadas,

3. GustavoEso;

assim como

* listas;

* não-ordenadas

e imagens. Lembre que todas as imagens devem estar em uma subpasta *img*.

![](logo.png)

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
