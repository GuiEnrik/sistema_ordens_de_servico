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
/phpmyadmin/
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
5. Crie a estrutura abaixo necessária para a aplicação PHP em `htdocs/`, considerando as consultas e operações de `cliente.php`, `tecnico.php` e `ordem_servico.php`.

Banco utilizado pela aplicação: `sistema_ordens_servico` (MySQL/MariaDB).

### Tabela `cliente`

| Coluna | Tipo sugerido | Restrições (constraints) | Finalidade |
|---|---|---|---|
| `cliente_cpf` | `VARCHAR(14)` | **PRIMARY KEY**, **NOT NULL** | CPF do cliente; texto para preservar zeros à esquerda e permitir pontuação. |
| `nome` | `VARCHAR(150)` | **NOT NULL** | Nome do cliente. |
| `endereco` | `VARCHAR(255)` | **NOT NULL** | Endereço do cliente. |
| `telefone` | `VARCHAR(20)` | **NOT NULL** | Telefone; texto para permitir DDD, espaços, parênteses e hífen. |

### Tabela `tecnico`

| Coluna | Tipo sugerido | Restrições (constraints) | Finalidade |
|---|---|---|---|
| `tecnico_matricula` | `VARCHAR(20)` | **PRIMARY KEY**, **NOT NULL** | Matrícula do técnico; texto para aceitar letras e zeros à esquerda. |
| `nome` | `VARCHAR(150)` | **NOT NULL** | Nome do técnico. |
| `endereco` | `VARCHAR(255)` | **NOT NULL** | Endereço do técnico. |
| `telefone` | `VARCHAR(20)` | **NOT NULL** | Telefone do técnico. |
| `nivel_escolaridade` | `VARCHAR(80)` | **NOT NULL** | Nível de escolaridade. |

### Tabela `ordem_servico`

| Coluna | Tipo sugerido | Restrições (constraints) | Finalidade |
|---|---|---|---|
| `numero` | `INT UNSIGNED` | **PRIMARY KEY**, **AUTO_INCREMENT**, **NOT NULL** | Identificador único da ordem de serviço, gerado pelo banco. |
| `problema` | `TEXT` | **NOT NULL** | Descrição do problema relatado. |
| `solucao_problema` | `TEXT` | Pode ser **NULL** | Solução registrada; pode ficar vazia enquanto a ordem estiver em aberto. |
| `data_abertura` | `DATE` | **NOT NULL** | Data de abertura da ordem. |
| `data_fechamento` | `DATE` | Pode ser **NULL** | Data de fechamento; nula enquanto não houver fechamento. |
| `cliente_cpf` | `VARCHAR(14)` | **NOT NULL**, **FOREIGN KEY** → `cliente(cliente_cpf)` | Cliente associado à ordem. |
| `tecnico_matricula` | `VARCHAR(20)` | **NOT NULL**, **FOREIGN KEY** → `tecnico(tecnico_matricula)` | Técnico responsável pela ordem. |

### Relacionamentos

- **Cliente 1:N Ordem de Serviço** — um cliente pode possuir várias ordens; cada ordem pertence a um cliente.
- **Técnico 1:N Ordem de Serviço** — um técnico pode ser responsável por várias ordens; cada ordem referencia um técnico.
- `ordem_servico.cliente_cpf` referencia `cliente.cliente_cpf`.
- `ordem_servico.tecnico_matricula` referencia `tecnico.tecnico_matricula`.

## Conexão com o banco

O arquivo:

```text
htdocs/conecta.php
```

já está configurado para acessar:

```text
Servidor: localhost
Usuário: root
Senha: vazia
```

Bastando apenas especificar na variável `$banco` o nome banco de dados criado: **sistema_ordens_servico**

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
