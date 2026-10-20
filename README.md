# Estratégia de QA, Testes de Caixa-Preta & Gestão de Defeitos — SauceDemo

| Projeto | Metodologia Base | Notação de Cenários | Status da Homologação | Aplicação-Alvo |
| :--- | :--- | :--- | :--- | :--- |
| **QA Test Strategy** | ISTQB CTFL v4.0 | BDD (Gherkin) | Concluído (Com Ressalvas) | [SauceDemo (Swag Labs)](https://www.saucedemo.com/) |

---

## 1. Visão Geral do Projeto

Este repositório documenta o ciclo completo de **Engenharia de Qualidade de Software (QA)** aplicado à homologação funcional do e-commerce **SauceDemo (Swag Labs)**. 

O projeto simula um ambiente real de engenharia ágil, estruturando desde o planejamento estratégico e mapeamento de riscos de negócio até a modelagem matemática de dados de entrada (Caixa-Preta), execução de roteiros em BDD e reporte técnico de defeitos com evidências visuais.

---

## 2. Arquitetura da Documentação & Navegação Rápida

A estrutura do repositório está dividida nos quatro pilares fundamentais do ciclo de vida de testes (*STLC - Software Testing Life Cycle*):

| Ordem | Artefato / Documento | Código | Foco Técnico e Entregáveis |
| :---: | :--- | :---: | :--- |
| **01** | [**Plano de Testes (Test Plan)**](./docs/Test-Plan.md) | `TP-001` | Definição de escopo (*In-Scope* / *Out-of-Scope*), Matriz de Riscos de Negócio e Critérios de Aceite (*DoD*). |
| **02** | [**Design de Caixa-Preta (Black-Box)**](./docs/Black-Box-Design.md) | `BBD-001` | Modelagem de dados com **Partição de Equivalência (EP)** e **Análise de Valor Limite (BVA)** nos módulos críticos. |
| **03** | [**Caderno de Casos de Teste (BDD)**](./docs/Test-Cases.md) | `TC-001` | Roteiros estruturados em `Dado que / Quando / Então` e Matriz de Rastreabilidade de execução. |
| **04** | [**Relatório de Defeitos (Bug Reports)**](./docs/Bug-Reports.md) | `BR-001` | Documentação técnica de 3 falhas reais com classificação de *Severidade vs. Prioridade*, passos de reprodução e prints. |

### Estrutura de Diretórios do Repositório

* 📁 **`docs/`** — Documentação técnica e especificações de engenharia de testes:
  * 📄 [`Test-Plan.md`](./docs/Test-Plan.md)
  * 📄 [`Black-Box-Design.md`](./docs/Black-Box-Design.md)
  * 📄 [`Test-Cases.md`](./docs/Test-Cases.md)
  * 📄 [`Bug-Reports.md`](./docs/Bug-Reports.md)
* 📁 **`evidence/`** — Capturas de tela (*screenshots*) comprovando as anomalias reportadas:
  * 🖼️ [`bug-01-checkout-vazio.png`](./evidence/bug-01-checkout-vazio.png)
  * 🖼️ [`bug-02-validacao-checkout.png`](./evidence/bug-02-validacao-checkout.png)
  * 🖼️ [`bug-03-reset-app-state.png`](./evidence/bug-03-reset-app-state.png)
* 📄 **`README.md`** — Painel executivo e consolidação de métricas de qualidade.

---

## 3. Painel Executivo de Resultados (QA Metrics)

Consolidado quantitativo após a execução da suíte de testes planejada:

| Métrica Avaliada | Resultado | Percentual / Detalhe |
| :--- | :---: | :--- |
| **Total de Casos de Teste Planejados** | `9` | `CT-01` ao `CT-09` (100% Executados) |
| **Casos de Teste Aprovados (Passed)** | `8` | ✅ **88.9%** de aprovação nos fluxos mapeados |
| **Casos de Teste Reprovados (Failed)** | `1` | ❌ **11.1%** (`CT-09` - Falha de limite mínimo no carrinho) |
| **Total de Defeitos Reportados (Bugs)** | `3` | Identificados via Valor Limite, Equivalência e Teste Exploratório |

### Resumo de Defeitos Encontrados

| ID | Descrição Resumida da Anomalia | Severidade | Prioridade | Relatório Detalhado |
| :---: | :--- | :---: | :---: | :---: |
| **BUG-01** | Sistema permite finalizar compra (Checkout) com carrinho vazio (`$0.00`) | **High** | **P1** | [Ver Evidência](./docs/Bug-Reports.md#21-bug-01-sistema-permite-finalizar-compra-checkout-com-carrinho-vazio-000) |
| **BUG-02** | Formulário de Checkout aceita espaços em branco e caracteres inválidos no CEP | **Medium** | **P2** | [Ver Evidência](./docs/Bug-Reports.md#22-bug-02-formulário-de-checkout-aceita-espaços-em-branco-e-caracteres-inválidos-no-cep) |
| **BUG-03** | Opção *Reset App State* limpa o carrinho mas não reseta o botão *Remove* | **Low** | **P3** | [Ver Evidência](./docs/Bug-Reports.md#23-bug-03-opção-reset-app-state-limpa-o-carrinho-mas-não-atualiza-o-botão-remove-no-catálogo) |

---

## 4. Parecer Técnico de Qualidade (Quality Gate)

Com base nos **Critérios de Saída (Exit Criteria)** estabelecidos no [Plano de Testes (`TP-001`)](./docs/Test-Plan.md#42-critérios-de-saída-exit-criteria):

* **Fluxos Principais (Caminho Feliz):** A autenticação padrão, ordenação de catálogo, adição de itens e cálculo matemático de impostos (8%) no checkout operam com **100% de precisão**.
* **Ressalva de Negócio (`BUG-01`):** A presença de uma falha de severidade **Alta (P1)** na fronteira inferior do carrinho (`0` itens) viola a regra de negócio de faturamento mínimo. Recomenda-se o bloqueio do botão `Checkout` quando o estado do carrinho estiver vazio antes da liberação definitiva em produção.

---

## 5. Competências e Ferramentas Aplicadas

* **Engenharia de Testes (ISTQB):** Planejamento de testes, análise de riscos, Partição de Equivalência (EP) e Análise de Valor Limite (BVA).
* **Especificação Funcional:** Escrita de cenários em formato BDD (*Behavior-Driven Development* / Gherkin).
* **Gestão de Defeitos:** Triagem de severidade vs. prioridade, rastreabilidade de requisitos e documentação de evidências.
* **Ambiente & Inspeção:** Google Chrome DevTools, Git e documentação técnica estruturada em Markdown.