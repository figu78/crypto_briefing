# Fuentes: cuál usar para qué, y cuál falla

Esta es la parte del repositorio que de verdad ahorra tiempo. Son errores encontrados construyendo el informe, no teoría.

## Precios de BTC y ETH

**Usa**: CoinGecko como primaria, CoinDesk como contraste.

**Cuidado con esto**: las páginas de precio de CoinDesk a veces sirven un dato con varias horas de retraso, con la marca temporal en la propia página. Comprueba siempre la hora del dato, no solo la cifra. En una sesión con un movimiento del 5 %, dos fuentes consultadas con una hora de diferencia parecen contradecirse cuando en realidad ambas son correctas.

Si dos fuentes discrepan, mira primero si la diferencia se explica por la hora. Si se explica, dilo así ("el movimiento ocurrió después"). Si no se explica, da el rango y di que las fuentes no coinciden.

**Rangos de 24 h**: son especialmente útiles porque el máximo y el mínimo de las últimas 24 horas son niveles reales que ha puesto el mercado, no niveles teóricos dibujados por alguien. Un informe que usa el rango de 24 h como resistencia y soporte inmediatos está anclado en algo verificable.

## Flujos de ETF spot

**Usa**: [Farside Investors](https://farside.co.uk/btc/) como primaria.

**La trampa**: Farside rellena la tabla a lo largo de la mañana de Nueva York, conforme las gestoras reportan. Si consultas a primera hora europea, la fila del día anterior puede estar incompleta.

Caso real del 2 de septiembre de 2026: a las 13:20 hora de Madrid la tabla mostraba **+8,4 M$** para la sesión del 1 de septiembre. La cifra definitiva, consolidada al día siguiente, fue **−236,5 M$**. No es un error de Farside, es que estaba a medias.

**Cómo protegerse**: contrasta siempre con una nota de prensa del día (CoinDesk, Coinpaper, Bloomingbit). Si la tabla y la prensa no coinciden, la prensa suele ir con la cifra consolidada del día anterior. Y si no puedes resolverlo, describe el hecho sin la cifra: "la primera sesión de septiembre rompió la racha de entradas".

**Segunda trampa, las fechas**: muchos medios titulan con la fecha de publicación, no con la fecha de la sesión. "Los ETF captan 216,7 M$ el 1 de septiembre" puede referirse a la sesión del 31 de agosto reportada el 1. Verifica siempre a qué sesión corresponde la cifra antes de usarla.

**Tercera**: las rachas de sesiones consecutivas varían entre medios según cómo cuenten los días parciales. Si dos fuentes dicen 11 y 12 sesiones, escribe "once o doce", no elijas.

## Macro: DXY, rendimientos, probabilidades del FOMC

**Usa**: Trading Economics para rendimientos y calendario; FXStreet para DXY y análisis técnico; para las probabilidades del FOMC, contrasta al menos dos de estas tres — CME FedWatch, agregadores de mercados de predicción (DeFi Rate agrega Kalshi y Polymarket) y Trading Economics.

**La trampa**: las probabilidades del FOMC discrepan bastante entre fuentes y a veces son internamente incoherentes. Ejemplo real: una fuente daba simultáneamente un tipo implícito en diciembre por debajo del actual (o sea, recortes) y solo un 14 % de probabilidad de recorte. Eso no cuadra.

**Cómo protegerse**: da un rango ("entre el 58 % y el 70 % de probabilidad de subida") en vez de una cifra. Y si para un horizonte concreto las fuentes se contradicen, dilo y no des número.

**Sobre los máximos históricos**: cuando una fuente diga "máximo desde X", contrástalo. Es habitual que dos medios den fechas distintas para el mismo nivel. "Máximos de varios años" es preferible a una fecha concreta que no puedes verificar.

## Oro

**Usa**: Trading Economics o FXStreet para el spot; FXStreet para niveles técnicos.

**La trampa**: no mezcles spot con futuros. XAU/USD spot y el contrato de futuros del mes activo cotizan con diferencias de decenas de dólares. Si citas un precio, di cuál de los dos es.

**Lo interesante**: el oro suele ser el mejor termómetro cruzado. Cuando cae con una guerra activa, está diciendo que el mercado descuenta tipos altos por encima de todo lo demás — y eso es información directa sobre cripto. Merece la pena mirarlo aunque no operes oro.

## Calendario macro

**Usa**: el resumen diario de Investing.com ("X, Y and Z due Thursday") es más fiable de leer que las tablas de calendario, que se extraen mal.

**La trampa**: casi todos publican en hora del Este o en UTC. Convierte siempre, y comprueba qué horario estacional aplica hoy antes de convertir. Ver [programar-envio.md](programar-envio.md).

**Cobertura**: los resúmenes diarios cubren bien los tres o cuatro datos importantes y mal el resto. Si no puedes confirmar la hora exacta y el consenso de un dato secundario, dilo en lugar de inventarlo.

## Crudo

**Cuidado**: en jornadas de tensión geopolítica es donde más he visto discrepar a las fuentes. Un caso real con Brent citado en 88,88 $ por un medio y "por encima de 96 $" por otro el mismo día — un 8 %, que no se explica por la hora.

Si vas a mencionar el crudo, contrasta dos fuentes. Si no coinciden, di la dirección sin la cifra.

## Derivados: funding, open interest, liquidaciones

**El problema**: es el dato más difícil de conseguir actualizado y gratis, y el que más se cita mal.

**Qué pasa en la práctica**: encuentras un dato de funding de hace dos días acompañado de una cifra de open interest que no cuadra con la de otra fuente del mismo día, porque miden universos de exchanges distintos. Un artículo dice 318.600 BTC y otro 700.000 BTC. Ninguno miente; miden cosas diferentes.

**Cómo protegerse**: si no tienes lectura de hoy, dilo. Si el dato es de hace dos días, ponle la fecha y no lo presentes como actual. Y si dos cifras de open interest son incompatibles, di que lo son en lugar de elegir la que encaje con tu narrativa.

**Clústeres de liquidación**: CoinGlass da niveles muy útiles, pero el dato se publica con fecha. Un clúster de hace una semana sigue marcando un nivel relevante, pero el importe casi seguro ya no es ese — sobre todo si el precio ya lo ha visitado. Cita el nivel, fecha el importe.

## Tokens pequeños en DEX

Todo esto está desarrollado en [pulsechain.md](pulsechain.md), pero el resumen aplica a cualquier cadena:

- **No uses las fichas de CoinGecko para precios muy pequeños.** Los ceros en subíndice se pierden al extraer el texto y el precio sale mal por factores de miles. Los porcentajes sí valen.
- **Usa GeckoTerminal o DEX Screener**, que dan el número limpio y además el ratio directo entre los dos tokens del par.
- **Comprueba la liquidez antes que el precio.** Un pool con 900 $ de liquidez devuelve un precio, y ese precio no significa nada.
- **Verifica el contrato.** Es normal que existan varios tokens con el mismo nombre y símbolo. Uno tiene volumen real y el resto son polvo o trampas.
- **Comprobación de coherencia**: precio en USD ÷ precio del token base en USD debe cuadrar con el ratio del par. Si no cuadra, uno de los dos datos está mal.
