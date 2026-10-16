# Especificação de Testes de Caixa-Preta (Black-Box Test Design)

| Documento | Versão | Módulos Avaliados | Técnicas Formais Aplicadas |
| :--- | :--- | :--- | :--- |
| **BBD-001** | 1.0 | Autenticação & Checkout | Partição de Equivalência (EP) & Análise de Valor Limite (BVA) |

---

## 1. Visão Geral das Técnicas Aplicadas

Para otimizar o esforço de teste e garantir alta cobertura sem redundância, foram aplicadas as técnicas formais de teste baseadas em especificação (caixa-preta) preconizadas pelo **ISTQB (International Software Testing Qualifications Board)**:

* **Partição de Equivalência (Equivalence Partitioning - EP):** Divisão dos dados de entrada em classes válidas (comportamento aceito) e inválidas (comportamento rejeitado com mensagem de erro). O teste de um único elemento representativo da classe assume a validação de todos os demais membros da mesma partição.
* **Análise de Valor Limite (Boundary Value Analysis - BVA):** Foco nas fronteiras e extremos das classes de equivalência (valores mínimo, imediatamente abaixo do mínimo, máximo e imediatamente acima do máximo), onde estatisticamente reside a maior densidade de defeitos de software.

---

## 2. Módulo de Autenticação (Login)

### 2.1. Regras de Negócio
1. O acesso ao inventário só é concedido mediante credenciais válidas e ativas.
2. Usuários suspensos ou bloqueados (`locked_out_user`) devem ter o acesso impedido.
3. Não é permitida a submissão com campos `Username` ou `Password` em branco.

### 2.2. Tabela de Partição de Equivalência (EP) - Login

| ID | Parâmetro Avaliado | Classe de Entrada | Dado de Teste | Tipo de Classe | Resultado Esperado |
| :---: | :--- | :--- | :--- | :---: | :--- |
| **EP-L01** | Username / Password | Credenciais ativas e corretas | `standard_user` / `secret_sauce` | **Válida** | Login realizado; redirecionamento para `/inventory.html`. |
| **EP-L02** | Username | Usuário com bloqueio cadastral | `locked_out_user` / `secret_sauce` | **Inválida** | Acesso bloqueado; exibir: *"Epic sadface: Sorry, this user has been locked out."* |
| **EP-L03** | Username / Password | Usuário não cadastrado | `usuario_inexistente` / `secret_sauce` | **Inválida** | Acesso bloqueado; exibir: *"Username and password do not match any user in this service."* |
| **EP-L04** | Password | Senha incorreta para usuário válido | `standard_user` / `senha_invalida_123` | **Inválida** | Acesso bloqueado; exibir: *"Username and password do not match any user in this service."* |
| **EP-L05** | Username | Campo obrigatório omitido (vazio) | `""` (vazio) / `secret_sauce` | **Inválida** | Submissão barrada; exibir: *"Epic sadface: Username is required"*. |
| **EP-L06** | Password | Campo obrigatório omitido (vazio) | `standard_user` / `""` (vazio) | **Inválida** | Submissão barrada; exibir: *"Epic sadface: Password is required"*. |

---

## 3. Módulo de Checkout: Informações do Cliente

Ao avançar do carrinho para a etapa de informações pessoais (`/checkout-step-one.html`), o formulário exige:
* `First Name` (Nome)
* `Last Name` (Sobrenome)
* `Postal Code` (CEP / Código Postal)

### 3.1. Regras de Negócio
1. Todos os campos são de preenchimento estritamente obrigatório.
2. A aplicação não deve permitir o envio de campos preenchidos exclusivamente com espaços em branco (*whitespaces*).

### 3.2. Tabela de Partição de Equivalência (EP) - Formulário de Checkout

| ID | Campo Avaliado | Cenário / Entrada | Tipo de Classe | Resultado Esperado |
| :---: | :--- | :--- | :---: | :--- |
| **EP-C01** | Todos os campos | Todos os campos com valores válidos | **Válida** | Formulário aceito; avança para `/checkout-step-two.html`. |
| **EP-C02** | First Name | Campo omitido / vazio (`""`) | **Inválida** | Permanece na rota; exibe: *"Error: First Name is required"*. |
| **EP-C03** | Last Name | Primeiro nome preenchido, Sobrenome vazio | **Inválida** | Permanece na rota; exibe: *"Error: Last Name is required"*. |
| **EP-C04** | Postal Code | Nome e Sobrenome preenchidos, CEP vazio | **Inválida** | Permanece na rota; exibe: *"Error: Postal Code is required"*. |
| **EP-C05** | First Name | Preenchimento apenas com espaços (`"   "`) | **Inválida** | O sistema deve tratar como vazio e exigir caracteres alfanuméricos válidos. |
| **EP-C06** | Postal Code | Inclusão de caracteres especiais (`!@#$%`) | **Inválida** | Barrar caracteres de controle que violem formatos de endereçamento. |

---

## 4. Análise de Valor Limite (BVA): Carrinho de Compras

O inventário do SauceDemo disponibiliza exatamente **6 itens únicos** para aquisição.

* **Fronteira Mínima:** 1 item (quantidade mínima para justificar um pedido de compra comercial).
* **Fronteira Máxima:** 6 itens (catálogo completo adicionado ao carrinho).

### 4.1. Tabela de Análise de Valor Limite (BVA)

| ID | Ponto de Fronteira | Quantidade de Itens | Classificação | Comportamento Esperado |
| :---: | :--- | :---: | :---: | :--- |
| **BVA-01** | Abaixo do Limite Mínimo | **0 itens** | **Inválida (Borda Negativa)** | O botão de Checkout deve estar desabilitado ou barrar o avanço, impedindo checkout sem itens. *(Ponto clássico de defeito de negócio)* |
| **BVA-02** | Limite Mínimo Exato | **1 item** | **Válida (Borda Positiva)** | Contador do carrinho exibe `1`; avanço de checkout autorizado com subtotal correspondente. |
| **BVA-03** | Acima do Mínimo | **2 itens** | **Válida (Interior)** | Contador do carrinho exibe `2`; subtotal reflete a soma exata dos dois produtos. |
| **BVA-04** | Limite Máximo Exato | **6 itens** | **Válida (Borda Positiva)** | Contador exibe `6`; lista do checkout exibe todos os produtos com taxas calculadas. |
| **BVA-05** | Acima do Máximo | **7 itens** | **Inválida (Fora da Fronteira)** | Sistema não deve permitir inclusão de itens duplicados além da disponibilidade em tela. |

---

## 5. Mapeamento de Riscos de Negócio & Defeitos Identificados

* **Anomalia no Teste BVA-01 (Checkout com 0 itens):**
  * **Problema:** A aplicação permite que o usuário avance para o checkout mesmo com o carrinho contendo 0 itens, chegando até a tela final de confirmação de pedido com valor total zerado ($0.00).
  * **Severidade Técnica:** Média.
  * **Impacto de Negócio:** Alto (gera transações fictícias no banco de dados e onera a logística de pedidos).
  * **Ação:** Mapeado para abertura de *Bug Report* prioritário no repositório.