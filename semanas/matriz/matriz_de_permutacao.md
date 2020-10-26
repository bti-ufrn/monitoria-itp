## Questão
Prova matrizes, Matriz de permutação (0/1)

Monitor: João Marcos Bezerra

Nessa solução eu assumi que o leitor tem conhecimento sobre macros em C, aqui vai um resumo de macros antes da solução:

## Macros

```c
#define imprimir printf
#define inteiro int
#define principal main
```

Em momento de compilação, um macro substitui todo o conteúdo pelo texto que está na direita, então isso daqui é um "Olá mundo" todo em português:

```c
inteiro principal() {
    imprimir("Olá mundo\n");
}
```

O código acima é o mesmo do que:

```c
int main() {
    printf("Olá mundo\n");
}
```

Agora, voltando para a questão...

## Enunciado
​Uma matriz quadrada é chamada matriz de permutação se seus elementos são apenas 0’s e 1’s e se em cada linha e coluna da matriz existe um único valor 1.  Escreva um programa que verifica se uma dada matriz quadrada é de permutação ou não.

A primeira linha da entrada deve ser um único valor inteiro N com o tamanho da matriz quadrada. Em seguida, as N linhas seguintes conterão N valores inteiros, correspondentes aos valores da matriz. Seu programa deve enviar para a saída padrão 0, se a matriz não for matriz de permutação, ou 1, se for.

Obs: N será sempre menor ou igual a 20.

## Entrada e saída
Entrada | Saída
--- | ---
4       | 1
1 0 0 1 |
0 0 1 0 |
0 1 0 0 |
1 0 0 0 |
_ | _
4       | 0
1 0 0 0 |
0 0 1 0 |
0 1 0 0 |
1 0 0 0 |

# Solução
Precisamos ler a matriz, e depois verificar se todas as linhas possuem somente um único `1`, ou se alguma linha tem múltiplos `1`.

Primeiro vamos pegar a entrada, isto é, `matriz` e `tamanho`.
```c
#include <stdio.h>

int main()
{
    int tamanho;
    scanf("%d", &tamanho);

    int matriz[tamanho][tamanho];
    for (int i = 0; i < tamanho; ++i) {
        for (int j = 0; j < tamanho; ++j) {
            scanf("%d", &matriz[i][j]);
        }
    }

    // ...
}
```

Agora vamos fazer o código que verifica se é, ou não é uma matriz de permutação (sim ou não, opção boleana).

Para isso podemos usar `true` e `false`, vamos colocar alguns macros no topo para escrever um código mais legível, ao invés de usar `int`, `1` e `0`, vamos escrever `bool`, `true` e `false`.

```c
#define bool int
#define true 1
#define false 0
```

Então podemos implementar a checagem, vamos optar pelo raciocínio de verificar se existe alguma linha com mais de um `1`, se sim, marcamos como falso, se não encontrarmos isso, queremos que seja verdadeiro.

```c
int main()
{
    int tamanho;
    // ... input ...
    int matriz[tamanho][tamanho];
    // ... input ...

    // Assumimos que é verdadeiro até que se prove o contrário!
    bool eh_matriz_permutacao = true;

    // Para cada linha
    for (int i = 0; i < tamanho; ++i) {
        // Vamos contar a quantidade de ocorrências nessa linha
        int contador = 0;

        // Para cada coluna
        for (int j = 0; j < tamanho; ++j) {
            // Se for `1`, avançar o contador
            if (matriz[i][j] == 1) {
                contador += 1;
            }
        }

        // Se chegou em 2
        if (contador >= 2) {
            eh_matriz_permutacao = false;
        }
    }

    // true é 1 e false é 0 :)
    printf("%d\n", eh_matriz_permutacao);
}
```

Agora podemos tirar os nossos macros definidos, e colocar um include no lugar.

```c
// #define bool int
// #define true 1
// #define false 0
#include <stdbool.h>
```

Que é o correto a se fazer nesse caso, dentro do arquivo `stdbool.h` já temos esses macros definidos!!!!

É super recomendado que se utilize de `stdbool.h` nos programas, para evitar ter que escrever código confuso como `var = 0;`, quando queremos dizer que é falso, porque `0` também é usado para representar quantidades.

## Código final
```c
#include <stdio.h>
#include <stdbool.h>

int main()
{
    int tamanho;
    scanf("%d", &tamanho);

    int matriz[tamanho][tamanho];
    for (int i = 0; i < tamanho; ++i) {
        for (int j = 0; j < tamanho; ++j) {
            scanf("%d", &matriz[i][j]);
        }
    }

    // Assumimos que é verdadeiro até que se prove o contrário!
    bool eh_matriz_permutacao = true;

    // Para cada linha
    for (int i = 0; i < tamanho; ++i) {
        // Vamos contar a quantidade de ocorrências nessa linha
        int contador = 0;

        // Para cada coluna
        for (int j = 0; j < tamanho; ++j) {
            // Se for `1`, avançar o contador
            if (matriz[i][j] == 1) {
                contador += 1;
            }
        }

        // Se chegou em 2
        if (contador >= 2) {
            eh_matriz_permutacao = false;
        }
    }

    // true é 1 e false é 0 :)
    printf("%d\n", eh_matriz_permutacao);
}
```

Além disso, podemos adicionar alguns `break`s no código para parar de rodar os `for`s se descobrirmos a resposta antes.

Infelizmente, isso vai tornar o código mais confuso, já que em C (e na maioria das linguagens) não é possível sair de "nested loops" (loops aninhados) sem várias checagens ou um `goto statement`.

```c
#include <stdio.h>
#include <stdbool.h>

int main()
{
    int tamanho;
    scanf("%d", &tamanho);

    int matriz[tamanho][tamanho];
    for (int i = 0; i < tamanho; ++i) {
        for (int j = 0; j < tamanho; ++j) {
            scanf("%d", &matriz[i][j]);
        }
    }

    bool eh_matriz_permutacao = true;

    for (int i = 0; i < tamanho; ++i) {
        int contador = 0;

        for (int j = 0; j < tamanho; ++j) {
            if (matriz[i][j] == 1) {
                contador += 1;

                if (contador >= 2) {
                    eh_matriz_permutacao = false;
                    break; // Sair
                }
            }
        }

        // Saia desse também, logo após
        if (!eh_matriz_permutacao) {
            break;
        }
    }

    printf("%d\n", eh_matriz_permutacao);
}
```

# Extra
No lugar desse trecho:
```c
if (contador >= 2) {
    eh_matriz_permutacao = false;
}
```

Poderíamos usar operações lógicas.

```c
eh_matriz_permutacao = eh_matriz_permutacao && contador < 2;
```

Uma vez colocado como false, a operação acima nunca mais pode voltar a retornar verdadeiro, outro jeito:

```c
eh_matriz_permutacao &= contador < 2;
```

Novamente: interpreta-se que `eh_matriz_permutacao` é um boleano que pode mudar para false, mas não volta a true.

É como se estivessemos verificando que `contador < 2` aplica-se para todas as linhas, ou seja a afirmação se repete:

`linha_1 && linha_2 && linha_3 && linha_4`

Se isso der true, significa que não deu false em nenhuma das etapas, ou seja, deu verdadeiro em todas.

---
#### [Voltar para a página da semana](README.md)
#### [Voltar para a página inicial](https://github.com/bti-ufrn/monitoria-itp)
