# Projeto ASA - IFPB
**Alunos:** Pedro e Vercosa

## Descrição
Infraestrutura automatizada utilizando Vagrant (VirtualBox) e Ansible.

## Componentes:
- **ARQ:** DHCP, DNS (Bind9), NFS Server e LVM (15GB).
- **DB:** MariaDB.
- **APP:** Apache2.
- **CLI:** Host cliente para testes.

## Como rodar:
1. `vagrant up`
2. `ansible-playbook -i inventory.ini provimento.yml`
