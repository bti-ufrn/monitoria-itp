# Prova de Funções: Teorema de Pitágoras

Esta questão envolve conhecimentos sobre triângulo retângulo, mdc, primos entre si e um pouco de ordenação.

A ideia da questão é receber uma tripla de inteiros, descobrir se estes podem formar um triângulo retângulo. Caso possam, verificar se a tripla é primitiva (os três números são primos entre si) ou são apenas uma tripla pitagórica (tripla múltipla de uma tripla pitagórica primitiva). Caso a tripla não forme um triângulo retângulo apenas informar que é uma tripla qualquer.

Por exemplo:
> (3,4,5)

É uma tripla pitagórica primitiva, pois formam um triângulo retângulo `5²=3²+4²` e são primos entre si `mdc(3,4,5)=1`.
> (6,8,10)

É uma tripla pitagórica, pois formam um triângulo retângulo `10²=6²+8²`. Mas não são primos entre si, pois `mdc(6,8,10)=2`.
> (2,7,9)

Neste caso é apenas uma tripla, pois não formam um triângulo retângulo retângulo `9²≠2²+7²`.

Vamos criar quatro funções para resolver este problema, são elas:
```c
void ordena(int *vetor);
int mdc(int a, int b);
int eTriangulo(int *vetor); // é triângulo
int primosEntreSi(int *vetor);
```
À medida que formos resolvendo a questão elas iram sendo explicadas. Bom a idéia do algoritmo é a seguite
> 1º Recebo os três valores e os armazeno em um vetor.
> 2º Ordeno este vetor de forma crescente, pois preciso saber quem é o maior valor para o calculo do mdc e teorema de pitagóras.
> 3º Verifico se a tripla forma um triângulo retângulo.
> 4º Verifico se são primos entre si.
> 5º Com as informações dos passos 3 e 4 mostro minha mensagem.

Com base no algoritmo mostrado, nosso código tera mais ou menos essa forma abaixo. Sendo que apenas as assinaturas das funções estão expostas, faltam as implentações que vamos explicar a seguir e inserir no código.

```c
#include <math.h>
#include <stdio.h>
// Apenas as assinaturas
void ordena(int *vetor);
int mdc(int a, int b);
int eTriangulo(int *vetor);
int primosEntreSi(int *vetor);

int main() {
    int tripla[3];
    // Recebendo a tripla
    for (int i = 0; i < 3; i++) {
      scanf("%d", &tripla[i]);
    }

    // Ordenando a tripla para saber qual o maior valor
    ordena(tripla);
    // Descobrindo se é um triângulo retângulo
    int triangulo = eTriangulo(tripla),
    // Descobrindo se são primos entre si
    primos = primosEntreSi(tripla);

    // Retorno 1=Verdadeiro se for, caso contrário 0=Falso
    if (triangulo) {
        // Retorno 1 caso forem primos entre si
        if (primos == 1) {
          printf("tripla pitagorica primitiva\n");
        } else {
          printf("tripla pitagorica\n");
        }
    } else {
      printf("tripla\n");
    }

    return 0;
}
```

A função `void ordena(int *vetor);` Tem o seguinte corpo:
```c
void ordena(int *vetor) {
    int aux;

    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            if (vetor[i] < vetor[j]) {
                aux = vetor[i];
                vetor[i] = vetor[j];
                vetor[j] = aux;
            }
        }
    }
}
```

Ela ordena o vetor usando o algoritmo [Bubble Sort](https://pt.wikipedia.org/wiki/Bubble_sort). Esse algoritmo de ordenação compara todos com todos, logo ele executa **n²** vezes sendo **n** o tamanho do vetor. A ideia é fixar uma posição a cada vez no primeiro _for_ e compará-la com todas as posições do mesmo vetor no segundo _for_. Caso sua posição fixa no primeiro _for_ é maior que a do segundo, trocam de lugar. Perceba que com isto você está _movendo_ os maiores numeros para a direita e os menores para esquerda.
Vamos a um exemplo, considere um `vetor = {4,3,5}`:
> (_**4**_,3,5) 4<4? Não
> (**4**,_3_,5) 4<3? Não
> (**4**,3,_5_) 4<5? Sim --> (**5**,3,4)
> (_5_,**3**,4) 3<5? Sim -> (3,**5**,4)
> (3,_**5**_,4) 5<5? Não
> (3,**5**,_4_) 5<4? Não
> (_3_,5,**4**) 4<3? Não
> (3,5,**4**) 4<5? Sim --> (3,4,**5**)
> (3,4,_**5**_) 5<5? Não

A função `int mdc(int a, int b);` tem o seguite corpo:
```c
int mdc(int a, int b) {
    int aux;
    while(b != 0){
        aux = a;
        a = b;
        b = aux % b;
    }
    return a;
}
```

Ela utiliza do [algoritmo de euclides](https://pt.wikipedia.org/wiki/Algoritmo_de_Euclides#Defini%C3%A7%C3%A3o_do_algoritmo) para calcular o `mdc(a,b)`. Esta é a forma iterativa e não completa, pois não lida com casos em que `a,b` possam ser zero (mas não temos esse caso).

A função `eTriangulo(int *vetor);`tem o seguite corpo:
```c
int eTriangulo(int *vetor) {
    double h = pow(vetor[2], 2);
    double c = pow(vetor[0], 2) + pow(vetor[1], 2);
    return h == c ? 1 : 0;
}
```

Aqui utilizamos o [teorema de Pitagóras](https://pt.wikipedia.org/wiki/Teorema_de_Pit%C3%A1goras) para verificar se a tripla recebida é capaz de formar um triângulo retângulo. Tendo em vista que nosso vetor esta ordenado de maneira crescente, logo a posição `vetor[2]` é o maior valor e nossa hipotenusa. Armazeno o valor de `hipotenusa²` em `h` e a soma do quadrado dos catetos em `c`. E por fim com o operador ternário caso `h==c` retorno 1=Verdadeiro, caso contrário 0=Falso. Lembrando que estamos utilizando a função `pow(a,b)` que eleva a, b vezes. Logo preciso incluir a biblioteca `math.h` e na compilação a flag `-lm`.

A função `int primosEntreSi(int *vetor);` tem o seguite corpo:
```c
int primosEntreSi(int *vetor){
    int resultado;

    resultado = mdc(vetor[2], vetor[1]);
    resultado = mdc(vetor[0], resultado);

    return resultado;
}
```

Bom, aqui basicamente pegamos o resultado do mdc entre o maior e segundo menor número, usando esse resultado fazemos o mdc dele com o menor número na posição `0` do vetor. Depois da segunda chamada da função mdc temos na variável `resultado` o mdc entre os três números. Se for `1` são primos entre si, caso contrário não são.

Bom ao fim nosso código ficaria assim, encerrando a questão
```c
#include <math.h>
#include <stdio.h>
// Apenas as assinaturas
void ordena(int *vetor);
int mdc(int a, int b);
int eTriangulo(int *vetor);
int primosEntreSi(int *vetor);

int main() {
    int tripla[3];
    // Recebendo a tripla
    for (int i = 0; i < 3; i++) {
        scanf("%d", &tripla[i]);
    }

    // Ordenando a tripla para saber qual o maior valor
    ordena(tripla);
    // Descobrindo se é um triângulo retângulo
    int triangulo = eTriangulo(tripla),
    // Descobrindo se são primos entre si
    primos = primosEntreSi(tripla);

    // Retorno 1=Verdadeiro se for, caso contrário 0=Falso
    if (triangulo) {
        // Retorno 1 caso forem primos entre si
        if (primos == 1) {
            printf("tripla pitagorica primitiva\n");
        } else {
            printf("tripla pitagorica\n");
        }
    } else {
        printf("tripla\n");
    }

    return 0;
}

void ordena(int *vetor) {
    int aux;

    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            if (vetor[i] < vetor[j]) {
                aux = vetor[i];
                vetor[i] = vetor[j];
                vetor[j] = aux;
            }
        }
    }
}

int mdc(int a, int b) {
    int aux;
    while (b != 0) {
        aux = a;
        a = b;
        b = aux % b;
    }
    return a;
}

int eTriangulo(int *vetor) {
    double h = pow(vetor[2], 2);
    double c = pow(vetor[0], 2) + pow(vetor[1], 2);
    return h == c ? 1 : 0;
}

int primosEntreSi(int *vetor) {
    int resultado;

    resultado = mdc(vetor[2], vetor[1]);
    resultado = mdc(vetor[0], resultado);

    return resultado;
}
```

### Voltar para a página inicial
[Clique aqui](README.md).
