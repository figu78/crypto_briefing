# Elegir la hora del envío

La hora a la que se ejecuta el informe cambia qué datos existen. No es una preferencia estética.

## Qué hay disponible a cada hora

Horas en hora de Madrid; ajusta a tu zona.

| Hora | Qué tienes | Qué te falta |
|---|---|---|
| **07:00–09:00** | Cierre asiático, apertura europea, agenda macro del día completa por delante | Flujos de ETF del día anterior aún provisionales o sin publicar. Sin reacción americana. |
| **12:00–13:00** | Flujos de ETF normalmente ya consolidados. Agenda americana todavía por delante. | Nada relevante. **Es el mejor punto de equilibrio.** |
| **15:00–17:00** | Apertura americana ya digerida, primeros datos macro publicados | Los datos de la mañana ya han movido el mercado: el informe llega tarde para posicionarse ante ellos. |
| **18:00–20:00** | Sesión americana casi completa, todos los datos del día publicados | La agenda del día ya no sirve de aviso, solo de registro. Útil si prefieres revisar en vez de anticipar. |
| **Domingo noche** | Visión de semana, apertura asiática por delante | Sin flujos de ETF (no hay sesión). Útil como complemento, no como sustituto del diario. |

**Recomendación**: si solo vas a tener un envío, ponlo entre las 11:00 y las 13:00. Los flujos de ETF ya están consolidados y todavía te da margen antes de la sesión americana.

Si el informe te sale después de que se hayan publicado datos del día, el prompt ya está preparado para marcarlos como "ya publicado" y dejar el consenso para que contrastes.

## La trampa del horario de verano

**Las tareas programadas se definen en UTC.** Si tu zona cambia de hora dos veces al año, tu informe se desplazará una hora cuando llegue el cambio.

Para Madrid:

| Periodo | Zona | Para las 12:00 de Madrid, el cron va a |
|---|---|---|
| Último domingo de marzo → último domingo de octubre | CEST (UTC+2) | **10:00 UTC** |
| Último domingo de octubre → último domingo de marzo | CET (UTC+1) | **11:00 UTC** |

Es decir: un cron fijado en `0 10 * * 1-5` te llega a las 12:00 en verano y a las **11:00** en invierno.

**Tres formas de tratarlo:**

1. **Aceptarlo.** Si la hora exacta te da igual, no hagas nada. Una hora arriba o abajo no cambia gran cosa.
2. **Ajustarlo dos veces al año.** Apúntalo en el calendario para el último domingo de marzo y el de octubre. Pídele a Claude que cambie la hora de la tarea; se hace en un mensaje y conserva el historial.
3. **Elegir una hora que aguante el desplazamiento.** Si fijas el envío a las 12:00 de verano, en invierno te llega a las 11:00, que sigue siendo buena hora. Si lo fijas a las 09:00 de verano, en invierno te llega a las 08:00, que ya es antes de que Farside consolide. Elige una hora con margen por abajo.

Si tu zona no cambia de hora —Ciudad de México, Buenos Aires, Bogotá— nada de esto te afecta: fijas el cron una vez y ya está.

## Días de la semana

`1-5` en el campo de día de la semana significa de lunes a viernes. Tiene sentido para un informe centrado en flujos de ETF y macro, porque el fin de semana no hay ninguna de las dos cosas.

Cripto sí cotiza en fin de semana, así que si operas sábado y domingo puedes poner `*` para todos los días. El informe saldrá igual, simplemente con las secciones de ETF y agenda macro diciendo que no hay datos nuevos.

## Cómo cambiar la hora después

En cualquier conversación de Cowork:

> Cambia mi tarea del briefing para que se ejecute a las 08:30 hora de Madrid

No hace falta borrar y recrear la tarea. Cambiar la hora conserva el historial de ejecuciones, que sirve para ver si alguna falló.

## Probarlo sin esperar

> Ejecuta ahora la tarea del briefing

Se dispara una ejecución manual fuera de horario. Úsalo después de cualquier cambio en el prompt, y comprueba tres cosas: que el correo llega, que las horas de la agenda están bien convertidas, y que los niveles de precio se corresponden con el mercado de ese momento.
