# M1 – SQL: criar a base de dados e as tabelas

## Ordem
1. `CREATE DATABASE` + `USE`
2. Tabelas **sem FK** primeiro (as "de cima").
3. Depois as que têm FK (a tabela referenciada tem de existir já).

```sql
CREATE DATABASE bdNome_NomeConcorrente_C27;
USE bdNome_NomeConcorrente_C27;
```
(O nome exato vem no enunciado.)

## Molde de tabela
```sql
CREATE TABLE modalidades (
    id_modalidade INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(50) NOT NULL,
    descricao VARCHAR(100) NOT NULL,
    duracao INT NOT NULL CHECK (duracao > 0),
    preco DECIMAL(6,2) NOT NULL CHECK (preco > 0)
);
```
- Uma coluna por linha, **vírgula no fim, menos na última**.
- `);` a fechar.

## Tipos
| Dados | Tipo |
| --- | --- |
| Texto curto | `VARCHAR(50)` |
| Texto longo | `TEXT` |
| Inteiro | `INT` |
| Dinheiro | `DECIMAL(6,2)` (6 dígitos no total, 2 decimais) |
| Data | `DATE` |
| Hora | `TIME` |
| Sim/Não | `BOOLEAN` |
| Lista fixa de valores | `ENUM('a','b','c')` |
| Telefone, NIF | `VARCHAR` (nunca INT) |

## Restrições
- `NOT NULL` – obrigatório (põe na maioria).
- `UNIQUE` – não repete (email).
- `DEFAULT TRUE` – valor por omissão.
- `CHECK (preco > 0)` – validação.
- `AUTO_INCREMENT PRIMARY KEY` – só na PK.

## Tabela com FK
```sql
CREATE TABLE aulas (
    id_aula INT AUTO_INCREMENT PRIMARY KEY,
    data DATE NOT NULL,
    hora TIME NOT NULL,
    vagas_max INT NOT NULL CHECK (vagas_max > 0),
    estado ENUM('aberta','completa','cancelada') NOT NULL,
    id_modalidade INT NOT NULL,
    id_instrutor INT NOT NULL,
    FOREIGN KEY (id_modalidade) REFERENCES modalidades(id_modalidade),
    FOREIGN KEY (id_instrutor) REFERENCES instrutores(id_instrutor)
);
```
- A FK **não** leva `AUTO_INCREMENT`.
- `FOREIGN KEY (coluna) REFERENCES tabela(coluna)` vai **depois** das colunas.

## Tabela de ligação N:M
```sql
    ...
    UNIQUE (id_aluno, id_aula)
);
```
Impede o mesmo par duas vezes.

## Depois de escrever
Corre tudo e vê no phpMyAdmin se as 5 tabelas aparecem. Se der erro, lê o número do erro (ficheiro 08).
