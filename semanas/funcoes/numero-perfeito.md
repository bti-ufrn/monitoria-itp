# Monitoria ITP UFRN 2020.6

## Semana 5 - Funções

## Resolução da questão da prova do LOP "Funções - número perfeito"

### Agradecimentos

- Professor André Maurício Cunha Campos (Docente Coordenador)
- Professor Eduardo Henrique da Silva Aranha (Docente Orientador)
- Professor Emerson Moura de Alencar (Docente Orientador)
- Iury Lamonie Fernandes do Nascimento (Monitor Bolsista)
- João Marcos Pereira Bezerra (Monitor Bolsista)
- Marcio Tenório Júnior (Monitor Bolsista)
- Paulo Felipe Costa de Pontes (Monitor Bolsista e autor desse texto)

### Enunciado da questão

Em Matemática, um número perfeito é um número natural para o qual a soma de todos os seus divisores naturais próprio (excluindo ele mesmo) é igual ao próprio número. Por exemplo, o número 28 é , pois: 28=1+2+4+7+14 (<https://pt.wikipedia.org/wiki/N%C3%BAmero_perfeito>)

Implemente uma função que verifica se um número é perfeito. Se o número for perfeito, você deverá imprimir uma mensagem com os seus divisores. Caso não seja perfeito, você deverá imprimir a mensagem "X nao eh perfeito" informando que o número não é perfeito.

### Exemplos de entrada e saída

| Exemplo de entrada | Exemplo de saída                               |
| ------------------ | ---------------------------------------------- |
| 6                  | 6 = 1 + 2 + 3                                  |
| 496                | 496 = 1 + 2 + 4 + 8 + 16 + 31 + 62 + 124 + 248 |

### O que o enunciado pede e o que deve ser feito?

É dado um número e o seu programa deve verificar se esse número é perfeito. Se for perfeito então seu programa deve imprimir o número seguido do sinal de igualdade "=" e da soma de seus divisores (sem contar o próprio número e separando os divisores pelo sinal de adição "+"). Se não for perfeito então seu programa deve imprimir que o número não é perfeito.

### Como resolver o problema?

Inicialmente deve ser feita a leitura dos dados:

1. No topo do seu programa inclua o cabeçalho padrão da linguagem C com as funções de entrada e saída. Exemplo: `#include <stdio.h>`
2. Declare a rotina principal `main`. Exemplo: `int main(void) {}`
3. Dentro da função `main` declare uma variável do tipo inteiro para armazenar o número inserido na entrada. Exemplo: `int x;`
4. Faça a leitura do número com a função `scanf` e armazene o valor na variável que foi criada para isso. Exemplo: `scanf("%d", &x);`

Nesse momento seu programa deve estar parecido com esse exemplo:

```c
#include <stdio.h> // Passo 1

int main(void) { // Passo 2
    int x; // Passo 3
    scanf("%d", &x); // Passo 4
    return 0;
}
```

Agora que a leitura dos dados foi realizada com sucesso é necessário fazer o processamento para poder imprimir o resultado correto. Para isso será necessário criar uma função para verificar se o número é perfeito e um procedimento (função sem retorno) para imprimir os divisores desse número (excluindo o próprio número).

Implementação da função que verifica se o número é perfeito:

1. Antes da função `main` e depois da inclusão dos cabeçalhos declare uma função que retorne um tipo inteiro e tenha como parâmetro de entrada uma variável de tipo inteiro. O valor retornado vai ser sempre 0 ou 1, representando falso ou verdadeiro, respectivamente. Exemplo: `int ehPerfeito(int x) {}`
2. Dentro da função declarada crie uma variável de tipo inteiro para armazenar a soma dos divisores e inicialize ela como zero (inicialmente estamos supondo que o número não tem divisores, por isso a soma é inicializada como zero). Exemplo: `int soma = 0;`
3. Crie um laço `for` para encontrar os divisores do número, declare uma variável auxiliar dentro desse laço e inicialize ela como 1. A variável auxiliar vai ser incrementada até ser igual ao número (se o número for 1 então o laço nunca vai rodar e a soma dos divisores continuará sendo 0 embora isso não seja verdade porque o número 1 tem um único divisor que é ele mesmo, isso é feito de propósito porque o número 1 não é perfeito e nós não queremos contar ele como perfeito). Exemplo: `for (int i = 1; i < x; i++) {}`
4. Dentro do laço `for`, crie um `if` para verificar se a variável auxiliar é divisora do número. Exemplo: `if (x % i == 0) {}`
5. Dentro do `if`, incremente a variável da soma dos divisores com o valor da variável auxiliar. Exemplo: `soma += i;`
6. Ao final da execução do laço todos os divisores do número (sem contar o próprio número) foram somados e agora nós podemos verificar se o número é perfeito ou não. Portanto, para fazer essa verificação, crie um `if` após o laço `for` que verifica se o número é igual à soma de seus divisores (sem contar o próprio número). Exemplo: `if (soma == x) {}`
7. Dentro do `if` retorne o número 1 (que representa verdadeiro). Exemplo: `return 1;`
8. Após o `if` crie um `else` para tratar o caso em que o número não é perfeito. Exemplo: `if (...) {...} else {}`
9. Dentro do `else` retorne zero (que representa falso). Exemplo: `return 0;`

Nesse momento a função deve estar parecida com esse exemplo:

```c
int ehPerfeito(int x) { // Passo 1
    int soma = 0; // Passo 2
    for (int i = 1; i < x; i++) { // Passo 3
        if (x % i == 0) { // Passo 4
            soma += i; // Passo 5
        }
    }
    if (soma == x) { // Passo 6
        return 1; // Passo 7
    } else { // Passo 8
        return 0; // Passo 9
    }
}
```

Implementação do procedimento que imprime os divisores de um número (exceto o próprio número):

1. Antes da função `main` declare um procedimento (função sem retorno) que tenha como parâmetro de entrada uma variável de tipo inteiro. Exemplo: `void imprimirDivisores(int x) {}`
2. Dentro do procedimento crie duas variáveis de tipo inteiro. Inicialize a primeira variável como o número inteiro passado como parâmetro e a segunda variável como 0. A primeira variável vai armazenar o maior divisor do número (sem ser ele mesmo) para ser impresso no final. A segunda variável vai sempre armazenar 0 ou 1 (falso ou verdadeiro, respectivamente) para representar se o menor divisor do número maior que 1 já foi encontrado. Exemplo: `int maximo = x, minimo = 0;`
3. Imprima o número passado como parâmetro de acordo com a formatação da saída esperada. Exemplo: `printf("%d = ", x);`
4. Crie um laço `for` para buscar os divisores. Dentro do laço declare uma variável auxiliar e inicialize ela como 1. Incremente essa variável até ela ser igual à variável com o maior divisor do número. Exemplo: `for (int i = 1; i < maximo; i++) {}`
5. Dentro do laço crie um `if` para verificar se a variável auxiliar é divisor do número passado como parâmetro do procedimento. Exemplo: `if (x % i == 0) {}`
6. Dentro do `if` imprima a variável auxiliar seguindo a formatação do exemplo de saída esperada. Exemplo: `printf("%d + ", i);`
7. Após a impressão da variável auxiliar crie outro `if` para verificar se o menor divisor do número maior que 1 ainda não foi encontrado e se a variável auxiliar é maior que 1. Exemplo: `if (!minimo && i > 1) {}`
8. Dentro do `if` atualize o valor da variável que verifica se o menor divisor maior que 1 já foi encontrado para 1. Exemplo: `minimo = 1;`
9. Em seguida, atualize o valor da variável que armazena o maior divisor do número para o quociente do número pela variável auxiliar. Exemplo: `maximo = x / i;`
10. Para finalizar, imprima fora do laço a variável com o maior divisor do número. Exemplo: `printf("%d", maximo);`

Nesse momento o procedimento deve estar parecido com esse exemplo:

```c
void imprimirDivisores(int x) { // Passo 1
    int maximo = x, minimo = 0; // Passo 2
    printf("%d = ", x); // Passo 3
    for (int i = 1; i < maximo; i++) { // Passo 4
        if (x % i == 0) { // Passo 5
            printf("%d + ", i); // Passo 6
            if (!minimo && i > 1) { // Passo 7
                minimo = 1; // Passo 8
                maximo = x / i; // Passo 9
            }
        }
    }
    printf("%d", maximo); // Passo 10
}
```

Agora que as funções foram implementadas, basta encaixar os blocos que faltam dentro da rotina principal main:

1. Após o `scanf`, crie um `if` e dentro do parênteses chame a função que verifica se o número é perfeito passando como parâmetro da função o número lido da entrada. Exemplo: `if (ehPerfeito(x)) {}`
2. Dentro do `if` chame a função que imprime os divisores do número passando como parâmetro da função o número lido da entrada. Exemplo: `imprimirDivisores(x);`
3. Após o `if` crie um `else` para tratar o caso em que o número não é perfeito. Exemplo: `if (...) {...} else {}`
4. Dentro do `else` imprima a mensagem afirmando que o número não é perfeito. Exemplo: `printf("%d nao eh perfeito", x);`

Nesse momento sua função `main` deve estar parecida com esse exemplo:

```c
int main(void) {
    int x;
    scanf("%d", &x);
    if (ehPerfeito(x)) { // Passo 1
        imprimirDivisores(x); // Passo 2
    } else { // Passo 3
        printf("%d nao eh perfeito", x); // Passo 4
    }
    return 0;
}
```

Agora o problema foi resolvido com sucesso. Parabéns!

### Código-fonte completo da solução

```c
#include <stdio.h>

int ehPerfeito(int x) {
    int soma = 0;
    for (int i = 1; i < x; i++) {
        if (x % i == 0) {
            soma += i;
        }
    }
    if (soma == x) {
        return 1;
    } else {
        return 0;
    }
}

void imprimirDivisores(int x) {
    int maximo = x, minimo = 0;
    printf("%d = ", x);
    for (int i = 1; i < maximo; i++) {
        if (x % i == 0) {
            printf("%d + ", i);
            if (!minimo && i > 1) {
                minimo = 1;
                maximo = x / i;
            }
        }
    }
    printf("%d", maximo);
}

int main(void) {
    int x;
    scanf("%d", &x);
    if (ehPerfeito(x)) {
        imprimirDivisores(x);
    } else {
        printf("%d nao eh perfeito", x);
    }
    return 0;
}
```

### Voltar para a página inicial
[Clique aqui](README.md).
