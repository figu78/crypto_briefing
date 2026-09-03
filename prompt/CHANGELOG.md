# Por qué cada regla está donde está

Cada una de estas reglas se añadió porque el informe falló sin ella. Si vas a quitar alguna, esto es lo que perderías.

## "Nunca uses precios de memoria"

Sin esta línea, el modelo tira de precios plausibles de su entrenamiento en lugar de buscarlos. Salen informes coherentes con números de hace meses. Es el fallo más peligroso porque el resultado parece perfecto.

## "Si un dato no se puede verificar, dilo en lugar de estimarlo"

El impulso por defecto es rellenar el hueco con algo verosímil. Un funding rate inventado no se distingue de uno real hasta que operas con él. Declarar el hueco es más útil que taparlo.

## "Si dos fuentes dan cifras distintas, da el rango"

Sin esto, se elige una fuente en silencio y se pierde la información de que hay desacuerdo — que a veces es el dato más relevante del día.

## "Los niveles deben derivarse de los datos reales de hoy"

Sin esta regla, los niveles del informe de ayer reaparecen hoy. Se convierten en fijos y dejan de significar nada.

## "Cada nivel con una frase de por qué importa"

Obliga a que el nivel tenga una razón: máximo de 24 h, media móvil, clúster de liquidación, strike con volumen. Un nivel sin motivo es un número inventado con aspecto de análisis.

## "Cada escenario con invalidación numérica"

Un escenario sin invalidación no se puede equivocar, y lo que no se puede equivocar no informa.

## "Contrasta Farside con una nota de prensa del día"

Añadida el 3 de septiembre de 2026, después de que la tabla diera +8,4 M$ para una sesión que acabó siendo −236,5 M$. La tabla se rellena a lo largo de la mañana de Nueva York.

## "No uses los precios de CoinGecko para tokens pequeños"

Añadida el 3 de septiembre de 2026. Los ceros en subíndice se pierden al extraer el texto y el precio sale mal por factores de miles. Los porcentajes sí valen.

## "Comprueba la liquidez antes de fiarte de un precio de DEX"

Añadida el 3 de septiembre de 2026, tras encontrar pools de 900 $ de liquidez devolviendo precios con toda naturalidad.

## "Fija el contrato de cada token"

Añadida el 3 de septiembre de 2026. Cuatro contratos distintos con el mismo nombre, precios entre 0,00001 $ y 8.624 $.

## "Sin histórico del ratio no puedes decir que esté caro"

Añadida el 3 de septiembre de 2026. Es la regla que más tienta romper: una divergencia del 123 % pide a gritos la palabra "estirado", y no hay nada en el dato que la justifique.

## "Tono neutro, sin destinatario imaginado"

La primera versión del prompt describía a un usuario que comentaba en directo mientras operaba, y el informe se llenó de frases del tipo "esto no lo afirmes en antena". Si tu informe es personal, quita cualquier destinatario que no seas tú: cambia el tono de todo el texto.
