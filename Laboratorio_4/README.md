# Laboratorio 4 – ROS 2 Humble y Turtlesim

En este laboratorio trabajé con ROS 2 Humble y el simulador Turtlesim para controlar una tortuga desde un nodo en Python. El objetivo principal fue practicar la creación de nodos, la publicación de mensajes de velocidad y la suscripción a la pose, además de usar la consola de Linux y los conceptos básicos de ROS 2 vistos en los tutoriales de Linux, ROS 2 y Turtlesim. 
## Objetivos y contexto

El laboratorio busca que podamos:
- entender cómo se comunican los nodos en ROS 2 mediante tópicos,
- usar comandos básicos de Linux para navegar, compilar y ejecutar,
- y conectar un nodo en Python con Turtlesim para mover la tortuga y generar trayectorias.

En mi caso trabajé con Ubuntu 22.04 en una máquina virtual, ROS 2 Humble instalado desde los repositorios oficiales y un workspace llamado `ros_humble/Turtlesim_ws` donde está el paquete `my_turtle_controller`.

## Desarrollo y procedimiento

Dentro del paquete `my_turtle_controller` edité el archivo `move_turtle.py`. La idea fue que todo el control de la tortuga se hiciera desde este script, sin usar el nodo `turtle_teleop_key`, tal como se pide en el enunciado del laboratorio.

Primero implementé la parte de control manual. Para leer el teclado utilicé las funciones de bajo nivel de Python (`termios`, `tty` y `select`) de manera que pudiera capturar las flechas sin necesidad de presionar Enter. Cada flecha se mapea a un comando de velocidad lineal o angular y se publica en el tópico `/turtle1/cmd_vel`. De esta forma puedo avanzar, retroceder y girar izquierda/derecha usando la tortuga de Turtlesim.

Luego implementé un control “por posición” creando una función `go_to(x, y)`. Esta función se apoya en una suscripción al tópico `/turtle1/pose` para conocer la posición y la orientación actuales. Con esa información calculo el error de posición y de ángulo hacia el punto objetivo, uso un controlador proporcional en el ángulo y limito la velocidad lineal cuando el error angular es grande. Con esto la tortuga es capaz de desplazarse de forma razonablemente suave hacia coordenadas específicas del mundo de Turtlesim.

Sobre esa función de posicionamiento construí las letras asociadas a mis iniciales: **O**, **D**, **A** y **Z**. Cada letra corresponde a una combinación de puntos y segmentos:

- La **O** se aproxima con muchos puntos sobre un círculo pequeño cuyo centro se desplaza un poco a la izquierda de la posición actual, de modo que la letra “envuelve” la posición desde la que empiezo.
- La **D** se construye como un lado vertical izquierdo y un arco derecho generado con una media circunferencia de puntos, manteniendo un tamaño similar al de la O.
- La **A** se dibuja como un triángulo isósceles con dos diagonales y una barra horizontal intermedia. Ajusté la altura y el ancho para que la letra quedara proporcionada y se viera claramente en Turtlesim.
- La **Z** se forma con tres segmentos: una base inferior, una diagonal ascendente y una base superior, empezando desde la esquina inferior derecha.

En todos los casos la tortuga parte de su posición actual y las letras se escalan alrededor de ese punto, lo que facilita escribir varias letras en distintas zonas de la pantalla sin tener que reiniciar el simulador cada vez.

## Decisiones de diseño

Algunas decisiones que tomé durante el desarrollo fueron:

- Mantener todo el comportamiento en un solo nodo (`TurtleController`), que incluye la suscripción a la pose, la publicación de velocidades y la lógica de teclado. Esto simplifica la arquitectura del laboratorio y evita depender de otros nodos de teleoperación.
- Separar la lógica de bajo nivel (publicar comandos de velocidad) de la lógica de más alto nivel (`go_to` y las funciones de dibujo de letras). Así es más fácil modificar la forma de las letras sin tocar el manejo del teclado ni el control por posición.
- Definir un tamaño común (alto y ancho) para las letras para que el nombre sea legible y visualmente consistente. En lugar de intentar curvas perfectas, prioricé formas simples (rectas y arcos aproximados) que se entendieran bien en Turtlesim.
- Usar mensajes de log (`get_logger().info`) moderados para poder depurar el comportamiento del nodo sin saturar la terminal.

## Funcionamiento general

Para ejecutar el laboratorio, primero se levanta el simulador de Turtlesim:

```bash
ros2 run turtlesim turtlesim_node
cd ~/ros_humble
colcon build

En otra terminal, dentro del workspace, se compila y se lanza el nodo de control:


source /opt/ros/humble/setup.bash
source install/setup.bash
ros2 run my_turtle_controller move_turtle


Cuando el nodo move_turtle está corriendo, las flechas del teclado permiten mover la tortuga en modo manual: arriba y abajo controlan la velocidad lineal hacia adelante y hacia atrás, mientras que izquierda y derecha controlan la velocidad angular para girar sobre su propio eje. La barra espaciadora sirve para detener la tortuga en cualquier momento.

Además del movimiento manual, el nodo reconoce varias teclas para dibujar letras de forma automática. Al presionar o, d, a o z se llaman las funciones draw_O, draw_D, draw_A y draw_Z, que usan la función go_to para mover la tortuga entre puntos específicos en el plano de Turtlesim y generar cada letra a partir de segmentos rectos y arcos. En todos los casos la letra se construye tomando como referencia la posición actual de la tortuga, de modo que es posible ir escribiendo el nombre en diferentes zonas de la ventana. Finalmente, la tecla q cierra el nodo y termina la ejecución del laboratorio.


