# Laboratório de PostgreSQL

O propósito deste repositório é criar um conjunto de VMs para serem usadas como
laboratório de testes e aulas sobre PostgreSQL. Mas ele pode ser facilmente
adaptado para outros propósitos.

## Para o aluno

### Requisitos

* [Vagrant](https://www.vagrantup.com)
* [VirtualBox](https://www.virtualbox.org)

### Preparação

Faça o download do `Vagrantfile` e do `inventario.yml` em um diretório
vazio.

### Como instanciar o laboratório

Dentro do diretório que contém o `Vagrantfile`, execute:

* vagrant up

### Como acessar as máquinas virtuais

As três VMs podem ser acessadas do diretório que contém o `Vagrantfile` com:

* vagrant ssh pg-a
* vagrant ssh pg-1
* vagrant ssh pg-2

Ou, com usuário e senha _vagrant_:

* ssh -l vagrant 192.168.56.20
* ssh -l vagrant 192.168.56.30
* ssh -l vagrant 192.168.56.40

## Para o instrutor

### Requisitos

* [Vagrant](https://www.vagrantup.com)
* [VirtualBox](https://www.virtualbox.org)
* [Packer](https://packer.io)
* [Ansible](https://www.ansible.com)
* [git](https://git-scm.com)

### Preparação

Clone este repositório incluindo seus submódulos (`git clone
--recurse-submodules`).

### Como atualizar a box

* packer build packer.box.json

### Como atualizar a ova

* vagrant up
* vagrant halt
* VBoxManage export pg-a pg-1 pg-2 --ovf20 -o postgresql-lab.ova
* vagrant destroy

### Como instanciar o laboratório em sala de aula

* vagrant up --provision-with local
