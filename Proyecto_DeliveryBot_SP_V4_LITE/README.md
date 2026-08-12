# DeliveryBot – Gestión de Pedidos Internos de Cafetería

## Descripción

**DeliveryBot** es una solución de automatización construida con **n8n, Telegram y Google Sheets** para gestionar pedidos internos de cafetería en oficinas, universidades y centros de trabajo.

El sistema permite registrar usuarios, consultar el menú, construir un carrito, confirmar pedidos, validar y descontar inventario, consultar el estado de una orden y generar reportes automáticos de ventas para administración.

Esta versión corresponde al **MVP final del proyecto**, diseñado para ser sencillo de implementar, mantener y demostrar.

---

## Objetivos

- Implementar un sistema digital de pedidos mediante Telegram.
- Registrar los usuarios que interactúan con el bot.
- Centralizar menú, inventario, sesiones y pedidos en Google Sheets.
- Calcular automáticamente el total de cada pedido.
- Generar identificadores únicos para las órdenes.
- Descontar automáticamente el inventario al confirmar pedidos.
- Gestionar los estados: **Recibido, Preparación, En camino y Entregado**.
- Notificar a cocina cuando se genera un pedido.
- Permitir al usuario consultar el estado e historial de sus órdenes.
- Generar un reporte diario con indicadores de ventas.

---

## Tecnologías utilizadas

- **n8n** – Motor de automatización.
- **Telegram Bot API** – Canal de interacción con usuario, cocina y administrador.
- **Google Sheets** – Base de datos operativa.
- **JavaScript** – Lógica de negocio dentro de nodos Code de n8n.

---

## Arquitectura

```text
                      TELEGRAM
                          |
                  Telegram Trigger
                          |
                  Normalizar Evento
                          |
          +---------------+---------------+
          |               |               |
        MENU           USUARIOS        SESSIONS
          |               |               |
          +-----------+---+---------------+
                      |
                   PEDIDOS
                      |
              Sincronizar Datos
                      |
             Motor DeliveryBot Lite
             /       |        |        \
            /        |        |         \
       Usuario    Carrito   Pedido     Estado
                      |        |
                   Stock     Cocina
```

La solución está dividida en **dos workflows modulares**:

1. **Pedidos y Estados:** administra la operación transaccional del bot.
2. **Reporte Diario:** calcula y envía automáticamente indicadores de gestión.

---

## Modelo de datos

La base de datos utiliza cuatro hojas de Google Sheets.

### MENU

| Campo | Descripción |
|---|---|
| id_producto | Código único del producto |
| nombre | Nombre comercial |
| descripcion | Descripción del producto |
| precio | Precio de venta |
| categoria | Bebidas, Comidas o Snacks |
| stock | Inventario disponible |

### USUARIOS

| Campo | Descripción |
|---|---|
| telegram_id | Identificador único de Telegram |
| nombre_completo | Nombre del usuario |
| departamento_oficina | Lugar de entrega |
| puntos_lealtad | Puntos acumulados |

### SESSIONS

| Campo | Descripción |
|---|---|
| telegram_id | Identificador del usuario |
| pantalla_actual | Estado básico de la sesión |
| carrito_temporal | Carrito almacenado en JSON |
| ultimo_cambio | Fecha/hora de actualización |

### PEDIDOS

| Campo | Descripción |
|---|---|
| id_pedido | Número único de orden |
| id_usuario | Telegram ID del cliente |
| detalles_pedido | Detalle en JSON |
| total_pago | Total de la orden |
| estado | Estado actual |
| fecha | Fecha del pedido |
| hora | Hora del pedido |

---

## Uso del bot

### Consultar menú

```text
/start
```

o:

```text
/menu
```

### Agregar productos

Formato:

```text
AGREGAR <codigo_producto> <cantidad>
```

Ejemplo:

```text
AGREGAR BEB001 2
```

### Consultar carrito

```text
/carrito
```

### Confirmar pedido

```text
/confirmar
```

Al confirmar, DeliveryBot:

1. valida el inventario;
2. calcula el total;
3. genera un ID de pedido;
4. registra la orden;
5. descuenta el stock;
6. actualiza puntos de lealtad;
7. vacía el carrito;
8. notifica a cocina.

### Vaciar carrito

```text
/vaciar
```

### Consultar estado

```text
/estado
```

### Consultar historial

```text
/historial
```

---

## Gestión de estados por cocina

La cocina puede actualizar una orden con:

```text
ESTADO <id_pedido> PREPARACION
```

Ejemplo:

```text
ESTADO DB-20260812-228725 PREPARACION
```

Otros estados:

```text
ESTADO DB-20260812-228725 EN_CAMINO
ESTADO DB-20260812-228725 ENTREGADO
ESTADO DB-20260812-228725 CANCELADO
```

Cuando el estado cambia, DeliveryBot actualiza la fila correspondiente en `PEDIDOS` y envía una notificación al usuario.

---

## Reporte diario

El workflow `02_DeliveryBot_Reporte_Diario.json` se ejecuta automáticamente a las **20:00**, con zona horaria:

```text
America/Bogota
```

Calcula:

- Total vendido.
- Número de pedidos.
- Ticket promedio.
- Producto más vendido.
- Hora pico.

Ejemplo:

```text
📊 REPORTE DIARIO DELIVERYBOT

Fecha: 2026-08-12
💰 Total vendido: $399.000
🧾 Número de pedidos: 8
🎫 Ticket promedio: $49.875
⭐ Producto más vendido: Café americano
🕒 Hora pico: 12:00-13:00
```

---

## Estructura del repositorio

```text
Proyecto_DeliveryBot_PintoSergio/
│
├── README.md
├── ENTREGA_PROFESOR.md
├── ENLACE_GOOGLE_SHEETS.txt
├── .gitignore
│
├── workflows/
│   ├── 01_DeliveryBot_Pedidos_Estados.json
│   └── 02_DeliveryBot_Reporte_Diario.json
│
├── data/
│   └── DeliveryBot_DB.xlsx
│
└── docs/
    ├── Manual_Configuracion.md
    ├── Manual_Usuario.md
    ├── Casos_Prueba.md
    ├── Subir_a_GitHub.md
    └── Evidencias/
        └── README.md
```

---

## Configuración

Los workflows no incluyen tokens ni credenciales reales por seguridad.

Después de importar los JSON en n8n, se deben seleccionar:

### Google Sheets

- Credencial de Google Sheets.
- Documento real del proyecto.
- Hoja correspondiente a cada nodo.

### Telegram

- Credencial del bot.
- Chat ID de cocina.
- Chat ID del administrador.

Los valores como:

```text
REEMPLAZAR_GOOGLE_SHEET_ID
REEMPLAZAR_CHAT_COCINA_ID
REEMPLAZAR_CHAT_ADMIN_ID
```

son intencionales y deben ser reemplazados en la instancia donde se despliegue el proyecto.

---

## Google Sheets

**Enlace público de la base de datos:**

> PENDIENTE: pegar aquí el enlace configurado como "Cualquier persona con el enlace – Lector".

El enlace también debe registrarse en `ENLACE_GOOGLE_SHEETS.txt`.

---

## Prueba funcional recomendada

Ejecutar en este orden:

```text
/start
AGREGAR BEB001 2
/carrito
/confirmar
/estado
```

Después, desde cocina:

```text
ESTADO <id_pedido> PREPARACION
ESTADO <id_pedido> EN_CAMINO
ESTADO <id_pedido> ENTREGADO
```

Finalmente verificar:

- registro en `PEDIDOS`;
- descuento en `MENU.stock`;
- actualización del usuario;
- carrito vacío después de confirmar;
- notificación a cocina;
- notificación de estados;
- reporte diario.

---

## Seguridad

- No publicar el token del bot.
- No publicar credenciales OAuth de Google.
- No subir archivos `.env` con secretos.
- Compartir Google Sheets en modo lector para evaluación.
- Mantener las credenciales administradas dentro de n8n.

---

## Resultado

DeliveryBot permite automatizar de punta a punta el proceso básico de pedidos internos de cafetería, ofreciendo trazabilidad, control de inventario, registro de usuarios, seguimiento de órdenes e indicadores diarios de gestión.
