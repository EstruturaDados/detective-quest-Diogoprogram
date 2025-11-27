#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Estrutura da Sala (nó da árvore)
typedef struct Sala {
    char nome[50];
    struct Sala *esquerda;
    struct Sala *direita;
} Sala;

// Função para criar uma sala
Sala* criarSala(const char *nome) {
    Sala *novaSala = (Sala*) malloc(sizeof(Sala));
    if (!novaSala) {
        printf("Erro de alocação de memória!\n");
        exit(1);
    }
    strcpy(novaSala->nome, nome);
    novaSala->esquerda = NULL;
    novaSala->direita = NULL;
    return novaSala;
}

// Função para explorar salas
void explorarSalas(Sala *atual) {
    if (!atual) return;

    printf("Você está na sala: %s\n", atual->nome);

    // Nó-folha (sem filhos)
    if (!atual->esquerda && !atual->direita) {
        printf("Fim do caminho!\n");
        return;
    }

    char escolha;
    do {
        printf("Escolha o caminho: (e = esquerda, d = direita, s = sair) ");
        scanf(" %c", &escolha);

        if (escolha == 'e' && atual->esquerda) {
            explorarSalas(atual->esquerda);
            break;
        } else if (escolha == 'd' && atual->direita) {
            explorarSalas(atual->direita);
            break;
        } else if (escolha == 's') {
            printf("Você saiu da exploração.\n");
            break;
        } else {
            printf("Opção inválida ou caminho inexistente!\n");
        }
    } while (1);
}

int main() {
    // Construção da árvore da mansão
    Sala *hall = criarSala("Hall de Entrada");
    Sala *cozinha = criarSala("Cozinha");
    Sala *salaEstar = criarSala("Sala de Estar");
    Sala *quarto = criarSala("Quarto");
    Sala *banheiro = criarSala("Banheiro");

    // Ligando os nós (árvore binária)
    hall->esquerda = cozinha;
    hall->direita = salaEstar;
    cozinha->esquerda = quarto;
    cozinha->direita = banheiro;

    // Início da exploração
    explorarSalas(hall);

    // Liberar memória
    free(hall);
    free(cozinha);
    free(salaEstar);
    free(quarto);
    free(banheiro);

    return 0;
}
