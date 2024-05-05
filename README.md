#SISTEMA DE CADASTRO EM LINGUAGEM EM C

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <conio.h> // Para getch() e _getch()

// Função para limpar a tela
void limparTela() {
    system("cls"); // Limpar tela (para Windows)
}

// Estrutura para armazenar informações do usuário
struct Usuario {
    int id;
    char nome[50];
    char sexo;
    char estadoCivil[20];
    char nacionalidade[20];
};

// Função para verificar credenciais de login
int verificarLogin(char *usuario, char *senha) {
    // Verificar no seu sistema de armazenamento de dados (por exemplo, banco de dados ou arquivo)
    // Neste exemplo, usaremos credenciais fixas para fins de demonstração
    char usuarioCorreto[] = "usuario";
    char senhaCorreta[] = "senha123";

    if (strcmp(usuario, usuarioCorreto) == 0 && strcmp(senha, senhaCorreta) == 0) {
        return 1; // Login correto
    } else {
        return 0; // Login incorreto
    }
}

// Função para exibir usuários cadastrados em formato de tabela
void exibirUsuarios(struct Usuario usuarios[], int numUsuarios) {
    printf("\n--- Usuários Cadastrados ---\n");
    printf("| %-3s | %-20s | %-4s | %-12s | %-12s |\n", "ID", "Nome", "Sexo", "Estado Civil", "Nacionalidade");
    printf("+-----+----------------------+------+--------------+--------------+\n");
    for (int i = 0; i < numUsuarios; i++) {
        printf("| %-3d | %-20s | %-4c | %-12s | %-12s |\n",
               usuarios[i].id, usuarios[i].nome, usuarios[i].sexo,
               usuarios[i].estadoCivil, usuarios[i].nacionalidade);
    }
    printf("+-----+----------------------+------+--------------+--------------+\n");
    printf("\n");
}

// Função para editar um usuário existente
void editarUsuario(struct Usuario usuarios[], int numUsuarios) {
    int id;
    printf("\n--- Editar Usuário ---\n");
    printf("Digite o ID do usuário a ser editado: ");
    scanf("%d", &id);

    // Encontrar o índice do usuário com o ID especificado
    int index = -1;
    for (int i = 0; i < numUsuarios; i++) {
        if (usuarios[i].id == id) {
            index = i;
            break;
        }
    }

    if (index != -1) {
        printf("Nome: ");
        scanf("%s", usuarios[index].nome);

        printf("Sexo (M/F): ");
        scanf(" %c", &usuarios[index].sexo);

        printf("Estado Civil: ");
        scanf("%s", usuarios[index].estadoCivil);

        printf("Nacionalidade: ");
        scanf("%s", usuarios[index].nacionalidade);

        printf("Usuário editado com sucesso.\n");
        
    } else {
        printf("Usuário não encontrado.\n");
    }
}

// Função para tela de boas-vindas e gerenciamento de usuários
void telaBoasVindas() {
    struct Usuario usuarios[10]; // Array para armazenar até 10 usuários
    int numUsuarios = 0; // Número atual de usuários cadastrados

    int opcao;
    do {
        limparTela(); // Limpar tela

        printf("Bem-vindo!\n\n");
        printf("1. Cadastrar novo usuário\n");
        printf("2. Exibir usuários cadastrados\n");
        printf("3. Editar usuário\n");
        printf("4. Excluir usuário\n");
        printf("5. Sair do sistema\n");
        printf("\nEscolha uma opção: ");
        scanf("%d", &opcao);

        switch(opcao) {
            case 1: // Cadastrar novo usuário
                printf("\n--- Cadastro de Novo Usuário ---\n");
                printf("ID: %d\n", numUsuarios + 1);
                usuarios[numUsuarios].id = numUsuarios + 1;

                printf("Nome: ");
                scanf("%s", usuarios[numUsuarios].nome);

                printf("Sexo (M/F): ");
                scanf(" %c", &usuarios[numUsuarios].sexo);

                printf("Estado Civil: ");
                scanf("%s", usuarios[numUsuarios].estadoCivil);

                printf("Nacionalidade: ");
                scanf("%s", usuarios[numUsuarios].nacionalidade);

                numUsuarios++;
                break;
            case 2: // Exibir usuários cadastrados
                exibirUsuarios(usuarios, numUsuarios);
                break;
            case 3: // Editar usuário
                editarUsuario(usuarios, numUsuarios);
                break;
            case 4: // Excluir usuário
                if (numUsuarios > 0) {
                    int id;
                    printf("\n--- Excluir Usuário ---\n");
                    printf("Digite o ID do usuário a ser excluído: ");
                    scanf("%d", &id);

                    // Encontrar o índice do usuário com o ID especificado
                    int index = -1;
                    for (int i = 0; i < numUsuarios; i++) {
                        if (usuarios[i].id == id) {
                            index = i;
                            break;
                        }
                    }

                    if (index != -1) {
                        // Remover usuário movendo os usuários restantes para trás no array
                        for (int i = index; i < numUsuarios - 1; i++) {
                            usuarios[i] = usuarios[i + 1];
                        }
                        numUsuarios--;
                        printf("Usuário excluído com sucesso.\n");
                    } else {
                        printf("Usuário não encontrado.\n");
                    }
                }
 else {
                    printf("Nenhum usuário cadastrado para excluir.\n");
                }
                break;
            case 5: // Sair do sistema
                return; // Retorna à tela de login
            default:
                printf("Opção inválida. Por favor, escolha uma opção válida.\n");
        }

        printf("\nPressione Enter para continuar...");
        while (getchar() != '\n'); // Limpar o buffer de entrada
        getchar(); // Aguardar pressionar Enter
    } while (1);
}

int main() {
    char usuario[50];
    char senha[50];

    while (1) {
        limparTela(); // Limpar tela

        printf("== Sistema de Login ==\n");
        printf("Usuário: ");
        scanf("%s", usuario);
        printf("Senha: ");

        // Oculta a senha durante a digitação
        int i = 0;
        char c;
        while ((c = _getch()) != '\r') {
            if (c == 8 && i > 0) { // 8 é o código ASCII do backspace
                printf("\b \b"); // Apaga o caractere anterior
                i--;
            } else {
                senha[i++] = c;
                printf("*");
            }
        }
        senha[i] = '\0';

        if (verificarLogin(usuario, senha)) {
            telaBoasVindas();
        } else {
            printf("\nCredenciais de login incorretas. Pressione Enter para continuar...");
            while (getchar() != '\n'); // Limpar o buffer de entrada
            getchar(); //  Aguardar pressionar Enter
        }
    }

    return 0;
}
