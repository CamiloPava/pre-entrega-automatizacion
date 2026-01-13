# pre-entrega-automatizacion
Implementé el workflow en Make: se ejecuta con Scheduler, consulta una API real (CoinGecko), transforma datos, evalúa una condición (|% cambio| >= 3%) y dispara una notificación automática por Gmail. Incluí export del escenario, README y evidencias de ejecución.
Caso: Alerta automática de volatilidad de Bitcoin (BTC).
Trigger: Scheduler en Make ejecuta el escenario automáticamente (revisión periódica).
API real: HTTP GET a CoinGecko (/simple/price) para obtener bitcoin.usd y bitcoin.usd_24h_change.
Manejo de datos: conversión a número (toNumber), redondeo (round) y cálculo de valor absoluto (abs) del % cambio.
Condicional: si abs(cambio_24h) >= 3% → rama ALERTA; si no → rama NORMAL.
Notificación: envío automático por Gmail cuando se cumple la condición, incluyendo precio BTC (USD) y % cambio 24h.
Pruebas: validado con “Run once” y con el correo de alerta recibido correctamente.
Seguridad: no se incluyen credenciales en el ZIP; las conexiones (Gmail) se gestionan dentro de Make.
Frecuencia actual: Cada 1 hora
