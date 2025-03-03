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
## Criando uma Subnet
   - 'resource "aws_subnet" ' - Cria uma subnet dentro da VPC;
   - 'vpc_id' - ID da VPC à qual a subnet pertence;
   - 'cidr_block' - Intervalo de Ips da subnet. Aqui, permite até 256 Ips;
   - 'availability_zone' - Zona de disponibilidade onde a subnet será criada. Aqui é definido a 'us-east-1a';
   - 'tags' - Metadados usados para identificar a subnet;
## Criando um Internet Gateway
   - 'resource "aws_internet_gateway" ' - Cria um Internet Gateway (IGW), que permite comunicação entre VPC e a internet;
   - 'vpc_id' - Id da VPC à qual o IGW está associado;
   - 'tags' - Metadados para identificar o IGW;
## Criando uma Tabela de Rotas
   - 'resource "aws_route_table" ' - Cria uma tabela para definir como o tráfego de rede é direcionado;
   - 'vpc_id' - ID da VPC á qual tabela de rotas ele pertence;
   - 'route' - Define uma rota
     - 'cidr_block' - Intervalo de Ips de destino, aqui, significa todo o tráfego;
     - 'gateway_id' - ID do gateway para onde o tráfego será direcionado, aqui é o IGW;
   - 'tags' - Metadados para identificar a tabela de rotas;
## Associando a Subnet à Tabela de Rotas
   - 'resource "aws_route_table_association" ' - Associa uma subnet a uma tabela de rotas;
     - 'subnet_id' - ID da Subnet;
     - 'route_table_id' - Id da Tabela de Rotas;
     - 'tags' - Metadados para identificar a associação;
## Criando um Security Group
   - 'resource "aws_security_group" ' - Cria um grupo de segurança, que serve como um firewall virtual;
      - 'name' - Nome do grupo de segurança;
      - 'description' - Descrição do grupo;
      - 'vpc_id' - ID da VPC a qual o grupo pertence;
      - 'ingress' - Regras de entrada;
         - 'description' - Descrição da regra;
         - 'from_port' e 'to_port' - Intervalo de portas. Aqui, 22 para SSH;
         - 'cidr_block' e 'ipv6_cidr_blocks' - Intervalos de Ips permitidos. Aqui permitem qualquer Ip;
      - 'egress' - Regras de saída;
          - 'from_port' e 'to_port' - "0" Signifca todas as portas;
          - 'portocol' - "-1" Significa todos os protocolos;
      - 'tags' - Metadados para identificar o grupo;
## Obtendo a AMI do Debian 12
  - 
