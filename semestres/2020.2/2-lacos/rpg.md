# RPG
Questão da semana 2, "RPG"
Monitor: João Marcos

## Enunciado
Alyson estava em uma tarde casual de RPG com colegas quando eles tiveram um problema: todos decidiram agir ao mesmo tempo para derrotar o dragão, salvar a princesa e conquistar o tesouro, e o Mestre da seção perdeu o controle do que estava acontecendo! Percebendo a confusão em que seu mestre se metera, Alyson decidiu pedir aos alunos de ITP para escreverem um programa que pudesse ajudar o mestre a entender o que havia acontecido!

Escreva um programa que receberá dois inteiros: o número de jogadas N e os pontos de vida (life, energia) V do dragão. A partir daí, seu programa deverá ler N linhas contendo as jogadas dos participantes na seguinte forma:

C A, onde C é a letra que identifica o jogador, e A é o valor do ataque que ele realizou.

Os ataques serão inteiros variando de 1 a 20.  O dragão é um ser feroz, resistente e ágil, então apenas ataques acima de 14 irão acertá-lo! Se o ataque atingir o dragão, o valor do dano associado ao jogador que acertou é deduzido dos pontos de vida V do dragão. Quando os pontos de vida do dragão forem igual ou menor a zero, o dragão é derrotado!

Os jogadores são:

| Nome do Jogador | Letra identificadora | Dano | Personagem         |
| ---             | ---                  | ---  | ---                |
| Alyson          | A                    | 10   | The Ice Giant      |
| Isaac           | I                    | 10   | The Good Carpenter |
| Rafaela         | R                    | 6    | The Mathematician  |
| Wellington      | W                    | 8    | Anonymous          |
| Salgado         | S                    | 4    | Leaf Hands         |
| Careca          | C                    | 1    | The ITPist         |

## Exemplos de entrada e saída
| Entrada    | Saída                      |
| :---:      | :---:                      |
| 10 6       | Rafaela derrotou o dragão! |
| A 5        | |
| I 2        | |
| R 4        | |
| W 11       | |
| C 3        | |
| S 20       | |
| A 3        | |
| I 1        | |
| R 20       | |
| C 20       | |
|            | |
|            | |
| 10 100     | Casa caiu!                 |
| A 20       | |
| I 20       | |
| R 20       | |
| C 20       | |
| S 20       | |
| A 20       | |
| W 20       | |
| A 20       | |
| C 20       | |
| R 20       | |


## Interpretação do enunciado:
Temos `N` linhas para analisar, vamos fazer um `for` que roda `N` vezes, para cada uma das vezes, a gente lê o identificador do jogador e o número tirado no dado, caso o número seja menor que 14, não fazemos nada, se for maior que 14, fazemos o ataque.

No ataque temos que fazer uma estrutura de decisão baseada no ID para descobrir qual o dano, o dragão foi derrotado caso sua vida seja igual ou menor que 0.

## Resolução
Primeiro com entrada dos dados:

```c
#include <stdio.h>

int main() {
    int numero_de_jogadores, vida_dragao;
    scanf("%d %d", &numero_de_jogadores, &vida_dragao);

    for (int i = 0; i < numero_de_jogadores; ++i) {
        char jogador;
        int dado;
        scanf(" %c %d", &jogador, &dado);

        // checar ataque
    }
}
```

Note que estou declarando duas variáveis fora do loop, e duas dentro, se não estiver acostumado com esse tipo de declaração não se preocupe, as declaração dentro do `for` **não vai** piorar a performance, o compilador sabe que é só uma variável. É recomendado que você coloque as variáveis com o menor tempo de vida possível, isto é, declarar logo antes do uso, isto deixa o código mais claro pois eu estou sempre unindo declaração com inicialização de valor, então é mais difícil de se perder scrollando para descobrir valor de coisas distantes.

Lembre-se, se você declara uma variável e não inicializa, o valor dela é INDEFINIDO, pode ser qualquer coisa, dependendo do compilador e da sua máquina. Isto não é um problema pois logo depois da declaração estou inicializando o valor com o `scanf` (não preciso colocar o valor inicial como _0_, por exemplo), sabemos que o `scanf` pode falhar e deixar os valores como estão caso a entrada não venha como especificado, mas estamos assumindo que a entrada (no LoP) é confiável no formato especificado, então não vai dar errado, mesmo que não estejamos VERIFICANDO se deu erado ou não.

Outra coisa, o segundo `scanf` tem um espaço antes do `"%c"`, isto porque o `scanf` que vem antes (seja o do loop depois da primeira iteração, ou o fora do loop para quando `i = 0`) termina com `"...%d"`, este especificador lê enquanto é um número, até antes que não seja, atenção ao "até antes", isso significa que ele vai deixar uma quebra de linha ali solta sim! (lembre-se, cada linha termina com um `'\n'` que é um caractere válido).

---

Agora para a parte do ataque, vamos colocar um `if` que pula a iteração atual do `for` usando o comando `continue`.

```c
#include <stdio.h>

int main() {
    int numero_de_jogadores, vida_dragao;
    scanf("%d %d", &numero_de_jogadores, &vida_dragao);

    for (int i = 0; i < numero_de_jogadores; ++i) {
        char jogador;
        int dado;
        scanf(" %c %d", &jogador, &dado);

        if (dado <= 14) {
            // Pula a parte do ataque
            continue;
        }

        // ataque
    }
}
```

Pronto, como está o código acima, para cada linha lida (cada ataque), se o dado for menor que o mínimo aceitável, esta linha será pulada.

Agora para a parte do ataque, vamos usar o bloco `switch-case` (conteúdo da semana anterior), é como um `if` seguido de vários `if else` para verificar a identidade de uma variável, dadas várias opções, assim vamos descobrir quanto de dano será causado.

```c
#include <stdio.h>

int main() {
    int numero_de_jogadores, vida_dragao;
    scanf("%d %d", &numero_de_jogadores, &vida_dragao);

    for (int i = 0; i < numero_de_jogadores; ++i) {
        char jogador;
        int dado;
        scanf(" %c %d", &jogador, &dado);

        if (dado <= 14) {
            // Pula a parte do ataque
            continue;
        }

        int dano = 0;
        switch (jogador) {
            case 'A':
                dano = 10;
                break;

            case 'I':
                dano = 10;
                break;

            case 'R':
                dano = 6;
                break;

            case 'W':
                dano = 8;
                break;

            case 'S':
                dano = 4;
                break;

            case 'C':
                dano = 1;
                break;

            default:
                printf("Erro! Caractere inesperado!\n");
                return 1;  // Return com código de erro
        }

        vida -= dano;
        if (vida <= 0) {
            // Dragão derrotado
        }
    }
}
```

Nesse caso, o switch case está atribuindo para a variável `dano`, e depois nós usamos ela.

Note o `return 1;`, nesse caso usamos `1` e não `0`, para indicar que o programa não rodou corretamente, existiu um erro na entrada, pois tivemos um caractere inesperado.

Pronto, o ataque já está sendo causado com o dano correto do jogador que atacou, agora só precisamos imprimir a mensagem correta para quando o dragão for derrotado.

## Código final
```c
#include <stdio.h>

int main() {
    int numero_de_jogadores, vida_dragao;
    scanf("%d %d", &numero_de_jogadores, &vida_dragao);

    for (int i = 0; i < numero_de_jogadores; ++i) {
        char jogador;
        int dado;
        scanf(" %c %d", &jogador, &dado);

        if (dado <= 14) {
            // Pula a parte do ataque
            continue;
        }

        int dano = 0;
        switch (jogador) {
            case 'A':
                dano = 10;
                break;

            case 'I':
                dano = 10;
                break;

            case 'R':
                dano = 6;
                break;

            case 'W':
                dano = 8;
                break;

            case 'S':
                dano = 4;
                break;

            case 'C':
                dano = 1;
                break;

            default:
                printf("Erro! Caractere inesperado!\n");
                return 1;  // Return com código de erro
        }

        vida_dragao -= dano;
        // Dragão derrotado
        if (vida_dragao <= 0) {
            switch (jogador) {
                case 'A':
                    printf("Alyson derrotou o dragão!\n");
                    break;

                case 'I':
                    printf("Isaac derrotou o dragão!\n");
                    break;

                case 'R':
                    printf("Rafaela derrotou o dragão!\n");
                    break;

                case 'W':
                    printf("Wellington derrotou o dragão!\n");
                    break;

                case 'S':
                    printf("Salgado derrotou o dragão!\n");
                    break;

                case 'C':
                    printf("Careca derrotou o dragão!\n");
                    break;
            }
            return 0;  // Finalizar o programa depois de derrotar o dragão
        }
    }

    // Se chegou aqui, é porque o dragão não foi derrotado
    printf("Casa caiu!\n");
}
```

Se o loop acabar e não cair no `if` que dá `return 0;`, então a "Casa caiu!", significa que o dragão não morreu.

Essa solução ficou complexa no quesito controle de fluxo do programa, juntamos `continue` no meio, e o `return 0;` no fim de um loop para controlar o fluxo sem ter que colocar vários `if`s e `else`s.

---

Seguem outras soluções para a mesma questão:

# Versão 2

```c
#include <stdio.h>

int main() {
    int numero_de_jogadores, vida_dragao;
    scanf("%d %d", &numero_de_jogadores, &vida_dragao);

    for (int i = 0; i < numero_de_jogadores; ++i) {
        char jogador;
        int dado;
        scanf(" %c %d", &jogador, &dado);

        if (dado <= 14) {
            // Fazer nada
        } else {
            switch (jogador) {
                case 'A':
                    vida_dragao -= 10;
                    if (vida_dragao <= 0) {
                        printf("Alyson derrotou o dragão!\n");
                        return 0;
                    }
                    break;

                case 'I':
                    vida_dragao -= 10;
                    if (vida_dragao <= 0) {
                        printf("Isaac derrotou o dragão!\n");
                        return 0;
                    }
                    break;

                case 'R':
                    vida_dragao -= 6;
                    if (vida_dragao <= 0) {
                        printf("Rafaela derrotou o dragão!\n");
                        return 0;
                    }
                    break;

                case 'W':
                    vida_dragao -= 8;
                    if (vida_dragao <= 0) {
                        printf("Wellington derrotou o dragão!\n");
                        return 0;
                    }
                    break;

                case 'S':
                    vida_dragao -= 4;
                    if (vida_dragao <= 0) {
                        printf("Salgado derrotou o dragão!\n");
                        return 0;
                    }
                    break;

                case 'C':
                    vida_dragao -= 1;
                    if (vida_dragao <= 0) {
                        printf("Careca derrotou o dragão!\n");
                        return 0;
                    }
                    break;

                default:
                    printf("Erro! Caractere inesperado!\n");
                    return 1;  // Return com código de erro
            }
        }
    }

    // Se chegou aqui, é porque o dragão não foi derrotado
    printf("Casa caiu!\n");
}
```

# Versão 3
```c
#include <stdbool.h>
#include <stdio.h>

int main() {
    int numero_de_jogadores, vida_dragao;
    scanf("%d %d", &numero_de_jogadores, &vida_dragao);

    bool dragao_vivo = true;

    for (int i = 0; i < numero_de_jogadores; ++i) {
        char jogador;
        int dado;
        scanf(" %c %d", &jogador, &dado);

        if (dado > 14) {
            if (jogador == 'A') {
                vida_dragao -= 10;
                if (vida_dragao <= 0) {
                    printf("Alyson derrotou o dragão!\n");
                    dragao_vivo = false;
                    break;
                }
            } else if (jogador == 'I') {
                vida_dragao -= 10;
                if (vida_dragao <= 0) {
                    printf("Isaac derrotou o dragão!\n");
                    dragao_vivo = false;
                    break;
                }
            } else if (jogador == 'R') {
                vida_dragao -= 6;
                if (vida_dragao <= 0) {
                    printf("Rafaela derrotou o dragão!\n");
                    dragao_vivo = false;
                    break;
                }
            } else if (jogador == 'W') {
                vida_dragao -= 8;
                if (vida_dragao <= 0) {
                    printf("Wellington derrotou o dragão!\n");
                    dragao_vivo = false;
                    break;
                }
            } else if (jogador == 'S') {
                vida_dragao -= 4;
                if (vida_dragao <= 0) {
                    printf("Salgado derrotou o dragão!\n");
                    dragao_vivo = false;
                    break;
                }
            } else if (jogador == 'C') {
                vida_dragao -= 1;
                if (vida_dragao <= 0) {
                    printf("Careca derrotou o dragão!\n");
                    dragao_vivo = false;
                    break;
                }
            } else {
                printf("Erro! Caractere inesperado!\n");
                return 1;  // Return com código de erro
            }
        }
    }

    // Mesmo que "if (dragao_vivo == true)"
    if (dragao_vivo) {
        printf("Casa caiu!\n");
    }
}
```

# Última versão

```c
#include <stdio.h>

int main() {
    int numero_de_jogadores, vida_dragao;
    scanf("%d %d", &numero_de_jogadores, &vida_dragao);

    for (int i = 0; i < numero_de_jogadores; ++i) {
        char jogador;
        int dado;
        scanf(" %c %d", &jogador, &dado);

        if (dado <= 14) {
            continue;
        }
        int dano;
        char* nome;  // Aponta para o nome do último atacante

        if (jogador == 'A') {
            dano = 10;
            nome = "Alyson";
        } else if (jogador == 'I') {
            dano = 10;
            nome = "Isaac";
        } else if (jogador == 'R') {
            dano = 6;
            nome = "Rafaela";
        } else if (jogador == 'W') {
            dano = 8;
            nome = "Wellington";
        } else if (jogador == 'S') {
            dano = 4;
            nome = "Salgado";
        } else if (jogador == 'C') {
            dano = 1;
            nome = "Careca";
        } else {
            printf("Erro! Caractere inesperado!\n");
            return 1;
        }

        vida_dragao -= dano;
        if (vida_dragao <= 0) {
            printf("%s derrotou o dragão!\n", nome);
            return 0;
        }
    }

    printf("Casa caiu!\n");
}
```



# Extra

Obs1: C é complicado, C pode ser difícil, mas "por que essa linguagem é tão rudimentar?", a resposta seria que C é uma linguagem de baixo nível enxuta o bastante para você fazer outras ferramentas em cima. Lembre-se que surgiu antes da internet, eram outros problemas e outras limitações.

Obs2: Ambos `printf` e `scanf` retornam um inteiro indicando se aconteceu algum erro, você precisa ler o manual dessas funções para ver o que cada número significaria, estamos ingenuamente usando essas funções para ler e escrever, o mais correto seria criar uma biblioteca de leitura que tem checagens que interrompe quando acontece um erro. Estamos assumindo que a entrada de dados do programa está 100% correta porque os professores estão colocando as atividades, mas você nunca deve esquecer de tratar erros na vida real.

Obs3: Os argumentos do `scanf` SEMPRE deverão ser ponteiros (endereços).

Obs4: `printf` pede valores ao invés de ponteiros, a única exceção é print de strings.

Obs5: `printf` e `scanf` vão pedir um número exato de argumentos na string de formatação, e no resto da função.
