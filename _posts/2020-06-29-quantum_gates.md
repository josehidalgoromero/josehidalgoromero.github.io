# Puertas cuánticas

Como comenté en posts anteriores, mi idea es no entrar demasiado (al menos al principio) en conceptos matemáticos de la computación cuántica. 
No obstante, voy a hacer un pequeño posts que me sirva también a mi de recordatorio de algunos conceptos relativos a los bloques de construcción de circuítos cuánticos.

## Notación

Seguro que habéis visto esa representación de qubits: |0> y |1>. Los símbolos alrededor de los valores únicamente indican que se trata de bits cuánticos, nada más.
Sería el equivalente al 0 y al 1 de bits normales.

Así, una puerta X aplicada sobre el valor \|0> devolverá el valor \|1>. Y viceversa...

## Notación matricial

Un qubit puede ser representado por un estado vectorial:

$$ \left[\begin{matrix} 1 \\\ 0 \end{matrix}\right] $$

Y la puerta X podemos representarla como una matriz unitaria tal que así:

$$ \left[\begin{matrix} 0 & 1 \\\ 1 & 0 \end{matrix}\right] $$

De esta forma veis que al aplicar la matrix que representa la puerta X al vector que representa el estado del qubit, se produce una inversión de valores, que es el mismo efecto que hemos visto en el apartado anterior.

$$ \left[\begin{matrix} 0 & 1 \\\ 1 & 0 \end{matrix}\right] * \left[\begin{matrix} 1 \\\ 0 \end{matrix}\right] = \left[\begin{matrix} 0 \\\ 1 \end{matrix}\right] $$

