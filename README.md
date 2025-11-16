# QA API – Postman (Reqres)

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

Scénáře v kolekci (základ):
- GET /users (200, obsahuje `data` a alespoň 1 záznam)
- POST /register (úspěch)
- POST /register (neúspěch – chybí heslo)
- PATCH /users/{id} (200, `updatedAt`)
- DELETE /users/{id} (204)
- DELETE /users/{id} (204; v některých případech může vrátit 401 – test je tolerantní a bere 204 i 401)

Poznámky:
- Kolekci si rozšiř tak, aby pokrývala i `GET /users/{id}`, případně `POST /login`.