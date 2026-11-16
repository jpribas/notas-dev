# M1 – Do texto ao modelo e ao ER

## 1. Descobrir as tabelas
- Sublinha no texto os **nomes** (coisas que a escola/empresa gere): cada um é uma tabela.
- Ex.: instrutores, modalidades, aulas, alunos, inscrições.
- Nome da tabela: **plural, minúsculas, sem acentos**.

## 2. Colunas
- Cada tabela tem `id_<singular>` como **PK** (ex.: `id_aluno`).
- As restantes colunas vêm do texto ("nome, telefone, ativo...").
- Sem acentos nos nomes (`descricao`, não `descrição`).

## 3. Chaves estrangeiras (FK)
- Para cada par de tabelas pergunta:
  - Um A pode ter **vários** B?
  - Um B pode ter **vários** A?
- Vários só de um lado → **1:N**. A FK fica na tabela do **N** ("muitos") e tem o **mesmo nome** que a PK de origem.
  - Uma modalidade tem várias aulas → `aulas.id_modalidade` FK.
- Vários dos dois lados → **N:M**. Cria uma tabela no meio (ex.: `inscricoes`) com as duas FK e os atributos da ligação. Põe `UNIQUE(fk1, fk2)` se a mesma ligação não pode repetir.

## 4. Escrever o modelo (formato)
```
aulas(id_aula PK, data, hora, vagas_max, estado, id_modalidade FK, id_instrutor FK)
```

## 5. ER em notação Chen (como no Palheiro)
- Retângulo = entidade. Losango = relação. Oval = atributo (PK **sublinhada**).
- FK aparece como oval com "FK" ao lado (como no ER do Palheiro).
- Relação N:M = losango com os seus atributos (id, datas, estado, as 2 FK).
- Traços de cardinalidade nas linhas (pé-de-galinha):
  - `||` um e só um
  - `|o` zero ou um
  - `>|` um ou muitos
  - `>o` zero ou muitos
- Cada aula tem exatamente 1 instrutor → `||` junto ao instrutor, `>o` junto às aulas.
- Ferramenta: draw.io. Exporta para **PNG ou PDF** e junta ao ZIP.
