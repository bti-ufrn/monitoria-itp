# Vídeo
https://youtu.be/Z114SUMiOCA

# Código
```c
#include <stdbool.h>
#include <stdio.h>

int main() {
    int n, m;
    scanf("%d %d", &n, &m);

    int coord_jedi_y, coord_jedi_x;
    scanf("%d %d", &coord_jedi_y, &coord_jedi_x);
    coord_jedi_y -= 1;
    coord_jedi_x -= 1;

    int matriz[n][m];
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < m; ++j) {
            scanf("%d", &matriz[i][j]);
        }
    }
    int bombas_achadas = 0;
    bool morreu = false;

    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < m; ++j) {
            bool eh_bomba = !(i > 0 && matriz[i - 1][j] >= matriz[i][j] ||
                              j > 0 && matriz[i][j - 1] >= matriz[i][j] ||
                              i < n - 1 && matriz[i + 1][j] >= matriz[i][j] ||
                              j < m - 1 && matriz[i][j + 1] >= matriz[i][j] ||
                              i > 0 && j > 0 && matriz[i - 1][j - 1] >= matriz[i][j] ||
                              i > 0 && j < m - 1 && matriz[i - 1][j + 1] >= matriz[i][j] ||
                              i < n - 1 && j > 0 && matriz[i + 1][j - 1] >= matriz[i][j] ||
                              i < n - 1 && j < m - 1 && matriz[i + 1][j + 1] >= matriz[i][j]);

            if (eh_bomba) {
                bombas_achadas += 1;
                printf("Local %d: %d %d\n", bombas_achadas, i + 1, j + 1);

                if (i == coord_jedi_y && j == coord_jedi_x) {
                    morreu = true;
                }
            }
        }
    }
    if (morreu) {
        puts("Descanse na Força...");
    } else {
        puts("Ao resgate!");
    }
}
```
