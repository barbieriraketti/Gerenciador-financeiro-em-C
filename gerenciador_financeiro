#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <unistd.h>
#include <string.h>

void extrato_arquivo(char gastos[], double valor, time_t timestamp) {
  FILE *file;
  file = fopen("extrato_arquivo.txt", "ab");
  fprintf(file, "%s %.2lf %ld \n", gastos, valor, (long)timestamp);
  fclose(file);
}

void historico_anual(void) {
  FILE *file;
  char gastos[1000];
  double valor;
  time_t data;
  file = fopen("extrato_arquivo.txt", "rb");
  FILE *ht;
  ht = fopen("Historico Anual.html", "wb");

  fprintf(ht, "<!DOCTYPE html><html><head><title>Document</title><style>h2 {text-align: center}table {width: 50%%; margin-left: auto; margin-right: auto; border: 2px solid white; background-color: lightblue; border-collapse: collapse}th,td {padding: 5px; width: 40%%; border: 2px solid white; background-color: lightblue; border-collapse: collapse}</style></head>");

  fprintf(ht, "<body><h2>Relatório de Histórico Anual</h2>\n<table>\n<tr><th>Data</th><th>Descrição</th> <th>Valor</th></tr>");

  while (fscanf(file, "%s %lf %ld", gastos, &valor, &data) != EOF) {
    if (data > (time(NULL) - (365 * 24 * 60 * 60))) {
      fprintf(ht, "<tr><td align=\"center\">%s</td><td align=\"center\">%s</td><td align=\"right\">$ %.2lf</td></tr>", ctime(&data), gastos, valor);
    }
  }
  fprintf(ht, "\n</table>");

  fclose(file);
  fclose(ht);
  printf("Relatório de Histórico Anual gerado com sucesso!\nConsulte o arquivo HTML!\n");
}

void historico_mensal(char categoria[]) {
  FILE *file;
  char gastos[1000];
  double valor;
  time_t data;
  file = fopen("extrato_arquivo.txt", "rb");
  FILE *ht;
  char filename[100] = "Historico Mensal-";
  strcat(filename, categoria);
  strcat(filename, ".html");
  ht = fopen(filename, "wb");

  fprintf(ht, "<!DOCTYPE html><html><head><title>Document</title><style>h2 {text-align: center}table {width: 50%%; margin-left: auto; margin-right: auto; border: 2px solid white; background-color: lightblue; border-collapse: collapse}th,td {padding: 5px; width: 40%%; border: 2px solid white; background-color: lightblue; border-collapse: collapse}</style></head>");
  fprintf(ht, "<body><h2>Histórico Mensal por Categoria</h2>\n<table>\n<tr><th>Data</th><th>Descrição</th> <th>Valor</th></tr>");

  while (fscanf(file, "%s %lf %ld", gastos, &valor, &data) != EOF) {
    if (strcmp(gastos, categoria) == 0 && data > (time(NULL) - (30 * 24 * 60 * 60))) {
      fprintf(ht, "<tr><td align=\"center\">%s</td><td align=\"center\">%s</td><td align=\"right\">$ %.2lf</td></tr>", ctime(&data), gastos, valor);
    }
  }
  fprintf(ht, "\n</table>");

  fclose(file);
  fclose(ht);
  printf("Histórico Mensal gerado com sucesso!\nConsulte o arquivo HTML!\n");
}

void ver_saldo() {
  double saldo;
  FILE *file;
  file = fopen("saldo.txt", "rb");
  if (file != NULL) {
    fscanf(file, "%lf", &saldo);
    printf("\n\n\nSaldo atual: %.2lf\n\n\n", saldo);
    fclose(file);
  } else {
    file = fopen("saldo.txt", "wb");
    fclose(file);
    ver_saldo();
  }
}

void gastos(char gastei[]) {
  double saldo, gst;
  printf("Digite o valor do débito: ");
  scanf("%lf", &gst);
  FILE *file;
  file = fopen("saldo.txt", "rb");
  if (file != NULL) {
    fscanf(file, "%lf", &saldo);
    saldo -= gst;
    extrato_arquivo(gastei, gst, time(NULL));
    file = fopen("saldo.txt", "wb");
    fprintf(file, "%.2lf", saldo);
    fclose(file);
    printf("Quantia debitada com sucesso.\nSaldo atualizado: %.2lf\n", saldo);
  } else {
    file = fopen("saldo.txt", "wb");
    fclose(file);
  }
}

void fazer_deposito() {
  double saldo, deposito;
  printf("Digite o valor do depósito: ");
  scanf("%lf", &deposito);
  FILE *file;
  file = fopen("saldo.txt", "rb");
  if (file != NULL) {
    fscanf(file, "%lf", &saldo);
    saldo += deposito;
    extrato_arquivo("Depósito", deposito, time(NULL));
    file = fopen("saldo.txt", "wb");
    fprintf(file, "%.2lf", saldo);
    fclose(file);
    printf("Depósito realizado com sucesso!\nSaldo atualizado: %.2lf\n", saldo);
  } else {
    file = fopen("saldo.txt", "wb");
    fclose(file);
  }
}

int menu() {
  printf("\n\n\t\tGERENCIADOR FINANCEIRO\n\n\n");
  int choice;

  while (1) {
    printf("1. Depósito\n");
    printf("2. Saldo Total\n");
    printf("3. Compras/Contas\n");
    printf("4. Histórico\n");
    printf("5. Sair\n\n\n");
    printf("Escolha uma opção: ");
    scanf("%d", &choice);

    switch (choice) {
      case 1:
        system("clear");
        fazer_deposito();
        break;

      case 2:
        system("clear");
        ver_saldo();
        break;

      case 3:
        printf("Você gastou em:\n");
        printf("1. Moradia\n");
        printf("2. Estudos\n");
        printf("3. Alimentação\n");
        printf("4. Trabalho\n");
        printf("5. Transporte\n");
        printf("0. Voltar ao menu\n");
        printf("Escolha: ");
        int choice_2;
        scanf("%d", &choice_2);
        switch (choice_2) {
          case 1:
            system("clear");
            gastos("Moradia");
            break;
          case 2:
            system("clear");
            gastos("Estudos");
            break;
          case 3:
            system("clear");
            gastos("Alimentação");
            break;
          case 4:
            system("clear");
            gastos("Trabalho");
            break;
          case 5:
            system("clear");
            gastos("Transporte");
            break;
          case 0:
            break;
          default:
            printf("Opção inválida!\n");
        }
        break;

      case 4:
        printf("Acessar movimentos:\n");
        printf("1. Mensais Moradia\n");
        printf("2. Mensais Estudos\n");
        printf("3. Mensais Alimentação\n");
        printf("4. Mensais Trabalho\n");
        printf("5. Mensais Transporte\n");
        printf("6. Anual\n");
        printf("7. Voltar ao menu\n");
        printf("Escolha: ");
        int choice_3;
        scanf("%d", &choice_3);
        switch (choice_3) {
          case 1:
            historico_mensal("Moradia");
            break;
          case 2:
            historico_mensal("Estudos");
            break;
          case 3:
            historico_mensal("Alimentação");
            break;
          case 4:
            historico_mensal("Trabalho");
            break;
          case 5:
            historico_mensal("Transporte");
            break;
          case 6:
            historico_anual();
            break;
          case 7:
            break;
          default:
            printf("Opção inválida!\n");
        }
        break;

      case 5:
        return 0;

      default:
        printf("Opção inválida!\n");
    }
  }
}

int main() {
  menu();
  return 0;
}
