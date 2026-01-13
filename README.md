# Pre-entrega — Automatización en Make (Alerta Crypto)

## Caso de uso
Automatización que monitorea el mercado de Bitcoin (BTC) y genera una alerta por email cuando la variación porcentual en 24 horas supera un umbral, registrando además el histórico en Google Sheets y evitando notificaciones repetidas usando un Data Store (anti-spam).

## Trigger / Ejecución
El escenario se ejecuta de forma programada en Make (Scheduler). *(El scheduler no aparece en este blueprint exportado; el flujo inicia desde el módulo HTTP.)*

## API utilizada (sin autenticación)
Se consulta la API pública de Binance Market Data:
`GET https://data-api.binance.vision/api/v3/ticker/24hr?symbol=BTCUSDT&type=FULL`
De esta respuesta se toman:
- lastPrice (precio BTC/USDT)
- priceChangePercent (% cambio 24h)

## Manejo de datos
Se calculan variables:
- PrecioUSD = lastPrice
- Cambio-Real = priceChangePercent
- Cambio-Absoluto = abs(priceChangePercent)
- Fecha = now

## Persistencia / Anti-spam (Data Store)
Se usa un Data Store llamado “BTC-Estado-Alerta” (key fija: `btc`) con estos campos:
- Alertando (boolean)
- Ultimo_Envio (texto)
- Ultimo_Cambio (número)
- Ultimo_Precio (número)

## Lógica condicional
Router con dos rutas:
1) Enviar Alerta (email + log + update datastore):
   - Condición: “Alertando ahora” = true y (Alertando anterior = null o false)
2) Rama Regular (log + reset datastore bajo condición):
   - Siempre registra en Google Sheets.
   - Si “Alertando ahora” = false y Alertando anterior = true, reinicia el estado en Data Store.

## Notificaciones y logs
- Email: se envía por Gmail cuando se cumple la condición de alerta.
- Google Sheets:
  - Hoja “Oportunidades de Compra-Venta” registra eventos de alerta.
  - Hoja “Registro Regular” registra ejecuciones regulares.
