# Teletransporte cuántico (de valores)

Vamos a ver una funcionalidad trivial en ordenadores clásicos que, debido a la naturaleza cuántica, no es tan trivial en los ordenadores cuánticos: el copiado de valores.
Esta asignación de valores no es posible hacerla en computación cuántica ya que, para ello, es necesario observar el valor del qubit que queremos copiar y, al hacerlo, destruímos su estado por lo que no podremos copiarlo.

Vamos a usar la propiedad de 'entanglement' o enlazado sobre el circuíto para poder hacer este 'teletransporte' de valores sin destruir el estado del qubit original.

## Comenzamos el circuito

Arrancamos Jupyter Notebooks y creamos un nuevo fichero Python3 en el entorno donde tengamos instalado Qiskit. Lo importamos y creamos un cirtuíto con 3 qubits y 3 bits, pero en lugar de hacer:

```python 
from qiskit import *
qr = QuantumRegister(3)
cr = ClassicalRegister(3)
circuit = QuantumCircuit(qr, cr)
```

Vamos a usar una forma abreviada, tal que así:

```python 
from qiskit import *
circuit = QuantumCircuit(3, 3)
```

Vamos a ir dibujando en cada paso el circuíto para ir viendo los diferentes cambios:

```python 
%matplotlib inline
circuit.draw(output='mpl')
```

![](/images/empty_circuit_3_3.png "Circuito vacio")

La idea es, partiendo del estado `|0>` del qubit 0, modificar su estado al valor `|1>` y transportarlo al qubit 2 usando las puertas que hemos visto hasta ahora.

## Asignando estado al qubit 0

Como hemos dicho, vamos a invertir el estado original del qubit 0, para tomarlo como origen en el valor a transferir:

```python
circuit.x(0)
circuit.barrier()
circuit.draw(output='mpl')
```

![](/images/circuit_3_3_x_0.png "Inversión de valor del qubit 0")

## Aplicando el algoritmo de teletransporte

Para aplicar el algoritmo hacen falta algunos pasos:

### Paso 1

Creamos un enlazamiento entre los qubits 1 y 2. Para ello aplicamos una puerta H sobre el qubit 1 y una puerta CX sobre los qubits 1 y 2.

```python
circuit.h(1)
circuit.cx(1,2)
```

![](/images/teleportation_protocol_1.png "Paso 1 de algoritmo de teletransporte")

Aplicar una puerta H al qubit 1 provocará que tenga un estado de superposición, es decir, que tenga la misma probabilidad de tener el estado `|0>` que el estado `|1>`.

La puerta CX puede funcionar de 2 maneras: como una puerta de control, en el que el primer qubit indicado hace de control y el segundo de objetivo. 
En este caso se aplica una inversión del valor del qubit objetivo cuando el qubit de control valga `|1>`. Es decir:

| qubit 0 | qubit 1 (antes de puerta CX) | qubit 1 (después de puerta CX) |
|-|-|-|
| 0 | 1 | 1 |
| 0 | 0 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

Si el qubit de control está en estado de superposición entonces la puerta CX crea un estado de enlazamiento entre el qubit de control y el qubit objetivo.

### Paso 2

Aplicamos una puerta CX entre el qubit 0 y el qubit 1. Posteriormente aplicamos una puerta H al qubit 0.

```python
circuit.cx(0,1)
circuit.h(0)
circuit.barrier()
circuit.draw(output='mpl')
```

![](/images/teleportation_protocol_2a.png "Paso 2a de algoritmo de teletransporte")

Vamos a aplicar medidas de los qubits 0 y 1 sobre los registros clásicos 0 y 1.

```python
circuit.measure([0,1], [0,1])
circuit.draw(output='mpl')
```

![](/images/teleportation_protocol_2b.png "Paso 2b de algoritmo de teletransporte")

### Paso 3

Finalmente aplicamos una puerta CX entre el qubit 1 y el qubit 2, y una puerta CZ entre el qubit 0 y el qubit 2.

```python
circuit.barrier()
circuit.cx(1,2)
circuit.cz(0,2)
circuit.draw(output='mpl')
```

![](/images/teleportation_protocol_3.png "Paso 3 de algoritmo de teletransporte")

La aplicación de la puerta CX ya la conocemos. La puerta CZ varia el estado `|+>` a `|->` y viceversa, si el qubit de control es `|1>`.

Si vemos el estado del qubit como una esfera, como mostramos en el post anterior, la puerta X haría algo así (La puerta Y haría lo mismo que la puerta X pero pivotando sobre el eje Y en lugar de sobre el X):

![](/images/gate-x.png "Puerta X")

Y la puerta Z haría algo así:

![](/images/gate-z.png "Puerta Z")

## Comprobación del resultado

Vamos a comprobar que realmente el estado del qubit 0 (que recordamos que es |1> tras la aplicación de la puerta X al comienzo del circuito) se ha teletransportado al qubit 2.

Aplicamos la medida sobre el qubit 2 y ejecutamos la simulación del circuito.

```python
circuit.barrier()
circuit.measure(2,2)
circuit.draw(output='mpl')
```

```python
simulator = Aer.get_backend('qasm_simulator')
result = execute(circuit, backend = simulator).result()
from qiskit.tools.visualization import plot_histogram
plot_histogram(result.get_counts(circuit))
```

![](/images/teleportation_protocol_results.png "Resultados del algoritmo de teletransporte")

Como vemos en los resultados del histograma. El bit 2, correspondiente a la medida del qubit 2, es siempre 1.

| bit 2 | bit 1 | bit 0 | probability |
|-|-|-|-|
| 1 | 0 | 0 | 0.258 |
| 1 | 0 | 1 | 0.248 |
| 1 | 1 | 0 | 0.259 |
| 1 | 1 | 1 | 0.235 |
