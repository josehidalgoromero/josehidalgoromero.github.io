# Puertas cuánticas

Como comenté en posts anteriores, mi idea es no entrar demasiado (al menos al principio) en conceptos matemáticos de la computación cuántica. 
No obstante, voy a hacer un pequeño posts que me sirva también a mi de recordatorio de algunos conceptos relativos a los bloques de construcción de circuítos cuánticos.

## Notación

Seguro que habéis visto esa representación de qubits: |0> y |1>. Los símbolos alrededor de los valores únicamente indican que se trata de bits cuánticos, nada más.
Sería el equivalente al 0 y al 1 de bits normales.

Así, una puerta X aplicada sobre el valor \|0> devolverá el valor \|1>. Y viceversa...

## Notación matricial

Un qubit puede ser representado por un estado vectorial:

$$ \left[\begin{matrix} 0 \\\ 1 \end{matrix}\right] $$

Y la puerta X podemos representarla como una matriz unitaria tal que así:

$$ \left[\begin{matrix} 0 & 1 \\\ 1 & 0 \end{matrix}\right] $$

De esta forma veis que al aplicar la matrix que representa la puerta X al vector que representa el estado del qubit, se produce una inversión de valores, que es el mismo efecto que hemos visto en el apartado anterior.

$$ \left[\begin{matrix} 0 & 1 \\\ 1 & 0 \end{matrix}\right] * \left[\begin{matrix} 0 \\\ 1 \end{matrix}\right] = \left[\begin{matrix} 1 \\\ 0 \end{matrix}\right] $$

## Notaciones en Qiskit

Vamos a ver estas representaciones en qiskit usando Jupyter Notebooks.

Abrimos un cuaderno nuevo de Jupyter e importamos qiskit y la visualización de matrices.

```python
from qiskit import *
from qiskit.tools.visualization import plot_bloch_multivector
```

Vamos a crear un circuíto básico, como hicimos en el post anterior, con 1 qubit y 1 bit. Aplicamos la puerta X sobre el qubit y ejecutamos el circuíto:

```python
circuit = QuantumCircuit(1,1)
circuit.x(0)
simulator = Aer.get_backend('statevector_simulator')
result = execute(circuit, backend = simulator).result()
```

Ahora vamos a usar una representación del qubit que nos será muy útil cuando empecemos a ver las distintas puertas cuánticas y como afectan al valor del qubit: 

```python
statevector = result.get_statevector()
%matplotlib inline
plot_bloch_multivector(statevector)
```

![](/images/plot_bloch_multivector.png "Representación esférica de qubit")

Podemos ver la representación matricial de la transformación aplicada al estado del qubit de la siguiente forma:

```python
circuit = QuantumCircuit(1,1)
circuit.x(0)
simulator = Aer.get_backend('unitary_simulator')
result = execute(circuit, backend = simulator).result()

unitary = result.get_unitary()
print(unitary)
```

Como véis, el resultado obtenido es la matriz de la puerta X que pintamos al comienzo de este post.
```
[[0.+0.j 1.+0.j]
 [1.+0.j 0.+0.j]]
 ```
