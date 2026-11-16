# M2 – Diagramas

Ferramenta: draw.io (ou o que o enunciado indicar). Exporta cada diagrama para **PNG/PDF**.
Antes de desenhar, lê o enunciado: **quem** usa o sistema e **o que** faz.

## 1. Diagrama de casos de uso (UML)
- **Atores** (bonecos, fora do retângulo): quem usa. Ex.: Administrador, Cliente.
- **Retângulo grande** = o sistema (com o nome dele em cima).
- **Elipses** dentro = o que o ator faz. Verbo no infinitivo: "Gerir barcos", "Criar reserva".
- Linha reta liga ator → caso de uso.
- Se o enunciado só tem um tipo de utilizador, só há um ator.
- Dica: para cada ecrã/funcionalidade do M3 há um caso de uso.

## 2. Fluxograma
| Forma | Significado |
| --- | --- |
| Oval | Início / Fim |
| Retângulo | Passo / ação |
| Losango | Decisão (Sim/Não) |
| Paralelogramo | Entrada/saída de dados |
- Setas sempre com sentido. Cada losango tem **duas** saídas rotuladas.
- Desenha **um processo** concreto do enunciado (ex.: "criar reserva").
- Modelo:
  1. Início
  2. Preencher formulário
  3. Dados válidos? **Não** → mostrar erro → volta ao passo 2
  4. **Sim** → há lugares livres? **Não** → mostrar erro; **Sim** → guardar na BD
  5. Mostrar mensagem de sucesso → Fim

## 3. Wireframes
- Esboços simples a preto e branco (retângulos, texto, botões), sem cores nem design.
- Faz **um por ecrã** pedido: lista, formulário, menu principal.
- Cada ecrã: título, tabela ou campos, botões (Guardar, Editar, Apagar).
- Mostra os campos que a BD tem (ex.: formulário de aula com data, hora, modalidade, instrutor, vagas, estado).
- Põe o nome do ecrã em cima.

## Entrega
Junta tudo numa pasta `M2` e faz o ZIP com o nome do enunciado (ex.: `WSC27_NomeConcorrente_M2`).
