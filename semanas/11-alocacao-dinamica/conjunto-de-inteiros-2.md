## Vídeo:
- [Conjunto de Inteiros 2](https://drive.google.com/file/d/13k0omFVmiuHb4U2zYm5YNE70QB7JhN0F/view?usp=sharing) (Felipe Costa - vídeo no Drive)

## Código base:
```c
#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>

typedef unsigned int UINT;

typedef struct {
  UINT size;   // tamanho do array alocado para os elementos do conjunto
  UINT num;    // número de elementos atualmente alocados
  int *array;  // array com os elementos
} Set;

Set* create() {
    Set *newSet = (Set*) malloc(sizeof(Set));
    newSet->num = 0;
    newSet->size = 10;
    newSet->array = (int*) malloc(newSet->size * sizeof(int));
    return newSet;
}

bool has(Set *s, int value) {
    for (int i = 0; i < s->num; i++) {
        if (s->array[i] == value) {
            return true;
        }
    }
    return false;
}

void add(Set *s, int value) {
    for (int i = 0; i < s->num; i++) {
        if (s->array[i] == value) {
            return;
        }
    }
    if (s->num == s->size) {
        s->size += 10;
        s->array = (int*) realloc(s->array, s->size * sizeof(int));
    }
    s->array[s->num] = value;
    s->num++;
}

int main(void) {
    Set *a = create(), *b = create(), *c = create();
    int size, element;
    scanf("%d", &size);
    for (int i = 0; i < size; i++) {
        scanf("%d", &element);
        add(a, element);
    }
    scanf("%d", &size);
    for (int i = 0; i < size; i++) {
        scanf("%d", &element);
        add(b, element);
    }
    for (int i = 0; i < a->num; i++) {
        if (has(b, a->array[i])) {
            add(c, a->array[i]);
        }
    }
    for (int i = 0; i < c->num; i++) {
        printf("%d ", c->array[i]);
    }
    printf("\n%d %d\n%d %d", a->num, a->size, b->num, b->size);
    return 0;
}
```

---
#### [Voltar para a página da semana](README.md)
#### [Voltar para a página inicial](https://github.com/bti-ufrn/monitoria-itp)
