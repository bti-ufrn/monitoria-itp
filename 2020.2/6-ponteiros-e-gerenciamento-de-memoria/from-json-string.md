# Vídeo
https://youtu.be/fUZsJszdgNg

# Código
```c
#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int *fromJsonString_int(char linha[], int *tamanho_do_vetor) {
    int tamanho = strlen(linha);
    int *vetor = malloc(sizeof(int[tamanho]));

    vetor[0] = atoi(&linha[1]);
    *tamanho_do_vetor = 1;

    for (int i = 0; i < tamanho; ++i) {
        if (linha[i] == ' ') {
            vetor[(*tamanho_do_vetor)++] = atoi(&linha[i + 1]);
        }
    }
    return vetor;
}

double *fromJsonString_double(char linha[], int *tamanho_do_vetor) {
    int tamanho = strlen(linha);
    double *vetor = malloc(sizeof(double[tamanho]));

    vetor[0] = atof(&linha[1]);
    *tamanho_do_vetor = 1;

    for (int i = 0; i < tamanho; ++i) {
        if (linha[i] == ' ') {
            vetor[(*tamanho_do_vetor)++] = atof(&linha[i + 1]);
        }
    }
    return vetor;
}

bool eh_vetor_de_double(char linha[]) {
    int tamanho = strlen(linha);
    for (int i = 0; i < tamanho; ++i) {
        if (linha[i] == '.') {
            return true;
        }
    }
    return false;
}

int main() {
    int quantidade_de_linhas;
    scanf("%d%*c", &quantidade_de_linhas);

    for (int i = 0; i < quantidade_de_linhas; ++i) {
        char linha[202];
        fgets(linha, 202, stdin);

        if (linha[1] == ']') {
            puts("vetor vazio");
            continue;
        }

        int tamanho_do_vetor;
        if (eh_vetor_de_double(linha)) {
            double *vetor = fromJsonString_double(linha, &tamanho_do_vetor);
            for (int i = 0; i < tamanho_do_vetor; ++i) {
                printf("%lf ", vetor[i]);
            }
            puts("");
            free(vetor);
        } else {
            int *vetor = fromJsonString_int(linha, &tamanho_do_vetor);
            for (int i = 0; i < tamanho_do_vetor; ++i) {
                printf("%d ", vetor[i]);
            }
            puts("");
            free(vetor);
        }
    }
}
```

---
#### [Voltar para a página da semana](README.md)
#### [Voltar para a página inicial](https://github.com/bti-ufrn/monitoria-itp)
