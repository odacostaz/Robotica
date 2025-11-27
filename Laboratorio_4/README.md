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

### 3.2. Lectura de teclado sin `turtle_teleo_
