# RPG
Questão da semana 2, "RPG"
Monitor: João Marcos

## Enunciado
Alyson estava em uma tarde casual de RPG com colegas quando eles tiveram um problema: todos decidiram agir ao mesmo tempo para derrotar o dragão, salvar a princesa e conquistar o tesouro, e o Mestre da seção perdeu o controle do que estava acontecendo! Percebendo a confusão em que seu mestre se metera, Alyson decidiu pedir aos alunos de ITP para escreverem um programa que pudesse ajudar o mestre a entender o que havia acontecido!

Escreva um programa que receberá dois inteiros: o número de jogadas N e os pontos de vida (life, energia) V do dragão. A partir daí, seu programa deverá ler N linhas contendo as jogadas dos participantes na seguinte forma:

C A, onde C é a letra que identifica o jogador, e A é o valor do ataque que ele realizou.

Os ataques serão inteiros variando de 1 a 20.  O dragão é um ser feroz, resistente e ágil, então apenas ataques acima de 14 irão acertá-lo! Se o ataque atingir o dragão, o valor do dano associado ao jogador que acertou é deduzido dos pontos de vida V do dragão. Quando os pontos de vida do dragão forem igual ou menor a zero, o dragão é derrotado!

Os jogadores são:

Nome do Jogador | Letra identificadora | Dano | Personagem
--- | --- | --- | ---
Alyson | A | 10 | The Ice Giant
Isaac | I | 10 | The Good Carpenter
Rafaela | R | 6 | The Mathematician
Wellington | W | 8 | Anonymous
Salgado | S | 4 | Leaf Hands
Careca | C | 1 | The ITPist

## Exemplos de entrada e saída
Entrada    | Saída
---   | ---
3 3   | 1 2 3 4 5
1 3 5 | 2 3 4 5 6
3 5 7 | 3 4 5 6 7
5 7 9 | 4 5 6 7 8
      | 5 6 7 8 9
_      | _
2 4     | 1 1 2 2 3 3 4
1 2 3 4 | 1 1 2 2 3 3 4
2 3 4 5 | 2 2 3 3 4 4 5

## Interpretação do enunciado:
Podemos criar duas matrizes, a de entrada e a de saída, ou já ler os valores de entrada em uma matriz maior.

Vamos optar pela primeira opção para deixar o código mais claro, com duas matrizes primeiro fazemos a leitura e depois nos preocupamos com o preenchimento da outra.

Para o preenchimento da matriz de saída, podemos dividir em 4 etapas.
1. Copiamos os elementos originais.
2. Calculamos os valores que possuem dois adjacentes na horizontal.
3. Calculamos os valores que possuem dois adjacentes na vertical.
4. Com os valores do passo 2 e 3, calculamos o resto (usando somente adjacência horizontal e vertical).

Usando os números `1`, `2`, `3` e `4` para ilustrar as etapas:

Usando a entrada:
```
1 _ 1 _ 1
_ _ _ _ _
1 _ 1 _ 1
_ _ _ _ _
1 _ 1 _ 1
```

Adjacentes na horizontal:
```
1 2 1 2 1
_ _ _ _ _
1 2 1 2 1
_ _ _ _ _
1 2 1 2 1
```

Adjacentes na vertical:
```
1 2 1 2 1
3 _ 3 _ 3
1 2 1 2 1
3 _ 3 _ 3
1 2 1 2 1
```

Usando `2` e `3`, calcular o que restou:
```
1 2 1 2 1
3 4 3 4 3
1 2 1 2 1
3 4 3 4 3
1 2 1 2 1
```

OBS¹: Note que é possível trocar a ordem de `2` e `3`, pois ambos valores independem uns dos outros, mas do jeito que fizemos, `4` depende dos dois, então tem de vir no final.

OBS²: Essa é uma das maneiras de resolver que passa os casos de teste, porque os casos de teste foram feitos seguindo esse raciocínio. Mas existem infinitas soluções para reescalar matrizes, que seria similar a reescalar uma imagem (a wikipedia cita [alguns](https://en.wikipedia.org/wiki/Image_scaling)).

## Resolução
Antes de sequer começar a fazer o código, podemos copiar o snippet dado no enunciado:
```c
Matrix* doubleMatrix(Matrix *mat);
```

Se temos que usar a struct `Matrix`, então vamos declara-la, e preparar nosso código com funções para alocar e desalocar.
```c
#include <stdio.h>
#include <stdlib.h>

typedef struct {
    int height;
    int width;
    int** vals;
} Matrix;
```

`Matrix` vai possuir a matriz de inteiros e os valores de altura (_height_) e largura (_width_).

Estamos importando `stdlib.h` para usar `malloc()`, e depois `free()`, que será necessário pois precisamos criar matrizes de inteiros com tamanho variável, isso dentro de structs não é possível sem alocação dinâmica.

```c
Matrix* createMatrix(int height, int width) {
    Matrix* matrix = malloc(sizeof(Matrix));
    matrix->height = height;
    matrix->width = width;

    ...
}
```

Primeiros criamos `matrix` com `malloc`, agora temos que criar a matriz em si.

Não é possível criar duas dimensões logo de cara, precisamos ir de uma em uma, primeiro criando todas as linhas, isto é, um array de arrays, já que cada linha representa um array.

Se precisamos criar um array de arrays, precisamos de um array de ponteiros, isso explica porque usamos `int** vals;` na declaração da struct, se tratarmos o `int**` como `int*[]`, temos um array de ponteiros, para cada ponteiro dentro dele, que seria `int*`, podemos tratar como `int[]`, e é nesse ponto que podemos realmente inserir os elementos, no array de inteiros.

Então `int** vals` vai ser uma lista de ponteiros, podemos acessar ponteiros individualmente usando `vals[i]`, e após isso, para cada ponteiro teremos vários elementos, podemos acessar eles com `vals[i][j]`.

```c
Matrix* createMatrix(int height, int width) {
    Matrix* matrix = malloc(sizeof(Matrix));
    matrix->height = height;
    matrix->width = width;

    matrix->vals = malloc(sizeof(int*) * matrix->height);
    for (int i = 0; i < matrix->height; ++i) {
        matrix->vals[i] = malloc(sizeof(int[matrix->width]));
    }
    return matrix;
}
```

Feito, primeiro alocamos o array de ponteiros (`int**`) com ponteiros (`int*`) e a quantidade de linhas (altura/_height_).

Depois, para cada linha (`int*`) alocamos inteiros (`int`) usando a quantidade de colunas (largura/_width_).

Agora para desalocar, precisamos fazer o procedimento na ordem contrária, de fora para dentro (na analogia de uma torre, se quisermos construir um andar, precisamos construir primeiro a base e ir subindo, se quisermos destruir um andar, precisamos destruir o de cima, e depois ir descendo de 1 em 1).

```c
void destroyMatrix(Matrix* matrix) {
    for (int i = 0; i < matrix->height; ++i) {
        free(matrix->vals[i]);
    }
    free(matrix->vals);
    free(matrix);
}
```

Então, liberamos os recursos de cada ponteiro (`int*`) da nossa matriz de inteiros (`int**`), depois liberamos a nossa matriz de inteiros (`int**`), e após isso, a própria struct `Matrix` que contém as 3 variáveis declaradas e também foi alocada dinamicamente.

Agora vamos criar a `main` e preencher os valores de entrada, vamos também mostrar como está o código até então.

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct {
    int height;
    int width;
    int** vals;
} Matrix;

Matrix* createMatrix(int height, int width) {
    Matrix* matrix = malloc(sizeof(Matrix));
    matrix->height = height;
    matrix->width = width;

    matrix->vals = malloc(sizeof(int*) * matrix->height);
    for (int i = 0; i < matrix->height; ++i) {
        matrix->vals[i] = malloc(sizeof(int[matrix->width]));
    }
    return matrix;
}

void destroyMatrix(Matrix* matrix) {
    for (int i = 0; i < matrix->height; ++i) {
        free(matrix->vals[i]);
    }
    free(matrix->vals);
    free(matrix);
}

// Ainda temos que fazer essa função
Matrix* doubleMatrix(Matrix* mat);

int main() {
    int height, width;
    scanf("%d %d", &height, &width);

    Matrix* original = createMatrix(height, width);
    for (int i = 0; i < original->height; ++i) {
        for (int j = 0; j < original->width; ++j) {
            scanf("%d", &original->vals[i][j]);
        }
    }

    // ...
    // ...

    destroyMatrix(original);
}
```

Agora é só criar a função seguindo as 4 etapas que eu expliquei na **interpretação do enunciado**:
```c
Matrix* doubleMatrix(Matrix* mat) {
    // Com os tamanhos explicados no enunciado
    Matrix* new_matrix = createMatrix(mat->height * 2 - 1, mat->width * 2 - 1);

    // Etapa `1`, para cada elemento de `mat` copiar para `new_matrix`
    //
    // 1 _ 1 _ 1
    // _ _ _ _ _
    // 1 _ 1 _ 1
    // _ _ _ _ _
    // 1 _ 1 _ 1
    for (int i = 0; i < mat->height; ++i) {
        for (int j = 0; j < mat->width; ++j) {
            // "Os valores da matriz original vão para para uma posição na nova matriz cujos índices
            // são o dobro dos índices de suas posições originais"
            new_matrix->vals[i * 2][j * 2] = mat->vals[i][j];
        }
    }

    // Etapa `2`, adjacentes de `1` na horizontal
    //
    // 1 2 1 2 1
    // _ _ _ _ _
    // 1 2 1 2 1
    // _ _ _ _ _
    // 1 2 1 2 1
    for (int i = 0; i < new_matrix->height; i += 2) {
        for (int j = 1; j < new_matrix->width; j += 2) {
            new_matrix->vals[i][j] = new_matrix->vals[i][j - 1] + new_matrix->vals[i][j + 1];
            new_matrix->vals[i][j] /= 2;
        }
    }

    // Etapa `3`, adjacentes de `1` na vertical
    //
    // 1 2 1 2 1
    // 3 _ 3 _ 3
    // 1 2 1 2 1
    // 3 _ 3 _ 3
    // 1 2 1 2 1
    for (int i = 1; i < new_matrix->height; i += 2) {
        for (int j = 0; j < new_matrix->width; j += 2) {
            new_matrix->vals[i][j] = new_matrix->vals[i - 1][j] + new_matrix->vals[i + 1][j];
            new_matrix->vals[i][j] /= 2;
        }
    }

    // Etapa `4`, adjacentes de `2` e `3` na horizontal e vertical, respectivamente
    //
    // 1 2 1 2 1
    // 3 4 3 4 3
    // 1 2 1 2 1
    // 3 4 3 4 3
    // 1 2 1 2 1
    for (int i = 1; i < new_matrix->height; i += 2) {
        for (int j = 1; j < new_matrix->width; j += 2) {
            new_matrix->vals[i][j] = new_matrix->vals[i][j - 1] + new_matrix->vals[i][j + 1];
            new_matrix->vals[i][j] += new_matrix->vals[i - 1][j] + new_matrix->vals[i + 1][j];
            new_matrix->vals[i][j] /= 4;
        }
    }
    return new_matrix;
}
```

Agora precisamos chamar a função na main imprimir o resultado e liberar a memória.

## Código final:
```c
#include <stdio.h>
#include <stdlib.h>

typedef struct {
    int height;
    int width;
    int** vals;
} Matrix;

Matrix* createMatrix(int height, int width) {
    Matrix* matrix = malloc(sizeof(Matrix));
    matrix->height = height;
    matrix->width = width;

    matrix->vals = malloc(sizeof(int*) * matrix->height);
    for (int i = 0; i < matrix->height; ++i) {
        matrix->vals[i] = malloc(sizeof(int[matrix->width]));
    }
    return matrix;
}

void destroyMatrix(Matrix* matrix) {
    for (int i = 0; i < matrix->height; ++i) {
        free(matrix->vals[i]);
    }
    free(matrix->vals);
    free(matrix);
}

Matrix* doubleMatrix(Matrix* mat) {
    Matrix* new_matrix = createMatrix(mat->height * 2 - 1, mat->width * 2 - 1);

    // Etapa `1`, para cada elemento de `mat` copiar para `new_matrix`
    for (int i = 0; i < mat->height; ++i) {
        for (int j = 0; j < mat->width; ++j) {
            new_matrix->vals[i * 2][j * 2] = mat->vals[i][j];
        }
    }

    // Etapa `2`, adjacentes de `1` na horizontal
    for (int i = 0; i < new_matrix->height; i += 2) {
        for (int j = 1; j < new_matrix->width; j += 2) {
            new_matrix->vals[i][j] = new_matrix->vals[i][j - 1] + new_matrix->vals[i][j + 1];
            new_matrix->vals[i][j] /= 2;
        }
    }

    // Etapa `3`, adjacentes de `1` na vertical
    for (int i = 1; i < new_matrix->height; i += 2) {
        for (int j = 0; j < new_matrix->width; j += 2) {
            new_matrix->vals[i][j] = new_matrix->vals[i - 1][j] + new_matrix->vals[i + 1][j];
            new_matrix->vals[i][j] /= 2;
        }
    }

    // Etapa `4`, adjacentes de `2` e `3` na horizontal e vertical, respectivamente
    for (int i = 1; i < new_matrix->height; i += 2) {
        for (int j = 1; j < new_matrix->width; j += 2) {
            new_matrix->vals[i][j] = new_matrix->vals[i][j - 1] + new_matrix->vals[i][j + 1];
            new_matrix->vals[i][j] += new_matrix->vals[i - 1][j] + new_matrix->vals[i + 1][j];
            new_matrix->vals[i][j] /= 4;
        }
    }
    return new_matrix;
}

int main() {
    int height, width;
    scanf("%d %d", &height, &width);

    Matrix* original = createMatrix(height, width);
    for (int i = 0; i < original->height; ++i) {
        for (int j = 0; j < original->width; ++j) {
            scanf("%d", &original->vals[i][j]);
        }
    }

    Matrix* estendido = doubleMatrix(original);
    for (int i = 0; i < estendido->height; ++i) {
        for (int j = 0; j < estendido->width; ++j) {
            printf("%d ", estendido->vals[i][j]);
        }
        puts("");
    }

    destroyMatrix(original);
    destroyMatrix(estendido);
}
```
