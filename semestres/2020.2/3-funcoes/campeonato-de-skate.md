# Campeonato de Skate
Questão da semana 3, "Campeonato de Skate"
Monitor: João Marcos

## Enunciado
Você está ajudando o departamento de educação física no primeiro campeonato de skate da UFRN. Nesse campeonato o que importa mais é a regularidade. Para isso, o campeonato ocorre em 3 dias consecutivos e em cada dia o skatista terá 3 scores diferentes. O score do dia é definido como o score que nem é o maior, nem o menor daquele dia. Se houver score repetido, o score será dado pelo score que se repetiu (seja por 2 ou 3 vezes). O score final do skatista é dado da mesma forma, mas considerando os 3 scores resultantes de cada dia.

Por exemplo, suponha que o skatista pontuou 3, 7 e 2 no primeiro dia, 3, 4 e 4 no segundo e 2, 2, 8 no terceiro. Nesse caso, o score do primeiro dia será 3, do segundo dia será 4 e do terceiro dia será 2. Considerando os scores 3, 4 e 2 obtidos, o score final será 3.

Seu programa deverá ler 18 números: os 9 scores do skatista A, seguidos dos 9 scores do skatista B. O programa deve escrever "A" se o score final de A foi maior, "B" se tiver sido o de B ou "empate" se ocorrer um empate.

Para resolver essa questão, seu programa deve fazer bom uso de funções.​

## Exemplos de entrada e saída
| Entrada                               | Saída |
| :---:                                 | :---: |
| 9 0 2 1 9 8 1 1 0 5 3 2 6 2 4 1 5 7   |   B   |
| 3 10 6 5 7 2 6 1 8 8 3 4 2 6 1 0 1 2  |   A   |

## Interpretação do enunciado:
Primeiramente precisamos perceber que por _"O score do dia é definido como o score que nem é o maior, nem o menor daquele dia"_, entendemos que o Score é a **MEDIANA** dos números.
Isto é, nem o maior, nem o menor, o número do meio desses 3, se precisamos fazer isso várias vezes, que tal fazermos uma função e reutilizar ela? Voltamos a falar sobre isso depois.

Agora sobre a entrada, temos 18 números indicando a pontuação, 9 para cada um dos 2 jogadores.
Então podemos criar uma função que lida com a pontuação de um jogador, e chamar ela na `main` duas vezes (uma para cada jogador).

Ok, mas sobre essa tal função que lida com a pontuação de um jogador, como ela vai funcionar?
Ela teria que ler 9 inteiros, 3 para cada dia, e em cada dia precisa realizar a operação de escolher a mediana.
Bom, se uma operação tem que ser repetida três vezes, podemos fazer disso também uma função, e chamar-la 3 vezes.

## Resolução
Primeiro vamos preparar a `main`:
```c
#include <stdio.h>

// TODO: finalizar essa função
int calcular_score_de_jogador() {
    // Código de retorno qualquer temporário só para o código compilar
    return 0;
}

int main() {
    int jogador_a = calcular_score_de_jogador();
    int jogador_b = calcular_score_de_jogador();
    if (jogador_a > jogador_b) {
        printf("A\n");
    } else if (jogador_b > jogador_a) {
        printf("B\n");
    } else {
        printf("empate\n");
    }
}
```

Então nossa `main` já está pronta, utilizamos a _velha técnica de chamar uma função que não funciona e se preocupar com isso depois_.

A variável `jogador_a` recebe a pontuação do jogador `a`, e o mesmo acontece para o jogador `b`, então verificamos quem ganhou, e depois temos que tentar resolver a implementação dessa função.

Como já foi explicado, essa função precisaria ler 9 números, 3 para cada dia, mas podemos subdividir o problema novamente para uma função que lida com cada dia individualmente.

```c
#include <stdio.h>

int mediana_de_3(int a, int b, int c) {
    return 0;
}

int calcular_score_do_dia() {
    return 0;
}

int calcular_score_de_jogador() {
    int primeiro_dia = calcular_score_do_dia();
    int segundo_dia = calcular_score_do_dia();
    int terceiro_dia = calcular_score_do_dia();
    return mediana_de_3(primeiro_dia, segundo_dia, terceiro_dia);
}

int main() {
    int jogador_a = calcular_score_de_jogador();
    int jogador_b = calcular_score_de_jogador();
    if (jogador_a > jogador_b) {
        printf("A\n");
    } else if (jogador_b > jogador_a) {
        printf("B\n");
    } else {
        printf("empate\n");
    }
}
```

Agora o código está tomando forma, para cada `calcular_score_de_jogador` chamamos `calcular_score_do_dia`, tiramos a mediana desses 3 números e retornamos, respeitando o que foi explicado no enunciado.

Próximo passo seria implementar a função de mediana, existem algumas maneiras de fazer isso, aqui vai uma:

```c
int mediana_de_3(int a, int b, int c) {
    if (b <= a && a <= c || c <= a && a <= b) {
        return a;
    } else if (a <= b && b <= c || c <= b && b <= a) {
        return b;
    } else {
        return c;
    }
}
```

Checamos se `b <= a <= c` ou se `c <= a <= b`, ou seja, se `a` está no meio dos outros números, repetimos o processo para `b`, e se não der certo, sabemos que só sobrou o `c` para ser a resposta.

Usamos `<=` ao invés de `<` pois existem casos como `[1, 1, 2]`, onde `1` é a mediana, mas ele não é estritamente menor que o outro `1`, esse código acima com `<=` cobre esse caso.

Colando isso no nosso código:
```c
#include <stdio.h>

int mediana_de_3(int a, int b, int c) {
    if (b <= a && a <= c || c <= a && a <= b) {
        return a;
    } else if (a <= b && b <= c || c <= b && b <= a) {
        return b;
    } else {
        return c;
    }
}

int calcular_score_do_dia() {
    return 0;
}

int calcular_score_de_jogador() {
    int primeiro_dia = calcular_score_do_dia();
    int segundo_dia = calcular_score_do_dia();
    int terceiro_dia = calcular_score_do_dia();
    return mediana_de_3(primeiro_dia, segundo_dia, terceiro_dia);
}

int main() {
    int jogador_a = calcular_score_de_jogador();
    int jogador_b = calcular_score_de_jogador();
    if (jogador_a > jogador_b) {
        printf("A\n");
    } else if (jogador_b > jogador_a) {
        printf("B\n");
    } else {
        printf("empate\n");
    }
}
```

Agora faltou a parte mais importante, fazer a função `calcular_score_do_dia`, poderíamos complicar e subdividir mais uma vez criando uma função para lê um inteiro, mas vamos deixar como está, porque em um `scanf` só podemos ler 3 valores de uma vez, então criar mais outra função iria aumentar a repetição.

Não queremos aumentar nossa repetição, queremos diminuir ela ao máximo para fazer separação de preocupações, e evitar escrever dois pedaços de código que fazem a mesma coisa, quando podemos modularizar.

Isto porque com dois pedaços de código que fazem a mesma coisa, podemos mudar em um lugar, e esquecer de mudar nos outros, o trabalho é maior de editar diferentes partes do código, ao invés de editar somente uma.

Voltando nossos olhos para a implementação de `calcular_score_do_dia`, para isso precisamos ler 3 inteiros, tirar a mediana, e retornar (releia o enunciado caso isto não esteja claro).

```c
int calcular_score_do_dia() {
    int pontuacao_1, pontuacao_2, pontuacao_3;
    scanf("%d %d %d", &pontuacao_1, &pontuacao_2, &pontuacao_3);
    return mediana_de_3(pontuacao_1, pontuacao_2, pontuacao_3);
}
```

A `calcular_score_do_dia` lê 3 inteiros, é chamada 3 vezes por `calcular_score_do_dia`, então 2 vezes pela `main`.
```
3 * 3 * 2 = 9 * 2 = 18
```
Que é a quantidade na entrada.

## Código final
```c
#include <stdio.h>

int mediana_de_3(int a, int b, int c) {
    if (b <= a && a <= c || c <= a && a <= b) {
        return a;
    } else if (a <= b && b <= c || c <= b && b <= a) {
        return b;
    } else {
        return c;
    }
}

int calcular_score_do_dia() {
    int pontuacao_1, pontuacao_2, pontuacao_3;
    scanf("%d %d %d", &pontuacao_1, &pontuacao_2, &pontuacao_3);
    return mediana_de_3(pontuacao_1, pontuacao_2, pontuacao_3);
}

int calcular_score_de_jogador() {
    int primeiro_dia = calcular_score_do_dia();
    int segundo_dia = calcular_score_do_dia();
    int terceiro_dia = calcular_score_do_dia();
    return mediana_de_3(primeiro_dia, segundo_dia, terceiro_dia);
}

int main() {
    int jogador_a = calcular_score_de_jogador();
    int jogador_b = calcular_score_de_jogador();
    if (jogador_a > jogador_b) {
        printf("A\n");
    } else if (jogador_b > jogador_a) {
        printf("B\n");
    } else {
        printf("empate\n");
    }
}
```

---
#### [Voltar para a página da semana](README.md)
#### [Voltar para a página inicial](https://github.com/bti-ufrn/monitoria-itp)
