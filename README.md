# ec-ia-config

Los catálogos del plano de control de IA de [`ec-demo1`](https://github.com/miguelperezcolom/ec-demo1),
como YAML. Un fichero por entrada; el campo `kind` dice de qué catálogo es.

El plano de control **no clona este repositorio**: lee `ia/` por la API de GitHub y se reconcilia
con cada push, a través de un webhook verificado por HMAC. No hay build y no hay despliegue.

## Las reglas que importan

- **Git posee sólo lo que git creó.** Una entrada que escribe el reconciliador queda registrada como
  gestionada por git: quita su fichero y el siguiente sync la borra. Una entrada hecha en la consola
  no está en ese registro, así que un sync **nunca la toca** — ni la sobreescribe, ni la borra —
  y **un id que la consola ya tiene se salta**, aunque este repo lo nombre.
- **Los secretos nunca entran aquí.** Una entrada `llm` nombra una variable en `credentialEnv`, y el
  plano de control la resuelve de su propio Secret al sincronizar. Este repo dice *cuál* leer, nunca
  su valor.
- **Lo dispara un push, no un reloj.** El poll periódico está apagado por defecto, para que un
  arreglo rápido hecho en la consola sobreviva hasta el siguiente push.

## Estado actual

Los LLMs, los servidores MCP, la fuente RAG y el agente de aquí **reflejan** lo que hoy es propiedad
de la consola: están escritos para que este repo sea la fuente durable el día que se borren de la
consola, pero mientras tanto el reconciliador los salta y los reporta como `skipped`.

La única entrada que git posee de verdad hoy es `ia/budgets/daily-per-user.yaml`, porque ningún id
del catálogo de budgets estaba ocupado.

## El esquema

Los ficheros llevan un modeline que ata el esquema publicado, así que VS Code (extensión YAML de Red
Hat) e IntelliJ dan autocompletado y validación sin configurar nada por máquina.
