# Cypress Testing 🧪

![Status](https://img.shields.io/badge/status-completed-green?style=for-the-badge)
![Cypress](https://img.shields.io/badge/Cypress-E2E%20%2B%20API%20Testing-green?style=for-the-badge&logo=cypress)

End-to-end and API tests for the Adopet platform — covering registration, login, and API validation with custom commands, data-driven testing via fixtures, network stubbing, and Mochawesome HTML reports.

---

## Overview

This project tests the [Adopet](https://adopet-frontend-cypress.vercel.app) pet adoption platform using Cypress. The test suite covers UI flows (registration and login — happy and error paths) and API calls (`cy.request()`). Custom commands abstract repeated interactions, a JSON fixture drives data-driven mass registration tests, and `cy.intercept()` stubs a POST endpoint to force a login failure scenario. Reports are generated in HTML format using Mochawesome.

---

## Architecture

```
cypress-testing/
├── cypress.config.js               # Cypress config — video, Mochawesome reporter, results dir
├── cypress.env.json                # Environment variables — API auth headers
└── cypress/
    ├── e2e/
    │   ├── cadastro-correto.cy.js  # Happy path — successful registration using cy.cadastrar()
    │   ├── cadastro-incorreto.cy.js # Error path — submit empty form, assert validation messages
    │   ├── cadastro-massa.cy.js    # Data-driven — registers 6 users from fixtures/usuarios.json
    │   ├── login-correto.cy.js     # Happy path — successful login using cy.login()
    │   ├── login-incorreto.cy.js   # Error path — empty form + stubbed 400 response via cy.intercept()
    │   └── api-mensagens.cy.js     # API test — GET request with auth headers, assert status + body
    ├── fixtures/
    │   └── usuarios.json           # Test data — 6 users (name, email, password) for mass registration
    └── support/
        ├── commands.js             # Custom commands — cy.login() and cy.cadastrar()
        └── e2e.js                  # Global support file
```

### Key Concepts Applied

| Concept | How it shows up |
|---|---|
| **Custom commands** | `cy.login(email, senha)` and `cy.cadastrar(nome, email, senha)` defined in `commands.js` — reused across test files |
| **Data-driven testing** | `cadastro-massa.cy.js` imports `usuarios.json` and runs the same test for each of 6 users via `forEach` |
| **Fixtures** | `usuarios.json` stores test data separately from test logic |
| **Network stubbing** | `cy.intercept('POST', url, { statusCode: 400 })` forces a server error to test the failure UI |
| **API testing** | `cy.request()` hits the Adopet REST API directly — asserts `status`, `body` not empty, and `msg` property |
| **Environment variables** | `cypress.env.json` stores auth headers used in API tests via `Cypress.env()` |
| **beforeEach** | Navigation and setup shared across tests — avoids repetition |
| **Mochawesome** | Reporter configured in `cypress.config.js` — generates timestamped HTML reports |
| **data-test attributes** | All selectors use `[data-test="..."]` — decoupled from CSS classes |

---

## Test Cases

| File | Scenario |
|---|---|
| `cadastro-correto.cy.js` | Successful registration with valid data |
| `cadastro-incorreto.cy.js` | Empty form submission — validates all required field error messages |
| `cadastro-massa.cy.js` | Registers 6 different users from JSON fixture |
| `login-correto.cy.js` | Successful login and authentication |
| `login-incorreto.cy.js` | Empty form errors + stubbed 400 response failure message |
| `api-mensagens.cy.js` | GET request to Adopet API — asserts 200 status and response body |

---

## How to Run

**Prerequisites:** Node.js installed

```bash
git clone https://github.com/victorhubarb/cypress-testing.git
cd cypress-testing
npm install

# Open interactive runner
npx cypress open

# Run headless with Mochawesome report
npx cypress run
```

Reports are saved as HTML files in `cypress/results/`.

---

## Technologies

- **Cypress** — E2E and API testing framework
- **Mochawesome** — HTML test report generator
- **JavaScript** — test language
- **JSON fixtures** — test data management

---

## Author

**Victor Hugo Barbosa**
CS Student — MassBay Community College
[GitHub](https://github.com/victorhubarb) · [LinkedIn](https://www.linkedin.com/in/victorhbarbosa/)
