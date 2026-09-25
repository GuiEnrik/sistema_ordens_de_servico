# Sistema de Ordens de Serviço

## Objetivo da atividade

Este projeto utiliza **PHP, PDO e MySQL**.

O objetivo é que os alunos criem o banco de dados e toda a sua estrutura a partir da análise do código PHP existente.

O sistema possui funcionalidades para cadastro de clientes, técnicos e ordens de serviço. As tabelas, campos, tipos de dados, chaves e relacionamentos necessários **não são criados automaticamente pelo projeto**.

## Preparação do banco de dados

1. Inicie o MySQL pelo ambiente utilizado em aula, como XAMPP, WAMP ou outro servidor MySQL.

2. Acesse o phpMyAdmin.

3. Abra a área de comandos SQL.

4. Crie um banco de dados com o nome:

```text
sistema_ordens_servico
```

5. Depois de criar o banco, selecione-o no menu lateral do phpMyAdmin.

6. Analise os arquivos PHP deste projeto para identificar e criar:

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

- `cliente.php`
- `tecnico.php`
- `ordem_servico.php`

Observe principalmente os comandos SQL presentes nesses arquivos, como:

- `INSERT`
- `SELECT`
- `UPDATE`
- `DELETE`
- `JOIN`

Esses comandos fornecem as informações necessárias para descobrir a estrutura esperada pelo sistema.

## Conexão com o MySQL

O arquivo `conecta.php` já está configurado para tentar acessar o banco:

```text
sistema_ordens_servico
```

A configuração padrão considera:

```text
Servidor: localhost
Usuário: root
Senha: vazia
```

Caso o ambiente utilizado em aula tenha usuário ou senha diferentes, ajuste apenas essas informações no arquivo `conecta.php`.

## Executando o projeto

Depois de criar corretamente o banco e suas tabelas, execute o projeto no servidor PHP utilizado em aula.

Também é possível usar o servidor embutido do PHP:

```bash
php -S localhost:8000
```

Depois acesse:

```text
http://localhost:8000
```

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
