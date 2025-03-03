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
  - 'data "aws_ami" ' - Obtém uma AMI ( Amazon Machine Image) da AWS.
     - 'most_recent' - Retorna a AMI mais recente;
     - 'filter' - Filtros para encontrar a AMI desejada;
        - 'name' - Filtra por nome;
        - 'virtualization-type' - Filtra por tipo de virtualização;
     - 'owners' - Id do proprietário da AMI;

## Criando uma instância EC2
   - 'resource "aws_instance" ' -  Cria uma instância EC2;
      - 'ami' - Id da AMI usada para instância;
      - 'instance_type' - Tipo da instância;
      - 'subnet_id' - Id da Subnet onde a instância será criada;
      - 'key_name' - Nome do par de chaves para SSH;
      - 'security_groups' - Lista de grupos de segurança associados a instância;
      - 'associate_public_ip_adress' - Define se a instância terá um IP público;
      - 'root_block_device' - Configura o disco raiz da instância;
         - 'volume_size' - Tamanho do disco em GB;
         - 'volume_type' - Tipo de volume;
         - 'delete_on_termination' - Define se o disco será excluído quando a instância for encerrada.
      - 'user_data' - Script executado na inicialização da instância;
      - 'tags' - Metadados para identificar a instância;
## Outputs 
  - 'output' - Define os valores que serão exibidos após a execução do Terraform;
     - 'private_key' - Exibe a chave privada gerada para acessar a instância. O valor é marcado como sensitive para evitar exibição no log;
     - 'ec2_public_ig' - Exibe o ip público da instância EC2;
   
# Resumo
Esse código configura o provedor AWS na região us-east-1;
Define variáveis para o nome do projeto e do candidato;
Gera uma chave privada para SSH e registra a mesma na AWS; 
Cria uma VPC, uma subnet, um Internet Gateway e uma tabela de rotas;
Configura um grupo de segurança para permitir SSH e todo o tráfego de saída; 
Obtém a AMi mais recente do Debian 12;
Cria uma instância EC2 com um ip público, usando a AMI do Debian 12; 
Exibe a chave privada e o ip público da instância após a criação.

# Tarefa 2

## Melhorias de Segurança

 # Restringir o Acesso SSH
  - Como o security group está permitindo o acesso do SSH de qualquer lugar, acaba se tornando um pouco arriscado, logo, precisamos restringir esse acesso.
     - Se substituirmos a regra de entrada pela seguinte:
          ingress {
              description      = "Allow SSH from trusted IPs"
              from_port        = 22
              to_port          = 22
              protocol         = "tcp"
              cidr_blocks      = ["IP Público/32"] 
            }
# Usar um Security Group específico para o SSH
 - Devemos criar um grupo separado para o SSH e associar o mesmo a instância;
    - resource "aws_security_group" "ssh_sg" {
  name        = "${var.projeto}-${var.candidato}-ssh-sg"
  description = "Permitir SSH apenas de IPs confiáveis"
  vpc_id      = aws_vpc.main_vpc.id

  ingress {
    description      = "Allow SSH from trusted IPs"
    from_port        = 22
    to_port          = 22
    protocol         = "tcp"
    cidr_blocks      = ["SEU_IP_PUBLICO/32"] # Substitua pelo seu IP público
  }

  tags = {
    Name = "${var.projeto}-${var.candidato}-ssh-sg"
  }
}
- Por fim, devemos adicionar o Security Group á instância: "security_groups = [aws_security_group.main_sg.name, aws_security_group.ssh_sg.name] "
