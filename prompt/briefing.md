# Prompt del briefing diario

Sustituye `{{EMAIL}}`, `{{ZONA_HORARIA}}`, `{{DESFASE_INVIERNO}}` y `{{DESFASE_VERANO}}` antes de usarlo. Todo lo que va debajo de la línea es el prompt.

---

Genera el briefing diario de trading y envíalo por correo. Escríbelo íntegramente en español de España. Trabaja de forma autónoma: no hagas preguntas, no esperes respuesta, no uses AskUserQuestion.

TONO: es un informe personal. Distingue con claridad el hecho confirmado del dato no verificado, en tono neutro y directo ("dato sin verificar", "las fuentes no coinciden", "esto es interpretación, no dato"). Nada de relleno ni de entusiasmo impostado.

ALCANCE: el informe es de mercado. No incluyas carteras ni posiciones personales.

## PASO 1 — INVESTIGA ANTES DE ESCRIBIR

Nunca uses precios de memoria; todo debe venir de búsquedas web hechas en esta ejecución. Consulta como mínimo:

- Precio de BTC y ETH, variación 24 h, rango de 24 h, variación semanal y de 30 días. CoinGecko como fuente primaria, contrastado con CoinDesk.
- Flujos de los ETF spot de BTC y de ETH de la sesión anterior, con la cifra exacta y la racha de sesiones. Farside Investors (farside.co.uk/btc/ y farside.co.uk/eth/) es la fuente primaria. **Contrasta siempre con una nota de prensa del día**: la tabla de Farside se rellena a lo largo de la mañana de Nueva York y a primera hora puede estar provisional.
- Nivel del DXY y su variación, rendimientos del Treasury, y probabilidades de mercado para la próxima reunión del FOMC y para la de diciembre (Trading Economics, CME FedWatch, Kalshi/Polymarket).
- Precio del oro y sus niveles técnicos.
- Agenda macro del día. Convierte SIEMPRE las horas a {{ZONA_HORARIA}}. Los calendarios suelen publicar en UTC o en hora del Este: tu zona es UTC{{DESFASE_VERANO}} en horario de verano y UTC{{DESFASE_INVIERNO}} en horario de invierno. Comprueba cuál aplica hoy antes de convertir.
- Noticias cripto relevantes del día y titulares macro que puedan mover el mercado.

Comprueba la fecha y hora exacta con `TZ={{ZONA_HORARIA}} date` antes de fechar el informe.

<!-- MÓDULO OPCIONAL: si quieres la sección de PulseChain, pega aquí el contenido de prompt/modulo-pulsechain.md -->

## PASO 2 — ESCRIBE EL INFORME con esta estructura exacta

```
# Briefing Trading — [día de la semana] [fecha] de [año]

Datos consultados alrededor de las [hora], hora de [tu ciudad].
```

### Lectura rápida

Dos o tres frases: dónde está el mercado, qué lo está moviendo, y qué zona decide el próximo tramo. Si hay una tensión entre dos señales que se contradicen, dila aquí: es lo más valioso del informe.

### BTC: precio, estructura y liquidez

Lista con precio, variación 24 h, rango 24 h, semanal y 30 días. Después una lista de resistencias y otra de soportes, **cada nivel con una frase de por qué importa** — un nivel sin motivo no vale nada. Cierra con un párrafo "Lectura:" con la interpretación honesta, incluyendo el riesgo de posicionamiento.

Los niveles deben salir de los datos reales de hoy: máximo y mínimo de las 24 h, medias móviles, retrocesos, clústeres de liquidación, strikes con volumen. Nunca copiados de un informe anterior.

### Flujos y sentimiento

Tres bloques claramente separados:

- **Hechos confirmados**: solo datos con fuente y fecha.
- **Datos incompletos**: di explícitamente qué no has podido verificar y por qué. Típicamente funding, open interest y liquidaciones del día. Si un dato es de hace dos días, dilo: un dato viejo presentado como actual es peor que ningún dato.
- **Rumores**: si no hay rumores fiables, dilo y punto.

### ETH y oro

ETH con precio y variaciones, resistencias, soportes, señal de debilidad y confirmación alcista. Oro con precio, soportes, resistencias y qué lo mueve hoy — y si el oro se está comportando de forma contraintuitiva respecto a la geopolítica, explica por qué, porque suele decir algo sobre tipos.

### Agenda macro — hora de {{ZONA_HORARIA}}

Lista de eventos con la hora ya convertida, consenso y anterior. Si el informe se genera después de que un dato se haya publicado, márcalo como "ya publicado" y deja el consenso para poder contrastar. Añade un párrafo sobre dónde se concentra el riesgo de barrido, relacionándolo con los niveles concretos de BTC.

### Escenarios BTC

Tres párrafos: Alcista, Bajista y Lateral. Cada uno con disparador, objetivos e invalidación **concretos y numéricos**. Un escenario sin invalidación no es un escenario, es una opinión.

Cierra con un descargo de una línea ("Informe informativo, no asesoramiento financiero") y una sección "Fuentes" con lo consultado.

## REGLAS DE FONDO

1. Los niveles se derivan de los datos reales de hoy, nunca se copian de un informe anterior.
2. Distingue siempre hecho de interpretación.
3. **Si un dato no se puede verificar, dilo en lugar de estimarlo.** Un hueco declarado es útil; un hueco rellenado con algo verosímil es un error que se propaga.
4. Si dos fuentes dan cifras distintas, da el rango y di que no coinciden. No elijas una en silencio.
5. Si una cifra es de una fecha anterior, ponle la fecha.

## PASO 3 — ENVÍA POR CORREO

Manda el informe con la herramienta de Gmail (`mcp__Gmail__send_message`) a {{EMAIL}}.

- Asunto: `Briefing Trading — [fecha]`
- Usa `htmlBody` con el informe maquetado en HTML legible: encabezados h1/h2, listas ul/li, negritas para los niveles de precio. Que se lea bien en el móvil. No mandes markdown en crudo.
- Rellena también `body` con una versión en texto plano.
- No adjuntes ficheros: el informe va en el cuerpo del correo.

Si la herramienta de Gmail no está disponible o el envío falla, escribe el informe en un archivo `.md`, entrégalo con SendUserFile y di claramente en el chat que el correo no se pudo enviar.

Termina con un resumen de dos o tres líneas en el chat: el sesgo del día, el nivel clave de BTC y el evento macro que más importa.
