# Sistema de Ordens de Serviço

## Objetivo da atividade

Este projeto utiliza **PHP, PDO e MySQL** em um ambiente preparado para **GitHub Codespaces**.

O objetivo é que os alunos criem o banco de dados e toda a sua estrutura a partir da análise do código PHP existente.

O sistema possui funcionalidades para cadastro de clientes, técnicos e ordens de serviço. As tabelas, campos, tipos de dados, chaves e relacionamentos necessários **não são criados automaticamente pelo projeto**.

## Estrutura do projeto

A aplicação está dentro da pasta:

```text
htdocs/
```

Estrutura principal:

```text
.devcontainer/
htdocs/
    index.php
    conecta.php
    cliente.php
    tecnico.php
    ordem_servico.php
README.md
```

## Usando o GitHub Codespaces

1. No GitHub, clique em **Code**.
2. Abra a aba **Codespaces**.
3. Clique em **Create codespace on main**.
4. Aguarde o ambiente abrir no VS Code pelo navegador.

O Codespaces já prepara:

- PHP 8.2;
- Apache;
- PDO para MySQL;
- servidor de banco compatível com MySQL;
- phpMyAdmin;
- extensões úteis do VS Code.

O Apache publica automaticamente os arquivos da pasta `htdocs`.

## Acessando a aplicação e o phpMyAdmin

No Codespaces, abra a porta **80** na aba **Ports**.

A URL principal abre o sistema.

Para acessar o phpMyAdmin, acrescente:

```text
/phpmyadmin
```

ao final da URL da porta 80.

No phpMyAdmin, use:

```text
Usuário: root
Senha: deixe em branco
```

## Preparação do banco de dados

1. Acesse o phpMyAdmin.
2. Abra a área de comandos SQL.
3. Crie um banco de dados com o nome:

```text
sistema_ordens_servico
```

4. Depois de criar o banco, selecione-o no menu lateral do phpMyAdmin.
5. Analise os arquivos PHP da pasta `htdocs` para identificar e criar:

   - as tabelas necessárias;
   - os campos de cada tabela;
   - os tipos de dados adequados;
   - as chaves primárias;
   - as chaves estrangeiras;
   - os relacionamentos entre as tabelas;
   - os campos que devem aceitar valores nulos;
   - o campo que deve gerar automaticamente o número da ordem de serviço.

## Arquivos para análise

Os principais arquivos são:

- `htdocs/cliente.php`
- `htdocs/tecnico.php`
- `htdocs/ordem_servico.php`

Observe principalmente os comandos SQL presentes nesses arquivos, como:

- `INSERT`
- `SELECT`
- `UPDATE`
- `DELETE`
- `JOIN`

Esses comandos fornecem as informações necessárias para descobrir a estrutura esperada pelo sistema.

## Conexão com o banco

O arquivo:

```text
htdocs/conecta.php
```

já está configurado para acessar:

```text
Banco: sistema_ordens_servico
Servidor: localhost
Usuário: root
Senha: vazia
```

O ambiente do Codespaces já utiliza essa mesma configuração.

## Teste da estrutura criada

Quando terminar a criação do banco, teste no sistema:

1. Cadastro de clientes.
2. Edição de clientes.
3. Cadastro de técnicos.
4. Edição de técnicos.
5. Cadastro de ordens de serviço.
6. Edição e fechamento de ordens.
7. Exclusão de registros.

Se ocorrer um erro de banco de dados, analise a mensagem apresentada e revise a estrutura criada no MySQL.
