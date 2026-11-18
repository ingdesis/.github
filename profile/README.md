# Snippet Searcher

Plataforma web para guardar, compartir, ejecutar, formatear, lintear y testear snippets de código **PrintScript**.

Trabajo práctico de Ingeniería de Sistemas, Universidad Austral (2026).

## Por dónde empezar

El README de [SnippetSearcher](https://github.com/ingdesis/SnippetSearcher) es la **fuente de verdad** del proyecto: arquitectura, user stories, contrato con la UI y decisiones técnicas.

## Ambientes

| Ambiente | Dirección | Se despliega desde |
|---|---|---|
| dev | https://snippetsearcher-dev.duckdns.org | rama `dev` (`master` en la UI) |
| prod | https://snippetsearcher-prod.duckdns.org | rama `main` |

Para entrar hace falta iniciar sesión: el login es con Auth0.

## Repos

| Repo | Qué es |
|---|---|
| [SnippetSearcher](https://github.com/ingdesis/SnippetSearcher) | Snippet Service (Java + Spring Boot): el servicio central. Su README documenta todo el sistema |
| [SnippetPermission](https://github.com/ingdesis/SnippetPermission) | Permission Service: quién es el owner de cada snippet y con quién se comparte |
| [SnippetPrintScript](https://github.com/ingdesis/SnippetPrintScript) | Servicio de PrintScript: valida, lintea, formatea y ejecuta el código por HTTP y WebSocket |
| [PrintScript](https://github.com/ingdesis/PrintScript) | Librería del lenguaje (lexer, parser, intérprete, linter y formatter). La usa SnippetPrintScript |
| [SnippetSearcher-ui](https://github.com/ingdesis/SnippetSearcher-ui) | UI web (React). Fork de la UI de la cátedra |
| [SnippetSearcher-infra](https://github.com/ingdesis/SnippetSearcher-infra) | Infraestructura: stack de Docker Swarm, proxy con HTTPS, Auth0 y New Relic como código, y los deploys |

## Cómo se conectan

UI → proxy (nginx, HTTPS) → Snippet Service → Permission Service, servicio de PrintScript y Asset Service (el bucket de la cátedra).

Cada servicio tiene su propia base Postgres. El lint y el formateo corren en segundo plano con Redis Streams. Todo se monitorea con New Relic.

## Cómo se trabaja

- Cada tarea va en su rama, con un PR hacia `dev`. Lo probado en dev pasa a `main` con un PR `dev` → `main`.
- Cada push a `dev` o `main` publica la imagen en `ghcr.io/ingdesis/…` y la despliega en la VM del ambiente.
- Nada se cambia a mano en las VMs.
