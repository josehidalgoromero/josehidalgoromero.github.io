# Hello Quantum

A pesar de que en las siguientes entradas quiero profundizar un poco en los componentes de la computación cuántica (qubits, puertas, circuitos, propiedades, etc.) me gustaría hacer un típico 'Hola Mundo' con Qiskit, que va a ser la creación de un circuito muy simple y la visualización de las salidas.

## Creación del circuito básico

Abrimos, si no lo habéis hecho ya, Jupyter Notebooks y creamos un nuevo cuaderno. 

Importamos de qiskit todo con la línea (recordad que los bloques de código se ejecutan con shift+enter):
```python
from qiskit import *
```

Creamos dos registros: uno cuántico (de 2 qubits) y otro clásico (de 2 bits). El registro clásico lo vamos a usar para aplicar las mediciones sobre él y poder representarlo gráficamente.
```python
qr = QuantumRegister(2)
cr = ClassicalRegister(2)
```

Ahora creamos nuestro primer circuito con la siguiente instrucción:
```python
first_circuit = QuantumCircuit(qr, cr)
```

Si queréis ir viendo como va quedando el circuito, podéis ejecutar este bloque en cualquier momento. No tenéis porque volver a escribirlo en un nuevo bloque, podéis simplemente posicionaros en ese bloque del cuaderno y volver a ejecutarlo:
```python
%matplotlib inline
first_circuit.draw()
```

Ahora mismo os pintará los registros vacíos, no os preocupéis, que enseguida empezaremos a visualizar cosas...

![](/images/first_circuit_draw.png "Circuito vacio")

Ahora vais a tener que hacer un pequeño acto de fe, vamos a agregar una puerta Hadamard (H) sobre el primer qubit del registro cuántico para crear un 'entanglement'. Ya profundizaremos en las puertas de los circuitos algo más adelante para intentar comprenderlas...
```python
first_circuit.h(qr[0])
first_circuit.draw()
```

![](/images/first_circuit_hadamard.png "Puerta de Hadamard")

Vemos la puerta Hadamard en nuestro circuito. Fantástico. Ahora vamos a aplicar la primera operación que va a operar sobre los 2 qubits del registro cuántico: una puerta controlada, que es como una instrucción IF en un lenguaje de programación.
```python
first_circuit.cx(qr[0], qr[1])
first_circuit.draw()
```

![](/images/first_circuit_controlled_gate.png "Puerta controlada")

Con este circuito hemos creado el 'entanglement' entre el qubit 0_0 y el qubit 0_1. Vamos a guardar los valores de nuestros qubits en los bits del registro clásico (cr) para poder representar los valores gráficamente:
```python
first_circuit.measure(qr, cr)
first_circuit.draw()
```

![](/images/first_circuit_measure.png "Midiendo nuestros qubits")

Bueno, con el circuito construido ahora sólo falta 'encenderlo'. ¿Como encendemos nuestro circuito? Fácil. Vamos a hacerlo primero simulando un ordenador cuántico en nuestro ordenador y posteriormente ejecutaremos el circuito en un ordenador cuántico real de IBM para ver la diferencia de los resultados obtenidos.
Podemos obtener un simulador desde el componente Air de Qiskit con la siguiente instrucción (QASM viene de Quantum Assembly Language, que es el lenguaje al que se traducen las instrucciones):

```python
quantum_simulator = Aer.get_backend('qasm_simulator')
```

Y ejecutamos el circuito en este simulador usando la instrucción:

```python
result = execute(first_circuit, backend = quantum_simulator).result()
```

Para ver lo que contiene los resultados, vamos a importar las herramientas de visualización de Qiskit, específicamente la representación de histogramas.

```python
from qiskit.tools.visualization import plot_histogram
plot_histogram(result.get_counts(first_circuit))
```

![](/images/first_circuit_histogram_result.png "Representando los resultados")

## Ejecutando el circuito en un ordenador cuántico

Si habéis seguido los pasos del post anterior tendréis la cuenta de acceso a IBMQ almacenada en vuestro equipo. Vamos a cargar la cuenta para poder acceder a los ordenadores cuánticos y lanzar nuestro experimento allí.

```python
from qiskit import IBMQ
IBMQ.load_account()
```

Ahora necesitamos obtener una referencia al ordenador donde vamos a lanzar el código. Esto lo hacemos en dos partes:
1. Obtenemos el proveedor de IBM-Q
2. Seleccionamos el backend donde deseemos ejecutar el código.

Los backends disponibles podéis verlos en la parte derecha de la web de [IBM-Q](https://quantum-computing.ibm.com/login) donde hicimos la cuenta.

![](/images/ibmq-backends.png "Backends de IBM-Q")

```python
provider = IBMQ.get_provider('ibm-q')
quantum_computer = provider.get_backend('ibmq_16_melbourne')
```

La ejecución en un ordenador cuántico no es inmediata, como en nuestro ordenador local, y tendremos que lanzar un proceso de ejecución (job) y esperar que finalice para obtener los resultados del mismo.

```python
job = execute(first_circuit, backend=qcomp)
quantum_computer = provider.get_backend('ibmq_16_melbourne')
```

Para monitorizar el estado de nuestro job vamos a importar job_monitor:

```python
from qiskit.tools.monitor import job_monitor
job_monitor(job)
```

![](/images/job_queued.png "Esperando la ejecución")

La salida del comando de monitorización se irá refrescando automáticamente reflejando nuestra posición en la cola o la ejecución del job.
Una vez termine (Job Status: job has successfully run) podemos obtener los resultados de la ejecución y representar un histograma de la misma manera que lo hicimos anteriormente:

```python
result = job.result()
plot_histogram(result.get_counts(first_circuit))
```

![](/images/first_circuit_quantum_histogram.png "Resultado de ejecución en ordenador cuántico")

Como véis la diferencia más notable es que en esta última ejecución hemos obtenido unos valores intermedios 01 y 10 que no aparecían en la ejecución local. 
La razón de ello es que en nuestro equipo se simula un ordenador cuántico perfecto mientras que los ordenadores cuánticos tienen unos errores propios del hardware.
Con el tiempo, es de esperar que estos errores vayan disminuyendo y los resultados se aproximen cada vez más a las simulaciones.
