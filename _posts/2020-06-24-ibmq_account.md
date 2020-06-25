# Crear una cuenta en IBM-Q

Aunque normalmente ejecutaremos nuestros programas en un equipo local, una vez depurados nos gustaría poder ejecutarlas en un ordenador cuántico real. 
Para ello podemos usar los que IBM nos pone a nuestra disposición. Sólo nos hará falta un token que se obtiene desde la misma página de IBM previo registro de una cuenta, que es lo que vamos a hacer a continuación.

## Obtener un token para IBM-Q

![](/images/ibmq-my-account.png "IBMQ My Account")

1. Entra en la web de [IBM-Q](https://quantum-computing.ibm.com/login) y create una cuenta (si no la tienes ya).
2. Haz click en 'My Account'
3. Haz click en **Copy token**

![](/images/ibmq-copy-token.png "IBMQ copy token")

## Guardar el token

Una vez obtenido el token deberemos almacenarlo en nuestro equipo en un fichero llamado `qiskitrc`. Para ello abrimos Anaconda Navigator y pulsamos 'Launch' en el botón de la aplicación Jupyter Notebook.

Una vez abierto Jupyter Notebook haced click en el botón New -> Python 3.

![](/images/jupyter-notebook-new.png "new jupyter notebook")

Se os abrirá un cuaderno con la primera línea en blanco para comenzar a desarrollar. Os recomiendo hacer click sobre el nombre del cuaderno (Untitled) para darle un nombre descriptivo a lo que vayais a hacer.
Cada bloque de código del cuaderno Jupyter se ejecuta con la combinación `shift + enter`.

![](/images/jupyter-notebook-empty.png "empty jupyter notebook")

Escribid las siguientes líneas en vuestro cuaderno Jupyter:

```python
import qiskit
```

```python
from qiskit import IBMQ
```

```python
IBMQ.save_account('<YOUR TOKEN>')
```

Recordad pulsar `shift + enter` después de cada línea (veréis una salida OUT tras cada ejecución). Mientras el bloque está en ejecución aparecerá el número del bloque como un asterisco.

Puedes comprobar que todo ha ido bien y que tienes acceso a los ordenadores cuánticos de IBM ejecutando el siguiente comando:

```python
IBMQ.load_account('')
```

Tu cuaderno Jupyter debe ser algo así:

![](/images/importing-ibmq-token.png "Importing IBMQ token")
