# Estrutura do banco de dados — Sistema de Ordens de Serviço

Este documento descreve a estrutura necessária para a aplicação PHP em `htdocs/`, considerando as consultas e operações de `cliente.php`, `tecnico.php` e `ordem_servico.php`.

Banco utilizado pela aplicação: `sistema_ordens_servico` (MySQL/MariaDB).

## 1. Tabela `cliente`

| Coluna | Tipo sugerido | Restrições (constraints) | Finalidade |
|---|---|---|---|
| `cliente_cpf` | `VARCHAR(14)` | **PRIMARY KEY**, **NOT NULL** | CPF do cliente; texto para preservar zeros à esquerda e permitir pontuação. |
| `nome` | `VARCHAR(150)` | **NOT NULL** | Nome do cliente. |
| `endereco` | `VARCHAR(255)` | **NOT NULL** | Endereço do cliente. |
| `telefone` | `VARCHAR(20)` | **NOT NULL** | Telefone; texto para permitir DDD, espaços, parênteses e hífen. |

## 2. Tabela `tecnico`

| Coluna | Tipo sugerido | Restrições (constraints) | Finalidade |
|---|---|---|---|
| `tecnico_matricula` | `VARCHAR(20)` | **PRIMARY KEY**, **NOT NULL** | Matrícula do técnico; texto para aceitar letras e zeros à esquerda. |
| `nome` | `VARCHAR(150)` | **NOT NULL** | Nome do técnico. |
| `endereco` | `VARCHAR(255)` | **NOT NULL** | Endereço do técnico. |
| `telefone` | `VARCHAR(20)` | **NOT NULL** | Telefone do técnico. |
| `nivel_escolaridade` | `VARCHAR(80)` | **NOT NULL** | Nível de escolaridade. |

## 3. Tabela `ordem_servico`

| Coluna | Tipo sugerido | Restrições (constraints) | Finalidade |
|---|---|---|---|
| `numero` | `INT UNSIGNED` | **PRIMARY KEY**, **AUTO_INCREMENT**, **NOT NULL** | Identificador único da ordem de serviço, gerado pelo banco. |
| `problema` | `TEXT` | **NOT NULL** | Descrição do problema relatado. |
| `solucao_problema` | `TEXT` | Pode ser **NULL** | Solução registrada; pode ficar vazia enquanto a ordem estiver em aberto. |
| `data_abertura` | `DATE` | **NOT NULL** | Data de abertura da ordem. |
| `data_fechamento` | `DATE` | Pode ser **NULL** | Data de fechamento; nula enquanto não houver fechamento. |
| `cliente_cpf` | `VARCHAR(14)` | **NOT NULL**, **FOREIGN KEY** → `cliente(cliente_cpf)` | Cliente associado à ordem. |
| `tecnico_matricula` | `VARCHAR(20)` | **NOT NULL**, **FOREIGN KEY** → `tecnico(tecnico_matricula)` | Técnico responsável pela ordem. |

## Relacionamentos

- **Cliente 1:N Ordem de Serviço** — um cliente pode possuir várias ordens; cada ordem pertence a um cliente.
- **Técnico 1:N Ordem de Serviço** — um técnico pode ser responsável por várias ordens; cada ordem referencia um técnico.
- `ordem_servico.cliente_cpf` referencia `cliente.cliente_cpf`.
- `ordem_servico.tecnico_matricula` referencia `tecnico.tecnico_matricula`.

## SQL de referência (MySQL/MariaDB)

O SQL abaixo cria as tabelas com tipos e constraints compatíveis com as operações atualmente implementadas na aplicação.

```sql
CREATE DATABASE IF NOT EXISTS sistema_ordens_servico
  CHARACTER SET utf8mb4
  COLLATE utf8mb4_unicode_ci;

USE sistema_ordens_servico;

CREATE TABLE cliente (
    cliente_cpf VARCHAR(14) NOT NULL,
    nome VARCHAR(150) NOT NULL,
    endereco VARCHAR(255) NOT NULL,
    telefone VARCHAR(20) NOT NULL,
    CONSTRAINT pk_cliente PRIMARY KEY (cliente_cpf)
) ENGINE=InnoDB;

CREATE TABLE tecnico (
    tecnico_matricula VARCHAR(20) NOT NULL,
    nome VARCHAR(150) NOT NULL,
    endereco VARCHAR(255) NOT NULL,
    telefone VARCHAR(20) NOT NULL,
    nivel_escolaridade VARCHAR(80) NOT NULL,
    CONSTRAINT pk_tecnico PRIMARY KEY (tecnico_matricula)
) ENGINE=InnoDB;

CREATE TABLE ordem_servico (
    numero INT UNSIGNED NOT NULL AUTO_INCREMENT,
    problema TEXT NOT NULL,
    solucao_problema TEXT NULL,
    data_abertura DATE NOT NULL,
    data_fechamento DATE NULL,
    cliente_cpf VARCHAR(14) NOT NULL,
    tecnico_matricula VARCHAR(20) NOT NULL,
    CONSTRAINT pk_ordem_servico PRIMARY KEY (numero),
    CONSTRAINT fk_ordem_cliente
        FOREIGN KEY (cliente_cpf)
        REFERENCES cliente (cliente_cpf)
        ON UPDATE CASCADE
        ON DELETE RESTRICT,
    CONSTRAINT fk_ordem_tecnico
        FOREIGN KEY (tecnico_matricula)
        REFERENCES tecnico (tecnico_matricula)
        ON UPDATE CASCADE
        ON DELETE RESTRICT,
    CONSTRAINT chk_datas_ordem
        CHECK (data_fechamento IS NULL OR data_fechamento >= data_abertura)
) ENGINE=InnoDB;
```

## Observações importantes

1. **CPF e matrícula são texto, não números.** Isso evita perder zeros à esquerda e permite armazenar pontuação no CPF, caso seja digitado.
2. **A aplicação não informa `numero` ao inserir uma ordem.** Por isso, `numero` precisa ser `AUTO_INCREMENT).
3. **As chaves estrangeiras exigem que o cliente e o técnico já existam** antes de cadastrar uma ordem de serviço.
4. **`ON DELETE RESTRICT`** impede excluir cliente ou técnico que ainda tenha ordens relacionadas. A aplicação atual tenta excluir diretamente; nesse caso, o banco recusará a exclusão até que as ordens relacionadas sejam removidas ou reassociadas.
5. O `CHECK` de datas é suportado nas versões atuais do MySQL e MariaDB. Se a versão do ambiente não o aplicar, valide essa regra também na aplicação.
6. Os campos obrigatórios acompanham os formulários atuais. A solução e a data de fechamento aceitam `NULL`, pois os formulários permitem que a ordem permaneça em aberto.

> Este documento descreve a estrutura do banco; não executa o SQL no servidor de banco de dados. Antes de aplicar em uma base que já contenha dados, confira a estrutura existente e faça backup.
