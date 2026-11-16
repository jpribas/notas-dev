# M3 – Front end (HTML + fetch)

## Estrutura de uma página (ex.: instrutores)
```html
<!doctype html>
<html lang="pt">
<head><meta charset="utf-8"><title>Instrutores</title>
<link rel="stylesheet" href="styles.css"></head>
<body>
  <a href="index.html">← Menu</a>
  <h1>Instrutores</h1>
  <p id="msg"></p>

  <form id="form">
    <input type="hidden" name="id_instrutor" id="id_instrutor">
    <input name="nome" placeholder="Nome">
    <input name="telefone" placeholder="Telefone">
    <button>Guardar</button>
  </form>

  <table>
    <thead><tr><th>Nome</th><th>Telefone</th><th></th></tr></thead>
    <tbody id="lista"></tbody>
  </table>
  <script src="instrutores.js"></script>
</body></html>
```
- O campo **hidden** com o id decide: vazio = criar (POST), preenchido = editar (PUT).

## JS
```js
const form = document.getElementById('form');
const lista = document.getElementById('lista');
const msg = document.getElementById('msg');

function mostrar(texto, erro) {
  msg.textContent = texto;
  msg.style.color = erro ? 'red' : 'green';
}

async function carregar() {
  const r = await fetch('/api/instrutores');
  const dados = await r.json();
  lista.innerHTML = '';
  for (const i of dados) {
    lista.innerHTML += `<tr>
      <td>${i.nome}</td><td>${i.telefone}</td>
      <td><button onclick="editar(${i.id_instrutor})">Editar</button>
          <button onclick="apagar(${i.id_instrutor})">Apagar</button></td></tr>`;
  }
}

form.addEventListener('submit', async (e) => {
  e.preventDefault();
  const dados = Object.fromEntries(new FormData(form));
  const id = dados.id_instrutor;
  const r = await fetch(id ? '/api/instrutores/' + id : '/api/instrutores', {
    method: id ? 'PUT' : 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(dados)
  });
  const res = await r.json();
  if (!r.ok) return mostrar(res.erro, true);
  mostrar('Guardado com sucesso', false);
  form.reset();
  carregar();
});

async function editar(id) {
  const r = await fetch('/api/instrutores/' + id);
  const i = await r.json();
  form.id_instrutor.value = i.id_instrutor;
  form.nome.value = i.nome;
  form.telefone.value = i.telefone;
}

async function apagar(id) {
  if (!confirm('Apagar?')) return;
  const r = await fetch('/api/instrutores/' + id, { method: 'DELETE' });
  const res = await r.json();
  if (!r.ok) return mostrar(res.erro, true);
  mostrar('Apagado', false);
  carregar();
}

carregar();
```

## `<select>` com dados da BD (ex.: modalidade nas aulas)
```js
const r = await fetch('/api/modalidades');
for (const m of await r.json()) {
  form.id_modalidade.innerHTML += `<option value="${m.id_modalidade}">${m.nome}</option>`;
}
```
Chama isto no arranque, **antes** de editar (para o valor já existir).

## Regras
- Mostra **sempre** mensagem de sucesso e de erro.
- `confirm()` antes de apagar.
- Menu `index.html` com links para todas as páginas.
- Depois de guardar/apagar: `form.reset()` e `carregar()`.
- Não percas tempo com estilo: tabela legível e botões chegam.
