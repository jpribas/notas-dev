# M3 – Back end (Node + Express + MySQL)

## Estrutura
```
projeto/
  server.js
  package.json
  .env
  public/
    index.html
    aulas.html
    aulas.js
    styles.css
```

## Instalar (1 vez)
```
npm init -y
npm install express mysql2
```
No `package.json` põe `"type": "module"` e:
```json
"scripts": { "start": "node --env-file=.env server.js" }
```

## .env
```
DB_HOST=localhost
DB_PORT=3307
DB_USER=root
DB_PASSWORD=
DB_NAME=nome_da_base_de_dados
PORT=3000
```
(Porta **3307** se o XAMPP estiver nessa. Confirma no XAMPP.)

## server.js – esqueleto
```js
import express from 'express';
import mysql from 'mysql2/promise';

const app = express();
app.use(express.json());
app.use(express.static('public'));

const pool = mysql.createPool({
  host: process.env.DB_HOST,
  port: process.env.DB_PORT,
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_NAME
});

// LISTAR
app.get('/api/instrutores', async (req, res) => {
  const [rows] = await pool.query('SELECT * FROM instrutores ORDER BY nome');
  res.json(rows);
});

// UM
app.get('/api/instrutores/:id', async (req, res) => {
  const [rows] = await pool.query('SELECT * FROM instrutores WHERE id_instrutor = ?', [req.params.id]);
  if (rows.length === 0) return res.status(404).json({ erro: 'Não encontrado' });
  res.json(rows[0]);
});

// CRIAR
app.post('/api/instrutores', async (req, res) => {
  const { nome, telefone } = req.body;
  if (!nome || !telefone) return res.status(400).json({ erro: 'Preenche nome e telefone' });
  const [r] = await pool.query(
    'INSERT INTO instrutores (nome, telefone) VALUES (?, ?)', [nome, telefone]);
  res.status(201).json({ id_instrutor: r.insertId });
});

// EDITAR
app.put('/api/instrutores/:id', async (req, res) => {
  const { nome, telefone } = req.body;
  await pool.query(
    'UPDATE instrutores SET nome = ?, telefone = ? WHERE id_instrutor = ?',
    [nome, telefone, req.params.id]);
  res.json({ ok: true });
});

// APAGAR
app.delete('/api/instrutores/:id', async (req, res) => {
  await pool.query('DELETE FROM instrutores WHERE id_instrutor = ?', [req.params.id]);
  res.json({ ok: true });
});

// ERROS
app.use((err, req, res, next) => {
  if (err.code === 'ER_DUP_ENTRY') return res.status(409).json({ erro: 'Já existe' });
  if (err.code === 'ER_ROW_IS_REFERENCED_2') return res.status(409).json({ erro: 'Tem dados associados, não pode apagar' });
  console.error(err);
  res.status(500).json({ erro: 'Erro no servidor' });
});

app.listen(process.env.PORT, () => console.log('http://localhost:' + process.env.PORT));
```

## Regras
- **Sempre `?`** nas queries e valores em array. Nunca juntar texto na query (SQL injection).
- `GET` lê, `POST` cria, `PUT` edita, `DELETE` apaga, `PATCH` muda um campo (ex.: estado).
- Códigos: `200` ok, `201` criado, `400` dados inválidos, `404` não existe, `409` conflito, `500` erro nosso.
- Resposta de erro: `{ "erro": "mensagem" }`. O front mostra essa mensagem.
- `r.insertId` = id acabado de criar.
- Para listas com JOIN, faz a query com JOIN no `GET` (ex.: aulas com nome da modalidade e do instrutor).
- Uma tabela = um bloco de 5 rotas. Copia e troca nomes.

## Correr
```
npm start
```
Abre `http://localhost:3000`.
