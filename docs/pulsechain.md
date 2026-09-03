# PulseChain: contratos, pares y profundidad

Datos verificados en septiembre de 2026. Los contratos no cambian; las profundidades sí. Vuelve a comprobarlas cada pocas semanas.

## Contratos canónicos

| Token | Contrato | Qué es |
|---|---|---|
| HEX | `0x2b591e99afE9f32eAA6214f7B7629768c40Eeb39` | HEX nativo de PulseChain |
| eHEX | `0x57fde0a71132198bbec939b98976993d8d89d225` | HEX de Ethereum, puenteado a PulseChain |
| PLSX | `0x95b303987a60c71504d99aa1b13b4da07b0790ab` | Token del DEX PulseX |
| INC | `0x2fa878ab3f87cc1c9737fc071108f904c0b0c95d` | Token de recompensa de farming de PulseX |
| PRVX | `0xF6f8Db0aBa00007681F8fAF16A0FDa1c9B030b11` | ProveX |
| PLS / WPLS | nativo / Wrapped Pulse | Token de gas de la cadena |

**HEX y eHEX son dos tokens distintos** que cotizan a precios distintos en la misma cadena. El ratio entre ambos ronda 2,5 eHEX por HEX. Confundirlos invalida cualquier cálculo.

## El caso ProveX: por qué hay que fijar el contrato

Buscando "ProveX PulseChain" aparecen al menos cuatro contratos distintos, con precios que van de `$0,00001` a `$8.624`. Uno tiene volumen real; el resto son polvo o impostores.

Ejemplo de lo que se encuentra:

- `0xF6f8Db…0b11` — **el bueno.** ~150 K$ de volumen diario, máximo histórico en marzo de 2026.
- `0xa06c13…0f3c` — 2,39 K$ de liquidez, 156 $ de volumen diario. Polvo.
- Un contrato listado como "PrivatePower" que aparece bajo el nombre ProveX a 8.624 $.

**Regla general para cualquier cadena**: no busques un token por nombre, fija el contrato una vez y úsalo siempre.

## Profundidad por par

Septiembre de 2026, liquidez agregada entre DEXes.

### Fiables

| Par | Liquidez | Volumen 24 h |
|---|---|---|
| PLSX / WPLS | ~2,8 M$ | ~196 K$ |
| INC / PLSX | ~2,0 M$ | ~53 K$ |
| HEX / WPLS | ~1,9 M$ | ~466 K$ |
| INC / WPLS | ~1,1 M$ | ~104 K$ |
| HEX / PLSX | ~409 K$ | ~62 K$ |
| PRVX / PLSX | ~111 K$ | ~34 K$ |

### Finos — no usar como precio de referencia

| Par | Liquidez | Volumen 24 h |
|---|---|---|
| INC / HEX directo | ~17 K$ | ~1,3 K$ |
| INC / eHEX | ~17 K$ | ~918 $ |
| PLSX / eHEX | ~8,6 K$ | ~689 $ |
| INC / PRVX | ~2,0 K$ | ~128 $ |
| HEX / PRVX | ~1,9 K$ | ~126 $ |

Y hay bastante peor. En una búsqueda cualquiera aparecen pools con **922 $ de liquidez y 16 $ de volumen diario**, que devuelven un precio con toda naturalidad. Ese precio es ruido.

## Consecuencias prácticas

**INC↔HEX no se rota en directo.** El par directo tiene 17 K$. Enruta por PLSX o por WPLS, que tienen cien veces más.

**PRVX se enruta por PLSX**, no por WPLS: PRVX/PLSX tiene 111 K$ y es el mejor camino disponible.

**eHEX es el eslabón débil.** Todos sus pares son finos. El ratio HEX/eHEX se calcula con precisión —dos rutas independientes lo dan idéntico hasta el cuarto decimal— pero **ejecutarlo es otra cosa**: cualquier tamaño relevante mueve el pool. Calculable no es lo mismo que operable, y conviene tenerlo separado en la cabeza.

**PRVX quema un 2 % por operación.** Ida y vuelta son ~4 % solo de quema, antes de comisiones y deslizamiento. Cualquier movimiento de ratio por debajo de eso no compensa.

## Verificar que los datos son correctos

Dos rutas independientes deben coincidir:

1. **Ruta pool**: el ratio que da directamente el par en GeckoTerminal.
2. **Ruta USD**: precio del token en USD ÷ precio de WPLS en USD.

Si coinciden dentro de un 1-2 %, los datos son buenos. Si se separan más, uno de los dos está mal — normalmente porque el pool elegido es fino o porque el precio en USD viene de una ficha con el subíndice corrompido.

Contraste real de septiembre de 2026 entre datos agregados y de pool suelto:

| Token | Desviación |
|---|---|
| HEX | +1,22 % |
| eHEX | +0,28 % |
| INC | −0,29 % |
| PLSX | +1,46 % |
| PRVX | −0,45 % |

Todo dentro del ±1,5 %. Eso es lo que quieres ver.

## El fallo de CoinGecko con estos tokens

Las fichas de CoinGecko muestran los precios muy pequeños con ceros en subíndice. Al extraer el texto, esos subíndices se pierden y el precio sale corrompido:

| Token | Lo que se extrae | Precio real | Factor de error |
|---|---|---|---|
| PLSX | `$0.058530` | `$0.0000087` | ~6.700× |
| PLS | `$0.059999` | `~$0.0000110` | ~5.400× |

**Los porcentajes de variación (24 h, 7 d, 30 d) sí son correctos** y son útiles: CoinGecko es la fuente más cómoda para las series de 7 y 30 días, que GeckoTerminal no da tan a mano.

Así que la división es: **porcentajes de CoinGecko, precios de GeckoTerminal o DEX Screener.**

Comprobación rápida de que un precio es plausible: FDV ÷ suministro total debe darte aproximadamente el precio. Si no cuadra por órdenes de magnitud, el precio está corrompido.
