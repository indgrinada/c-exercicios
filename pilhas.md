# Pilhas

- Úteis quando precisamos inverter a ordem de uma sequência.
- Seguem a política de acesso LIFO (Last-In / First-Out)


## Operações em "pilha.h"

- pilha(m): cria pilha vazia, com tamanho m.
- destroip(&P): destrói a pilha P.
- vaziap(P): 1, se a pilha estiver vazia.
- cheiap(P): 1, se a pilha estiver cheia.
- empilha(x, P): insere x no topo de P.
- desempilha(P): remove e devolve o item no topo de P.
- topo(P): devolve o item no topo de P.

Obs.: #include <stdio.h> e #include "pilha.h" , pois stdio.h é um arquivo padrão, enquanto pilha.h é definido pelo programador.


### Estrutura do tipo Pilha

```
typedef char Itemp; \* dá o apelido "Itemp" ao tipo existente "char" *\
typedef struct pilha {
int max;
int topo;
Itemp *item;
} *Pilha;
```

### Função para criar pilha vazia

```
Pilha pilha(int m) {
Pilha P = malloc(sizeof(struct pilha));
P->max = m;
P->topo = -1;
return P;
}
```


## Observações importantíssimas

- O tipo "Pilha" é um ponteiro, pois em "pilha.h" foi definida a estrutura typedef struct pilha {...} *Pilha, ou seja, "Pilha" aponta o endereço da estrutura "struct pilha".

- O tipo de uma função, como no caso Pilha pilha(int m){...}, indica o tipo de coisa que ela retorna. No exemplo, a função pilha, devolve um ponteiro, pois Pilha é um ponteiro que aponta o endereço da estrutura "struct pilha".

- Esse monte de "pilha" confunde, mas encare como se "Pilha", "struct pilha" e "pilha" fossem coisas distintas, já que se tratam de ponteiro, estrutura e função, respectivamente.

- Acredito que ajude pensar em "Pilha" como um tipo especial de ponteiro, um ponteiro que aponta o inicio de uma struct pilha.

- "typedef char Itemp;" é o mesmo que dar um apelido, "Itemp", para o tipo existente "char". Faz-se isso para facilitar a edição do código. Por exemplo, se eu quisesse que a minha struct pilha armazenasse inteiros, apenas trocaria "char" por "int".

- O ponteiro "item" aponta o início do vetor (com tamanho definido) que armazena os itens da pilha.


## Reflexão do dia

A pilha é uma invenção, uma abstração. Na verdade, os seus itens ficam em um vetor de tamanho reservado na memória. Devemos definir a estrutura "struct pilha", em vez de simplesmente usar o vetor, para facilitar a organização e administração da abstração que denominamos "pilha".


# Exercícios


1. Ordenação crescente: Crie um programa que usa duas pilhas A e B para ordenar uma sequência de n números dados pelo usuário. A ideia é organizar a pilha A de modo que nenhum item seja empilhado sobre outro menor (use a pilha B apenas para manobra) e, depois, descarregar e exibir os itens da pilha A.

```
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
```


2. Ordenação decrescente e sem repetição: Faça a alteração mínima necessária para que o programa do exercício anterior ordene os números em ordem decrescente, eliminando números repetidos.

```
#include <stdio.h>
#include "pilha.h"

int main (void){
	int n, x; 
 
	printf("n = "); scanf("%d", &n);  /* no arquivo pilha.h Itemp eh int, por isso considerei int x */

	Pilha A = pilha(n);  /* tipo ponteiro Pilha */
	Pilha B = pilha(n);  /* tipo ponteiro Pilha */

	for(int i=1; i<=n; i++){
		printf("%do. num: ", i); scanf("%d", &x);
		while(!vaziap(A) && x < topo(A)) empilha(desempilha(A), B);
		if(vaziap(A) || x > topo(A)) empilha(x, A);
		while(!vaziap(B)) empilha(desempilha(B), A);
		}
	puts("sequencia ordenada: ");
	while(!(vaziap(A))) printf("%d ", desempilha(A));
	puts("\n");
	destroip(&A);
	destroip(&B);
	return 0;
}
```

