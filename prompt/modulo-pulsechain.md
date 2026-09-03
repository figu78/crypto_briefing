# Módulo opcional: PulseChain y ratio trading

Pega este bloque en `prompt/briefing.md`, donde está el comentario `<!-- MÓDULO OPCIONAL -->`. Si sigues otros tokens en otra cadena, cambia los contratos y la tabla de profundidades: el método es el mismo.

Antes de usarlo, lee [docs/pulsechain.md](../docs/pulsechain.md) y [docs/ratio-trading.md](../docs/ratio-trading.md).

---

## PASO 1-B — DATOS DE PULSECHAIN

Seis tokens del ecosistema, con ratio trading entre ellos. Contratos canónicos — úsalos siempre, hay homónimos e impostores en la cadena:

- HEX nativo de PulseChain: `0x2b591e99afE9f32eAA6214f7B7629768c40Eeb39`
- eHEX (HEX from Ethereum, puenteado): `0x57fde0a71132198bbec939b98976993d8d89d225`
- PLSX (PulseX): `0x95b303987a60c71504d99aa1b13b4da07b0790ab`
- INC (PulseX Incentive Token): `0x2fa878ab3f87cc1c9737fc071108f904c0b0c95d`
- PRVX (ProveX): `0xF6f8Db0aBa00007681F8fAF16A0FDa1c9B030b11`
- PLS es el token nativo; su proxy negociable es WPLS (Wrapped Pulse).

**REGLA DE FUENTES — esto importa mucho:**

- **NO uses los precios de las fichas de CoinGecko para estos tokens.** Los precios muy pequeños se muestran con ceros en subíndice y al extraer el texto salen corrompidos (PLSX aparece como `$0.058530` cuando el precio real es `$0.0000087`, un factor de 6.700). Los **porcentajes** de variación de CoinGecko (24 h, 7 d, 30 d) sí son fiables y puedes usarlos.
- Las fuentes primarias de precio y ratio son **DEX Screener** y **GeckoTerminal** (`geckoterminal.com/pulsechain/tokens/<contrato>` y `/pools/<pool>`). Prefiere el dato agregado entre DEXes: en PulseChain el mismo par existe en pulsex, 9mm, 9inch, switchx y liberty-swap, y la profundidad agregada es bastante mayor que la de un pool suelto.
- Usa siempre el par más profundo y comprueba liquidez y volumen 24 h antes de fiarte de un precio. Hay pools muertos con 80-900 $ de liquidez cuyos precios son ruido. **Por debajo de ~50.000 $ de liquidez, o no lo uses o marca el dato como poco fiable.**
- **Método para los ratios**: saca el precio de cada token EN WPLS desde su par profundo contra WPLS, y deriva desde ahí todos los cruces. Comprobación de coherencia: precio en USD ÷ precio de WPLS en USD debe cuadrar con el ratio del par; si se desvía más de un 2 %, dilo.
- **Profundidad por par** (referencia de septiembre de 2026; vuelve a comprobarla, cambia con el tiempo).
  - Profundos y fiables: PLSX/WPLS (~2,8 M$), INC/PLSX (~2,0 M$), HEX/WPLS (~1,9 M$), INC/WPLS (~1,1 M$), HEX/PLSX (~409 K$), PRVX/PLSX (~111 K$).
  - Finos, no usar como precio de referencia: INC/HEX directo (~17 K$), INC/eHEX (~17 K$), PLSX/eHEX (~8,6 K$), HEX/PRVX (~1,9 K$), INC/PRVX (~2,0 K$).
- Consecuencia práctica que debes reflejar: **INC↔HEX no se rota en directo**, se enruta vía PLSX o WPLS. **PRVX se enruta por PLSX**, no por WPLS. Y **eHEX solo tiene pools finos**, así que el ratio HEX/eHEX se puede calcular con precisión pero ejecutarlo tiene deslizamiento real; dilo cuando lo menciones.

## Secciones adicionales del informe

Van después de "ETH y oro" y antes de "Agenda macro".

### PulseChain: precios del ecosistema

Lista con los seis tokens: precio en USD, variación 24 h, 7 d y 30 d, y liquidez y volumen 24 h del par usado. Una línea por token diciendo de dónde sale el dato y si la profundidad da para fiarse. Si algún token no se puede verificar hoy, dilo en lugar de estimarlo. Cierra con la comprobación de coherencia entre el dato agregado y el del pool suelto.

### PulseChain: ratio trading

El objetivo es **acumular más unidades** rotando entre tokens, no salir a fiat. Todo el análisis va en unidades de un token por otro, nunca en dólares — el coste medio en fiat es irrelevante para decidir una rotación.

- Lista los ratios del día con su valor actual: HEX/eHEX, INC/HEX, INC/PLSX, HEX/PLSX, PLSX/PLS, PRVX/PLS y PRVX/HEX.
- Para cada uno, el movimiento de 24 h y, cuando tengas los porcentajes de 7 d y 30 d de ambos tokens, la variación relativa derivada: `ratio ≈ (1+a)/(1+b) − 1`. Di explícitamente que es una variación **derivada de dos series**, no un histórico del ratio leído de un gráfico.
- Señala qué token actúa de rezagado y cuál de líder, y qué rotación aumentaría unidades si el ratio revierte. Indica por qué ruta se ejecutaría, según la tabla de profundidad.
- **LIMITACIÓN QUE DEBES DECLARAR SIEMPRE**: sin un histórico del propio ratio no se puede afirmar que esté "caro" o "barato" respecto a su banda normal. Un ratio que se ha movido un 120 % en 30 días puede ser una banda estirada que revierte o un cambio de régimen, y con los datos de un solo día las dos lecturas son igual de compatibles. Si no has consultado ese histórico, dilo y limítate a describir el movimiento.
- **Coste real de rotar**: deslizamiento en los pares finos, comisión del DEX, y en PRVX un 2 % de quema por operación (ida y vuelta ≈ 4 % solo de quema). Un ratio que se ha movido un 2-3 % no cubre necesariamente el coste de la rotación.
