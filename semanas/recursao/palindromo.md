# Prova de Recursão: Palíndromo com recursão

Basicamente a questão quer que se verifique se uma palavra/frase é um palíndromo usando a abordagem recursiva. A questão pede que se crie uma função que recebe somente um argumento (que seria a string a ser analisada) e retorna _1_ caso seja palíndromo e _0_ caso contrário.

De inicio devemos nos atentar que nossa string de entrada vem com os seguites caracteres especiais:
>\n
>\r

Uma explicação para estes caracteres está [aqui](https://stackoverflow.com/questions/15433188/r-n-r-and-n-what-is-the-difference-between-them) . Então precisamos elimina-lós trocando eles por `\0` que indica o fim da string. Existem duas abordagens:
1 - Usando
```c
scanf("%[^\r\n]",  entrada);
```
que vai armazenar em `entrada` a string fornecida até receber um `\n` ou `\r` mas não os incluindo na string salva em `entrada`.
2- Usando
```c
fgets(entrada, 100, stdin);
```
e posteriormente rodando um for como esse para retira-lós da string
```c
for (int i = 0; i < strlen(frase); i++) {
    if (frase[i] == '\n' || frase[i] == '\r') {
        frase[i] = '\0';
    }
}
```

Depois de retirar os caracteres indesejados da nossa entrada, vamos analisar os possiveis casos de palíndromos. Teremos:
>ata
>a  torre  da  derrota

Perceba que neste segundo caso, não podemos usar a abordagem de simplesmente comparar o primeiro caracter com o último, pois na segunda iteração teriamos `' ' == t` o que claramente está errado. Então precisamos sanitizar nossa string de entrada retirando os espaços em branco. Vamos usar essa função:
```c
void  retirarEspacos(char  *str,  char  *dest){
	int  i,j;
	for  (i  =  0,  j=0;  i  <  strlen(str);  i++){
		if(str[i]  !=  '  '){
			dest[j]  =  str[i];
			j++;
		}
	}
	dest[j+1]  =  '\0';
}
```

Onde passamos duas strings: `str` a string com espaços e `dest` a string sanitizada sem os espaços. A ideia é usar o `i` para iterar `str` e `j` para indexar `dest`. Logo quando um espaço for encontrado nosso contador de `dest` não é afetado e continuara indexando de maneira correta (sequencial) os caracteres em `dest`. Por fim, marcamos o fim de `dest` com a posição atual + 1 usando `\0`.

Então vamos pra função que retorna se é um palíndromo ou não. Esta é sua cara:
```c
int palindromo(char *str) {
    if (strlen(str) == 1 || strlen(str) == 0) {
        return 1;
    } else {
        if (str[0] == str[strlen(str) - 1]) {
            char sub[100];
            int i;
            for (i = 1; i < strlen(str) - 1; i++) {
                sub[i - 1] = str[i];
            }
            sub[i - 1] = '\0';
            palindromo(sub);
        } else {
            return 0;
        }
    }
}
```

1 - Começamos com nosso caso base, se o tamanho da string for 1 ou 0 trata-se de um palíndromo, logo retorno _1_ .
2 - Se a string for maior que 1 verifico se o primeiro e o último caractere são iguais (lembre-se que nesse ponto nossa string não tem mais espaços), se forem, gero uma substring apartir do indice _1_ (já que o 0 descobri ser igual ao último) até a posição anterior a última e chamo a função `palindromo(sub)`.
3- Caso o primeiro e o último caractere não sejam iguais, retorno _0_ pois posso concluir que não se trata de um palíndromo.

Exemplo:
>1ª Chamada:
>palindromo("ovo");
>Tamanho de "ovo" = 3
>1ª posição == 3ª posição ---> "Verdade"
>Substring = "v"
>2ª Chamada:
>palindromo("v")
>Tamanho de "v" = 1
>return 1;

Assim nosso código ficaria dessa forma
```c
#include <stdio.h>
#include <string.h>

int palindromo(char *str);
void retirarEspacos(char *str, char *dest);

int main()
{
    char entrada[101];
    char frase[101];

    scanf("%[^\r\n]", entrada);

    retirarEspacos(entrada, frase);

    if(palindromo(frase) == 1){
        printf("O texto %s é palíndromo\n",entrada);
    }
    else{
        printf("O texto %s não é palíndromo\n",entrada);
    }
    return 0;
}

int palindromo(char *str){
  if(strlen(str) == 1 || strlen(str) == 0){
      return 1;
  }
  else{
      if(str[0] == str[strlen(str) - 1]){
          char sub[100];
          int i;
          for(i=1; i < strlen(str) - 1; i++){
              sub[i-1] = str[i];
          }
          sub[i-1] = '\0';
          palindromo(sub);
      }
      else{
          return 0;
      }
  }
}

void retirarEspacos(char *str, char *dest){
  int i,j;
  for (i = 0, j=0; i < strlen(str); i++){
      if(str[i] != ' '){
          dest[j] = str[i];
          j++;
      }
  }
  dest[j+1] = '\0';
}
```

---
#### [Voltar para a página da semana](README.md)
#### [Voltar para a página inicial](https://github.com/bti-ufrn/monitoria-itp)
