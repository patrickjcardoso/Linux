# Linux

## Material Didático

Você vai encontrar aqui um resumo dos principais comandos sobre linux. Esse materia foi baseado no material didático fornecido pela [LPI](https://learning.lpi.org/pt/).

[Referências: Linux Essentials](https://learning.lpi.org/pt/learning-materials/010-160/)



## Introdução sobre linha de comando (CLI)

As distribuições Linux modernas oferecem uma ampla variedade de interfaces gráficas de usuário,
mas um administrador sempre precisará saber como trabalhar com a linha de comando, ou shell. O
shell é um programa que permite a comunicação por texto entre o sistema operacional e o usuário.
Trata-se normalmente de um programa em modo texto que lê os dados inseridos pelo usuário e os
interpreta na forma de comandos para o sistema.

Existem vários shells diferentes no Linux; estes são apenas alguns exemplos:

• Bourne-again shell (Bash)

• C shell (csh ou tcsh, a versão aprimorada do csh)

• Korn shell (ksh)

• Z shell (zsh)

No Linux, o mais comum é o __shell Bash__.

No Ubuntu ou Debian GNU/Linux, o prompt de um usuário comum provavelmente será assim:

```
patrick@server01:~$
```
O prompt do superusuário terá a seguinte aparência:
```
root@mycomputer:~#
```

### Estrutura da Linha de Comando

A maioria dos comandos na linha de comando segue a mesma estrutura básica:

    comando [opção(ões)/parâmetro(s)...] [argumento(s)...]

Veja, por exemplo, o seguinte comando:
```
$ ls -l /home
```

### Tipos de comportamento dos comandos


## Variáveis

Página 86

As variáveis são espaços de armazenamento para dados, como texto ou números.

Na maioria dos shells do Linux, existem dois tipos de variáveis:

* Variáveis locais

Essas variáveis estão disponíveis apenas para o processo atual do shell. Se você criar uma variável
local e, em seguida, iniciar outro programa nesse shell, a variável não poderá mais ser acessada por esse programa. Como não são herdadas por subprocessos, essas variáveis são chamadas variáveis locais.

* Variáveis de ambiente

Essas variáveis estão disponíveis tanto em uma sessão de shell específica quanto em subprocessos gerados a partir dessa sessão de shell. Essas variáveis podem ser usadas para transmitir dados de configuração para os comandos executados. Como os programas podem acessar essas variáveis, elas são chamadas variáveis de ambiente. A maioria das variáveis de ambiente aparece em letras maiúsculas (por exemplo, PATH, DATE, USER). Um conjunto de variáveis de ambiente padrão fornece, por exemplo, informações sobre o diretório inicial ou o tipo de terminal do usuário. Em certos casos, nos referimos ao conjunto completo de todas as variáveis de ambiente como ambiente.




## Redes


* O comando ip link show exibe uma lista de todas as interfaces de rede disponíveis e seus endereços da camada de link, além de outras informações sobre o tamanho máximo do pacote;


    ip link show

### Configuração IPv4

Existem duas maneiras principais de configurar endereços IPv4 em um computador: atribuir os endereços manualmente ou usar o protocolo de configuração dinâmica de host (Dynamic Host Configuration Protocol ou DHCP) para configuração automática.

Também é possível adicionar endereços IP manualmente a uma interface usando o comando ip addr add. Aqui, adicionamos o endereço 192.168.0.5 à interface ens33. A rede usa a máscara de rede 255.255.255.0, que equivale a /24 em notação CIDR:

    sudo ip addr add 192.168.0.5/255.255.255.0 dev ens33


* Consultado o endereço IPv4:


    ip addr show

### Roteamento IPv4

O comando ip route show lista a tabela atual de roteamento IPv4:

    ip route show

Para adicionar uma rota padrão, basta ter o endereço interno do roteador que será o gateway padrão. Se, por exemplo, o roteador tiver o endereço 192.168.0.1, o seguinte comando o configurará como uma rota padrão:

    sudo ip route add default via 192.168.0.1


### Configurando IPv6

O método para atribuir manualmente um endereço IPv6 a uma interface é o mesmo do IPv4:

    sudo ip addr add 2001:db8:0:abcd:0:0:0:7334/64 dev ens33


### DNS

Os resolvedores usados pelo Linux para procurar dados DNS estão no arquivo de configuração /etc/resolv.conf:

```
/etc/resolv.conf:
$ cat /etc/resolv.conf
search lpi
nameserver 192.168.0.1
```

Quando o resolvedor realiza uma pesquisa de nome, ele primeiro verifica o arquivo /etc/hosts para ver se encontra um endereço para o nome solicitado.

Para realizar uma pesquisa no DNS, use o comando host:

    host learning.lpi.org

O comando dig oferece informações mais detalhadas:

    dig learning.lpi.org


### Sockets

Um socket é um terminal de comunicação para dois programas conversando entre si. Se o socket estiver conectado por uma rede, os programas poderão ser executados em dispositivos diferentes, como um navegador web no laptop de um usuário e um servidor web no data center de uma empresa.

Existem três tipos principais de sockets:

* Sockets Unix

Conecta os processos em execução no mesmo dispositivo.


* Sockets UDP (User Datagram Protocol)

Conectam aplicativos usando um protocolo rápido, mas pouco resistente.

* Sockets TCP (Transmission Control Protocol)

Mais confiáveis que os sockets UDP; por exemplo, eles confirmam a recepção de dados.

O TCP e o UDP usam portas para endereçar vários sockets no mesmo endereço IP. Embora a porta de origem de uma conexão geralmente seja aleatória, as portas de destino são padronizadas para um serviço específico. O HTTP, por exemplo, geralmente é hospedado na porta 80; o HTTPS é executado na porta 443. O SSH, um protocolo para fazer login com segurança em um sistema Linux remoto, escuta na porta 22.


O comando ss permite que um administrador investigue todos os sockets em um computador Linux.

O ss pode ser executado com um conjunto de opções, além de uma expressão de filtro para
limitar as informações mostradas.

Opções:

-4 Restringir o protocolo IPv4

-6 Restringir o protocolo IPv6

-p Processo ques está utilizando o socket

-s Resumo dos sockets

-u UDP


* Lista todos os sockets TCP atualmente em uso:

    
    ss -t




### Atividade sobre Redes

Realize os exercícios Guiados da página 348 da apostila.


## Usuários e Permissões


Para listar as informações atuais de um usuário na linha de comando, basta um comando de duas letras: id. A saída vai depender de seu ISD de login:

    id

Para listar a última vez em que os usuários se logaram no sistema, usamos o comando last:

    last

Os comandos who e w listam somente os logins ativos no sistema:

    who
    
    w


Atualmente, na maioria dos sistemas Linux, o comando su é usado apenas para conceder privilégios de root ao usuário padrão (se um nome de usuário não for especificado após o nome do comando). Embora possa ser usado para alternar para outro usuário, não é uma boa prática: os usuários devem fazer login a partir de outro sistema, pela rede, console físico ou terminal no sistema.


    su -
    
* Nunca mude para o superusuário (root) sem passar o parâmetro do shell de login (-). A menos que haja instruções explícitas do fornecedor do SO ou do software, quando o su for necessário sempre se deve executar su -, com pouquíssimas exceções.

* __su__

Alterna para outro usuário com um shell de login ou executa comandos como aquele usuário,
contornando a senha desse usuário.

* __sudo__

Switch User Do (Substituição de Usuário) ou Superuser Do (Fazer como Superusuário). Caso esteja autorizado, o usuário atual digita sua própria senha (se necessário) para aumentar os privilégios.


### Criando Usuários e Grupos

#### O Arquivo /etc/passwd

É um arquivo legível por todos contendo uma lista de usuários em linhas separadas:

    patrick:x:1000:1000:patrick,1,45,45:/home/patrick:/bin/bash

Cada linha consiste em sete campos delimitados por dois pontos:

* Nome de usuário

O nome usado quando o usuário se loga no sistema.

* Senha

A senha criptografada (ou um x no caso de senhas shadow).

* ID de usuário (UID)

O número de identificação atribuído ao usuário no sistema.

* ID de grupo (GID)

O número do grupo principal do usuário no sistema.

* GECOS

Um campo de comentário opcional, usado para adicionar informações extras sobre o usuário (como o nome completo). O campo pode conter diversas entradas separadas por vírgula.

* Diretório inicial

O caminho absoluto do diretório inicial do usuário.

* Shell

O caminho absoluto do programa que é iniciado automaticamente quando o usuário efetua login no sistema (geralmente um shell interativo como /bin/bash).


#### O arquivo /etc/group

É um arquivo legível por todos contendo uma lista de grupos em linhas separadas:

    docker:x:999:patrick

Cada linha consiste em quatro campos delimitados por dois pontos:

* Nome do grupo

O nome do grupo.

*Senha do grupo

A senha criptografada do grupo (ou um x se forem usadas senhas shadow).

*ID do grupo (GID)

O número de identificação atribuído ao grupo no sistema.

* Lista de membros

Uma lista delimitada por vírgulas de usuários pertencentes ao grupo, exceto aqueles cujo grupo principal é este.



### Adicionando e removendo contas de usuário

No Linux, adicionamos uma nova conta de usuário com o comando useradd e excluímos uma conta de usuário com o comando userdel.


* Exemplo: Adicionar um novo usuário chamado Pedro

```
userdd pedro

# Criar a senha para o novo usuário
passwd pedro
```
* Requer acesso de autoridade


#### Removendo um usuário

    userdel -r pedro

A opção -r remove o diretório do usuário e todo o seu conteúdo.


### Adicionando e excluindo grupos

```
# Criar um grupo
groupadd -g 1090 developer
# A opção -g deste comando cria um grupo com um GID específico.


# Deletar um grupo
groupdel developer
```



### Gerenciando permissões e donos de arquivos.

Página 400 da apostila.









