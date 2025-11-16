# QA API – Postman (Reqres)

[![API tests](https://github.com/OfficialV1R0/qa-api-postman/actions/workflows/postman.yml/badge.svg)](https://github.com/OfficialV1R0/qa-api-postman/actions/workflows/postman.yml)

Cíl: Ukázka základních API testů nad veřejným API [Reqres](https://reqres.in) s CI běhy (Newman + HTML report).

Obsah repozitáře:
- Kolekce: `collections/reqres.postman_collection.json`
- Environment: `collections/reqres.postman_environment.json`
- CI workflow: `.github/workflows/postman.yml`

Jak spustit lokálně:
1) Importuj kolekci i environment do Postmanu.
2) Spusť Collection Runner.

CI:
- GitHub Actions spouští Newman na každý push do `collections/**`.
- Výstupní HTML report najdeš v artefaktu běhu Actions.
	- Na stránce běhu otevři záložku Summary a stáhni artefakt „postman-report“ (soubor `out/newman.html`).
	- Přímý odkaz na workflow: https://github.com/OfficialV1R0/qa-api-postman/actions/workflows/postman.yml

Scénáře v kolekci (základ):
- GET /users (200, obsahuje `data` a alespoň 1 záznam)
- GET /users/{id} (200; při 401 kvůli chybějícímu API klíči se test nezhroutí)
- POST /register (úspěch)
- POST /register (neúspěch – chybí heslo)
- POST /login (neúspěch – chybí heslo)
- PATCH /users/{id} (200, `updatedAt`)
- DELETE /users/{id} (204)
- DELETE /users/{id} (204; v některých případech může vrátit 401 – test je tolerantní a bere 204 i 401)
  
Autorizace a změny v Reqres:
- Veřejné API Reqres občas vyžaduje hlavičku `x-api-key` pro zápisové operace (POST/PATCH/DELETE) a může vracet `401 {"error": "Missing API key"}`.
- Kolekce proto používá env proměnnou `apiKey` (viz `collections/reqres.postman_environment.json`). Pokud klíč nemáš, testy pro zápisové operace tolerují i `401` a ověřují jen přítomnost chybové hlášky.
- Pokud máš klíč, nastav `apiKey` a běhy by měly vracet 200/204/400 dle scénáře.

Poznámky:
- Kolekci si rozšiř tak, aby pokrývala i `GET /users/{id}`, případně `POST /login`.