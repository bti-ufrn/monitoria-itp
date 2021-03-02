# Vídeo
https://youtu.be/99MRGr2DHv0

# Código
```c
#include <stdbool.h>
#include <stdio.h>

void ler_e_desenhar_barco(char matriz[10][10], int tamanho) {
    if (tamanho == 1) {
        int y, x;
        scanf("%d %d", &y, &x);
        matriz[y][x] = '*';
        return;
    }

    int coordenadas_y[tamanho];
    int coordenadas_x[tamanho];
    for (int i = 0; i < tamanho; ++i) {
        scanf("%d %d", &coordenadas_y[i], &coordenadas_x[i]);
    }

    bool esta_na_horizontal = coordenadas_y[0] == coordenadas_y[1];

    if (esta_na_horizontal) {
        int y = coordenadas_y[0];
        matriz[y][coordenadas_x[0]] = '<';
        for (int i = 1; i < tamanho - 1; ++i) {
            matriz[y][coordenadas_x[i]] = '=';
        }
        matriz[y][coordenadas_x[tamanho - 1]] = '>';
    } else {
        int x = coordenadas_x[0];
        matriz[coordenadas_y[0]][x] = '^';
        for (int i = 1; i < tamanho - 1; ++i) {
            matriz[coordenadas_y[i]][x] = '|';
        }
        matriz[coordenadas_y[tamanho - 1]][x] = 'v';
    }
}

void imprimir(char matriz[10][10]) {
    for (int i = 0; i < 10; ++i) {
        for (int j = 0; j < 10; ++j) {
            printf("%c", matriz[i][j]);
        }
        puts("");
    }
}

int main() {
    char matriz[10][10];
    for (int i = 0; i < 10; ++i) {
        for (int j = 0; j < 10; ++j) {
            matriz[i][j] = '~';
        }
    }
    ler_e_desenhar_barco(matriz, 1);
    ler_e_desenhar_barco(matriz, 2);
    ler_e_desenhar_barco(matriz, 3);
    ler_e_desenhar_barco(matriz, 4);
    imprimir(matriz);
}
```

---
#### [Voltar para a página da semana](README.md)
#### [Voltar para a página inicial](https://github.com/bti-ufrn/monitoria-itp)
