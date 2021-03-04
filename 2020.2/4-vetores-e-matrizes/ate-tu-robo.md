# Vídeo
https://drive.google.com/file/d/1DcRQ3lbwPWPBm2Fpafpx4sju0d2Sx09P/view?usp=sharing

# Código
```c
#include <stdio.h>

void lerMapa(int linhas, int colunas, char mapa[linhas][colunas]) {
    for (int i = 0; i < linhas; i++) {
        for (int j = 0; j < colunas; j++) {
            scanf(" %c", &mapa[i][j]);
        }
    }
}

char virarEsquerda(char direcao) {
    switch (direcao) {
        case 'N':
            return 'O';
        case 'L':
            return 'N';
        case 'S':
            return 'L';
        case 'O':
            return 'S';
    }
}

char virarDireita(char direcao) {
    switch (direcao) {
        case 'N':
            return 'L';
        case 'L':
            return 'S';
        case 'S':
            return 'O';
        case 'O':
            return 'N';
    }
}

void moverRobo(int x, int y, char mapa[x][y]) {
    int linhaAtual, colunaAtual, linhaAlvo, colunaAlvo, movimentos;
    char direcao;
    scanf("%d %d %c %d", &linhaAtual, &colunaAtual, &direcao, &movimentos);
    linhaAtual--;
    colunaAtual--;
    for (int i = 0; i < movimentos; i++) {
        linhaAlvo = linhaAtual;
        colunaAlvo = colunaAtual;
        switch (direcao) {
            case 'N':
                linhaAlvo--;
                break;
            case 'L':
                colunaAlvo++;
                break;
            case 'S':
                linhaAlvo++;
                break;
            case 'O':
                colunaAlvo--;
                break;
        }
        if (linhaAlvo < 0 || linhaAlvo >= x || colunaAlvo < 0 || colunaAlvo >= y || mapa[linhaAlvo][colunaAlvo] == '_' || mapa[linhaAlvo][colunaAlvo] == '|') {
            direcao = virarDireita(direcao);
        } else if (mapa[linhaAlvo][colunaAlvo] == '*') {
            direcao = virarEsquerda(direcao);
        } else {
            linhaAtual = linhaAlvo;
            colunaAtual = colunaAlvo;
        }
    }
    printf("Posição final: %d %d", linhaAtual + 1, colunaAtual + 1);
}

int main(void) {
    int linhas, colunas;
    scanf("%d %d", &linhas, &colunas);
    char mapa[linhas][colunas];
    lerMapa(linhas, colunas, mapa);
    moverRobo(linhas, colunas, mapa);
    return 0;
}
```

---
#### [Voltar para a página da semana](README.md)
#### [Voltar para a página inicial](https://github.com/bti-ufrn/monitoria-itp)
