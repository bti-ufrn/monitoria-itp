# Vídeo
https://youtu.be/2o4zcj0h6rg

# Código
```c
#include <stdio.h>
#include <string.h>

int main() {
    char string_a[1024];
    char string_b[1024];
    scanf("%[^\n] %[^\n]", string_a, string_b);

    char resultado[2048];

    int tamanho_a = strlen(string_a);
    int tamanho_b = strlen(string_b);

    int i = 0;    // para resultado
    int i_a = 0;  // string_a
    int i_b = 0;  // string_b

    while (i_a < tamanho_a && i_b < tamanho_b) {
        resultado[i++] = string_a[i_a++];
        resultado[i++] = string_b[i_b++];
    }

    while (i_a < tamanho_a) {
        resultado[i++] = string_a[i_a++];
    }

    while (i_b < tamanho_b) {
        resultado[i++] = string_b[i_b++];
    }

    resultado[i] = '\0';
    printf("%s\n", resultado);
}
```
