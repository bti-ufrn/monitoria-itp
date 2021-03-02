# Horários das Rondas
Questão da semana 3 (funções), "Horários das Rondas"
Monitor: João Marcos

## Enunciado
Um grupo de policiais precisa realizar periodicamente algumas rondas para minimizar os impactos da criminalidade no bairro. Horários periódicos como a cada 30 minutos são fáceis de calcular, mas muito previsíveis. Os policiais pediram sua ajuda para listar os horários das rondas no formato hh:mm:ss (com as horas entre 0 e 23 e os minutos e os segundos entre 0 e 59). A entrada do programa consiste em 3 inteiros contendo horas, minutos e segundos da primeira ronda. O programa deve escrever na saída os horários das rondas seguintes obedecendo os seguintes acréscimos em relação ao horário da primeira ronda: 1h, 2h10m30s, 4h40m50s e 12h5m5s.

Para resolver essa questão, seu programa deve implementar uma função que escreve um horário na tela.

## Exemplos de entrada e saída
| Entrada  | Saída    |
| :---:    | :---:    |
| 05:05:23 | 4 5 23   |
| 06:15:53 |          |
| 08:46:13 |          |
| 16:10:28 |          |
|          |          |
| 00:40:10 | 23 40 10 |
| 01:50:40 |          |
| 04:21:00 |          |
| 11:45:15 |          |

## Interpretação do enunciado:
Recebemos 3 valores inteiros, `horas`, `minutos` e `segundos`, depois disso precisamos imprimir o horário das rondas, que acontecem se nós somarmos `1h`, `2h10m30s`, `4h40m50s` e `12h5m5s` ao horário.

Podemos usar uma função para evitar repetir o processo de tratar o horário.

## Resolução
Primeiro vamos preparar a `main`:
```c
#include <stdio.h>

int main() {
    int h, m, s;
    scanf("%d %d %d", &h, &m, &s);
}
```

Agora usaremos a _velha técnica de usar função que não existe e se preocupar com isso depois_.

```c
#include <stdio.h>

int main() {
    int h, m, s;
    scanf("%d %d %d", &h, &m, &s);
    imprimeHorario(h + 1, m, s);
    imprimeHorario(h + 2, m + 10, s + 30);
    imprimeHorario(h + 4, m + 40, s + 50);
    imprimeHorario(h + 12, m + 5, s + 5);
}
```

Estamos passando para a função os horários já somados em cada componente, precisamos pensar agora: _"o que essa função precisa fazer para funcionar?"_.

Não é só printar na tela, caso o um dos horários seja `23h50m0s`, por exemplo, e eu somar `4h40m`, o valor resultante é `27h90m0s`.

O que são 27 horas e 90 minutos?
Talvez 28 horas e 30 minutos.
Ou melhor, 1 dia, 4 horas e 30 minutos.

OK MAS não estamos contando dias, então vamos descartar aquela parte.

Acabamos com `4h30m`, para programar esse comportamento vamos usar a operação módulo, cuidado para não confundir com o módulo de um número (VALOR ABSOLUTO), a "operação módulo" foi traduzida do inglês para o português e ficou com o nome estranho mesmo.

Essa operação tira o resto da divisão de um número pelo outro, por exemplo, `102 % 10 = 2`, porque o resto da **divisão inteira** de 102 por 10 é 2.

Aplicado para o nosso problema:
70 minutos teria que virar 10 no relógio, porque o relógio conta entre 0 e 59

`70 % 60 = 10`
Sim, o resto da divisão de algo por X vai de 0 até X - 1 (inclusive).

Mas quando temos 70 minutos, também temos que jogar uma hora lá para o contador de horas, para isso vamos usar a divisão.
`70 / 60 = 1`.

Vamos começar com:
```c
void imprimeHorario(int h, int m, int s) {
    // Se segundos passarem de 60, jogar para minutos
    m += s / 60;
    // Se minutos passarem de 60, jogar para horas
    h += m / 60;
    // Continue...
}
```

Ok, primeiro jogamos de segundos para minutos, caso alcance 60, então jogamos de minutos para horas se também passar do limite.

Do jeito que estamos fazendo, se cair 130 minutos, então ele vai jogar duas vezes para horas. E o resto da divisão de 130 por 60 daria 10.

Agora a parte dos restos.

```c
void imprimeHorario(int h, int m, int s) {
    // Se segundos passarem de 60, jogar para minutos
    m += s / 60;
    // Se minutos passarem de 60, jogar para horas
    h += m / 60;
    // Tirar módulo de todos os números
    s = s % 60;
    m = m % 60;
    h = h % 24;
    printf("%02d:%02d:%02d\n", h, m, s);
}
```

Agora está feito, o código acima ajusta os valores que passam dos limites, jogando para o maior, segundos joga para minutos, e minutos joga para horas, depois tiramos o módulo para o resultado caber dentro de 24, 60 e 60.

## Código final
```c
#include <stdio.h>

void imprimeHorario(int h, int m, int s) {
    m += s / 60;
    h += m / 60;

    s %= 60;
    m %= 60;
    h %= 24;
    printf("%02d:%02d:%02d\n", h, m, s);
}

int main() {
    int h, m, s;
    scanf("%d %d %d", &h, &m, &s);
    imprimeHorario(h + 1, m, s);
    imprimeHorario(h + 2, m + 10, s + 30);
    imprimeHorario(h + 4, m + 40, s + 50);
    imprimeHorario(h + 12, m + 5, s + 5);
}
```

## Extras:
Cuidado com o valor do resto de divisão com números negativos.

---
#### [Voltar para a página da semana](README.md)
#### [Voltar para a página inicial](https://github.com/bti-ufrn/monitoria-itp)
