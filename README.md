# Proyecto-Quantum Circuit - Foco Encendido o Apagado
Este proyecto simula un circuito cuántico que muestra si un foco está encendido o apagado, usando Qiskit y visualizaciones gráficas. El resultado se representa mediante imágenes de un foco y explicaciones del experimento.

1. Entorno o Plataforma
El código está diseñado para ejecutarse en Python utilizando la biblioteca Qiskit, una herramienta cuántica de IBM. 
2. Dependencias y Bibliotecas
Antes de ejecutar el código, necesitas instalar las siguientes bibliotecas:
Qiskit: Para crear y simular circuitos cuánticos.
Matplotlib: Para graficar los resultados.
Pillow (PIL): Para cargar y mostrar las imágenes.
Requests: Si cargas imágenes desde un repositorio de GitHub.
Para instalar las dependencias, ejecuta los siguientes comandos en tu terminal o entorno de Python:
pip install qiskit matplotlib pillow requests

CODIGOS:

#Bibliotecas que se deben importar
from qiskit import QuantumCircuit, Aer, execute
import matplotlib.pyplot as plt  
from PIL import Image
from qiskit.visualization import plot_histogram, plot_bloch_multivector

# Cargar las imágenes
foco_prendido = Image.open('foco_prendido.jpg')  # Asegúrate de que la imagen esté en la misma carpeta
foco_apagado = Image.open('foco_apagado.jpg')

# Función para mostrar si el foco está encendido o apagado
def show_foco(result):
    if result == '0':
        print("El foco está APAGADO (0)")
        plt.imshow(foco_apagado)  # Muestra imagen de foco apagado
        plt.title("Foco Apagado")
        # Explicación para el caso en que el resultado es 0
        print("\n¡Es como el gato de Schrödinger!")
        print("El qubit está en el estado |0> (apagado), al igual que el gato está en un estado muerto.")
    else:
        print("El foco está ENCENDIDO (1)")
        plt.imshow(foco_prendido)  # Muestra imagen de foco prendido
        plt.title("Foco Prendido")
        # Explicación para el caso en que el resultado es 1
        print("\n¡Es como el gato de Schrödinger!")
        print("El qubit está en el estado |1> (encendido), al igual que el gato está en un estado vivo.")
        
    plt.axis('off')  # Quitar los ejes
    plt.show()

# Crear un circuito cuántico con 1 qubit y 1 bit clásico
qc = QuantumCircuit(1, 1)

# Inicializar el qubit en |0> (esto es opcional, ya que el estado inicial predeterminado es |0>)
qc.initialize([1, 0], 0)

# Aplicar puerta Hadamard al qubit para crear una superposición
qc.h(0)

# Medir el qubit en el bit clásico
qc.measure(0, 0)

# Dibujar el circuito cuántico
qc.draw('mpl')
plt.show()

# Usar el simulador cuántico de Qiskit (Aer) para obtener los conteos de medición
simulator = Aer.get_backend('qasm_simulator')

# Ejecutar el circuito
job = execute(qc, simulator, shots=1000)  # Ejecutar el circuito 1000 veces
result = job.result()
counts = result.get_counts(qc)

# Mostrar el histograma de los resultados (con dos barras)
plot_histogram(counts, title="Histograma de Mediciones")
plt.show()

# Visualizar el estado cuántico en la esfera de Bloch
simulator_sv = Aer.get_backend('statevector_simulator')
job_sv = execute(qc, simulator_sv)
statevector = job_sv.result().get_statevector()

plot_bloch_multivector(statevector)
plt.show()

# Obtener el resultado (0 o 1) del experimento
foco_state = max(counts, key=counts.get)

# Mostrar si el foco está encendido o apagado
show_foco(foco_state)
