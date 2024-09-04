# Atividade-38

#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>
#include <string.h>

#define MAX 100

typedef struct {
    char items[MAX];
    int top;
} Stack;

// Função para inicializar a pilha
void init(Stack *s) {
    s->top = -1;
}

// Função para verificar se a pilha está vazia
int isEmpty(Stack *s) {
    return s->top == -1;
}

// Função para adicionar um elemento à pilha
void push(Stack *s, char c) {
    if (s->top < MAX - 1) {
        s->items[++(s->top)] = c;
    }
}

// Função para remover um elemento da pilha
char pop(Stack *s) {
    if (!isEmpty(s)) {
        return s->items[(s->top)--];
    }
    return '\0';  // Retorna um caractere nulo se a pilha estiver vazia
}

// Função para obter o elemento do topo da pilha
char peek(Stack *s) {
    if (!isEmpty(s)) {
        return s->items[s->top];
    }
    return '\0';  // Retorna um caractere nulo se a pilha estiver vazia
}

// Função para verificar a precedência dos operadores
int precedence(char op) {
    switch (op) {
        case '+':
        case '-':
            return 1;
        case '*':
        case '/':
            return 2;
        case '^':
            return 3;
        default:
            return 0;
    }
}

// Função para converter uma expressão infixa para pós-fixa
void infixToPostfix(const char *infix, char *postfix) {
    Stack s;
    init(&s);
    int i, k = 0;

    for (i = 0; infix[i]; i++) {
        if (isspace(infix[i])) {
            continue;  // Ignorar espaços em branco
        }

        if (isalnum(infix[i])) {
            postfix[k++] = infix[i];
        } else if (infix[i] == '(') {
            push(&s, infix[i]);
        } else if (infix[i] == ')') {
            while (!isEmpty(&s) && peek(&s) != '(') {
                postfix[k++] = pop(&s);
            }
            pop(&s);  // Remover '(' da pilha
        } else {  // Operadores
            while (!isEmpty(&s) && precedence(peek(&s)) >= precedence(infix[i])) {
                postfix[k++] = pop(&s);
            }
            push(&s, infix[i]);
        }
    }

    // Pop todos os operadores restantes na pilha
    while (!isEmpty(&s)) {
        postfix[k++] = pop(&s);
    }

    postfix[k] = '\0';  // Adicionar o caractere nulo no final da string
}

int main() {
    char infix[MAX], postfix[MAX];

    printf("Digite a expressao aritmetica infixa: ");
    fgets(infix, sizeof(infix), stdin);
    infix[strcspn(infix, "\n")] = '\0';  // Remover nova linha

    infixToPostfix(infix, postfix);
    printf("Expressao pos-fixa: %s\n", postfix);

    return 0;
}
