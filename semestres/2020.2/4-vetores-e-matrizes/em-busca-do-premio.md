# Vídeo
https://youtu.be/mMPBKr62HPY

# Código
```c
#include <stdio.h>

void remover_pela_posicao(int vetor[], int tamanho, int pos) {
    for (int i = pos; i < tamanho - 1; ++i) {
        vetor[i] = vetor[i + 1];
    }
}

int main() {
    int tamanho, amigos;
    scanf("%d %d", &tamanho, &amigos);

    // Vetor de símbolos
    int vetor[tamanho];
    for (int i = 0; i < tamanho; ++i) {
        vetor[i] = i;
    }

    for (int i = 0; i < amigos; ++i) {
        int quantidade_do_amigo;
        scanf("%d", &quantidade_do_amigo);

        for (int j = 0; j < quantidade_do_amigo; ++j) {
            int simbolo;
            scanf("%d", &simbolo);

            for (int k = 0; k < tamanho; ++k) {
                if (vetor[k] == simbolo) {
                    remover_pela_posicao(vetor, tamanho, k);
                    tamanho -= 1;
                }
            }
        }
    }

    // Imprimir
    printf("[");
    if (tamanho > 0) {
        printf(" %d", vetor[0]);
        for (int i = 1; i < tamanho; ++i) {
            printf(", %d", vetor[i]);
        }
    }
    printf(" ]\n");
}
```
