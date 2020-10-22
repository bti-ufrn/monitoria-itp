# Cifra de César
Questão da semana 5, funções, solução do monitor João Marcos

## Enunciado
Júlio César usava um sistema de criptografia, agora conhecido como _Cifra de
César_, que trocava cada letra pelo equivalente em duas posições à esquerda no
alfabeto (por exemplo, **'C'** vira **'A'**, **'T'** vira **'R'**, etc.). Ao começo do alfabeto nós
voltamos para o fim, isto é **'A'** vira **'Y'**. Nós podemos, é claro, tentar trocar as letras
com quaisquer número de posições.

## Entrada
A entrada contém vários casos de teste. A primeira linha de entrada contém um
inteiro _N_ que indica a quantidade de casos de teste. Cada caso de teste é
composto por duas linhas. A primeira linha contém uma string com até _50_
caracteres maiúsculos _(**'A'**-**'Z'**)_, que é a sentença após ela ter sido codificada
através desta Cifra de César modificada. A segunda linha contém um número que
varia de _0_ a _25_ e que representa quantas posições cada letra foi deslocada para a
direita.

## Saída
Para cada caso de teste de entrada, imprima uma linha de saída com o texto
decodificado (transformado novamente para o texto original) conforme as regras
acima e o exemplo abaixo.

Atenção: você deverá utilizar obrigatoriamente funções para a resolução do
exercício.

Fonte: https://www.urionlinejudge.com.br/repository/UOJ_1253.html

## Exemplos de entrada e saída
| Entrada    | Saída      |
---------------------------
| 6          | TOPCODER   |
| VQREQFGT   | TOPCODER   |
| 2          | AXCHMA     |
| TOPCODER   | CAMOBAP    |
| 0          | HELLOWORLD |
| ZWBGLZ     |            |
| 25         |            |
| DBNPCBQ    |            |
| 1          |            |
| LIPPSASVPH |            |
| 4          |            |

## Interpretação do enunciado:
Primeiro vamos desmistificar a ambiguidade do enunciado, o que "mover para a esquerda/direita quer dizer?"

A cifra de César faz uma operação de codificação e decodificação reversível, no momento de codificar, as letras são avançadas _(**'a'** -> **'b'**)_, e na hora de decodificar, as letras "diminuem" _(**'b'** -> **'a'**)_.

Sendo assim, precisamos ler uma linha de texto, a quantidade de posições utilizadas para codificar, e então, chamar uma função para decodificar essa linha de texto

## Resolução
### Input
Vamos começar apontando alguns erros no momento de capturar a input desse programa input do programa:
```c
#include <stdio.h>

int main()
{
    // Quantidade de testes
    int teste;
    scanf("%d", &teste);

    // Onde vamos guardar a linha de texto atual
    char texto[50];
}
```

ERRO!

O código acima já está errado, vamos rever, o enunciado diz que o texto pode ter até 50 caracteres maiúsculos, então devemos colocar `char texto[50]`?

Não, uma string em `c` é um vetor de `char` que termina com o char `'\0'`, veja, as linhas abaixo são equivalentes:

```c
char texto1[] = "asd";
char texto2[4] = {'a', 's', 'd', '\0'};
char texto3[] = "asd";
char texto4[4] = {'a', 's', 'd', '\0'};
```

Embora `strlen(texto)` nesse caso mostre _3_, o tamanho para armazenamento do array é _4_, ou seja, precisamos mudar o tamanho para:

```c
char texto[51];
```

Continuando a captura da input:

```c
#include <stdio.h>

int main()
{
    int teste;  // Quantidade de testes
    scanf("%d", &teste);

    char texto[51];

    // Para cada teste, leia a linha e a quantidade de posições
    // Depois disso, chame a função
    for (int i = 0; i < teste; ++i) {
        scanf("%[^\n]", texto);
        int posicoes;
        scanf("%d", &posicoes);

        // cifra_de_cesar(...) funcao
    }
}
```

ERRO!

Dessa vez o problema está no uso de scanf no momento de receber a input, estou usando `scanf` para exemplificar, mas o mesmo erro ocorre se você usar `fgets` para receber `texto`. Como o `scanf` funciona? Precisamos entender isso, pois independentemente do uso de `fgets`, precisamos do `scanf` para ler os inteiros `%d`.

Acontece que quando queremos ler um valor numérico como `%d` ou `%f`, o `scanf` ignora espaços e quebras de linha (_'\n'_) que aparecem antes, isso porque essas entradas não são contabilizadas, é por esse motivo que podemos escrever:

```c
scanf("%d %d", &a, &b);
// ou
scanf("%d", &a);
scanf("%d", &b);
```

Porém, objetivamente o `scanf` consome o número inteiro e para no seu último dígito, ANTES do _'\n'_ que é a quebra de linha que está posterior, ou seja, ele deixa lá para quem vier depois ler ou ignorar.

Isso não é problema quando o próximo valor que vamos ler é um outro número, mas quando queremos ler uma linha, queremos ler até a quebra da linha, ou seja, até um _'\n'_, como expliquei acima, o `scanf` deixou um _'\n'_ na última vez que rodou, então vamos obter uma linha vazia.

Estamos obtendo uma linha vazia pois o `scanf("%[^\n]")` ou o `fgets` está esperando até um _'\n'_, porém, o primeiro caractere já é um _'\n'_, então essas funções pensam que já acabou.

Solução:

```c
#include <stdio.h>

int main()
{
    int teste;
    scanf("%d\n", &teste); // Necessário '\n'

    char texto[51];

    for (int i = 0; i < teste; ++i) {
        scanf("%[^\n]\n", texto); // Opcional '\n'
        int posicoes;
        scanf("%d\n", &posicoes); // Necessário '\n'

        // ...
    }
}
```

Ao adicionar um _'\n'_ no fim do texto do `scanf`, estamos dizendo que não só ele deve ler um inteiro, mas também deve ignorar um _'\n'_ para as próximas leituras que acontecerem no programa, ou seja, ele "pula" o caractere.

Leia os comentários, os `scanf`s que leem `teste` e `posicoes` ambos precisam de _'\n'_ no fim pois seguindo o fluxo do programa acima, é possível que a leitura da linha para `texto` ocorra depois desses dois `scanf`s, e isso (como acabei de explicar) vai causar erros de leitura de linha vazia.

Porém, o _'\n'_ na leitura de `texto` é opcional pois o `scanf` abaixo é para inteiros, e ignora o que vier antes de espaço e quebra de linha.

O comportamento padrão do `fgets` é consumir o _'\n'_ e colocar no fim do `texto`, nesse caso o tamanho deveria ser **[52]**, e não **[51]**.

### A função da cifra
Finalmente, agora que resolvemos todos os problemas de input para a entrada especificada na questão, vamos fazer uma função que decodifica o texto codificado.

```c
#include <string.h>

void cifra_de_cesar(char texto[], int posicoes)
{
    // Procura por '\0' e acha a quantidade de elementos
    int tamanho = strlen(texto);

    // Para cada elemento (letra)
    for (int i = 0; i < tamanho; ++i) {
        // Volte a letra algumas casas
        texto[i] -= posicoes;

        // Se a subtração passou do limite do alfabeto, adicione 26 para
        // fazer o loop começando em 'Z'
        //
        // Exemplo: 'A' - 1 + 26  ->  'Z'
        // Note que 'A' - 1 é menor que 'A', então entra no `if`
        if (texto[i] < 'A') {
            texto[i] += 26;
        }
    }
}
```

Para entender a manipulação do valor inteiro dos `char`s, vamos ver a tabela _ascii_.

![tabela-ascii](https://i.pinimg.com/originals/75/28/b1/7528b199208cc9078adfa6830be7f072.jpg)

Relembremos que `char` nada mais é do que um inteiro de 1 byte (8 bits) que vai até 256, e é mapeado até 128 na tabela _ascii_ com um caractere. Porém, é só um mapeamento, `char` continua sendo somente um número inteiro.

Veja na imagem o valor da coluna `Dec` (decimal) e compare com o `Chr` mapeado em _ascii_.

O char _'A'_ significa _65_, então podemos realizar a operação _'A'_ + _25_, isto é igual a _90_ ou _'Z'_, como preferir chamar, significa que quando nós avançamos _'A'_ em _25_ letras, obtemos _'Z'_, a última letra do alfabeto.

Então o que estamos fazendo no nosso código é diminuindo a quantidade de casas definidas na entrada que vai de 0 até 25, e depois vemos se está abaixo de _'A'_, se estiver, remapeamos para o fim do alfabeto com `+ 26`, seguindo a regra de decodificação especificada no enunciado pois o alfabeto possue 26 letras.

### Juntando o código
Agora só precisamos adicionar um `printf` para mostrar a output e pronto.

```c
#include <stdio.h>
#include <string.h>

void cifra_de_cesar(char texto[], int posicoes)
{
    int tamanho = strlen(texto);

    for (int i = 0; i < tamanho; ++i) {
        texto[i] -= posicoes;

        if (texto[i] < 'A') {
            texto[i] += 26;
        }
    }
}

int main()
{
    int teste;
    scanf("%d\n", &teste);

    char texto[51];

    for (int _ = 0; _ < teste; ++_) {
        scanf("%[^\n]\n", texto);
        int posicoes;
        scanf("%d\n", &posicoes);

        cifra_de_cesar(texto, posicoes); // Altera texto
        printf("%s\n", texto); // Mostra texto
    }
}
```

## Notas extras:
#### 1
As assinaturas de função abaixo são equivalentes:
```c
void cifra_de_cesar(char texto[], int posicoes) { ... } // Ponteiro implícito, array de tamanho desconhecido
void cifra_de_cesar(char texto[51], int posicoes) { ... } // Array de tamanho desconhecido disfarçado
void cifra_de_cesar(char texto[55], int posicoes) { ... } // Array de tamanho desconhecido disfarçado
void cifra_de_cesar(char texto[56], int posicoes) { ... } // Array de tamanho desconhecido disfarçado
void cifra_de_cesar(char *texto, int posicoes) { ... } // Ponteiro explícito
```

Em C não há nenhuma checagem se o `array` passado tem algum tamanho específico, ou seja, se escrevermos `texto[51]` na assinatura de uma função, estamos fazendo só pela estética (para ficar bonitinho), na prática essa função NÃO SABE o tamanho que alocamos para essa variável, como é uma `string`, usamos `strlen` para obter, senão teríamos de passar como um argumento da função.

#### 2
_'\n'_ é uma quebra de linha em sistemas `unix`, no `windows` é _"\r\n"_.

#### 3
Podemos reescrever o `for` assim:
```c
for (int _ = 0; _ < teste; ++_) {
    scanf("%[^\n]\n", texto);
    int posicoes;
    scanf("%d\n", &posicoes);

    cifra_de_cesar(texto, posicoes);
    printf("%s\n", texto);
}
```

Trocando `i` por `_` (underline), é uma prática comum de programação colocar um `_` (underline) para explicitar que você não está usando essa variável dentro do seu loop, ela só está sendo utilizada pelo `for` para rodar **N** vezes, nesse caso, `teste` vezes, isso torna o seu código mais fácil de ler adicionando menos nomes de variáveis que aumentam a complexidade do seu programa.

#### 4
Percebam que eu coloquei uma declaração dentro de um loop:
```c
int posicoes;
```
Isso não é sempre possível em `C99`, não em loops, somente em outros escopos como funções e `if`s, porém, é EXTREMAMENTE recomendável que você não declare todas as variáveis do seu programa no topo da função, isso porque fica muito ambíguo qual é o uso de cada uma delas, é EXTREMAMENTE² recomendado que suas variáveis tenham o _mínimo escopo possível_.

---

Para qualquer dúvidas, questionamentos, ou correções, falar com João Marcos Bezerra no Discord.

### Voltar para a página inicial
[Clique aqui](README.md).
