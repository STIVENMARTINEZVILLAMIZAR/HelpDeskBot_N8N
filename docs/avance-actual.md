# Avance actual del workflow

Este documento resume el estado visible del workflow de HelpDeskBot con base en la captura compartida del editor de `n8n` el `26 de abril de 2026`.

## Infraestructura observada

En las capturas de terminal se observa que:

- Docker esta instalado y operativo.
- El usuario ya puede ejecutar `docker ps`.
- `n8n` corre en un contenedor llamado `n8n`.
- El puerto publicado es `0.0.0.0:5678->5678/tcp`.

Esto confirma que el proyecto usa una instalacion local en contenedor Docker, no una instalacion directa del paquete `n8n` sobre el sistema.

## Workflow observado

Nombre visible:

- `My workflow`

Nodos observados en el flujo:

1. `Telegram Trigger`
2. `Validar Usuario`
3. `Obtener Estado Actual`
4. `Router de Estados`
5. `Enviar Menú Principal`
6. `Actualizar Log a Menu`
7. `Opción 1: Crear?`
8. `Preguntar Tipo`
9. `Actualizar Log a Wizard 1`
10. `Preguntar Prioridad`
11. `Log Tipo y Mover a Prioridad`

## Lo que ya se evidencia implementado

- Entrada de mensajes desde Telegram.
- Validacion inicial del usuario contra Google Sheets.
- Consulta del estado conversacional actual.
- Enrutamiento por estados mediante un router.
- Envio del menu principal por Telegram.
- Registro de eventos en la hoja `LOGS`.
- Inicio del wizard de creacion de solicitud.
- Paso de tipo y avance hacia prioridad.

## Lectura funcional del flujo actual

Con base en la secuencia visible, el comportamiento actual parece ser este:

1. El usuario escribe al bot en Telegram.
2. El workflow valida si el usuario existe o puede operar.
3. Se consulta el estado actual del usuario.
4. Un router decide si se debe mostrar el menu o continuar un paso del wizard.
5. Si el usuario esta en menu, se envia el mensaje principal y se registra en `LOGS`.
6. Si el usuario elige crear solicitud, se pregunta el tipo.
7. Luego se registra ese paso y se prepara el avance hacia prioridad.

## Pendientes para completar el alcance requerido

Segun los requisitos definidos del proyecto, todavia deberian existir o completarse estos bloques:

1. Captura de prioridad con validacion completa.
2. Captura de descripcion de la solicitud.
3. Mensaje de confirmacion antes de guardar.
4. Generacion de `id_ticket`.
5. Registro final en la hoja `SOLICITUDES`.
6. Mensaje de exito con numero de ticket.
7. Consulta de estado de solicitud por `id_ticket`.
8. Opcion `Mis solicitudes`.
9. Opcion `Reportes`.
10. Opcion `Configuracion`.
11. Manejo de cancelacion con opcion `9`.
12. Respuesta controlada para opciones invalidas.

## Observacion tecnica importante

En la captura se ven iconos de alerta roja en varios nodos. Eso normalmente indica que aun hay configuraciones pendientes, campos obligatorios sin completar o credenciales sin enlazar correctamente.

Antes de la entrega conviene revisar, al menos, estos puntos en cada nodo:

- Credencial seleccionada.
- Hoja correcta de Google Sheets.
- Columnas de entrada bien mapeadas.
- Expresiones de Telegram correctas para `chat_id` y `message`.
- Condiciones del router y del nodo `If`.

## Recomendacion inmediata

Para que el proyecto quede presentable y funcional, el siguiente tramo mas importante del workflow es:

1. `Preguntar Prioridad`
2. `Guardar prioridad en LOGS o estado`
3. `Preguntar Descripcion`
4. `Mostrar Confirmacion`
5. `Guardar en SOLICITUDES`
6. `Enviar Ticket Creado`

Ese bloque te completa el caso de uso principal y ya te da una demo muy defendible.
