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


- "Variable" define uma variável que pode ser reutilizada em todo o nosso código.
    - Projeto
      - É uma variável que armazena o nome do projeto. O valor por padrão é "VExpenses".
    - Candidato
      - É uma variável que armazena o nome do candidato. O valor por padrão é "SeuNome".

- Description
  - É a descrição da variável, muito útil para documentar.

- Type
  - Define o tipo de dado que a variável irá armazenar, aqui é string (texto).

- Default
  - Valor padrão da variável, caso nada seja fornecido para a mesma.

## Gerando uma chave privada para SSH
   - 'resource "tls_private_key" ' - Cria uma chave privada utilizando o provedor tls ( Transport Layer Security);
   - O algoritmo de criptografia utilizado foi o RSA;
   - O tamanho da chave em bits foi de 2048 bits (Tamanho seguro);
## Criando um par de chaves na AWS
   - 'resource "aws_key_pair" ' - Cria um par de chaves na AWS para acessar as instâncias EC2 via SSH antes já definido;
   - 'key name' - Nome do par de chaves, aqui, ele utiliza uma interpolação que faz incluir o nome candidato e do projeto;
   - 'public_key' - Chave pública gerada pelo 'tls_private_key'. A chave privada que será usada para ter acesso a instância;
## Criando uma Virtual Private Cloud (VPC)
   - 'resource "aws_vpc" ' - Cria uma VPC, é uma rede virtual isolada na AWS;
   - 'cidr_block' - Define o intervalo de IPs da VPC. No nosso caso, como temos /16 é '2^16', logo, permite até 65.536 Ips;
   - 'enable_dns_support' - Habilita suporte a DNS na VPC;
   - 'tags' - Metadados para identificar o recurso. Onde o nome da VPC está incluindo o projeto e o candidato;
