.
Descripción detallada de la solución planteada

La solución propuesta para el Laboratorio No. 05 integra en un mismo proyecto el control del manipulador Phantom X Pincher X100 con servomotores Dynamixel AX-12, el uso de ROS 2 Humble y la visualización en RViz, todo operado desde una interfaz HMI desarrollada en Python.

En primer lugar, se implementa un nodo de ROS 2 en Python (PincherController) que se comunica directamente con los servomotores a través de la librería dynamixel_sdk. Este nodo configura el puerto serie, la velocidad en baudios y los IDs de cada motor, y habilita el torque, la velocidad y los límites de operación. El controlador ofrece funciones para mover articulaciones individuales, enviar comandos simultáneos a todas las juntas, llevar el robot a una posición HOME y ejecutar una parada de emergencia que desactiva el torque cuando sea necesario. Además, el nodo publica de forma periódica el estado articular real en el tópico /joint_states, convirtiendo los valores en “ticks” de los Dynamixel a radianes y respetando el signo de cada articulación para que la representación en RViz coincida con el movimiento físico.

Sobre este controlador se construye la interfaz gráfica de usuario (PincherGUI), desarrollada en tkinter con un sistema de pestañas. En la parte superior de la ventana se muestran los datos del integrante del grupo (Omar David Acosta Zambrano) y el logo de la Universidad, cumpliendo el requisito de identificación del autor. La GUI incluye botones globales de HOME y parada de emergencia, así como mensajes de estado que informan al usuario sobre la ejecución de los movimientos y posibles errores.

La primera pestaña implementa el control en espacio articular mediante deslizadores: cada articulación cuenta con un slider que permite modificar su posición dentro de un rango seguro, de forma que no se excedan los límites físicos del manipulador. Al mover un slider, se envía el comando correspondiente al nodo de ROS 2 y se actualiza el valor mostrado para esa articulación. La segunda pestaña corresponde al ingreso numérico articular: el usuario puede escribir los valores deseados (en ticks) para cada motor y mover una junta de manera individual o enviar un comando conjunto para todas las articulaciones, lo que facilita reproducir configuraciones específicas de prueba.

Adicionalmente, en la pestaña de ingreso numérico se implementa la selección de poses predefinidas. Se programan cinco configuraciones articulares basadas en los ángulos sugeridos en el enunciado del laboratorio (0, 25, −35, 85, 80 grados, etc.), trasladadas a la escala de los servomotores tomando como referencia la posición HOME en 512 ticks. Cada vez que el usuario selecciona una pose, el sistema mueve el robot de forma secuencial: primero se posiciona la base, luego el hombro, codo, muñeca y finalmente el efector final, insertando una pausa de un segundo entre articulaciones para facilitar la observación y la grabación de video. Esto permite demostrar claramente que el robot alcanza cada una de las cinco configuraciones requeridas.

La tercera pestaña corresponde a la visualización en RViz. Desde esta pestaña se puede lanzar, mediante un proceso externo, el archivo de lanzamiento display.launch.py del paquete de descripción del Phantom X Pincher. Este launch inicia robot_state_publisher y RViz con el modelo URDF del manipulador. Gracias a la publicación continua de /joint_states desde el nodo PincherController, los movimientos ejecutados en la GUI se reflejan en tiempo real en el modelo 3D mostrado en RViz, permitiendo comparar la configuración del robot físico con su representación virtual.

Finalmente, la arquitectura del proyecto está pensada para integrarse con un módulo de cinemática directa basado en la tabla DH medida en el laboratorio. A partir de los valores articulares reales (leídos desde los tópicos de los controladores), este módulo permite calcular la pose cartesiana del TCP (posición X, Y, Z y orientación en ángulos Roll-Pitch-Yaw). Con estos resultados se puede completar la pestaña de control en el espacio de la tarea (sliders en X, Y, Z y RPY) y la pestaña de visualización numérica de la pose cartesiana, cumpliendo así todos los requisitos planteados en la guía del laboratorio.


## Diagrama de flujo de la solucion planteada

```mermaid
flowchart TD
    A[Inicio de la aplicación] --> B[Inicializar nodo ROS 2<br/>PincherController]
    B --> C[Configurar comunicación Dynamixel<br/>(puerto, baudrate, IDs)]
    C --> D[Habilitar torque y velocidad<br/>en cada motor]
    D --> E[Inicializar GUI Tkinter<br/>PincherGUI con pestañas]

    %% Bucle principal de interacción
    E --> F{Acción del usuario}

    %% --- Control articular por sliders ---
    F --> G[Control por sliders<br/>(Pestaña espacio articular)]
    G --> H[Leer valor del slider<br/>para cada articulación]
    H --> I[Enviar posición objetivo<br/>al motor correspondiente]
    I --> J[Actualizar posición articular interna]
    J --> K[Publicar /joint_states]
    K --> L[RViz actualiza modelo 3D]

    %% --- Ingreso numérico y poses predefinidas ---
    F --> M[Ingreso numérico y poses<br/>predefinidas (Pestaña valores)]
    M --> N[Usuario ingresa valores<br/>o selecciona 1 de las 5 poses]
    N --> O[Generar secuencia:<br/>mover una articulación a la vez<br/>con pausa de 1 s]
    O --> P[Enviar posiciones a los motores<br/>en el orden definido]
    P --> Q[Actualizar GUI<br/>(sliders, entries, estado)]
    Q --> K

    %% --- Control en espacio de la tarea ---
    F --> R[Control en espacio de la tarea<br/>(Pestaña TCP)]
    R --> S[Usuario ajusta sliders X, Y, Z, Roll, Pitch, Yaw]
    S --> T[Calcular cinemática inversa<br/>(TCP → ángulos articulares)]
    T --> U[Enviar posiciones articulares<br/>a PincherController]
    U --> J

    %% --- Visualización en RViz ---
    F --> V[Visualización en RViz<br/>(Pestaña RViz)]
    V --> W[Lanzar ros2 launch<br/>pincher_description display.launch.py]
    W --> L

    %% --- Visualización numérica de la pose cartesiana ---
    F --> X[Visualización numérica TCP<br/>(Pestaña pose cartesiana)]
    X --> Y[Tomar ángulos articulares reales]
    Y --> Z[Calcular cinemática directa<br/>(X, Y, Z, RPY)]
    Z --> AA[Actualizar valores numéricos<br/>en la interfaz]

    %% --- Parada de emergencia ---
    F --> AB[Parada de emergencia]
    AB --> AC[Desactivar torque de todos los motores]
    AC --> AD[Detener movimientos<br/>y mostrar estado de EMERGENCIA]
    AD --> F

    %% Cierre de la aplicación
    F --> AE[Cerrar aplicación]
    AE --> AF[Apagar torque y cerrar puerto<br/>Destruir nodo ROS 2]

```
