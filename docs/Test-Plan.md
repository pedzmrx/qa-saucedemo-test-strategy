# Plano de Testes (Test Plan) - Plataforma E-commerce SauceDemo

| Documento | Versão | Responsável | Status | Aplicação-Alvo |
| :--- | :--- | :--- | :--- | :--- |
| **TP-001** | 1.0 | QA Team | Em Execução | [SauceDemo (Swag Labs)](https://www.saucedemo.com/) |

---

## 1. Visão Geral & Objetivo

O objetivo deste plano de testes é estabelecer a estratégia, o escopo, a matriz de riscos e os critérios de aceitação para a homologação funcional da aplicação web **SauceDemo (Swag Labs)**. 

A finalidade principal é assegurar que os fluxos críticos de negócio — desde o acesso autenticado até a conclusão do pedido de compra (*checkout*) — operem com integridade, previsibilidade e estabilidade, prevenindo falhas de regras de negócio e regressões em produção.

---

## 2. Escopo dos Testes

### 2.1. Dentro do Escopo (In-Scope)
* **Módulo de Autenticação:**
  * Login com credenciais válidas (`standard_user`).
  * Tratamento de bloqueio cadastral (`locked_out_user`).
  * Validação de mensagens de erro para usuário inválido ou inexistente.
  * Validação de campos obrigatórios não preenchidos (Username / Password em branco).
* **Módulo de Inventário & Catálogo:**
  * Renderização correta dos itens, títulos, descrições e valores monetários.
  * Mecanismos de ordenação da listagem de produtos (Nome A-Z, Z-A; Preço Low-to-High, High-to-Low).
  * Atualização dinâmica do contador numérico (*badge*) do carrinho de compras.
* **Módulo de Carrinho de Compras:**
  * Adição e remoção de produtos a partir da página de catálogo e da página do carrinho.
  * Persistência de estado dos itens ao navegar entre rotas.
* **Módulo de Checkout (Finalização de Compra):**
  * Validação de preenchimento dos dados do comprador (`First Name`, `Last Name`, `Postal Code`).
  * Integridade dos cálculos matemáticos de subtotal (*Item total*), taxas (*Tax*) e valor final (*Total*).
  * Conclusão do fluxo com exibição da mensagem de confirmação (*Thank you for your order!*).

### 2.2. Fora do Escopo (Out-of-Scope)
* Testes não-funcionais de performance, estresse ou carga massiva (ex: simulação de concorrência com 10.000 usuários simultâneos).
* Testes de segurança ofensiva (*PenTest* / injeção de SQL / invasão de infraestrutura).
* Testes de compatibilidade em navegadores obsoletos (ex: Internet Explorer).
* Testes em dispositivos móveis físicos (o escopo atual contempla testes em navegadores Desktop).

---

## 3. Matriz de Avaliação de Riscos

A matriz abaixo orienta a ordem de prioridade de execução dos testes com base no impacto de negócio e na probabilidade de ocorrência de falhas:

| ID | Área / Funcionalidade | Risco Mapeado | Impacto | Probabilidade | Nível de Risco | Estratégia de Mitigação |
| :---: | :--- | :--- | :---: | :---: | :---: | :--- |
| **R-01** | Checkout | Erro no cálculo de taxas ou quebra no fechamento do pedido | Crítico | Baixa | **Alto** | Priorizar cobertura de testes funcionais manuais e validações matemáticas de valores. |
| **R-02** | Autenticação | Usuário bloqueado conseguir acessar o sistema | Alto | Média | **Alto** | Criar casos de teste negativos específicos para perfis de acesso restrito. |
| **R-03** | Carrinho de Compras | Itens adicionados sumirem ao navegar entre telas | Alto | Baixa | **Médio** | Testar persistência de estado do carrinho durante a transição de rotas. |
| **R-04** | Formulário de Checkout | Submissão de dados sem preenchimento de campos obrigatórios | Médio | Média | **Médio** | Aplicar técnicas de partição de equivalência com entradas vazias e caracteres especiais. |
| **R-05** | Catálogo | Ordenação de preços (maior/menor) exibir ordem incorreta | Baixo | Média | **Baixo** | Executar validação de ordenação alfabética e numérica na listagem. |

---

## 4. Critérios de Qualidade (DoD - Definition of Done)

### 4.1. Critérios de Entrada (Entry Criteria)
* Ambiente web do SauceDemo acessível e estável via protocolo HTTPS.
* Credenciais de teste disponibilizadas pelo ambiente de homologação.
* Plano de Testes revisado e aprovado pela equipe de QA.

### 4.2. Critérios de Saída (Exit Criteria)
* 100% dos testes classificados como **Críticos** e **Altos** executados com sucesso.
* Ausência de bugs em aberto com severidade **Blocker** ou **Critical**.
* Todas as falhas de severidade **Medium** ou **Low** devidamente documentadas no repositório de *Issues* com planos de contingência definidos.

---

## 5. Ambiente de Homologação

* **Navegadores Homologados:** Google Chrome (v120+ / Versão estável mais recente) e Mozilla Firefox (v120+).
* **Resolução Padrão de Tela:** 1920x1080 (Full HD Desktop).
* **Ferramental de Inspeção:** Google Chrome DevTools (Abas *Console* para erros de scripts e *Network* para verificação de requisições HTTP).