# Calculadora — Frontend

Interfaz web simple (HTML + JavaScript + nginx) para consumir la API de la calculadora. Esta página no calcula nada por sí misma: le manda los datos a la API por HTTP y muestra lo que responde.

Este repositorio es un **fork** de [`MatyAlts/calculadora-frontend`](https://github.com/MatyAlts/calculadora-frontend), el repositorio base provisto por la cátedra de **Programación 3** (Tecnicatura Universitaria en Programación, UTN FRM) para el Trabajo Práctico Integrador de la unidad de despliegue en un VPS.

## Integrantes del grupo

| Nombre | Legajo |
|---|---|
| Mariano Chirino | 41031 |
| Facundo Quiroga | 52737 |
| Andrés Fabre | 53885 |

## Qué hace

- Formulario con dos valores numéricos y una operación (suma, resta, multiplicación, división, **potencia**).
- Envía la petición a la API vía `fetch`, mostrando el resultado o el mensaje de error correspondiente (por ejemplo, división por cero).
- Lista las últimas operaciones guardadas en el historial (si la API tiene persistencia disponible).

## Cambios propios sobre el repositorio original

- Se agregó el botón de la operación **potencia** (`x²`), consistente con la nueva operación implementada en el backend.

## Configuración

Al arrancar el contenedor, `docker-entrypoint.sh` lee la variable de entorno `API_URL` y genera `config.js` con la URL de la API a consumir. Por eso la misma imagen sirve para cualquier despliegue, sin tocar el código:

| Variable | Descripción |
|---|---|
| `API_URL` | URL completa de la API, sin barra final ni ruta de endpoint. Ej. `https://api.marianochirino.me` |

## Despliegue

Desplegado en un VPS propio administrado con [Easypanel](https://easypanel.io), con HTTPS emitido automáticamente por Let's Encrypt.

- Frontend en producción: `https://calculadora.marianochirino.me`
- API que consume: `https://api.marianochirino.me`

## Nota sobre CORS

Este frontend y la API viven en **orígenes distintos** (subdominios diferentes), por lo que toda petición que no sea una lectura simple (como el `POST` a `/api/calcular`, que envía JSON) dispara una verificación previa (`preflight`, método `OPTIONS`) antes de la petición real. El servidor de la API debe declarar explícitamente este origen como permitido (variable `ORIGENES_PERMITIDOS` del backend) para que el navegador entregue la respuesta.
