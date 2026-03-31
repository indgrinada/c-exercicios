1. Crie um programa que usa duas pilhas A e B para ordenar uma sequência de n números dados pelo usuário. A ideia é organizar a pilha A de modo que nenhum item seja empilhado sobre outro menor (use a pilha B apenas para manobra) e, depois, descarregar e exibir os itens da pilha A.

'''
#include <stdio.h>
#include "pilha.h"

int main (void){
	int n, x; 
 
	printf("n = "); scanf("%d", &n);  /* no arquivo pilha.h Itemp eh int, por isso considerei int x */

	Pilha A = pilha(n);  /* tipo ponteiro Pilha */
	Pilha B = pilha(n);  /* tipo ponteiro Pilha */

	for(int i=1; i<=n; i++){
		printf("%do. num: ", i); scanf("%d", &x);
		while(!vaziap(A) && x > topo(A)) empilha(desempilha(A), B);
		empilha(x, A);
		while(!vaziap(B)) empilha(desempilha(B), A);
		}
	puts("sequencia ordenada: ");
	while(!(vaziap(A))) printf("%d ", desempilha(A));
	puts("\n");
	destroip(&A);
	destroip(&B);
	return 0;
}
'''