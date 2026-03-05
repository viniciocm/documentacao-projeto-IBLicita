---
id: HU67
title: Excluir cartão de atividade
status: Em Revisão
version: 1.1
last_updated: 2026-03-04
---

# HU67 - Excluir Cartão de Atividade

> **Aplicação:** Sistema de Gestão de Projetos de Parcerias (EPE)  
> **Módulo:** Gestão de Projetos / Kanban

## 🎯 Objetivo da História
Eu, enquanto **usuário autorizado**, quero poder **excluir cartões de atividades**, para remover tarefas que não são mais necessárias, movendo-as para a lixeira sem perder o histórico ou dados vinculados.

---

## 📑 Critérios de Aceitação (CA)

### CA01 — Confirmação de Exclusão
- **Dado que** sou um usuário com permissão de "Excluir cartão";
- **E** existe um cartão visível em uma das colunas;
- **Quando** aciono a opção "Excluir" no menu do cartão;
- **Então** o sistema deve exibir um modal de confirmação: *"Deseja realmente excluir o cartão?"*.

### CA02 — Execução da Exclusão (Soft Delete)
- **Dado que** confirmei a exclusão no modal;
- **Quando** o sistema processa a solicitação;
- **Então** o cartão deve sumir do quadro principal;
- **E** ser movido para a interface de **Lixeira** (conforme HU68).

---

## ⚙️ Regras de Negócio (RN)

| ID | Regra | Descrição Técnica |
| :--- | :--- | :--- |
| **RN01** | **Soft Delete** | Os registros não devem ser apagados fisicamente do banco de dados, apenas marcados como excluídos. |
| **RN02** | **Registro de Auditoria** | Toda exclusão deve gerar uma entrada no log: `{usuário} moveu este cartão para a lixeira em {data} às {hora}`. |
| **RN03** | **Permissão Estrita** | Apenas usuários com o perfil vinculado à ação `Excluir Cartão` podem visualizar esta opção no menu. |

---

## 🎨 Interface e UX
* **Protótipo Navegável:** [Link para o Figma](https://www.figma.com/proto/...)
* **Componente de Alerta:** Utilizar o padrão de modal `danger` (botão de confirmação em vermelho).

!!! info "Nota de Implementação"
    A exclusão de um cartão deve cancelar automaticamente qualquer notificação de e-mail pendente configurada na HU66.

---

## 🔄 Histórico de Alterações

| Data | Autor | Versão | Descrição |
| :--- | :--- | :--- | :--- |
| 2026-03-04 | Vinicio Cerqueira | 1.1 | Ajuste na regra de auditoria para incluir milissegundos. |
| 2025-05-27 | Vinicio Cerqueira | 1.0 | Criação inicial do artefato. |