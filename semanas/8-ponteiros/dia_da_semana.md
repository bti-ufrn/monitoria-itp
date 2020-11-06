## Resolução: Prova Ponteiros - Dia da Semana

A questão quer que você desenvolva um programa que recebe uma data como entrada (sendo string) no formato "dia/mes/ano", então devemos retornar que dia da semana aquela data corresponde e se o dia ocorreu em um ano bissexto.
O enunciado nos fornece a seguinte estrutura:
```c
#include <stdio.h>
#include <string.h>

int diadasemana(/* seu código aqui */) {
    // seu código aqui
}

int main() {
    int status = 0, bissexto = 0;
    char entrada[11] = {0};
    char *saida = NULL;
    fgets(entrada, 11, stdin);

    status = diadasemana(/* seu código aqui */);

    if (status > 0) {
        printf("%s.%s\n", saida, bissexto ? " Ano bissexto." : "");
    } else {
        if (status == -1)
            printf("Data inexistente.\n");
        else
            printf("Entrada invalida.\n");
        return 1;
    }

    return 0;
}
```

Nossa tarefa é implementar a função `int diadasemana()` para tratar as entradas das datas. Temos como parâmetros obrigatórios: a string com a data, uma referência para a resposta do dia da semana e uma outra referência com a flag de ano bissexto (0 para ano não-bissexto e 1 para ano bissexto). Já o retorno são os três casos possíveis:
> 1 --> Sucesso
> -1 --> Falha: data inexistente
> -2 --> Falha: entrada inválida.

Bom, vamos implementar! Nossa função ficara com a seguite assinatura
```c
int diadasemana(char data[11], char **saida, int *bissexto)
```

Jajá vamos entender o por que de `char **saida`, calma.
Bom, precisamos validar nossa entrada
```c
if (strstr(data, "/") == NULL || strlen(data) != 10) {
    return -2;
}
```
nesse trecho avaliamos se a formatação está correta e se a string tem o tamanho correto. Usamos a função `strstr()` passando como argumento as strings `data` e  `/`. A função retorna o endereço da primeira ocorrência de `/` na string `data`, caso não ache retorna `NULL` e nossa outra condição é ver se a string tem o tamanho 10, pois por exemplo teremos algo como: "06/01/1996", então usamos `strlen(data)` e verificamos se é diferente de 10. Caso alguma dessas condições sejam satisfeitas retornaremos -2, indicando como entrada inválida.

 A seguir vamos extrair o dia, mês e ano da string data
```c
sscanf(data, "%d/%d/%d", &d, &m, &y);
```
com a função `sscanf()` podemos ler dados de uma string com uma entrada formatada. Logo passo a string `data`, o formato e as variáveis que irei guardar os dados (d=dia, m=mes, y=ano).

Continuando, agora vamos validar a data
```c
if (y < 1900 || y > 9999)
    return -1;

if (m < 1 || m > 12)
    return -1;

if (d < 1 || d > 31)
    return -1;
```
no primeiro if garanto que o intervalo do ano é entre 1900 e 9999. No segundo if garanto não ter mês maior que 12 e menor que 1 e no terceiro garanto não ter dia menor que 1 e maior que 31. Caso entre em algum dos ifs retorno -1 indicando data inexistente.

Ainda validando a data, precisamos descobrir se o ano é bissexto. No caso do mês na entrada ser fevereiro precisamos avaliar a quantidade de dias. Então com o seguinte cálculo verificamos
```c
*bissexto = (((y % 4 == 0) && (y % 100 != 0)) || (y % 400 == 0));

if (m == 2) {
    if (*bissexto) {
        if (d > 29) {
            return -1;
        }
    } else {
        if (d > 28) {
            return -1;
        }
    }
}
```
vamos avaliar se o ano é bissexto realizando uma operação lógica para avaliar: Se o ano é multiplo de 400, o ano é multiplo de 4 e não é multiplo de 100 então é um ano bissexto. Assim a expressão retorna 1 se for verdadeiro ou 0 caso seja falso. No caso de ano bissexto, avalio se os dias são maiores que 29, caso contrario a avaliação é se são maiores que 28.

E para terminar nossa avaliação da data vamos verificar se os meses tem dias corretos
```c
if (m == 4 || m == 6 || m == 9 || m == 11) {
    if (d > 30) {
        return -1;
    }
}
```
Agora vamos usar a fórmula do enunciado para calcular o dia da semana da data, criaremos uma matriz de strings chamada weekDay com os nomes dos dias da semana e então utilizaremos um swtich case  para atribuir a referência da string com o dia da semana na variável `saida` e retornaremos 1 de sucesso
```c
int dia = (23 * m / 9 + d + 4 + (m < 3 ? y-- : y - 2) + y / 4 - y / 100 + y / 400) % 7;

char weekDay[7][10] = {"Domingo", "Segunda", "Terça", "Quarta", "Quinta", "Sexta", "Sábado"};

switch (dia) {
    case 0:
        *saida = weekDay[0];
        break;
    case 1:
        *saida = weekDay[1];
        break;
    case 2:
        *saida = weekDay[2];
        break;
    case 3:
        *saida = weekDay[3];
        break;
    case 4:
        *saida = weekDay[4];
        break;
    case 5:
        *saida = weekDay[5];
        break;
    case 6:
        *saida = weekDay[6];
        break;
}

return 1;
```
Agora vamos entender o por que precisamos ter como parâmetro um ponteiro para ponteiro da variável `saida`. Se tivessemos apenas um ponteiro como parâmetro e fizessemos a atribuição `saida = weekDay[0]` por exemplo, ao terminar a função perderiamos essa referência e teriamos `saida=NULL` na impressão. Para `saida` ter a referência mesmo ao fim da função passamos seu endereço para a função e alteramos. No trecho `*saida  =  weekDay[0];` estamos mudando para aonde `saida` aponta e assim não perderemos essa referência ao termino da função.
Assim terminamos nossa implementação e nosso código completo ficara assim
```c
#include <stdio.h>
#include <string.h>

int diadasemana(char data[11], char **saida, int *bissexto) {
    int d, m, y;

    if (strstr(data, "/") == NULL || strlen(data) != 10) {
        return -2;
    }

    sscanf(data, "%d/%d/%d", &d, &m, &y);

    if (y < 1900 || y > 9999)
        return -1;
    if (m < 1 || m > 12)
        return -1;
    if (d < 1 || d > 31)
        return -1;

    *bissexto = (((y % 4 == 0) && (y % 100 != 0)) || (y % 400 == 0));

    if (m == 2) {
        if (*bissexto) {
            if (d > 29) {
                return -1;
            }
        }

        else {
            if (d > 28) {
                return -1;
            }
        }
    }

    if (m == 4 || m == 6 || m == 9 || m == 11) {
        if (d > 30) {
            return -1;
        }
    }

    int dia = (23 * m / 9 + d + 4 + (m < 3 ? y-- : y - 2) + y / 4 - y / 100 + y / 400) % 7;
    char weekDay[7][10] = {"Domingo", "Segunda", "Terça", "Quarta", "Quinta", "Sexta", "Sábado"};

    switch (dia) {
        case 0:
            *saida = weekDay[0];
            break;
        case 1:
            *saida = weekDay[1];
            break;
        case 2:
            *saida = weekDay[2];
            break;
        case 3:
            *saida = weekDay[3];
            break;
        case 4:
            *saida = weekDay[4];
            break;
        case 5:
            *saida = weekDay[5];
            break;
        case 6:
            *saida = weekDay[6];
            break;
    }

    return 1;
}

int main() {
    int status = 0, bissexto = 0;
    char entrada[11] = {0}, *saida = NULL;

    fgets(entrada, 11, stdin);

    status = diadasemana(entrada, &saida, &bissexto);

    if (status > 0)
        printf("%s.%s\n", saida, bissexto ? " Ano bissexto." : "");

    else {
        if (status == -1)
            printf("Data inexistente.\n");
        else
            printf("Entrada invalida.\n");
    }

    return 0;
}
```
