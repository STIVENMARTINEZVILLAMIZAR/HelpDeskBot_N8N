# Instalacion y configuracion local

Este documento describe una forma clara de dejar reproducible el proyecto cuando `n8n Community Edition` corre localmente en Docker, que es el escenario observado en el avance actual del proyecto.

## 1. Requisitos

- `Docker` instalado.
- `Docker Compose` disponible.
- Una cuenta de Telegram.
- Un bot de Telegram creado con `@BotFather`.
- Una cuenta de Google con acceso a `Google Sheets`.
- El documento `HelpDeskBot_DB`.

## 2. Crear el bot de Telegram

1. Abrir Telegram y conversar con `@BotFather`.
2. Ejecutar `/newbot`.
3. Definir nombre y `username` del bot.
4. Guardar el token generado.

Ese token no debe subirse al repositorio.

## 3. Crear la base de datos en Google Sheets

Crear un documento llamado `HelpDeskBot_DB` con tres hojas:

### Hoja `SOLICITUDES`

```text
id_ticket | tipo | prioridad | descripcion | estado | creado_por | fecha_creacion
```

### Hoja `USUARIOS`

```text
telegram_user | nombre | rol | activo
```

### Hoja `LOGS`

```text
timestamp | telegram_user | pantalla | opcion | resultado
```

## 4. Configurar credenciales en n8n

### Credencial de Telegram

- Crear una credencial de Telegram usando el token del bot.
- Asociarla al nodo que recibe y envia mensajes.

### Credencial de Google Sheets

- Configurar acceso con OAuth2 o cuenta de servicio.
- Autorizar lectura y escritura sobre `HelpDeskBot_DB`.

## 5. Variables locales

Usar el archivo [.env.example](/home/zeven/Documentos/HelpDeskBot_N8N/.env.example) como referencia.

Variables recomendadas:

- `TELEGRAM_BOT_TOKEN`
- `GOOGLE_SHEETS_DOCUMENT_ID`
- `N8N_HOST`
- `N8N_PORT`
- `N8N_PROTOCOL`
- `WEBHOOK_URL`
- `N8N_EDITOR_BASE_URL`
- `N8N_SECURE_COOKIE`
- `N8N_PROXY_HOPS`

## 6. Levantar n8n con Docker

1. Crear el archivo local de variables:

```bash
cp .env.example .env
```

2. Ajustar en `.env` los valores reales del entorno.

3. Levantar el contenedor:

```bash
docker compose up -d
```

4. Verificar que el contenedor este en ejecucion:

```bash
docker ps
```

5. Abrir la interfaz en:

```text
http://localhost:5678
```

El archivo [docker-compose.yml](/home/zeven/Documentos/HelpDeskBot_N8N/docker-compose.yml:1) deja este arranque reproducible dentro del repositorio.

## 7. Alternativa equivalente con docker run

El montaje observado en terminal puede expresarse tambien asi:

```bash
docker run -d \
  --name n8n \
  -p 5678:5678 \
  -e N8N_SECURE_COOKIE=false \
  -e N8N_HOST=0.0.0.0 \
  -e N8N_PORT=5678 \
  -e WEBHOOK_URL=http://localhost:5678/ \
  -v n8n_data:/home/node/.n8n \
  --restart always \
  docker.n8n.io/n8nio/n8n
```

Para la entrega, `docker-compose.yml` suele ser mejor que un comando largo porque deja la configuracion mas clara.

## 8. Importar el workflow

1. Abrir `n8n`.
2. Importar el archivo `.json` del workflow principal.
3. Revisar nodos de Telegram, Google Sheets y notificaciones.
4. Asignar credenciales.
5. Guardar y activar el workflow.

La convencion sugerida para guardar el export en este repo esta en [workflows/README.md](/home/zeven/Documentos/HelpDeskBot_N8N/workflows/README.md).

## 9. Pruebas minimas

Antes de entregar, validar como minimo:

1. El bot responde el mensaje de bienvenida.
2. La opcion `1` crea una solicitud completa.
3. La solicitud queda registrada en `SOLICITUDES`.
4. La interaccion queda registrada en `LOGS`.
5. La consulta por ticket responde correctamente.
6. El usuario inactivo no puede operar.

## 10. Nota importante sobre Telegram en local

Si tu implementacion depende de recibir eventos desde internet hacia tu equipo local, debes dejar una de estas dos opciones listas para la revision:

1. Exponer temporalmente `n8n` con un tunel publico.
2. Entregar una demo en video mostrando el flujo funcionando de punta a punta.

Eso evita que la evaluacion dependa de que tu equipo local este siempre accesible externamente.

## 11. Riesgo clave con Telegram Trigger

Si trabajas con `Telegram Trigger`, debes tener presente que `http://localhost:5678/` sirve para abrir el editor desde tu propio equipo, pero no sirve como `WEBHOOK_URL` para que Telegram entregue eventos al bot.

Para que Telegram funcione correctamente contra una instancia local:

1. `n8n` debe quedar accesible desde una URL publica.
2. Esa URL debe configurarse en `WEBHOOK_URL`.
3. Para Telegram, esa URL debe usar `HTTPS`.

Si no haces eso, es normal que el bot no reciba mensajes entrantes aunque el contenedor este arriba y la interfaz web abra bien.

## 12. Nota sobre pruebas y produccion en Telegram

Telegram permite un solo webhook activo por bot. En la practica, eso significa que si alternas entre pruebas en el editor y el workflow publicado, una modalidad puede sobrescribir la otra.

Para evitar confusion durante la validacion final:

1. Usa un bot de pruebas y otro de entrega, o
2. Desactiva temporalmente el workflow publicado cuando vayas a probar desde el editor.
