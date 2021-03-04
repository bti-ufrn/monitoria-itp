# Vídeo
https://drive.google.com/file/d/1PpmMUrWl1ACNAwadsMvOr64P6SD_ZkMX/view?usp=sharing

# Código
```c
#include <stdio.h>

int main(void) {
    char string[61];
    fgets(string, 61, stdin);
    for (int i = 0; string[i] != '\0'; i++) {
        if ((i == 0 || string[i - 1] == ' ') && string[i] >= 'a' && string[i] <= 'z') {
            string[i] -= 'a' - 'A';
        }
        if ((i > 0 && string[i - 1] != ' ') && string[i] >= 'A' && string[i] <= 'Z') {
            string[i] += 'a' - 'A';
        }
    }
    printf("%s\n", string);
    return 0;
}
```

---
#### [Voltar para a página da semana](README.md)
#### [Voltar para a página inicial](https://github.com/bti-ufrn/monitoria-itp)
