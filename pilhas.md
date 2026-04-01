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


3. Inversão de palavras: Usando uma pilha, crie um programa para inverter a ordem das letras nas palavras de uma frase, sem inverter a ordem das palavras na frase. Por exemplo, se for digitada a frase "apenas um teste", o programa deverá produzir a seguinte saída: sanepa mu etset.

```
#include <stdio.h>
#include <string.h>
#include "pilha.h"

int main (void){
	char s[60];
	int i, j;

	printf("digite a frase: "); gets(s);
	Pilha A = pilha(strlen(s));
	i = j = 0;

	for( ; i < strlen(s); i++){
		if(s[i] != ' ') empilha(s[i], A);
		else{
			 while(!vaziap(A)) s[j++] = desempilha(A);
			 s[j++] = s[i]; // o mesmo que s[j++] = " "
		}

	} // neste ponto, empilhamos todos os chars antecessores ao "\0"

	while(!vaziap(A)) s[j++] = desempilha(A);
	s[j] = '\0';

	puts("saida: ");
	puts(s);
	destroip(&A);
	return 0;
}
```


4. Balanceamento de parênteses: Usando pilha, crie um programa para verificar se uma expressão composta apenas por chaves, colchetes e parênteses, representada por uma cadeia de caracteres, está ou não balanceada. Por exemplo, as expressões "[{()()}{}]" e "{[([{}])]}" estão balanceadas, mas as expressões "{[(}])" e "{[)()(]}" não estão.

```
#include <stdio.h>
#include <string.h>
#include "pilha.h"

int main (void){

	char s[40], lixo, resultado;
	printf("digite a expressao: "); gets(s);
	Pilha A = pilha(strlen(s));

	for(int i=0; i < strlen(s); i++){
		switch(s[i]){
			case ')': if(!vaziap(A) && topo(A)=='(') lixo = desempilha(A); else empilha ('!', A); break;
			case ']': if(!vaziap(A) && topo(A)=='[') lixo = desempilha(A); else empilha ('!', A); break;
			case '}': if(!vaziap(A) && topo(A)=='{') lixo = desempilha(A); else empilha ('!', A); break;
			case '(': 
			case '[': 
			case '{': empilha(s[i], A); break;
			default: ; break; // nao foi proposital, mas isso faz o programa ler expressoes como "(a + b) * [c]" considerando apenas o balanceamento de (), [] e/ou {} kk
		}
	}

	if(vaziap(A)) puts("balanceada"); else puts("desbalanceada");
	return 0;
}
```


5. Pilha de strings: considerando o código abaixo, qual será a saída, se o usuário digitar as strings "um", "dois" e "tres"? Por quê?

```
#include <stdio.h>
#include <string.h>
#include "pilha.h" // pilha de char*

int main(void) {
	Pilha A = pilha(5);
	char s[11];
	
	for(int i=1; i<=3; i++) {
		printf("? "); gets(s);
		empilha(s, A);
	}
	
	while(!vaziap(A)) puts(desempilha(A));
	return 0;
}
```
Resposta: A saída será "tres tres tres". Nesse caso, foi necessário modificar o arquivo "pilha.h" para que a struct pilha armazenasse char* e não mais int como itens. Sendo assim, como o tipo char* é ponteiro, sempre que usamos get(s) sobrescrevemos um mesmo endereço de memória, o qual será empilhado como item em A.


6. Pilha de strings: Use _strdup(s), declarada em string.h, para corrigir o programa do exercício anterior. Essa função duplica a cadeia s numa área de memória, alocada pela função malloc(), e devolve o endereço dessa área. Depois de usada, essa cópia pode ser destruída com a função free().

```
#include <stdio.h>
#include <string.h>
#include "pilha.h" // pilha de char*

int main(void) {
	Pilha A = pilha(5);
	char s[11];
	
	for(int i=1; i<=3; i++) {
		printf("? "); gets(s);
		empilha(strdup(s), A);
	}
	
	while(!vaziap(A)){ 
		puts(topo(A));
		free(desempilha(A));
	}
	return 0;
}
```


