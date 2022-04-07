<h1 align="center">AWS</h1>
<p align="center">Nesse documento vamos aprender a criar uma instância na AWS usando o EC2, subir uma aplicação Java e tentar acessar de fora da instância.</p>

## Pré-requisitos:
Ter uma conta na AWS:
https://aws.amazon.com/pt/

Faça login no console

Entrar -> Usuário root

<h2 align="center">CRIANDO UMA INSTÂNCIA</h2>

Na barra de prequisa -> **EC2**

Executar instância -> Botão: Executar instância -> Executar instância

### Etapa 1 - Selecione a AMI
Buscar a imagem: Ubuntu Server 20.04 LTS (HVM), SSD Volume Type (Gratuita) -> Selecionar - > Proximo

### Etapa 2 - Escolha um tipo de instância
Manter a instância selecionada na qualificação gratuita -> Proximo

### Etapa 3 - Configure os detalhes da instância
Manter as configurações -> Proximo

#### Etapa 4 - Adicionar armazenamento
Manter as configurações -> Proximo

### Etapa 5 - Adicionar Tags
Adicionar tag -> Chave: Name / Valor: (nome identificador da sua maquina) teste-maquina-aws
Manter o restante das configurações -> Proximo

### Etapa 6 - Configure o security group
Adicionar regra -> Tipo: Regra personalizada TCP / Intervalo de portas: 8080 / Origem: Personalizado 0.0.0.0/0
Verificar e ativar

### Etapa 7 - Análise
Apenas um resumo da máquina criada
Executar

Selecione um par de chaves existente ou crie um novo par de chaves
Criar um novo par de chaves.
Tipo de par de chaves: RSA
Nome do par de chaves: (essas chaves podem ser reutilizados para acessar outras máquinas, tente colocar um nome genérico) teste-par-chave
Fazer download do par de chaves

Coloque o arquivo gerado numa pasta fácil de achar, pois sempre que precisarmos acessar o cli da aws vamos precisar referenciar esse arquivo.
Coloquei na pasta Documentos -> AWS -> teste-par-chave.pem

Executar instâncias

É esperado que tenha essa mensagem em tela: Sua instância está sendo iniciada. Aguarde ela ser finalizada.

Volte para o painel EC2
Clique em: Instâncias (em execução) 

Sua instância deve aparecer nessa listagem, aguarde até que a coluna: Verificação de status tenha o valor de: 2/2 verificações aprovadas

<h2 align="center">ACESSANDO A INSTÂNCIA CRIADA</h2>
Clique com o botão direito sobre a instância criada -> Conectar -> Cliente SSH
Copie o exemplo mostrado no final na página

Abra o terminal:

```bash
# Acesse a pasta onde você salvou o arquivo teste-par-chave.pem
$ ssh -i "teste-par-chave.pem" (endereço do seu servidor)

# Vai aparecer a mensagem
Are you sure you want to continue connecting (yes/no/[fingerprint])? Yes

# O root do seu terminal irá mudar para o root da máquina que você está acessando
ubuntu@ip-172-31-25-181: 
```

<h2 align="center">ATUALIZANDO O AMBIENTE</h2>

Abra o terminal:

```bash
# Vamos atualizar o linux
$ sudo apt update

# Vamos instalar a JDK
$ sudo apt-get install default-jdk --yes

# Vamos instalar o Maven
$ sudo apt-get -y install maven
```

<h2 align="center">SUBINDO UMA APLICAÇÃO JAVA DENTRO DA MAQUINA</h2>
Criamos um repositorio público com um endpoint, apenas para fazer um teste.

Link repositorio:
https://github.com/BotaUmCroppedEEstuda/endpoint-teste

Endpoint local:
http://localhost:8080/qualquerstringaqui

```bash
# Clone o projeto
$ git clone https://github.com/BotaUmCroppedEEstuda/endpoint-teste.git

# Vamos entrar na pasta do projeto
$ cd endpoint-teste

# Rodar o Maven
$ mvn clean install

# Criar a parta target
$ mvn clean install

# Entre dentro da pasta target
$ cd target

# Liste os arquivos dentro da pasta target
$ ls
classes                           generated-sources       maven-status
demo-0.0.1-SNAPSHOT.jar           generated-test-sources  surefire-reports
demo-0.0.1-SNAPSHOT.jar.original  maven-archiver          test-classes

# Vamos executar o arquivo demo-0.0.1-SNAPSHOT.jar
$ java -jar demo-0.0.1-SNAPSHOT.jar

```

Isso vai subir a sua aplicação na máquina.

<h2 align="center">CHAMANDO O ENDPOINT QUE ESTÁ RODANDO DENTRO DA INSTÂNCIA</h2>
Acesse o Painel EC2 e dentro da listagem das instâncias, olhe a coluna: Endereço IPv4 público -> Copie o IP da instância que acabamos de criar.
Cole o IP da sua instância, coloque a porta que declaramos quando criamos nossa instância (8080) e passe qualquer string no endpoint.

Exemplo: http://54.90.76.143:8080/leticia

