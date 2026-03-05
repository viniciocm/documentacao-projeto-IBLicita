---
id: req-epe-gestao-kanban
title: Quadro de Gestão de Projetos (Kanban)
description: Especificação funcional do módulo de acompanhamento visual de atividades, organização de colunas, histórico e gestão do ciclo de vida de cartões.
author: Analista de Negócios / Equipa de Produto
last_updated: 2026-03-04
version: 2.1.0
---

# 1. Visão Geral
**Módulo:** Gestão de Projetos (EPE)  
**Objetivo de Negócio:** Fornecer uma interface de controlo visual ágil para acompanhamento, organização e gestão das entregas vinculadas aos projetos, garantindo rastreabilidade total de modificações.

## 1.1. Épicos e Histórias Relacionadas
A presente funcionalidade consolida os requisitos levantados nas seguintes histórias:
* HU61 - Exibir quadro de atividades  
* HU62, HU63, HU64 - Gestão de Colunas (Incluir, Editar, Excluir)
* HU65, HU66, HU67, HU69 - Gestão de Cartões (Incluir, Editar, Excluir, Visualizar)
* HU68, HU70 - Gestão de Lixeira e Restauro  

---

# 2. Regras de Negócio (Business Rules - RN)
Diretrizes inegociáveis que o sistema deve validar na camada de *backend*.

* **RN01 - Estrutura Fixa do Quadro:** A primeira e a última coluna de qualquer quadro possuem posições fixas de controle. O sistema deve impedir que estas colunas sejam reordenadas ou excluídas sob qualquer contexto.
* **RN02 - Bloqueio de Ordenação com Filtros:** O sistema deve bloquear qualquer movimentação manual de cartões ou colunas (*Drag and Drop*) se existir algum critério de filtragem ativo na interface.
* **RN03 - Integridade na Exclusão (Soft Delete):** A exclusão de uma coluna que contenha cartões associados implica o envio automático de todos os cartões nela contidos para a Lixeira, exigindo confirmação explícita do utilizador. Nada deve ser excluído fisicamente da base de dados.
* **RN04 - Retenção e Ordenação da Lixeira:** Cartões na lixeira devem manter o seu estado e ser listados com ordenação decrescente (mais recentemente excluídos no topo). Caso a coluna de origem de um cartão tenha sido apagada, os campos "Coluna" e "Posição" ficarão em branco para reatribuição durante o processo de restauro.
* **RN05 - Dependência de Notificações:** A funcionalidade de "Notificar por e-mail" depende estritamente do preenchimento prévio da "Data limite para entrega". Ao ativá-la, a "Periodicidade" e a atribuição de "Responsáveis" tornam-se de preenchimento obrigatório para salvar a operação.
* **RN06 - Rastreabilidade Contínua (Audit Trail):** Qualquer inclusão, alteração de campos, exclusão, ou restauro de um cartão deve gerar um registo cronológico e imutável no Histórico, armazenando: Utilizador da ação, Campo Modificado (De/Para), Data e Hora.

---

# 3. Comportamento e Interface (UX/UI)
Mapeamento dos elementos de ecrã e microinterações.

* **Limites Visuais:**
  * O contador numérico nas colunas apresenta até 3 dígitos (limite visual exibido como "999+").
  * O limite de responsáveis visíveis em formato miniatura (*pill*) por cartão na visão de quadro é de 5. Ultrapassando este limite, a sexta miniatura agregará o remanescente no formato "+N".
* **Codificação de Cores (Prazos):** O sistema aplica etiquetas com cores condicionais: Verde (Dentro do prazo), Amarela (Até 24h para vencimento) e Vermelha (Prazo expirado).
* **Interface de Filtros:** A pesquisa permite combinações avançadas (Título, Coluna, Data, Descrição, Etiqueta, Responsável e Data de Conclusão). Filtros aplicados manifestam-se no topo da listagem como *tags* individuais que podem ser removidas clicando no ícone "X".
* **Empty States:** Colunas criadas sem cartões devem exibir uma mensagem de chamada à ação incentivando a criação ou o arrastar de uma tarefa para o local.

---

# 4. Critérios de Aceite (Acceptance Criteria - QA)

- [ ] Dado que o utilizador acessa o quadro, todas as colunas cadastradas e os respetivos cartões devem ser renderizados e a primeira/última coluna não devem exibir opção de exclusão ou de mudança de posição no menu.
- [ ] Dado que um filtro dinâmico está aplicado, a funcionalidade de arrastar e soltar elementos (cartões ou colunas) deve estar completamente bloqueada.
- [ ] Dado que o utilizador tenta excluir a coluna intermediária "Validação" que possui 3 cartões, o sistema deve alertar que os 3 cartões irão para a lixeira.
- [ ] Dado que um utilizador acede à Lixeira e restaura um cartão cuja coluna de origem já não existe, o sistema deve obrigá-lo a selecionar uma nova coluna e posição.
- [ ] Dado que um cartão sofreu 5 edições ao longo do ciclo de vida (ex: mudança de título, alteração de prazo, adição de responsável), a aba "Histórico" deve refletir os 5 eventos exatos, em ordem cronológica decrescente e com carregamento assíncrono (sem *refresh* de página).

---

# 5. Controle de Versionamento (Changelog)

| Data | Versão | Autor | Modificação | Escopo / Origem |
| :--- | :--- | :--- | :--- | :--- |
| 2026-03-04 | 1.0.0 | Consultoria / Analista | Criação da Feature Spec unificada (Docs-as-Code). | Ref. Histórias HU61 a HU70 |