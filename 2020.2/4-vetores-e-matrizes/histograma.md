# Vídeo
https://drive.google.com/file/d/1MJ_6fZaiY0Hrm4FWpxKQ251oTq1COcgA/view?usp=sharing

# Código
```c
#include <stdio.h>

void createHistogram(int c, int h[c]) {
    for (int i = 0; i < c; i++) {
        h[i] = 0;
    }
    int qtdNotas;
    float nota;
    scanf("%d", &qtdNotas);
    for (int i = 0; i < qtdNotas; i++) {
        scanf("%f", &nota);
        for (int j = c - 1; j >= 0; j--) {
            if (nota >= (10.0 / c) * j) {
                h[j]++;
                break;
            }
        }
    }
}

void joinHistograms(int c, int h1[c], int h2[c], int joint[c]) {
    for (int i = 0; i < c; i++) {
        joint[i] = h1[i] + h2[i];
    }
}

void printHistogram(int c, int h[c]) {
    int maximo = 0;
    for (int i = 0; i < c; i++) {
        if (h[i] > maximo) {
            maximo = h[i];
        }
    }
    for (int i = maximo; i > 0; i--) {
        printf("_");
        for (int j = 0; j < c; j++) {
            if (h[j] >= i) {
                if (j < c - 1) {
                    printf("##__");
                } else {
                    printf("##");
                }
            } else {
                if (j < c - 1) {
                    printf("____");
                } else {
                    printf("__");
                }
            }
        }
        printf("_\n");
    }
    printf("\n");
}

int main() {
    int numCateg;
    scanf("%i", &numCateg);

    int histA[numCateg];
    int histB[numCateg];
    int histAll[numCateg];

    createHistogram(numCateg, histA);
    createHistogram(numCateg, histB);
    joinHistograms(numCateg, histA, histB, histAll);

    printHistogram(numCateg, histA);
    printHistogram(numCateg, histB);
    printHistogram(numCateg, histAll);

    return 0;
}
```

---
#### [Voltar para a página da semana](README.md)
#### [Voltar para a página inicial](https://github.com/bti-ufrn/monitoria-itp)
