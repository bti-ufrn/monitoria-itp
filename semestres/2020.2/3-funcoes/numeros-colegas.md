# Números Colegas
Questão da semana 3 (funções), "Números Colegas"
Monitor: João Marcos

## Enunciado
"Amigo é coisa pra se guardar" -- como diz Milton Nascimento. Os números também podem ser amigos entre si!

Para saber se dois números são amigos verifica-se se a soma dos divisores próprios de um é igual ao outro e vice-versa. Por exemplo: 220 e 284 são amigos pois os divisores próprios de 220 são 1, 2, 4, 5, 10, 11, 20, 22, 44, 55 e 110, cuja soma é 284; e os divisores de 284 são 1, 2, 4, 71 e 142, cuja soma é 220. Sendo D(x) a soma dos divisores próprios, então dois números inteiros A e B são amigos se, e somente se, D(A) = B e D(B) = A.

O problema é que os números amigos são raros! Os primeiros números amigos são (220, 284), (1184, 1210), (2620, 2924).

Então você resolveu descobrir o que chamou de números colegas. Dois números inteiros A e B são colegas se |D(A) - B| <= 2 e |D(B) - A| <= 2, ou seja, a soma dos divisores próprios de um número pode ter uma diferença de até 2 para o outro número.

Por exemplo, 140 e 195 são colegas, pois a soma dos divisores próprios de 140 é 196 (diferença de 1 para 195) e a soma dos divisores próprios de 195 é 141 (diferença de 1 para 140).

O seu programa deve ler dois inteiros (suponha que sejam sempre diferentes) e escrever S caso sejam colegas e N caso contrário.​

Além disso seu programa deve utilizar duas funções: uma para somar os divisores próprios de um número e outra para retornar se dois números são colegas ou não.

## Exemplos de entrada e saída
| Entrada | Saída |
| :---:   | :---: |
| 1 2     | S     |
|         |       |
| 6 8     | S     |

## Resolução
A solução ideal para essa questão seria subdividindo em duas funções para evitar a repetição desnecessária de código.

Precisamos calcular a soma dos divisores duas vezes, então vamos começar criar uma função para fazer isso.

```c
int somarDivisores(int num) {
    int somatorio = 0;
    for (int i = 1; i < num; i++) {
        if (num % i == 0) {
            somatorio += i;
        }
    }
    return somatorio;
}

int main() {

}
```

Para cada número entre `1` e `num - 1`, se esse número divide o valor de `num`, então somamos.

Ok, agora vamos ler a entrada.

```c
#include <stdio.h>

int somarDivisores(int num) {
    int somatorio = 0;
    for (int i = 1; i < num; i++) {
        if (num % i == 0) {
            somatorio += i;
        }
    }
    return somatorio;
}

int main() {
    int num1, num2;
    scanf("%d %d", &num1, &num2);
}
```

E então, vamos chamar uma função que verifica se os números são amigos, por enquanto, não vamos nos preocupar com a implementação dessa função.

```c
#include <stdio.h>

int somar_divisores(int num) {
    int somatorio = 0;
    for (int i = 1; i < num; i++) {
        if (num % i == 0) {
            somatorio += i;
        }
    }
    return somatorio;
}

int is_friend(int num1, int num2) {
    return 1;
    // return 0;
}

int main() {
    int num1, num2;
    scanf("%d %d", &num1, &num2);

    char number_is_friend = is_friend(num1, num2);
    if (number_is_friend == 1) {
        printf("S\n");
    } else {
        printf("N\n");
    }
}
```

Essa função vai retornar 1 se os números forem amigos, e 0 se os números não forem.

O próximo passo é validar a expressão `|D(A) - B| <= 2 e |D(B) - A| <= 2`

Onde `|x|` é o valor absoluto de `x`, para usar a função `abs()`, precisamos antes incluir a `stdlib.h`.

```c
#include <stdlib.h>
```

Agora:

```c
#include <stdio.h>
#include <stdlib.h>

int somar_divisores(int num) {
    int somatorio = 0;
    for (int i = 1; i < num; i++) {
        if (num % i == 0) {
            somatorio += i;
        }
    }
    return somatorio;
}

int is_friend(int num1, int num2) {
    int somaNum1 = somar_divisores(num1);
    int somaNum2 = somar_divisores(num2);
    if (abs(num1 - somaNum2) <= 2 && abs(num2 - somaNum1) <= 2) {
        return 1;
    } else {
        return 0;
    }
}

int main() {
    int num1, num2;
    scanf("%i %i", &num1, &num2);

    int number_is_friend = is_friend(num1, num2);
    if (number_is_friend == 1) {
        printf("S\n");
    } else {
        printf("N\n");
    }
}
```

Calculamos a soma dos divisores de cada um, e verificamos se a diferença é `<= 2`.

O código acima já funciona, mas tem algo muito feio, o uso de `1` para verdadeiro e `0` na função `is_friend` pode ser confundido em algum momento, porque são números e nós demos um significado especial para eles.

Poderíamos adicionar comentários para ilustrar, mas melhor que isso seria adicionar boleanos.

Em C sabemos que `0` é falso e o resto é verdadeiro, mesmo sabendo que funciona assim, seria legal poder escrever `true`, `false`, e `bool` ao invés de `int`.

Poderíamos fazer essas definições na mão, mas já tem pronto, é só incluir `"stdbool.h"`.

```c
#include <stdbool.h>
```

E mudar o que está escrito no código para

```c
bool is_friend(int num1, int num2) {
    int somaNum1 = somar_divisores(num1);
    int somaNum2 = somar_divisores(num2);
    if (abs(num1 - somaNum2) <= 2 && abs(num2 - somaNum1) <= 2) {
        return true;
    } else {
        return false;
    }
}

int main() {
    int num1, num2;
    scanf("%i %i", &num1, &num2);

    bool number_is_friend = is_friend(num1, num2);
    if (number_is_friend == true) {
        printf("S\n");
    } else {
        printf("N\n");
    }
}
```

Perceba que estamos retornando true para caso a condição do if seja true, e retornando false para caso a condição do if seja false, sendo assim (identidade), podemos só retornar a condição diretamente.


```c
bool is_friend(int num1, int num2) {
    int somaNum1 = somar_divisores(num1);
    int somaNum2 = somar_divisores(num2);
    return abs(num1 - somaNum2) <= 2 && abs(num2 - somaNum1) <= 2;
}
```

Outra coisa: `number_is_friend == true` retorna `true` caso `number_is_friend` seja `true`... mas se ele já é `true`, não precisamos comparar, é só tacar lá dentro!
```c
    if (number_is_friend == true) {
        printf("S\n");
    } else {
        printf("N\n");
    }
```

Vira:
```c
    if (number_is_friend) {
        printf("S\n");
    } else {
        printf("N\n");
    }
```

## Código final
```c
#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>

int somar_divisores(int num) {
    int somatorio = 0;
    for (int i = 1; i < num; i++) {
        if (num % i == 0) {
            somatorio += i;
        }
    }
    return somatorio;
}

bool is_friend(int num1, int num2) {
    int somaNum1 = somar_divisores(num1);
    int somaNum2 = somar_divisores(num2);
    return abs(num1 - somaNum2) <= 2 && abs(num2 - somaNum1) <= 2;
}

int main() {
    int num1, num2;
    scanf("%d %d", &num1, &num2);

    bool number_is_friend = is_friend(num1, num2);
    if (number_is_friend) {
        printf("S\n");
    } else {
        printf("N\n");
    }
}
```

## Extras:

Dá para trocar
```c
if (number_is_friend) {
    printf("S\n");
} else {
    printf("N\n");
}
```

Por isso, usando operador ternário:
```c
printf(number_is_friend ? "S\n" : "N\n");
```

---
#### [Voltar para a página da semana](README.md)
#### [Voltar para a página inicial](https://github.com/bti-ufrn/monitoria-itp)
