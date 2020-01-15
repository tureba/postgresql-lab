# Laboratório de PostgreSQL

O propósito deste repositório é criar um conjunto de VMs para serem usadas como
laboratório de testes e aulas sobre PostgreSQL. Mas ele pode ser facilmente
adaptado para outros propósitos.

Atenção: A execução do provisionador do vagrant irá preparar o ssh local para o
acesso às VMs. Isso significa que irá criar um par de chaves assimétricas como
.ssh/id\_rsa e .ssh/id\_rsa.pub se não existir, adicionar os hosts como
apelidos no .ssh/config e limpar as assinaturas destes hosts passados caso
existam.

## Requisitos

* Vagrant
* Ansible
* VirtualBox

## Como criar

Dois passos:

- vagrant up
- vagrant reload

O primeiro cria as VMs e configura o acesso da máquina externa para elas.

O segundo reinicia as VMs. Infelizmente ele é necessário para aplicar algumas
das configurações (sshd, kernel etc). Poderíamos fazer o ansible reiniciar na
primeira etapa, mas isso irá causar um erro no `vagrant up`.

## Como acessar

As três VMs podem ser acessadas diretamente pelo usuário com:

* ssh pg-a
* ssh pg-1
* ssh pg-2

