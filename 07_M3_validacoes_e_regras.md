# M3 – Validações e regras de negócio

As validações têm de estar **no servidor** (o front pode também, mas não basta).
Cada validação do enunciado = um `if` antes do INSERT/UPDATE, a devolver `400` com mensagem.

## Padrões
```js
// obrigatório
if (!nome || nome.trim() === '') return res.status(400).json({ erro: 'Nome obrigatório' });

// número positivo
if (!Number.isInteger(Number(vagas)) || Number(vagas) <= 0)
  return res.status(400).json({ erro: 'Vagas tem de ser inteiro positivo' });

if (!(Number(preco) > 0)) return res.status(400).json({ erro: 'Preço tem de ser maior que 0' });

// valor de uma lista
if (!['aberta','completa','cancelada'].includes(estado))
  return res.status(400).json({ erro: 'Estado inválido' });

// email
if (!/^[^@\s]+@[^@\s]+\.[^@\s]+$/.test(email))
  return res.status(400).json({ erro: 'Email inválido' });

// data e hora
if (!/^\d{4}-\d{2}-\d{2}$/.test(data)) return res.status(400).json({ erro: 'Data inválida' });
if (!/^\d{2}:\d{2}(:\d{2})?$/.test(hora)) return res.status(400).json({ erro: 'Hora inválida' });
```

## Duplicados
- Coluna `UNIQUE` (email) → a BD dá `ER_DUP_ENTRY`; o handler de erros devolve `409`.
- Par `UNIQUE(id_aluno, id_aula)` → mesma coisa (inscrição repetida).

## Existe a FK?
Antes de criar, confirma que o pai existe, ou deixa a BD recusar (`ER_NO_REFERENCED_ROW_2` → `400`).

## Regra de capacidade (vagas)
Conta as inscrições ativas e compara com as vagas:
```js
const [[aula]] = await pool.query('SELECT vagas_max, estado FROM aulas WHERE id_aula = ?', [id_aula]);
if (!aula) return res.status(404).json({ erro: 'Aula não existe' });
if (aula.estado !== 'aberta') return res.status(400).json({ erro: 'Aula não está aberta' });

const [[c]] = await pool.query(
  "SELECT COUNT(*) AS n FROM inscricoes WHERE id_aula = ? AND estado <> 'cancelada'", [id_aula]);
if (c.n >= aula.vagas_max) return res.status(400).json({ erro: 'Aula sem vagas' });
```
(Se o enunciado diz que passa a "completa" quando enche, faz um `UPDATE aulas SET estado='completa'`.)

## Mudar só o estado (PATCH)
```js
app.patch('/api/inscricoes/:id/estado', async (req, res) => {
  const { estado } = req.body;
  if (!['pendente','paga','cancelada'].includes(estado))
    return res.status(400).json({ erro: 'Estado inválido' });
  await pool.query('UPDATE inscricoes SET estado = ? WHERE id_inscricao = ?', [estado, req.params.id]);
  res.json({ ok: true });
});
```

## Como lidar com a lista do enunciado
Faz uma tabela: validação → rota onde entra. Vai riscando uma a uma. As que são `NOT NULL`/`CHECK`/`UNIQUE` já estão cobertas pela BD, mas **devolve mensagem clara** no servidor.
