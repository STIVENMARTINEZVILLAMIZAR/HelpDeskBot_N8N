# Que entregar del proyecto

Si `n8n` esta instalado localmente, normalmente no se entrega la instalacion completa dentro del repositorio. Lo correcto es entregar los artefactos que permitan entender, revisar y reproducir el sistema.

## Entrega minima recomendada

1. Repositorio con documentacion funcional y tecnica.
2. Export del workflow de `n8n` en formato `.json`.
3. Evidencia del modelo de datos en `Google Sheets`.
4. Capturas o video corto del bot funcionando en Telegram.
5. Instrucciones de configuracion local.

## Archivos que deberias subir al repositorio

```text
README.md
docker-compose.yml
docs/
workflows/helpdeskbot-main.json
.env.example
.gitignore
```

Si tu solucion tiene mas de un workflow, puedes subir varios archivos `.json` dentro de `workflows/`.

## Archivos que NO deberias subir

- Tokens reales de Telegram.
- Credenciales de Google.
- Archivos privados de sesion de `n8n`.
- Bases locales con datos sensibles.
- Variables reales en `.env`.

## Que mostrar en la sustentacion o demo

1. Mensaje de bienvenida del bot.
2. Flujo completo de creacion de solicitud.
3. Registro exitoso en `SOLICITUDES`.
4. Registro de eventos en `LOGS`.
5. Consulta de estado por `id_ticket`.
6. Validacion de usuario activo.
7. Ejemplo de cambio de estado.

## Como justificar que n8n corre en local

Puedes explicarlo asi:

```text
El motor de automatizacion del proyecto es n8n Community Edition ejecutado localmente
en un contenedor Docker.
Para facilitar la revision, el repositorio incluye la documentacion completa,
el archivo exportado del workflow, un archivo docker-compose y las instrucciones
para reproducir la configuracion.
La evidencia funcional se presenta mediante pruebas reales con Telegram y Google Sheets.
```

## Recomendacion para una entrega academica solida

La mejor combinacion suele ser:

1. Repositorio bien documentado.
2. Workflow exportado.
3. Video corto de 1 a 3 minutos con el flujo funcionando.
4. Capturas de `Google Sheets` mostrando `SOLICITUDES` y `LOGS`.

Con eso reduces el riesgo de que la revision falle por temas de red, credenciales o acceso al entorno local.

## Checklist final antes de entregar

- El bot responde desde Telegram.
- El workflow esta exportado en `workflows/`.
- La documentacion explica instalacion, arquitectura y flujo.
- No hay secretos expuestos en el repo.
- El modelo de datos coincide con lo solicitado.
- Se demuestra la trazabilidad en `LOGS`.
