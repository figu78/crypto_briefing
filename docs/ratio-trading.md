# Ratio trading: el método

Ratio trading aquí significa rotar entre tokens que ya te interesan para acabar con **más unidades** de los que quieres, sin salir a fiat. Si estás interesado en varios tokens de un mismo ecosistema a la vez, el dinero no es la unidad de cuenta relevante: lo es el propio token.

Cambia bastante cómo se mira todo. Que tu cartera valga menos en dólares es irrelevante para decidir una rotación. Lo único que importa es si un token está barato **respecto a otro** comparado con lo habitual.

## Denominador común

**El problema**: los pares cruzados directos suelen ser finos. Si lees INC/HEX de un pool con 17.000 $ de liquidez, tu ratio es ruido.

**La solución**: saca el precio de cada token contra el token base de la cadena —WPLS en PulseChain, WETH en Ethereum, WBNB en BSC— usando en cada caso el par más profundo. Después deriva todos los cruces por división.

```
HEX/WPLS  = 274,75
eHEX/WPLS = 109,44
INC/WPLS  = 49.968
PLSX/WPLS = 0,8534

HEX/eHEX  = 274,75 / 109,44   = 2,5105
INC/HEX   = 49.968 / 274,75   = 181,87
HEX/PLSX  = 274,75 / 0,8534   = 321,95
INC/PLSX  = 49.968 / 0,8534   = 58.552
```

Ventajas: todos los cruces salen de pares profundos; son internamente consistentes (INC/HEX × HEX/PLSX = INC/PLSX exactamente); y añadir un token nuevo son dos consultas, no N.

**Validación**: el ratio derivado debe coincidir con el del par directo cuando este exista y tenga algo de profundidad. En el caso real: derivado 182,91 frente a 181,96 leído del par directo, un 0,5 % de diferencia. Eso confirma que el método funciona.

## Movimiento del ratio a partir de dos series

Si tienes la variación en USD de dos tokens, la del ratio sale así:

```
Δ(A/B) ≈ (1 + Δa) / (1 + Δb) − 1
```

Con HEX +13,54 % y PLSX −2,77 % en 24 h:

```
(1,1354) / (0,9723) − 1 = +16,8 %
```

**Es una derivación, no una medición.** Da la magnitud del movimiento relativo, y hay que decir siempre que es derivada. No sustituye a mirar el gráfico del propio ratio.

## Lo que este método NO puede decirte

Y es lo más importante del documento.

**Sin el histórico del ratio no puedes saber si un ratio está caro o barato.** Puedes decir que HEX/PLSX se ha movido un +123 % en 30 días. No puedes decir que esté estirado.

Las dos lecturas son igual de compatibles con ese dato:

- **Banda estirada**: el ratio oscila históricamente entre 150 y 350, está en el techo, y revierte.
- **Cambio de régimen**: PLSX ha perdido algo estructural —dilución, pérdida de utilidad, salida de liquidez— y el ratio nuevo es el correcto. No revierte; sigue.

Distinguirlas exige mirar el gráfico del ratio a un año, no solo la variación relativa de 30 días. Si no lo has mirado, dilo y limítate a describir el movimiento.

**Una divergencia grande no es por sí sola una señal.** Es una pregunta, y hay que ir a buscar la respuesta fuera de los números.

## Coste de rotar

Una rotación de ida y vuelta paga:

1. **Comisión del DEX** — normalmente 0,25-0,30 % por operación, cuatro operaciones si vas y vuelves.
2. **Deslizamiento** — depende de tu tamaño frente a la profundidad del pool. En un pool de 17 K$ una operación de 2.000 $ mueve el precio de forma notable.
3. **Impuesto del token, si lo tiene** — PRVX quema un 2 % por operación. Ida y vuelta, ~4 %.
4. **Gas** — despreciable en PulseChain, no en Ethereum.

**Suelo práctico**: en tokens sin impuesto y con pools profundos, una rotación de ida y vuelta cuesta del orden del 1-2 %. Con PRVX, del 5-6 %.

Un ratio que se ha movido un 2-3 % no cubre el coste. Necesitas movimientos de dos cifras para que la rotación tenga sentido, y aun así solo si crees que revierte.

## Un patrón útil: pares estables y pares divergentes

Dos comportamientos opuestos, ambos accionables de forma distinta.

**Pares estables** — INC/HEX se movió −1,0 % en 30 días y +1,1 % en 24 h. Dos tokens que van al unísono. Entre ellos casi no hay ratio que capturar, y no merece la pena pagar el coste de rotar. Lo útil de identificarlos es **descartarlos**: si dos tokens se mueven juntos, elige el que prefieras por otras razones y olvida el par.

**Pares divergentes** — HEX/PLSX se movió +123 % en 30 días. Aquí sí hay algo, pero exige la pregunta del apartado anterior: ¿banda o régimen?

Un ecosistema sano tiene de los dos. Si todos los pares divergen a la vez, probablemente lo que se mueve es el conjunto y estás mirando ruido.

## Rutas de ejecución

Calcular un ratio y ejecutarlo son cosas distintas. Un ratio se calcula con precisión desde pares profundos; para ejecutarlo necesitas profundidad **en el camino concreto** que vas a recorrer.

Antes de rotar:

1. Mira la liquidez del par que vas a usar, no la del par del que sacaste el ratio.
2. Si el par directo es fino, enruta por el token base — dos operaciones en pools profundos suelen salir más baratas que una en un pool fino.
3. Calcula tu operación como porcentaje de la liquidez del pool. Por encima del 1 % ya empiezas a mover el precio contra ti.

Caso real: HEX/eHEX se calcula con precisión hasta el cuarto decimal por dos rutas independientes, pero todos los pares de eHEX son finos. El ratio es bueno; la ejecución, cara. Son dos hechos separados y conviene no confundirlos.
