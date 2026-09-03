# Briefing cripto diario

Un informe de mercado que se genera solo cada mañana y te llega por correo. Precios, flujos de ETF, macro del día y escenarios, con las fuentes citadas y una separación explícita entre lo que está verificado y lo que no.

No es un bot de señales. No te dice qué comprar. Te deja sobre la mesa los datos del día ya contrastados para que decidas tú.

## Qué genera

Un correo como [este](ejemplos/2026-09-03.md), todos los días laborables a la hora que tú elijas:

- **Lectura rápida** — dónde está el mercado y qué lo está moviendo.
- **BTC** — precio, rango, resistencias y soportes con el motivo de cada nivel, y una lectura honesta del riesgo de posicionamiento.
- **Flujos y sentimiento** — separado en tres bloques: hechos confirmados con fuente, datos que no se han podido verificar, y rumores.
- **ETH y oro**.
- **Agenda macro en tu huso horario**, con consenso y anterior.
- **Escenarios** alcista, bajista y lateral, cada uno con disparador, objetivos e invalidación numéricos.
- **[Opcional] PulseChain y ratio trading** — ver más abajo.

## Por qué está montado así

Casi todo el valor está en las reglas negativas: qué fuente NO usar para qué dato, cuándo una tabla está provisional, qué precio es ruido. Un informe diario generado sin esas reglas es plausible y falso, que es la peor combinación posible.

Tres ejemplos reales de lo que corrige:

- CoinGecko muestra los precios muy pequeños con ceros en subíndice. Al leerlos programáticamente salen corrompidos por factores de miles. Sus **porcentajes** sí son fiables; sus **precios**, para tokens pequeños, no.
- Farside publica los flujos de ETF a lo largo de la mañana de Nueva York. Si consultas a primera hora, la fila del día anterior puede estar a medias y darte una entrada neta donde en realidad hubo una salida de 236 millones.
- En cadenas con muchos DEX hay pools con 900 $ de liquidez cuyo precio no significa nada. Sin un mínimo de profundidad, el dato es ruido con aspecto de dato.

Todo eso está documentado en [docs/fuentes.md](docs/fuentes.md). Y en [prompt/CHANGELOG.md](prompt/CHANGELOG.md) está el motivo de cada regla del prompt: qué falló sin ella. Si vas a quitar alguna, léelo antes.

## Montarlo

Necesitas la app de Claude con Cowork, el conector de Gmail activado, y cinco minutos.

**1. Coge el prompt.** Abre [prompt/briefing.md](prompt/briefing.md) y sustituye:

- `{{EMAIL}}` → tu dirección de correo.
- `{{ZONA_HORARIA}}` → tu zona, por ejemplo `Europe/Madrid`.
- `{{DESFASE_INVIERNO}}` y `{{DESFASE_VERANO}}` → tu desfase respecto a UTC. Madrid es UTC+1 y UTC+2. Ciudad de México, UTC−6 todo el año. Buenos Aires, UTC−3 todo el año.

**2. Si quieres el módulo de PulseChain**, pega el contenido de [prompt/modulo-pulsechain.md](prompt/modulo-pulsechain.md) donde el prompt principal lo indica. Si no, sáltatelo: el informe funciona igual sin él.

**3. Créalo como tarea programada.** En una conversación de Cowork, pídeselo tal cual:

> Crea una tarea programada que se ejecute de lunes a viernes a las 08:00 hora de Madrid con este prompt: [pegas el prompt]

Elige la hora que te sirva. Hay quien lo quiere antes de la apertura europea, quien lo quiere a mediodía con la sesión americana ya en marcha, y quien lo quiere el domingo por la noche para preparar la semana. **Lee [docs/programar-envio.md](docs/programar-envio.md) antes de elegir**: la hora del informe cambia bastante qué datos están disponibles, y hay una trampa con el horario de verano que conviene conocer.

**4. Pruébalo.** Pídele que ejecute la tarea una vez de forma manual antes de dejarla en automático. Comprueba que el correo llega, que las horas de la agenda están bien convertidas y que los números cuadran.

## El módulo de PulseChain

Opcional y bastante específico. Cubre seis tokens del ecosistema —HEX, eHEX, PLS, PLSX, INC y PRVX— y añade dos secciones: precios con la profundidad del par de la que sale cada uno, y análisis de **ratio trading**.

El ratio trading aquí significa rotar entre tokens para acumular más unidades, sin salir a dólares. El informe lo expresa todo en unidades de un token por otro, no en dinero, porque en ese marco el coste medio en fiat es irrelevante.

Documentación:

- [docs/pulsechain.md](docs/pulsechain.md) — contratos canónicos, qué pares tienen profundidad de verdad y cuáles engañan.
- [docs/ratio-trading.md](docs/ratio-trading.md) — el método de usar WPLS como denominador común, cómo derivar los cruces y qué NO se puede afirmar sin histórico del ratio.

El método sirve para cualquier cadena con DEX. Si sigues otros tokens, cambia los contratos y la tabla de profundidades y funciona igual.

## Lo que este informe no hace

- No predice. Los escenarios son condicionales con invalidación, no pronósticos.
- No opera. No toca ninguna wallet ni tiene claves de nada.
- No sustituye tu criterio. Si un dato no se puede verificar, el informe lo dice en lugar de rellenar el hueco con algo verosímil — y eso es intencionado.
- No es asesoramiento financiero.

## Licencia

MIT. Ver [LICENSE](LICENSE). Cógelo, cámbialo, rómpelo.
