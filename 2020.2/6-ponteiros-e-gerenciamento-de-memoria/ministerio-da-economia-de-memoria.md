# Vídeo
https://drive.google.com/file/d/11l63asQWJo5NyqkAWExuy0v7Kq-321dA/view?usp=sharing

# Código
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int adicionarLinhas(int N, char* linhas[N], char str[10]) {
    for (int i = 0; i < N; i++) {
        if (linhas[i] == NULL) {
            linhas[i] = calloc(strlen(str) + 1, sizeof(char));
            strcpy(linhas[i], str);
            return 1;
        } else if (linhas[i][strlen(linhas[i]) - 1] != '\n') {
            linhas[i] = realloc(linhas[i], (strlen(linhas[i]) + strlen(str) + 1) * sizeof(char));
            strcat(linhas[i], str);
            return 1;
        }
    }
    return 0;
}

void imprimirLinhas(int N, char* linhas[N]) {
    for (int i = 0; i < N; i++) {
        printf("%s", linhas[i]);
    }
}

void liberarLinhas(int N, char* linhas[N]) {
    for (int i = 0; i < N; i++) {
        free(linhas[i]);
    }
}

int main() {
  char str[10];
  int N;
  scanf("%d\n", &N);
  char *linhas[N];

  for(int i=0; i<N; i++)
    linhas[i]=NULL;

  do{
    fgets(str,10,stdin);
  }while(adicionarLinhas(N, linhas,str));

  printf("Texto liberado pelo Ministro\n");
  imprimirLinhas(N, linhas);

  liberarLinhas(N, linhas);
  return 0;
}
```

---
#### [Voltar para a página da semana](README.md)
#### [Voltar para a página inicial](https://github.com/bti-ufrn/monitoria-itp)
