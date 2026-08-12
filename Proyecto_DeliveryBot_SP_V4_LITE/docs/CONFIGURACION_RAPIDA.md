# CONFIGURACIÓN RÁPIDA

## 1. Sube la base
Sube `data/DeliveryBot_DB_V4_LITE.xlsx` a Google Drive y conviértela en Google Sheets.

## 2. Importa el workflow principal
`workflows/01_DeliveryBot_V4_LITE_Pedidos.json`

## 3. Configura Google Sheets
En todos los nodos Google Sheets:
- selecciona tu credencial;
- cambia `REEMPLAZAR_GOOGLE_SHEET_ID` por tu archivo real.

Hojas:
- Leer MENU -> MENU
- Leer USUARIOS -> USUARIOS
- Leer SESSIONS -> SESSIONS
- Leer PEDIDOS -> PEDIDOS
- Guardar Usuario -> USUARIOS
- Guardar Session -> SESSIONS
- Guardar Pedido -> PEDIDOS
- Actualizar Stock -> MENU
- Actualizar Estado -> PEDIDOS

Columnas de coincidencia:
- Guardar Usuario -> telegram_id
- Guardar Session -> telegram_id
- Actualizar Stock -> id_producto
- Actualizar Estado -> id_pedido

## 4. Configura Telegram
Usa tu misma credencial en:
- Telegram Trigger
- Responder Usuario
- Avisar Cocina
- Notificar Estado Usuario

Reemplaza `REEMPLAZAR_CHAT_COCINA_ID`.

## 5. Prueba
Envía:
`/start`

Luego:
`AGREGAR BEB001 2`

Después:
`/carrito`

Finalmente:
`/confirmar`

## 6. Reporte
Importa `workflows/02_DeliveryBot_V4_LITE_Reporte_Diario.json`.
Configura PEDIDOS, tu credencial Telegram y `REEMPLAZAR_CHAT_ADMIN_ID`.

## IMPORTANTE
Importa la V4 LITE como workflow NUEVO para evitar nombres como Motor Conversacional1, 2 o 3.
