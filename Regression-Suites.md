# 🔁 Regression Suite — Login / Autenticação

**Módulo:** Autenticação de Usuário
**Autor:** Anemilton Leite
**Objetivo:** Garantir que funcionalidades já validadas continuam funcionando após novas alterações (deploys, correções de bugs, novas features)
**Critério de seleção:** casos de **alta prioridade** e/ou que cobrem **fluxos críticos de negócio e segurança**, retirados das suítes manual e de API já documentadas

---

## Critério de entrada na Regression Suite

Um caso entra aqui quando atende a pelo menos um dos critérios:
- É um fluxo core do sistema (ex.: login é pré-requisito de tudo)
- Já foi associado a um bug encontrado anteriormente (alto risco de reincidência)
- É crítico para segurança
- Está automatizado (baixo custo de reexecução, alto valor de rodar sempre)

---

## 📋 Casos selecionados para regressão

| ID | Cenário | Camada | Prioridade | Automatizado | Origem |
|---|---|---|---|:---:|---|
| TC-001 | Login com credenciais válidas | UI | Alta | ✅ | test_case_login.md |
| TC-002 | Login com senha incorreta | UI | Alta | ✅ | test_case_login.md |
| TC-003 | Login com e-mail não cadastrado | UI | Média | ✅ | test_case_login.md |
| TC-006 | Bloqueio após tentativas falhas | UI | Alta | ⬜ (bloqueado por BUG-001) | test_case_login.md |
| AT-001 | Login válido via API | API | Alta | ✅ | api_tests.md |
| AT-002 | Senha incorreta via API | API | Alta | ✅ | api_tests.md |
| AT-005 | Rate limiting via API | API | Alta | ⬜ (bloqueado por BUG-001) | api_tests.md |
| AT-006 | Injeção via payload | API | Alta | ✅ | api_tests.md |
| AT-007 | Token expirado | API | Média | ⬜ | api_tests.md |

---

## Execução

### Quando rodar
- ✅ Antes de todo merge para a branch principal (via CI)
- ✅ Após qualquer alteração no fluxo de autenticação
- ✅ Antes de cada release/deploy em produção
- ✅ Sempre que um bug relacionado a login for corrigido (para confirmar o fix e evitar regressão)

### Como rodar

**UI (Cypress):**
```bash
npx cypress run --spec "cypress/e2e/login.cy.js"
```

**API (Postman/Newman):**
```bash
newman run auth_api_tests.postman_collection.json -e staging.postman_environment.json
```

---

## 📊 Histórico de Execução

| Data | Build | Passou | Falhou | Bloqueado | Observações |
|---|---|:---:|:---:|:---:|---|
| 20/07/2026 | v1.2.3 | 6 | 0 | 3 | TC-006, AT-005 e AT-007 bloqueados/pendentes (ver BUG-001) |

> Atualize esta tabela a cada execução da suíte, seja manual ou via pipeline de CI.

---

### 💡 Notas para o portfólio

- Diferente das suítes de "Test Cases" e "API Tests" (que cobrem o comportamento completo da feature), a **Regression Suite** é deliberadamente enxuta — só o que é crítico o suficiente para justificar reexecução constante
- Casos bloqueados (TC-006, AT-005) permanecem listados aqui **de propósito**: assim que o BUG-001 for corrigido, eles devem ser automatizados e a coluna "Automatizado" atualizada, sem precisar redesenhar a suíte
- Esse arquivo é o mais indicado para linkar num pipeline de CI (GitHub Actions), já que representa o "mínimo necessário" para considerar um build saudável
