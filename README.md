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

# URLs de las imágenes en GitHub (reemplázalas con tus propias URLs)
url_prendido = 'https://github.com/usuario/Quantum-Circuit-Foco/raw/main/foco_prendido.jpg'
url_apagado = 'https://github.com/usuario/Quantum-Circuit-Foco/raw/main/foco_apagado.jpg'

# Descargar las imágenes
response_prendido = requests.get(url_prendido)
foco_prendido = Image.open(BytesIO(response_prendido.content))

response_apagado = requests.get(url_apagado)
foco_apagado = Image.open(BytesIO(response_apagado.content))
4. Código Principal
El siguiente código crea un circuito cuántico, simula su ejecución y muestra las imágenes de un foco encendido o apagado basado en el resultado del qubit:
from qiskit import QuantumCircuit, Aer, execute
import matplotlib.pyplot as plt  
from PIL import Image
from qiskit.visualization import plot_histogram, plot_bloch_multivector

# Función para mostrar si el foco está encendido o apagado
def show_foco(result):
    if result == '0':
        print("El foco está APAGADO (0)")
        plt.imshow(foco_apagado)  # Muestra imagen de foco apagado
        plt.title("Foco Apagado")
        print("\n¡Es como el gato de Schrödinger!")
        print("El qubit está en el estado |0> (apagado), como el gato en estado muerto.")
    else:
        print("El foco está ENCENDIDO (1)")
        plt.imshow(foco_prendido)  # Muestra imagen de foco prendido
        plt.title("Foco Prendido")
        print("\n¡Es como el gato de Schrödinger!")
        print("El qubit está en el estado |1> (encendido), como el gato en estado vivo.")
        
    plt.axis('off')  # Quitar los ejes
    plt.show()

# Crear un circuito cuántico con 1 qubit y 1 bit clásico
qc = QuantumCircuit(1, 1)

# Inicializar el qubit en |0> (opcional, ya que el estado inicial es |0>)
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

# Mostrar el histograma de los resultados
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
5. Ejecución del código
Para ejecutar este código, asegúrate de tener las imágenes foco_prendido.jpg y foco_apagado.jpg en la misma carpeta que el script o cargarlas desde GitHub usando las URLs correctas.

