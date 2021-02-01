# Inserir String Em Uma Posição
Questão da semana 10, "Inserir string em uma posição", solução do monitor João Marcos

## Enunciado
Seja a struct

```c
typedef struct string_t {
    char val[101];
} string;
```
Implemente a função

```c
void insereStrEm(string *result, string *srt1, int pos, string *str2)
```
Essa função deverá inserir a string str1 na posição pos da string str2. O resultado deve ser preenchido na string result, recebida como argumento.

Escreva um programa que utilize a função descrita acima. Observe que não é adicionado um espaço em branco após inserir a string.

## Exemplos de entrada e saída
Entrada    | Saída
--- | ---
muito legal   | muito legalITP
ITP           |
11            |
_             | _
muito legal   | ITP nãomuito legal
ITP não       |
0             |

## Interpretação do enunciado:
A partir da assinatura da função e struct, percebemos que temos de escrever o resultado da operação para `result`.

Se precisamos de um resultado com a string `str2` no meio de uma string `str1`, baseado na posição `pos` (ver função do enunciado), então podemos copiar todo o conteúdo de `str1` até antes da posição `pos`, depois copiamos todo o conteúdo de `str2`, e então terminamos de copiar o resto de `str1`, dessa maneira teremos, de certo modo, inserido `str2` dentro de `str1` na posição `pos`.

A string de `result` vai se parecer com isso:
```
str1[0..pos] + str2 + str1[pos..]
```

## Resolução
Primeiro vamos copiar o código da função e struct dadas no enunciado, então vamos criar nossa função `main`, fazer o input, chamar a função e mostrar o resultado.

Depois vamos nos preocupar em implementar a função corretamente.

```c
#include <stdio.h>

typedef struct string_t {
    char val[101];
} string;

void insereStrEm(string *result, string *str1, int pos, string *str2) {
}

int main() {
    string str1;
    string str2;
    scanf("%[^\n]%*c", str1.val);
    scanf("%[^\n]%*c", str2.val);

    int insert_position;
    scanf("%d", &insert_position);

    string result;
    insereStrEm(&result, &str1, insert_position, &str2);
    // printf("%s\n", result.val);
}
```

Não vamos imprimir `result.val` ainda, porque esse pedaço de memória não foi inicializado, então o `printf` pode nunca achar o `'\0'`, o que significa que ele vai rodar demais e causar um segmentation fault.

Um truque "novo" usado no `scanf` foi o `%*c`, que é similar ao `%c`, mas usando o asterisco, o asterisco depois do `%` em um `scanf` significa que esse campo será ignorado ao invés de lido para uma variável, com isso estamos lendo as duas primeiras linhas.

Note essas duas linhas em específico:
```c
scanf("%d", &insert_position);
insereStrEm(&result, &str1, insert_position, &str2);
```

Perceba o uso do `&` em ambas, na primeira para o `scanf`, que é uma função que recebe ponteiros para editar os valores, como `insert_position` não é um ponteiro, podemos usar `&` para obter seu endereço, agora o `scanf` funciona, esse é um ótimo truque para entender quando ou não usar um `&`, você tem que saber o que sua função ou variável quer receber, então você checa o tipo da variável, se já for um ponteiro, é só passar, senão, usa o `&`.

No caso da linha com `insereStrEm`, precisamos passar 3 ponteiros de `string`, que é nossa struct, então usamos a mesma lógica de obter os ponteiros com o operador de endereçamento `&`, caso `result`, `str1` e `str2` já fossem ponteiros, o que é comum quando usamos alocação dinâmica (próximas aulas), não precisaríamos adicionar o `&` antes! Se a função recebe um ponteiro, é só passar ele.

Agora para a implementação da função, vamos inicializar o bloco `result->val` e depois preencher-lo com os pedaços das strings que explicamos no começo do texto:

```c
void insereStrEm(string *result, string *str1, int pos, string *str2) {
    // Preenchendo memória ainda não inicializada
    for (int i = 0; i < 101; ++i) {
        result->val[i] = '\0';
    }

    int tamanho_str1 = strlen(str1->val);
    int tamanho_str2 = strlen(str2->val);

    // Copiando primeira metade de str1
    for (int i = 0; i < pos; ++i) {
        result->val[i] = str1->val[i];
    }

    // Copiando str2 inteira nas próximas posições
    for (int i = 0; i < tamanho_str2; ++i) {
        result->val[i + pos] = str2->val[i];
    }

    // Copiando o resto de str1 nas próximas posições
    // O resto seria a segunda metade de str1, a partir de [pos]
    for (int i = 0; i < tamanho_str1 - pos; ++i) {
        result->val[pos + tamanho_str2 + i] = str1->val[i + pos];
    }
}
```

Código final:
```c
#include <stdio.h>
#include <string.h>

typedef struct string_t {
    char val[101];
} string;

void insereStrEm(string *result, string *str1, int pos, string *str2) {
    for (int i = 0; i < 101; ++i) {
        result->val[i] = '\0';
    }

    int tamanho_str1 = strlen(str1->val);
    int tamanho_str2 = strlen(str2->val);

    for (int i = 0; i < pos; ++i) {
        result->val[i] = str1->val[i];
    }

    for (int i = 0; i < tamanho_str2; ++i) {
        result->val[i + pos] = str2->val[i];
    }

    for (int i = 0; i < tamanho_str1 - pos; ++i) {
        result->val[pos + tamanho_str2 + i] = str1->val[i + pos];
    }
}

int main() {
    string str1;
    string str2;
    scanf("%[^\n]%*c", str1.val);
    scanf("%[^\n]%*c", str2.val);

    int insert_position;
    scanf("%d", &insert_position);

    string result;
    insereStrEm(&result, &str1, insert_position, &str2);
    printf("%s\n", result.val);
}
```

## Notas extras:
#### 1
Em C as strings terminam com `'\0'`, então poderíamos ter escrito:
```c
result->val[tamanho_str1 + tamanho_str2] = '\0';
```
Para deixar a string imprimível com o `printf`, e manipulável no geral, mas ao invés disso, nesse código optamos por preencher tudo com `'\0'`, então temos a garantia que o que sobrar depois da manipulação está corretamente terminado com `'\0'`. São duas maneiras diferentes de atingir o mesmo objetivo.

#### 2
Quando quiser acessar o valor de uma struct:
```c
estrutura.valor;
```

Se estrutura for um ponteiro para a struct:
```c
(*estrutura).valor;
```

Mas o jeito de escrever acima é horrível para quando temos ponteiros de ponteiros, ficaria assim:
```c
(*(*(*primeira).segunda).terceira).valor;
```

Então podemos trocar as duas linhas acima usando o operador `->`!
```c
estrutura->valor;
primeira->segunda->terceira->valor;
```

Bem melhor, caso esteja confuso ainda, aqui vai uma regra de dedão boba (rule of thumb):
- Se for struct, use o ponto (`.`) para acessar os membros internos.
- Se for ponteiro para struct, troque o ponto (`.`) por uma setinha (`->`).

Usamos a setinha e o ponto no código da solução! reveja para revisar esses usos.

---
#### [Voltar para a página da semana](README.md)
#### [Voltar para a página inicial](https://github.com/bti-ufrn/monitoria-itp)
