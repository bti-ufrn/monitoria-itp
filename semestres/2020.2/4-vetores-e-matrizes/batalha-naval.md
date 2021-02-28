# Vídeo
https://youtu.be/ZpIoLN6wP7o

# Código
```c
#include <stdbool.h>
#include <stdio.h>

void imprimir(char matriz[10][10]) {
    for (int i = 0; i < 10; ++i) {
        for (int j = 0; j < 10; ++j) {
            printf("%c", matriz[i][j]);
        }
        puts("");
    }
}

void ler_e_desenhar(char matriz[10][10], int tamanho) {
    int coordenadas_y[tamanho];
    int coordenadas_x[tamanho];
    for (int i = 0; i < tamanho; ++i) {
        scanf("%d %d", &coordenadas_y[i], &coordenadas_x[i]);
    }

    if (tamanho == 1) {
        matriz[coordenadas_y[0]][coordenadas_x[0]] = '*';
        return;
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

int main() {
    char matriz[10][10];
    for (int i = 0; i < 10; ++i) {
        for (int j = 0; j < 10; ++j) {
            matriz[i][j] = '~';
        }
    }
    ler_e_desenhar(matriz, 1);
    ler_e_desenhar(matriz, 2);
    ler_e_desenhar(matriz, 3);
    ler_e_desenhar(matriz, 4);
    imprimir(matriz);
}
```
