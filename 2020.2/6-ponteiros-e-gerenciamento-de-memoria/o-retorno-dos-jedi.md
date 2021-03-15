# Vídeo
https://drive.google.com/file/d/1jCPrHVPTz5_YSYUkp0OV2RoFTtE_7iuu/view?usp=sharing

# Código
```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

char** separa(char *str, int *quantidadeTextos){
  char **textoSeparado = NULL;
  char *texto = strtok(str," ");
  int cont = 0;
  while(texto != NULL){
    textoSeparado = realloc(textoSeparado, (cont + 1) * sizeof(char*));
    textoSeparado[cont++] = texto;
    texto = strtok(NULL," ");
  }
  *quantidadeTextos = cont;
  return textoSeparado;
}

char* juntar(char **vetor_strings, int tamanho_vetor){
  int tamanho_final = 0;
  char *strings_unidas = NULL;
  for(int i = 0; i < tamanho_vetor; i++) {
      tamanho_final += strlen(vetor_strings[i]) + 1;
  }
  strings_unidas = calloc(tamanho_final, sizeof(char));
  strings_unidas[0] = '\0'; //inicializa com uma string vazia
  for(int i=0; i<tamanho_vetor; i++){
    strcat(strings_unidas,vetor_strings[i]);
    strcat(strings_unidas," ");
  }
  return strings_unidas;
}

char* embaralha(char* T) {
    int S = 5940, k = 0, N;
    srand(S);
    char **E = NULL;
    char **M = separa(T, &N);
    int histograma[N];
    for (int i = 0; i < N; i++) {
        histograma[i] = 0;
    }
    while (1) {
        E = realloc(E, (k + 1) * sizeof(char*));
        int n = rand(), sorteado = 1;
        E[k++] = M[n % N];
        histograma[n % N]++;
        for (int i = 0; i < N; i++) {
            if (histograma[i] == 0) {
                sorteado = 0;
                break;
            }
        }
        if (sorteado) {
            break;
        }
    }
    char *R = juntar(E, k);
    free(E);
    free(M);
    return R;
}

int main(void) {
    char mensagem[501], *resultado;
    fgets(mensagem, 501, stdin);
    resultado = embaralha(mensagem);
    printf("%s", resultado);
    free(resultado);
    return 0;
}
```

---
#### [Voltar para a página da semana](README.md)
#### [Voltar para a página inicial](https://github.com/bti-ufrn/monitoria-itp)
