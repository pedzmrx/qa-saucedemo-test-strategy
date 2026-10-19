# Caderno de Casos de Teste (Test Cases) - Plataforma E-commerce SauceDemo

| Documento | Versão | Responsável | Status | Notação Aplicada |
| :--- | :--- | :--- | :--- | :--- |
| **TC-001** | 1.0 | QA Team | Concluído | BDD (Dado / Quando / Então) |

---

## 1. Visão Geral & Estrutura dos Cenários

Este documento detalha os roteiros de teste executáveis derivados da especificação de Caixa-Preta (`BBD-001`). Os cenários utilizam a estrutura **Behavior-Driven Development (BDD)** para padronizar a comunicação entre Engenharia, QA e Produto:

* **Dado que (Given):** Define a pré-condição e o estado inicial do sistema antes da ação.
* **Quando (When):** Descreve a ação ou interação realizada pelo usuário na interface.
* **E (And):** Complementa condições, preenchimentos ou validações adicionais.
* **Então (Then):** Especifica o comportamento esperado e os critérios de aceitação do teste.

---

## 2. Módulo de Autenticação (Login)

### 2.1. Pré-condição Base
* **Dado que** o usuário acessa a página inicial de login da aplicação (`https://www.saucedemo.com/`).

### 2.2. Especificação dos Cenários de Login

* **CT-01: Login com sucesso utilizando credenciais válidas (Ref: EP-L01)**
  * **Tags:** `Smoke` | `Positivo` | `Prioridade: Alta`
  * **Quando** preenche o campo `Username` com `standard_user`
  * **E** preenche o campo `Password` com `secret_sauce`
  * **E** aciona o botão `Login`
  * **Então** o sistema deve autenticar o usuário e redirecionar para a rota `/inventory.html`
  * **E** o cabeçalho principal da página deve exibir o título `Products`.

* **CT-02: Tentativa de login com usuário bloqueado (Ref: EP-L02)**
  * **Tags:** `Negativo` | `Segurança` | `Prioridade: Alta`
  * **Quando** preenche o campo `Username` com `locked_out_user`
  * **E** preenche o campo `Password` com `secret_sauce`
  * **E** aciona o botão `Login`
  * **Então** o sistema deve permanecer na tela inicial de login
  * **E** deve exibir o alerta de bloqueio: *"Epic sadface: Sorry, this user has been locked out."*

* **CT-03: Tentativa de login com senha incorreta (Ref: EP-L04)**
  * **Tags:** `Negativo` | `Prioridade: Média`
  * **Quando** preenche o campo `Username` com `standard_user`
  * **E** preenche o campo `Password` com `senha_invalida_123`
  * **E** aciona o botão `Login`
  * **Então** o sistema deve impedir o acesso ao catálogo
  * **E** deve exibir o alerta: *"Epic sadface: Username and password do not match any user in this service"*.

* **CT-04: Tentativa de login com campo obrigatório em branco (Ref: EP-L05)**
  * **Tags:** `Negativo` | `Prioridade: Média`
  * **Quando** mantém o campo `Username` vazio (`""`)
  * **E** preenche o campo `Password` com `secret_sauce`
  * **E** aciona o botão `Login`
  * **Então** o sistema deve bloquear a submissão do formulário
  * **E** deve exibir a validação: *"Epic sadface: Username is required"*.

---

## 3. Módulo de Catálogo & Carrinho de Compras

### 3.1. Pré-condição Base
* **Dado que** o usuário está autenticado com o perfil `standard_user` e encontra-se na rota `/inventory.html`.

### 3.2. Especificação dos Cenários de Catálogo e Carrinho

* **CT-05: Ordenação de produtos por preço do menor para o maior**
  * **Tags:** `Funcional` | `Prioridade: Baixa`
  * **Quando** seleciona a opção `Price (low to high)` no seletor de filtros
  * **Então** o primeiro item exibido na grade deve ser `Sauce Labs Onesie` com valor de `$7.99`
  * **E** o último item exibido na grade deve ser `Sauce Labs Fleece Jacket` com valor de `$49.99`.

* **CT-06: Adição e remoção de item único no carrinho (Ref: BVA-02)**
  * **Tags:** `Funcional` | `Valor Limite` | `Prioridade: Alta`
  * **Quando** aciona o botão `Add to cart` no produto `Sauce Labs Backpack`
  * **Então** o botão do item deve alternar seu rótulo para `Remove`
  * **E** o ícone superior do carrinho deve exibir o contador numérico `1`
  * **Quando** aciona o botão `Remove` do mesmo produto
  * **Então** o contador numérico do ícone do carrinho deve ser ocultado.

---

## 4. Módulo de Checkout (Finalização de Compra)

### 4.1. Pré-condição Base
* **Dado que** o usuário está autenticado com o perfil `standard_user`.

### 4.2. Especificação dos Cenários de Checkout

* **CT-07: Finalização de pedido ponta a ponta - Caminho Feliz (Ref: EP-C01)**
  * **Tags:** `Smoke` | `E2E` | `Prioridade: Crítica`
  * **Dado que** o usuário adicionou o produto `Sauce Labs Backpack` (`$29.99`) ao carrinho e acessou `/cart.html`
  * **Quando** aciona o botão `Checkout`
  * **E** preenche `First Name` com `Pedro`, `Last Name` com `Silva` e `Postal Code` com `69000-000`
  * **E** aciona o botão `Continue`
  * **Então** o sistema deve avançar para a tela de revisão em `/checkout-step-two.html`
  * **E** deve exibir `Item total: $29.99`, `Tax: $2.40` e `Total: $32.39`
  * **Quando** aciona o botão `Finish`
  * **Então** o sistema deve redirecionar para `/checkout-complete.html` exibindo a mensagem *"Thank you for your order!"*.

* **CT-08: Bloqueio de avanço no checkout por omissão de CEP (Ref: EP-C04)**
  * **Tags:** `Negativo` | `Prioridade: Alta`
  * **Dado que** o usuário possui um item no carrinho e iniciou o preenchimento em `/checkout-step-one.html`
  * **Quando** preenche `First Name` com `Pedro` e `Last Name` com `Silva`
  * **E** mantém o campo `Postal Code` vazio (`""`)
  * **E** aciona o botão `Continue`
  * **Então** o sistema deve barrar o avanço para a etapa de pagamento
  * **E** deve exibir o alerta: *"Error: Postal Code is required"*.

* **CT-09: Tentativa de realizar checkout com carrinho vazio - 0 itens (Ref: BVA-01)**
  * **Tags:** `Negativo` | `Valor Limite` | `Prioridade: Alta`
  * **Dado que** o carrinho de compras possui `0` produtos adicionados
  * **Quando** o usuário navega para a tela do carrinho em `/cart.html`
  * **Então** o botão `Checkout` deve estar desabilitado ou impedir o avanço com alerta de carrinho vazio.
  * **Nota de Homologação:** Cenário reprovado na prática — o sistema permite concluir um pedido de `$0.00` sem produtos.

---

## 5. Matriz de Rastreabilidade e Resultados da Execução

Resumo consolidado após a execução manual dos cenários no ambiente de homologação:

| ID | Cenário de Teste | Ref. Caixa-Preta | Prioridade | Status | Observação / Bug Report |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **CT-01** | Login com credenciais válidas | `EP-L01` | Alta | ✅ **Passou** | Fluxo operando normalmente. |
| **CT-02** | Login com usuário bloqueado | `EP-L02` | Alta | ✅ **Passou** | Bloqueio e alerta validados. |
| **CT-03** | Login com senha incorreta | `EP-L04` | Média | ✅ **Passou** | Alerta exibido corretamente. |
| **CT-04** | Login com usuário vazio | `EP-L05` | Média | ✅ **Passou** | Campo obrigatório validado. |
| **CT-05** | Ordenação de preço (menor para maior) | - | Baixa | ✅ **Passou** | Ordenação numérica íntegra. |
| **CT-06** | Adição e remoção de item | `BVA-02` | Alta | ✅ **Passou** | Contador atualiza em tempo real. |
| **CT-07** | Compra ponta a ponta (E2E) | `EP-C01` | Crítica | ✅ **Passou** | Cálculo de taxa (8%) exato. |
| **CT-08** | Checkout sem Postal Code | `EP-C04` | Alta | ✅ **Passou** | Validação de formulário ativa. |
| **CT-09** | Checkout com carrinho vazio (0 itens) | `BVA-01` | Alta | ❌ **Falhou** | **BUG-01:** Permite fechar compra de $0.00. |