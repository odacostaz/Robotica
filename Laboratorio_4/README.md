# Laboratorio 4 – Intro a ROS 2 Humble con Turtlesim

Repositorio del Laboratorio No. 04 de **Robótica de Desarrollo – 2025-II**, cuyo objetivo principal es controlar la tortuga de *turtlesim* usando ROS 2 Humble y un script propio en Python.  

En este trabajo implementé un nodo que:

- Permite mover la tortuga con las flechas del teclado (modo manual).
- Dibuja automáticamente las letras **O, D, A y Z**, correspondientes a mis iniciales:  
  **O**mar **D**avid **A**costa **Z**ambrano.
- Controla todo el movimiento **exclusivamente** desde el archivo `move_turtle.py`, sin usar `turtle_teleop_key`.  

---

## 1. Objetivos del laboratorio

- Recordar el manejo básico de la terminal en Linux.  
- Configurar un workspace de ROS 2 y crear un paquete en Python.  
- Conectar nodos de ROS 2 mediante tópicos (`/turtle1/cmd_vel` y `/turtle1/pose`).  
- Diseñar un controlador propio para *turtlesim* que reciba entradas de teclado y genere trayectorias en forma de letras.

---

## 2. Entorno de trabajo

- **Sistema operativo:** Ubuntu 22.04 (ejecutado en máquina virtual).
- **ROS:** ROS 2 Humble.
- **Simulador:** `turtlesim`.
- **Workspace:** `~/ros_humble/Turtlesim_ws`.
- **Paquete:** `my_turtle_controller` (tipo `ament_python`), creado dentro de `src/`.

Dentro del paquete, el archivo principal que desarrollé es:

- `my_turtle_controller/move_turtle.py`  
  Nodo `turtle_controller` que se encarga de todo el control.

---

## 3. Desarrollo del controlador

### 3.1. Nodo y tópicos utilizados

El nodo `turtle_controller` hace dos cosas principales:

- **Publica** en `/turtle1/cmd_vel` (tipo `geometry_msgs/Twist`) para enviar velocidades lineales y angulares a la tortuga.
- **Se suscribe** a `/turtle1/pose` (tipo `turtlesim/Pose`) para tener en todo momento la posición `(x, y)` y el ángulo `theta`.

Con esa información implementé tanto el modo manual como las funciones de dibujo de letras.

---

### 3.2. Lectura de teclado sin `turtle_teleop_key`

Una de las restricciones del laboratorio es no usar el nodo `turtle_teleop_key`, así que la lectura del teclado tenía que hacerse directamente en Python.  

Para eso usé:

- `termios` y `tty`: para poner la terminal en modo “raw” y leer teclas sin necesidad de presionar Enter.
- `select`: para no bloquear el programa mientras espero una tecla.

La función `get_key()`:

- Devuelve las secuencias de 3 caracteres para las flechas (`'\x1b[A'`, `'\x1b[B'`, etc.).
- Devuelve una sola letra para teclas normales (`'o'`, `'d'`, `'a'`, `'z'`, `'q'`, espacio, …).
- Usa un `timeout` pequeño para que el nodo pueda seguir ejecutando `rclpy.spin_once()` y procesando la pose de la tortuga.

---

### 3.3. Control de movimiento manual

En el bucle principal (`main`), según la tecla leída se publica un `Twist` diferente:

- **Flecha ↑**: velocidad lineal positiva → avanzar.
- **Flecha ↓**: velocidad lineal negativa → retroceder.
- **Flecha ←**: velocidad angular positiva → girar a la izquierda.
- **Flecha →**: velocidad angular negativa → girar a la derecha.
- **Espacio**: se publica un `Twist` en cero para detener la tortuga.

Las velocidades usadas (`manual_linear_speed` y `manual_angular_speed`) se definieron como parámetros de la clase para poder ajustarlas fácilmente.

---

### 3.4. Control por posición (`go_to`)

Para dibujar letras no basta con mandar velocidades constantes: necesito que la tortuga llegue a puntos específicos en el plano.  
Por eso implementé la función `go_to(x_goal, y_goal)`, que:

1. Calcula la diferencia `dx`, `dy` entre la posición objetivo y la actual.
2. Obtiene la distancia al objetivo y el ángulo deseado (`atan2(dy, dx)`).
3. Calcula el error angular (`ang_error`) y lo normaliza al rango `[-π, π]`.
4. Usa un control proporcional sobre ese error:
   - Si el error angular es grande, solo gira (velocidad lineal = 0).
   - Si el error angular es pequeño, avanza hacia el objetivo.
5. Se detiene cuando la distancia es menor a una tolerancia (`distance_tolerance`).

Con esta función puedo construir letras moviendo la tortuga a una secuencia de puntos `(x, y)`.

---

## 4. Letras personalizadas (O, D, A, Z)

A partir de mis iniciales implementé cuatro funciones: `draw_O()`, `draw_D()`, `draw_A()` y `draw_Z()`. Cada una calcula puntos clave y llama internamente a `go_to()`.

### 4.1. Letra O

- Se tomó un radio pequeño (`radius = 0.7`) para que la O no ocupe toda la pantalla.
- El centro del círculo se ubicó ligeramente a la izquierda de la posición actual (`cx = x - radius`, `cy = y`).
- La O se aproxima con `num_points` puntos igualmente espaciados en ángulo entre `0` y `2π`.
- Para cada punto se calcula:
  \[
  x = c_x + r\cos(\theta),\quad y = c_y + r\sin(\theta)
  \]
  y se llama a `go_to(x, y)`.

De esta forma la O es “redonda” y siempre se dibuja alrededor de donde esté la tortuga.

---

### 4.2. Letra D

La D se construyó a partir de un rectángulo y un arco:

- Punto inicial: esquina inferior izquierda de la letra.
- Se traza primero el lado vertical izquierdo (de abajo hacia arriba).
- Luego un pequeño segmento horizontal superior hacia la derecha.
- Después se genera el arco derecho como media circunferencia, usando varios puntos entre `π/2` y `-π/2`.
- Finalmente se cierra la letra regresando a la base izquierda.

Los parámetros `height`, `width` y `radius` se eligieron para que la D tenga una altura comparable con el diámetro de la O.

---

### 4.3. Letra A

La A se diseñó como un triángulo isósceles con una barra central:

- Punto inicial: esquina inferior izquierda.
- Primer segmento: diagonal hacia arriba y derecha hasta el vértice superior.
- Segundo segmento: diagonal hacia abajo hasta la esquina inferior derecha.
- Luego se dibuja la barra horizontal a mitad de altura, desde un punto cercano a la pierna derecha hasta un punto simétrico en la izquierda.
- Por último, se regresa a la base izquierda para cerrar la forma.

El ancho de la base y la altura se escogieron para que la A sea consistente con la D y la Z.

---

### 4.4. Letra Z

La Z se compone de tres segmentos rectos:

- Punto inicial: esquina inferior derecha.
- Segmento 1: base inferior horizontal, moviéndose hacia la izquierda.
- Segmento 2: diagonal ascendente desde la esquina inferior izquierda hasta la esquina superior derecha.
- Segmento 3: base superior horizontal, nuevamente hacia la izquierda.

Todos los puntos se calculan a partir de la posición inicial `(x0, y0)` y dos parámetros: `height` (altura de la Z) y `base_width` (ancho). De esta forma la Z mantiene proporciones similares a las otras letras.

---

## 5. Funcionamiento general y ejecución

Para ejecutar el laboratorio:

1. Compilar el workspace:

   ```bash
   cd ~/ros_humble
   colcon build
   source /opt/ros/humble/setup.bash
   source install/setup.bash
