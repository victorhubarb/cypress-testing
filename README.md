# Cypress Testing

End-to-end and API tests written with Cypress against Adopet, a demo pet-adoption web app ([adopet-frontend-cypress.vercel.app](https://adopet-frontend-cypress.vercel.app)). The tests are mine. The app is a public practice target and not something I built.

> Built as hands-on practice while following an Alura course.

The suite covers sign-up and login on the UI, one authenticated API call, and a stubbed server failure. Headless runs produce Mochawesome HTML reports.

## What's covered

| Spec | What it checks |
|---|---|
| `cadastro-correto` | Registers a new user with valid data |
| `cadastro-incorreto` | Submits the empty form and asserts the three validation messages |
| `cadastro-massa` | Data-driven: registers every user in `fixtures/usuarios.json` with `forEach` |
| `login-correto` | Logs in with valid credentials |
| `login-incorreto` | Empty-form errors, then a login whose `POST` is stubbed with `cy.intercept` to return 400, and asserts the failure message |
| `api-mensagens` | `cy.request` GET with an auth header: checks status 200, a non-empty body and a `msg` property |

## How it's organized

- **Custom commands.** `cy.login(email, senha)` and `cy.cadastrar(nome, email, senha)` live in `cypress/support/commands.js`, so the specs stay short.
- **Selectors.** Everything uses `data-test` attributes, so a CSS change does not break a test.
- **Test data.** The mass-registration users sit in a JSON fixture, separate from the test logic.
- **Reports.** `cypress.config.js` sets the Mochawesome reporter, which writes timestamped HTML to `cypress/results/`. Video recording is on.

## Run it

Requires Node.js 18 or newer.

```bash
git clone https://github.com/victorhubarb/cypress-testing.git
cd cypress-testing
npm install

npx cypress open   # interactive runner
npx cypress run    # headless, writes the report to cypress/results/
```

The specs hit a live site and a free-tier API, so the first request can be slow while it wakes up.

The API spec (`api-mensagens`) needs a bearer token. Copy `cypress.env.example.json` to `cypress.env.json` (git-ignored) and put a valid token in it.

## Known gaps

- `api-mensagens` reads its bearer token from `cypress.env.json`, which is now git-ignored. An old token was committed earlier and is still in the history. It expired in 2024, so the spec fails with an authorization error until you add a fresh token.
- `cadastro-correto` and `cadastro-massa` submit the form but do not assert the outcome yet.
- The tests depend on the live demo app and its seeded users, so they can break when that data changes.
- `npm test` is not wired to Cypress. Use the `npx` commands above.

---

Victor Hugo Barbosa · [GitHub](https://github.com/victorhubarb) · [LinkedIn](https://www.linkedin.com/in/victorhbarbosa/)
