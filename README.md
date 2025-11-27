# F1-Tenth_Proyect1
Código del la navegación autónoma del vehículo F1-Tenth.
Proyecto Primer Parcial: Controlador Reactivo F1Tenth
Estudiante: Braulio Lou Garcia
Pista Asignada: BrandsHatch

Descripción General
Este repositorio contiene la implementación de un controlador autónomo reactivo para el vehículo F1Tenth, diseñado para competir en la pista BrandsHatch. El sistema utiliza el algoritmo Follow The Gap Method (FGM) optimizado con librerías numéricas (numpy) para procesamiento de alto rendimiento en tiempo real. El proyecto incluye sistemas de telemetría para el conteo de vueltas y cronometraje.

Algoritmo Implementado (Follow The Gap)
El nodo competencia_pro.py ejecuta el pipeline de control a alta frecuencia:
1. Pre-procesamiento LiDAR:
  Filtrado de valores NaN e Inf para evitar errores matemáticos.
  Recorte del Campo de Visión (FOV) para enfocar la atención en la pista inmediata y evitar distracciones traseras.
2. Seguridad (Bubble Gap):
  Identificación del obstáculo más cercano al vehículo.
  Generación de una "burbuja de seguridad" (radio dinámico) alrededor del obstáculo, poniendo a cero esos valores en el array del LiDAR. Esto obliga al robot a mantener distancia de las paredes internas en las curvas.
3. Selección del Hueco (Max Gap):
  Identificación de todas las secuencias contiguas de espacio libre (superiores al umbral de 1.5m).
  Selección del hueco más ancho disponible para garantizar pasabilidad.
4. Punto Objetivo (Goal Point):
  Dentro del hueco seleccionado, se apunta hacia el punto de mayor profundidad (distancia máxima), permitiendo una conducción agresiva y rápida en rectas.
5. Control Dinámico:
  Dirección: Suavizada mediante un promedio móvil de las últimas 5 iteraciones para evitar oscilaciones (jitter).
  Velocidad: Ajuste dinámico basado en el ángulo de giro. (Rectas: 4.5 m/s, Curvas: 1.5 m/s).

Características Técnicas (Evidencias Parte B)
El nodo incluye un sistema de telemetría integrado que cumple con los requisitos de evaluación:
  Detección de Vuelta: Implementada mediante odometría euclidiana. Se utiliza una lógica de "armado" (alejarse 3m) y "disparo" (acercarse a 1m) para evitar falsos positivos en la línea de salida.
  Cronómetro en Vivo: Visualización en la terminal del tiempo transcurrido en tiempo real.
  Best Lap: Registro y almacenamiento automático de la vuelta más rápida de la sesión.

Instrucciones de Ejecución
1. Compilar el paquete
Asegúrese de estar en la raíz del workspace (F1Tenth-Repository).
colcon build --packages-select examen_pkg
source install/setup.bash

2. Lanzar Simulación
Asegurarse de que sim.yaml apunte al mapa BrandsHatch.
ros2 launch f1tenth_gym_ros gym_bridge_launch.py

3. Ejecutar Controlador
ros2 run examen_pkg competencia_pro
