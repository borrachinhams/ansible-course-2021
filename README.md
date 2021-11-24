# [Ansible](#ansbile)
  - [Dia 1](#day-1)
  - [Dia 2](#day-2)
  - [Dia 3](#day-3)
  - [Dia 4](#day-4)
  - [Dia 5](#day-5)


> Curso feito em 2021 pela LinuxTips.
Aqui vou adicionar minhas anotações sobre o curso de Ansible

## Day 1

Para o cenário vamos subir 2 maquinas na AWS.
No curso o Jeferson subia uma a mais para o ele executar de lá mesmo o Ansible, no meu caso vou usar o meu notebook mesmo.

### Conectando via SSH

Pulo do gato para conectar via SSH sem ficar passando toda vez a chave com o -i

```bash
ssh-agent bash/zsh #escolher entre bash ou zsh
ssh-add ~/ansible-linuxtips21/ansible.pem
ssh ubuntu@elliot-01
```

### Comandos ad-hoc

Primeiro comando ad-hoc

```bash
ansible -i hosts all -u ubuntu -m ping
```

Instalando pacote via ad-hoc

```bash
ansible -i hosts elliot-01 -u ubuntu -m apt -a "update_cache=yes name=cmatrix state=present" -b
```

Buscando todos os argumentos (ansible_facts)
```bash
ansible -i hosts all -u ubuntu -m setup 
```

### Rodando Playbook

Rodando o meu primeiro playbook

```bash
ansible-playbook -i hosts meu_primeiro_playbook.yml
```

> Referencias
  * [Repositório de Exemplos do Ansible](https://github.com/ansible/ansible-examples)
  * [Documentação Oficial](https://docs.ansible.com/ansible/latest/index.html)


## Day-2

Anotações do dia 2.
Fiz uma cópia da pasta day-1 para day-2. Assim não arrebento com o lab do dia 1.

A partir do dia 2 estaremos mudando o projeto ao invés de dias para o projeto-k8s-cluster. Este será o projeto que vamos trabalhar no decorrer do curso dos 06 dias.
 
O projeto será divido em:
```
- provisioning => Será onde o cluster esta hospedado (AWS/GCP/Azure).
- install_k8s => Instalação do Kubernetes propriamente dito.
- deploy_app => Deploy de uma aplicação de teste.
- extra => Ainda uma surpresa.
```

Para rodar o ansible na AWS é necessário o boto3

```bash
sudo apt install pip -y
pip install boto3
```

> Links importantes
https://www.ansible.com/integrations/cloud/amazon-web-services
https://docs.ansible.com/ansible/latest/scenario_guides/guide_aws.html
https://docs.ansible.com/ansible/latest/modules/list_of_all_modules.html
https://docs.ansible.com/ansible/latest/modules/list_of_cloud_modules.html
https://docs.ansible.com/ansible/latest/modules/ec2_module.html
https://docs.ansible.com/ansible/latest/modules/ec2_group_module.html
https://galaxy.ansible.com/docs/contributing/creating_role.html
https://www.makeareadme.com/


## Day-3

Aqui estaremos iniciando o projeto para instalar o Kubernetes.
No dia 2 provisionamos as instâncias e agora vamos instalar o K8S.

Neste projeto tive problemas para instalar o k8s, ele deu zika na parte do systemd, tive de fazer algumas modificações do projeto original.
Em install_k8s/install-k8s foi adicionado o esquema do systemd.

## Day-4

Neste dia vamos instalar o helm.

Dica de ouro para poder executar apenas uma determinada task.

```bash
ansible-playbook -i hosts main.yml --tags "install_helm3_role,install_monit_tools_role"
```

Da também para mandar um skip(pular) em outras tags, vai depender da sua estrategia.

```bash
ansible-playbook -i hosts main.yml --skip-tags "install_helm3_role,install_monit_tools_role"
```

Se você quiser ver o "plan" do ansible pode usar da seguinte forma
```bash
ansible-playbook -i hosts main.yml --tags "install_helm3_role,install_monit_tools_role" --list-tasks
```

> senha do grafana default: prom-operator

## Day-5

Dia 5 é quase uma sexta, bora lá...
Neste dia estaremos realizando um desafio.

Antes de desafio foi adicionado um handler para dar restart no kubelet.

Bom o desafio era postar todo o código no git, pois bem, aqui estamos.
