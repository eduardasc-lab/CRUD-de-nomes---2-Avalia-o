**_Nome: Eduarda Sousa Coêlho_**\
**_Matrícula: 20240005582_**\
**_Curso: BICT_**

**_Projeto: CRUD de nomes - 2° Avaliação_**\
**_Disciplina: Laboratório de programação em C_**

**_Código_**

#include <stdio.h> //printf, scanf, fgets\
#include <string.h> //strcmp, strcpy, strcspn

#define MAX_REGISTROS 50 //no máximo 50 registros\
#define MAX_LETRAS    30 //cada nome com no máximo 29 caracteres

//NOSSO BANCO DE DADOS (matrix de caracteres)\
char banco [MAX_REGISTROS] [MAX_LETRAS]; //nossa tabela de strings

//função p/ limpar todas as linhas, com \0, que indica vazio\
void inicializar_banco() {\
  for (int i=1; i<MAX_REGISTROS; i++) {\
    banco[i][0]='\0'; //linha vazia\
  }\
}

//função buscar em inserir, buscar, modificar e apagar\
//ela procura um nome e retorna o índice onde está ou -1 se não achar\
int buscar_nome(char nome_procurado[]) {\
  for(int i=1; i<MAX_REGISTROS; i++) {\
    if(banco[i][0]!='\0' && strcmp(banco[i], nome_procurado) == 0) {\
    //só compara se a linha NÃO estiver vazia\
      return 1; //ENCONTROU\
    }\
  }\
  return -1; //NÃO ENCONTROU\
}\
//strcmp retorna 0 quando as strings são iguais, "banco[i][0]!='\0'" é pra não comparar linhas vazias

//pede o nome\
//verifica se já existe\
//se não existir, procura a primeira linha vazia para salvar\
void inserir() {\
  char nome[MAX_LETRAS];\
  printf("Digite o nome a inserir: ");\
  fgets(nome, MAX_LETRAS, stdin);\
  nome[strcspn(nome, "\n")]='\0';

  // 1. Verifica se já existe\
  if (buscar_nome(nome) != -1) {\
    printf("Esse nome já existe! Tente outro nome único.\n");\
    return; //volta ao menu\
  }

  // 2. Procura uma linha vazia\
  for (int i=1; i<MAX_REGISTROS; i++) {\
    if (banco[i][0] == '\0') {\
      strcpy (banco[i], nome);\
      printf ("Nome inserido com sucesso na posição %d!\n", i);\
      return;\
    }\
  }

  // 3. Aqui não encontrou vaga\
  printf ("Banco de dados cheio! Não é possível inserir mais nomes.\n");\
}

//listar todos os nomes\
void listar () {\
  printf ("LISTA DE NOMES\n");\
  int encontrou_algum=0;\
  for (int i=1; i<MAX_REGISTROS; i++) {\
    if (banco[i][0] != '\0') {\
      printf("Posição %d: %s\n", i, banco[i]);\
      encontrou_algum=1;\
    }\
  }\
  if (!encontrou_algum) {\
    printf ("Nenhum nome cadastrado ainda.\n");\
  }\
}

//buscar e exibir índice\
void buscar (){\
  char nome[MAX_LETRAS];\
  printf ("Digite o nome a buscar: ");\
  fgets (nome, MAX_LETRAS, stdin);\
  nome [strcspn (nome, "\n")]='\0';

  int indice=buscar_nome(nome);\
  if (indice == -1) {\
    printf ("Nome não encontrado.\n");\
  }else{\
    printf ("Nome encontrado na posição %d.\n", indice);\
  }\
}

//apagar um nome\
void apagar () {\
  char nome[MAX_LETRAS];\
  printf ("Digite o nome a apagar: ");\
  fgets (nome, MAX_LETRAS, stdin);\
  nome [strcspn (nome, "\n")] = '\0';\
  int indice = buscar_nome(nome);\
  if (indice == -1) {\
    printf("Nome não encontrado para remoção.\n");\
  }else{\
    banco [indice][0] = '\0';  //"apaga" a linha\
    printf ("Nome removido com sucesso.\n");\
  }\
}

//modificar um nome\
//o novo nome não pode ser igual a outro já existente\
void modificar() {\
  char nome_antigo [MAX_LETRAS];\
  printf ("Digite o nome que deseja modificar: ");\
  fgets (nome_antigo, MAX_LETRAS, stdin);\
  nome_antigo [strcspn(nome_antigo, "\n")] = '\0';\
  int indice = buscar_nome(nome_antigo);\
  if (indice == -1) {\
    printf ("Nome não encontrado.\n");\
    return;\
  }
  
//Agora pede o novo nome\
  char novo_nome [MAX_LETRAS];\
  printf ("Digite o novo nome: ");\
  fgets (novo_nome, MAX_LETRAS, stdin);\
  novo_nome [strcspn (novo_nome, "\n")] = '\0';
  
// Verifica se o novo nome já existe (em OUTRA posição)\
  for (int i = 1; i < MAX_REGISTROS; i++) {\
    if (i != indice && banco[i][0] != '\0' && strcmp (banco[i], novo_nome) == 0) {\
      printf ("Já existe outro registro com esse nome. Modificação cancelada.\n");\
      return;\
    }\
  }
  
//Tudo ok, substitui\
  strcpy (banco [indice], novo_nome);\
  printf ("Nome modificado com sucesso!\n");\
}

//função principal com menu\
int main() {\
  inicializar_banco ();\
  int opcao;

  do {\
    printf("MENU\n");\
    printf("1. Inserir nome\n");\
    printf("2. Buscar nome\n");\
    printf("3. Modificar nome\n");\
    printf("4. Apagar nome\n");\
    printf("5. Listar todos\n");\
    printf("0. Sair\n");\
    printf("Escolha uma opção: ");\
    scanf("%d", &opcao);\
    getchar(); // limpa o \n deixado pelo scanf\
    switch(opcao) {\
      case 1: inserir(); break;\
      case 2: buscar(); break;\
      case 3: modificar(); break;\
      case 4: apagar(); break;\
      case 5: listar(); break;\
      case 0: printf("Encerrando programa...\n"); break;\
      default: printf("Opção inválida! Tente novamente.\n");\
    }\
  }while (opcao != 0);\
  return 0;\
}

**_Roteiro de testes_**

**Compile e execute.**\
**Teste listar, deve dizer "nenhum nome".**\
**Teste inserir alguns nomes.**\
**Teste listar para ver se aparecem.**\
**Tente inserir um nome repetido, deve bloquear.**\
**Teste buscar com um nome que existe e outro que não.**\
**Teste apagar e depois liste para ver se sumiu.**\
**Teste modificar: troque um nome por outro que ainda não existe.**\
**Teste modificar para um nome que já existe, deve recusar.**
