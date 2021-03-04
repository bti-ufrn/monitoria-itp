# Vídeo
https://drive.google.com/file/d/1GyLTIQyj5Icu7dtplAS1oqNVG6YGnxbB/view?usp=sharing

# Código
```c
#include <stdio.h>
#include <string.h>

const char S[40] =
    {'0','1','2','3','4','5','6','7','8','9',
     'A','B','C','D','E','F','G','H','I','J',
     'K','L','M','N','O','P','Q','R','S','T',
     'U','V','W','X','Y','Z','.',',','?',' '};

void limparString(char string[]) {
    for (int i = 0; string[i] != '\0'; i++) {
        if (string[i] == '\n') {
            string[i] = '\0';
            break;
        }
    }
}

int validarChave(char chave[]) {
    limparString(chave);
    if (strlen(chave) != 4) {
        return 0;
    } else {
        for (int i = 0; chave[i] != '\0'; i++) {
            if (chave[i] < '0' || chave[i] > '9') {
                return 0;
            }
        }
    }
    return 1;
}

int validarMensagem(char mensagem[]) {
    limparString(mensagem);
    for (int i = 0; mensagem[i] != '\0'; i++) {
        int valido = 0;
        for (int j = 0; j < 40; j++) {
            if (mensagem[i] == S[j]) {
                valido = 1;
                break;
            }
        }
        if (!valido) {
            return 0;
        }
    }
    return 1;
}

void criptografar(char chave[], char mensagem[]) {
    for (int i = 0; mensagem[i] != '\0'; i++) {
        for (int j = 0; j < 40; j++) {
            if (mensagem[i] == S[j]) {
                mensagem[i] = S[(j + (chave[i % 4] - '0')) % 40];
                break;
            }
        }
    }
}

int main(void) {
    char chave[11], mensagem[201];
    fgets(chave, 11, stdin);
    fgets(mensagem, 201, stdin);
    if (!validarChave(chave)) {
        printf("Chave invalida!\n");
    } else if (!validarMensagem(mensagem)) {
        printf("Caractere invalido na entrada!\n");
    } else {
        criptografar(chave, mensagem);
        printf("%s\n", mensagem);
    }
    return 0;
}
```

---
#### [Voltar para a página da semana](README.md)
#### [Voltar para a página inicial](https://github.com/bti-ufrn/monitoria-itp)
