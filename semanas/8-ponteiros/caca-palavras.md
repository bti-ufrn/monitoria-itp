```c
#include <ctype.h>
#include <stdbool.h>
#include <stdio.h>
#include <string.h>

void imprimePalavra(char* c, int salto, int tam) {
    if (c == NULL) {
        printf("Palavra nao encontrada\n");
        return;
    }
    for (int i = 0; i < tam; i++) {
        printf("%c", *c);
        c += salto;
    }
    printf(", salto:%d\n", salto);
}

// Vamos rodar um for para cada um dor 8 sentidos tentando retornar o tamanho do salto
// Vamos retornar 0 caso não acharmos
//
// Outra solução para essa questão, que evitaria a repetição excessiva seria
// guardar um vetor com todos os saltos possíveis (-11, -10, -9, -1, 1, 9, 10, 11)
// e fazer um for nele tentando cada um, a nossa abordagem, porém, foi fazer um teste
// para cada um desses, repetindo bastante código
//
// Antes também fazemos checagens de limites, o que pode ser removido usando um
// boleano que guarda o estado de compleção do for
int procurar_palavra(char palavra[], char tabela[10][10], int l, int c) {
    int tam = strlen(palavra);

    // Horizontal para a direita
    if (c + tam - 1 < 10) {
        for (int i = 0; i < tam; ++i) {
            if (palavra[i] != tabela[l][c + i]) {
                break;
            }
            if (i + 1 == tam) {
                return 1;
            }
        }
    }

    // Horizontal para a esquerda
    if (c - (tam - 1) >= 0) {
        for (int i = 0; i < tam; ++i) {
            if (palavra[i] != tabela[l][c - i]) {
                break;
            }
            if (i + 1 == tam) {
                return -1;
            }
        }
    }

    // Vertical para baixo
    if (l + tam - 1 < 10) {
        for (int i = 0; i < tam; ++i) {
            if (palavra[i] != tabela[l + i][c]) {
                break;
            }
            if (i + 1 == tam) {
                return 10;
            }
        }
    }

    // Vertical para cima
    if (l - (tam - 1) >= 0) {
        for (int i = 0; i < tam; ++i) {
            if (palavra[i] != tabela[l - i][c]) {
                break;
            }
            if (i + 1 == tam) {
                return -10;
            }
        }
    }

    /////////////

    // Diagonal baixo-direita
    if ((l + tam - 1 < 10) && (c + tam - 1 < 10)) {
        for (int i = 0; i < tam; ++i) {
            if (palavra[i] != tabela[l + i][c + i]) {
                break;
            }
            if (i + 1 == tam) {
                return 11;
            }
        }
    }

    // Diagonal baixo-esquerda
    if ((l + tam - 1 < 10) && (c - (tam - 1) >= 0)) {
        for (int i = 0; i < tam; ++i) {
            if (palavra[i] != tabela[l + i][c - i]) {
                break;
            }
            if (i + 1 == tam) {
                return 9;
            }
        }
    }

    // Diagonal cima-direita
    if ((l - (tam - 1) >= 0) && (c + tam - 1 < 10)) {
        for (int i = 0; i < tam; ++i) {
            if (palavra[i] != tabela[l - i][c + i]) {
                break;
            }
            if (i + 1 == tam) {
                return -9;
            }
        }
    }

    // Diagonal cima-esquerda
    if ((l - (tam - 1) >= 0) && (c - (tam - 1) >= 0)) {
        for (int i = 0; i < tam; ++i) {
            if (palavra[i] != tabela[l - i][c - i]) {
                break;
            }
            if (i + 1 == tam) {
                return -11;
            }
        }
    }
    return 0;
}

int main() {
    // Input
    int n;
    scanf("%d\n", &n);

    char palavras[n][11];
    for (int i = 0; i < n; ++i) {
        scanf("%s\n", palavras[i]);
    }

    char tabela[10][10];
    for (int i = 0; i < 10; ++i) {
        for (int j = 0; j < 10; ++j) {
            scanf("%c", &tabela[i][j]);
        }
        scanf("%*c");  // ou "\n"
    }
    // Fim da input

    // Criar `tabela_lower_case`
    char tabela_lower_case[10][10];
    for (int i = 0; i < 10; ++i) {
        for (int j = 0; j < 10; ++j) {
            tabela_lower_case[i][j] = tolower(tabela[i][j]);
        }
    }

    // Transformar `palavras` em lower case também
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < strlen(palavras[i]); ++j) {
            palavras[i][j] = tolower(palavras[i][j]);
        }
    }

    // Para cada palavra, procurar na tabela
    for (int i = 0; i < n; ++i) {
        int salto = 0;

        // Para cada linha e coluna
        for (int l = 0; l < 10; ++l) {
            for (int c = 0; c < 10; ++c) {
                salto = procurar_palavra(palavras[i], tabela_lower_case, l, c);

                // Se achou mesmo, mostrar palavra e sair dos `for`s de busca
                if (salto != 0) {
                    imprimePalavra(&tabela[l][c], salto, strlen(palavras[i]));
                    break;
                }
            }
            // Propagar saída
            if (salto != 0) {
                break;
            }
        }

        if (salto == 0) {
            puts("Palavra nao encontrada");
        }
    }
}
```
