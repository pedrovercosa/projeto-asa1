# Projeto ASA - DevOps com Vagrant e Ansible

Este projeto automatiza o provisionamento e a configuração de uma infraestrutura de rede local composta por quatro máquinas virtuais utilizando Vagrant e Ansible.

## Integrantes
* **Aluno 1:** Pedro (Matrícula Final: 36)
* **Aluno 2:** Vercosa (Matrícula Final: 36 também, mas fui editando!)

## Disciplina e Professor
* **Disciplina:** Administração de Sistemas Abertos (ASA)
* **Professor:** Leonidas Lima
* **Período:** 2026.1
* **Instituição:** Instituto Federal da Paraíba (IFPB) - Campus João Pessoa

---

## Estrutura da Infraestrutura
O ambiente é composto pelas seguintes máquinas virtuais (`debian/bookworm64`):
1. **arq (192.168.56.136):** Servidor de arquivos, LVM, DHCP, DNS Master e NFS Server.
2. **db (192.168.56.138):** Servidor de Banco de Dados (MariaDB), IP reservado por DHCP.
3. **app (192.168.56.137):** Servidor de Aplicação Web (Apache2), IP reservado por DHCP.
4. **cli (IP Dinâmico via DHCP):** Estação cliente com 1024 MB de RAM para testes de serviços de rede.

---

## Como Executar o Projeto

### Pré-requisitos:
* **VirtualBox** instalado.
* **Vagrant** instalado.
* **Git** instalado.

### Passo a Passo:
1. Clone este repositório na sua máquina:
   ```bash
   git clone <URL_DO_SEU_REPOSITORIO>
   cd <NOME_DA_PASTA>
2. Vagrant up.
