# Erros comuns e como resolver

## MySQL / phpMyAdmin
| Erro | Significa | Resolver |
| --- | --- | --- |
| #1452 | FK aponta para um id que não existe | `SELECT * FROM tabela_pai;` e usa os ids reais |
| #1451 | Apagar algo que tem filhos | Apaga primeiro os filhos |
| #1062 | Valor repetido (UNIQUE) | Muda o valor |
| #1064 | Erro de sintaxe | Vírgula a mais/a menos, `);` em falta |
| #1824 | FK para tabela que não existe | Cria a tabela pai antes, ou o nome está mal escrito |
| #1146 | Tabela não existe | Esqueceste o `USE`, ou nome errado |
| #1054 | Coluna desconhecida | Nome de coluna mal escrito |
| #1046 | Nenhuma base de dados selecionada | Faz `USE nome;` |

## Dicas SQL
- Última coluna sem vírgula.
- `AUTO_INCREMENT` só na PK.
- Na FK escreve **o mesmo nome** da PK e a tabela certa (plural).
- A cruz vermelha no editor do phpMyAdmin com `CHECK` é só aviso; corre na mesma.
- Telefone é `VARCHAR`, não `INT`.

## Node
| Problema | Resolver |
| --- | --- |
| `Cannot find module` | `npm install` na pasta do projeto |
| `ECONNREFUSED` | MySQL não está ligado no XAMPP, ou porta errada no `.env` |
| `Access denied` | Utilizador/palavra-passe no `.env` |
| `Unknown database` | Nome da BD no `.env` |
| `EADDRINUSE` | Porta ocupada: muda `PORT` ou fecha o outro Node |
| Página dá 404 | Ficheiro tem de estar em `public/` |
| `fetch` falha | Rota mal escrita (`/api/...`) ou servidor desligado |
| Alterei o código e nada muda | Para o servidor (Ctrl+C) e corre `npm start` |

## Front
- Abre a consola do browser (F12) e vê o separador **Console** e **Network**.
- O `body` do `fetch` tem de ser `JSON.stringify(...)` com `Content-Type: application/json`.
- O nome do campo no formulário tem de ser **igual** ao da coluna.
