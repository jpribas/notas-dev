# M1 – INSERT e consultas (SELECT)

## INSERT
```sql
INSERT INTO barcos (nome, max_pessoas, disponibilidade) VALUES
('Ria Azul', 12, true),
('Moliceiro 1', 8, true);
```
- Não escrevas o `id` (é automático).
- Texto e datas entre `'aspas'`; números e true/false sem aspas.
- Data: `'2026-11-20'`. Hora: `'10:30:00'`.

### Ordem dos INSERT
Primeiro as tabelas **sem FK**; depois as com FK; as de ligação por último.

### FK: usar ids que existem
Antes de inserir numa tabela com FK, faz `SELECT * FROM tabela_pai;` e vê os ids reais.
Um INSERT falhado **gasta um id** do AUTO_INCREMENT, por isso os ids podem não começar em 1.

## Consultas
Lê o enunciado: o que pede **mostrar** (colunas), de **onde** (tabelas), com que **filtro**, e em que **ordem**.

### Filtrar e ordenar
```sql
SELECT * FROM barcos WHERE disponibilidade = true ORDER BY nome;
SELECT nome, preco FROM percursos ORDER BY preco ASC;   -- DESC = decrescente
```

### JOIN (juntar tabelas)
Junta pela FK:
```sql
SELECT p.data, p.hora, b.nome AS barco, pe.nome AS percurso
FROM passeios p
JOIN barcos b ON p.id_barco = b.id_barco
JOIN percursos pe ON p.id_percurso = pe.id_percurso
WHERE p.disponibilidade = true;
```
- `p`, `b`, `pe` são **alias** das tabelas.
- `AS` dá nome novo à coluna do resultado.
- `JOIN ... ON fk = pk`.

### Contar e somar (GROUP BY)
```sql
SELECT p.id_passeio, COUNT(r.id_reserva) AS n_reservas
FROM passeios p
JOIN reservas r ON r.id_passeio = p.id_passeio
WHERE r.estado <> 'cancelada'
GROUP BY p.id_passeio;
```
- `COUNT`, `SUM`, `AVG`.
- Tudo o que está no SELECT e **não** é função vai no `GROUP BY`.

### Calcular
```sql
SELECT b.max_pessoas - COALESCE(SUM(r.n_pessoas), 0) AS lugares_livres ...
```
`COALESCE(x, 0)` troca NULL por 0.

### "Sem nada" (ex.: clientes sem reservas)
```sql
SELECT c.nome
FROM clientes c
LEFT JOIN reservas r ON r.id_cliente = c.id_cliente
WHERE r.id_reserva IS NULL;
```
`LEFT JOIN` + `IS NULL` = os que **não** têm correspondência.

### Filtro por valor
`WHERE id_instrutor = 1` ou, com nome: JOIN e `WHERE i.nome = 'Rui'`.

## Como trabalhar
Escreve **uma consulta, corre, confirma o resultado**, e só depois a seguinte. Guarda todas num ficheiro `.sql` com um comentário `-- 1.` antes de cada uma.
