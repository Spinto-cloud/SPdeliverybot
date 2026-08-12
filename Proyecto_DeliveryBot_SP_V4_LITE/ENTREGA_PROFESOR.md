# ENTREGA DEL PROYECTO – DELIVERYBOT

## 1. Repositorio GitHub

Nombre requerido:

`Proyecto_DeliveryBot_PintoSergio`

URL:

`PEGAR_AQUI_URL_DEL_REPOSITORIO_GITHUB`

---

## 2. Documentación técnica

Archivo principal:

`README.md`

Documentación complementaria:

- `docs/Manual_Configuracion.md`
- `docs/Manual_Usuario.md`
- `docs/Casos_Prueba.md`

---

## 3. Workflows n8n

### Workflow principal
`workflows/01_DeliveryBot_Pedidos_Estados.json`

Incluye:
- Telegram Trigger.
- Registro/actualización de usuarios.
- Consulta de menú.
- Persistencia del carrito.
- Confirmación de pedidos.
- Actualización de inventario.
- Notificación a cocina.
- Gestión de estados.
- Notificación al usuario.

### Workflow de reporte
`workflows/02_DeliveryBot_Reporte_Diario.json`

Incluye:
- Schedule Trigger.
- Lectura de pedidos.
- Cálculo de indicadores.
- Envío del reporte al administrador.

---

## 4. Google Sheets

URL:

`PEGAR_AQUI_URL_PUBLICA_GOOGLE_SHEETS`

El documento debe contener:

- MENU
- USUARIOS
- SESSIONS
- PEDIDOS

Permiso recomendado para evaluación:

**Cualquier persona con el enlace → Lector**

---

## 5. Datos de prueba

El repositorio incluye:

`data/DeliveryBot_DB.xlsx`

Este archivo contiene la estructura y datos de prueba del proyecto.

---

## 6. Evidencias recomendadas

Antes de entregar, agrega a `docs/Evidencias/`:

1. Menú mostrado en Telegram.
2. Producto agregado al carrito.
3. Pedido confirmado.
4. Pedido recibido por cocina.
5. Cambio de estado.
6. Reporte diario.
