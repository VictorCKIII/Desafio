# Desafio

## Palavras Reservadas

   - Provider
      - É usada para especificar o provedor de serviços em nuvem ou plataforma que o Terraform utilizará para gerenciar os recursos.
   - Variable
      - É usada para definir variáveis que podem ser reutilizadas em todo o código do Terraform.
   - Resources
      - É usada para definir um recurso de infraestrutura que será gerenciado pelo Terraform. Um recurso pode ser uma instância EC2, um banco de dados, etc.

# Tarefa 1

## Descrição detalhada de funcionamento do código

- Provider
   - Está a definir o provedor da nuvem que será utilizado, no nosso caso, é a AWS.
     
- Region
   - Está especificando a região da AWS onde os recursos serão criados, no nosso caso, é a região us-east-1.
   - A região é onde a AWS possui um local físico para os data centers.

# Variáveis

"Variable" define uma variável que pode ser reutilizada em todo o nosso código.
    - Projeto
      - É uma variável que armazena o nome do projeto. O valor por padrão é "VExpenses".
    - Candidato
      - É uma variável que armazena o nome do candidato. O valor por padrão é "SeuNome".

# Description
  É a descrição da variável, muito útil para documentar.

#Type
  Define o tipo de dado que a variável irá armazenar, aqui é string (texto).

#Default
  Valor padrão da variável, caso nada seja fornecido para a mesma.

