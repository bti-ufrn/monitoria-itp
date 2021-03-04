# Vídeo
https://drive.google.com/file/d/1h8nukhIRRY8SHlnwJ3Aher95fRwdmGO0/view?usp=sharing

# Código
```c
#include <stdio.h>

int main(void) {
    int tamanho, maximo = 0;
    scanf("%d", &tamanho);
    int andares[tamanho];
    for (int i = 0; i < tamanho; i++) {
        scanf("%d", &andares[i]);
    }
    for (int i = 0; i < tamanho - 1; i++) {
        for (int j = i + 1; j < tamanho; j++) {
            int distancia = andares[i] + andares[j] + j - i;
            if (distancia > maximo) {
                maximo = distancia;
            }
        }
    }
    printf("%d\n", maximo);
    return 0;
}
```

---
#### [Voltar para a página da semana](README.md)
#### [Voltar para a página inicial](https://github.com/bti-ufrn/monitoria-itp)
