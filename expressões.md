# Expressões

- Operandos: constantes numéricas.
- Operadores: operações aritméticas binárias (+, –, * e /).
- Delimitadores: parênteses de abertura e de fechamento.


## Notações

- Infixa: A + B
- Prefixa (polonesa): + A B
- Posfixa (polonesa reversa): A B +


## Conversão

- Considere a seguinte expressão infixa: ``` 2 * 3 + 8 / 4 ```
- Coloque parênteses de acordo com a prioridade dos operadores: ``` ((2*3) + (8/4)) ```
- Agora posfixe: ``` 2 3 * 8 4 / + ```


# Exercícios


1. Teste do programa: teste o seguinte programa com algumas expressões parentesiadas:

```
#include <stdio.h>
#include <string.h>
#include <ctype.h>
#include "pilha.h"

char *posfixa(char *e){  /* "char *e porque desejamos passar o endereço iniciar da string lida"*/
	static char s[256]; /*a expressão ficará armazenada nesse char*/
	int j = 0;
	Pilha P = pilha(256);
	for(int i=0; e[i]; i++)  /* e[i] == *(e + i) */
		if(isdigit(e[i])) s[j++] = e[i];
	    else if (strchr("+*-/", e[i])) empilha(e[i], P); /* o strchr() procura a primeira ocorrência de e[i] em "+*-/"*/
	    else if (e[i] == ')') s[j++] = desempilha(P);
	s[j] = '\0'; /* aspa simples para char, aspa dupla para string*/
	destroip(&P);
	return s;
}

int main(void){
	char e[513];
	printf("Expressao infixa (parentesiada): ");
	gets(e);
	printf("Expressao posfixa: %s\n", posfixa(e));
	return 0;
}
```


2. Teste do programa ver. 2: teste o seguinte programa com algumas expressões (parentesiadas ou não):

```
#include <stdio.h>
#include <string.h>
#include <ctype.h>
#include "pilha.h"

int prio(char o){
	switch(o){
		case '(': return 0;
		case '+':
		case '-': return 1;
		case '*':
		case '/': return 2;
	}
	return -1; /* operador inválido! */
} /* observe que o break não foi necessário, pois o return é um comando que já descarta os casos seguintes a ele*/

char *posfixa(char *e){
	static char s[256]; /*a expressão ficará armazenada nesse char*/
	int j = 0;
	Pilha P = pilha(256);
	for(int i=0; e[i]; i++)
		if (e[i] == '(') empilha('(', P);
	    else if (isdigit(e[i])) s[j++] = e[i];
	    else if (strchr("+*-/", e[i])){
			while(!vaziap(P) && prio(topo(P)) >= prio(e[i])){
				s[j++] = desempilha(P);
			}
			empilha(e[i], P);
		}
	    else if (e[i] == ')'){
			while(topo(P) != '('){
				s[j++] = desempilha(P);
			}
			desempilha(P);
		} 
	while(!vaziap(P)){
		s[j++] = desempilha(P);
	}
	s[j] = '\0'; /* aspa simples para char, aspa dupla para string*/
	destroip(&P);
	return s;
}

int main(void){
	char e[513];
	printf("Expressao infixa: ");
	gets(e);
	printf("Expressao posfixa: %s\n", posfixa(e));
	return 0;
}
```


3. Programa completo: Crie um programa completo para ler uma expressão aritmética na forma infixa e exibir sua forma posfixa correspondente, bem como seu valor numérico. Por exemplo, para a expressão infixa "2*3+8/4", o programa deve apresentar como saída a forma posfixa "23*84/+" e o valor numérico 8.

```
#include <stdio.h>
#include <string.h>
#include <ctype.h>
#include "pilha.h"

int prio(char o){
	switch(o){
		case '(': return 0;
		case '+':
		case '-': return 1;
		case '*':
		case '/': return 2;
	}
	return -1; /* operador inválido! */
}

char *posfixa(char *e){
	static char s[256]; /*a expressão ficará armazenada nesse char*/
	int j = 0;
	Pilha P = pilha(256);
	for(int i=0; e[i]; i++)
		if (e[i] == '(') empilha('(', P);
	    else if (isdigit(e[i])) s[j++] = e[i];
	    else if (strchr("+*-/", e[i])){
			while(!vaziap(P) && prio(topo(P)) >= prio(e[i])){
				s[j++] = desempilha(P);
			}
			empilha(e[i], P);
		}
	    else if (e[i] == ')'){
			while(topo(P) != '('){
				s[j++] = desempilha(P);
			}
			desempilha(P);
		} 
	while(!vaziap(P)){
		s[j++] = desempilha(P);
	}
	s[j] = '\0'; /* aspa simples para char, aspa dupla para string*/
	destroip(&P);
	return s;
}

int valor(char *e){
	Pilha P = pilha(256);
	for(int i=0; e[i]; i++)
		if(isdigit(e[i])) empilha(e[i]-'0', P);
	    else{
			int y = desempilha(P);
			int x = desempilha(P);
			switch(e[i]){
				case '+': empilha(x+y, P); break;
			    case '-': empilha(x-y, P); break;
			    case '*': empilha(x*y, P); break;
			    case '/': empilha(x/y, P); break;
			}
		}
	int z = desempilha(P);
	destroip(&P);
	return z;
}

int main(void){
	char e[513];
	printf("Expressao infixa: ");
	gets(e);
	char *expressao = posfixa(e);
	printf("Expressao posfixa: %s\n", expressao);
	printf("Valor numérico: %d\n", valor(expressao));
	return 0;
}
```


## Observações

- Nem sempre o "break" é necessário em switch-cases. Caso você use um comando mais forte que ele (como o "return") para impedir o fall-through, a sua omissão deixa o código mais limpo e menos redundante.
