# Filas

- Úteis quando precisamos manter a ordem de uma sequência.
- Seguem a política de acesso FIFO (First-in / First-Out).


## Operações e "fila.h"

- fila(m): cria fila vazia, com tamanho m.
- destroif(&F): destrói a fila F.
- vaziaf(F): 1 se a fila estiver vazia.
- cheiaf(F): 1 se a fila estiver cheia.
- enfileira(x, F): insere x no final da fila F.
- desenfileira(F): remove e devolve o item do início de F.

# Exercícios


1. Faça um programa que reconheça palíndromos como "Amor a Roma", usando a função "toupper()", declarada em "ctype.h"

```
#include <stdio.h>
#include <ctype.h>
#include "pilha.h"
#include "fila.h"

int main(void) {
   char s[256];
   Fila F = fila(256);
   Pilha P = pilha(256);
   printf("\nFrase? ");
   gets(s);
   for(int i=0; s[i]; i++)
      if( isalpha(s[i]) ) { 
         enfileira(s[i],F); 
         empilha(s[i],P); 
      }
   while( !vaziaf(F) && toupper(desenfileira(F))==toupper(desempilha(P)) );
   if( vaziaf(F)) puts("A frase e palindroma");
   else puts("A frase nao e palindroma");
   destroif(&F);
   destroip(&P);
   return 0;
}
```