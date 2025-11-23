# Laboratorio No. 03
# Robotica Industrial - Analisis y Operacion del Manipulador EPSON T3-401S.

* Omar David Acosta Zambrano

### Cuadro comparativo de características técnicas

| Característica                               | Motoman MH6                                      | ABB IRB 140-6/0.8                               | EPSON T3-401S (SCARA)                                  |
|---------------------------------------------|--------------------------------------------------|-------------------------------------------------|--------------------------------------------------------|
| **Tipo de robot / estructura**              | Articulado de 6 ejes                             | Articulado de 6 ejes                            | SCARA de 4 ejes                                        |
| **Grados de libertad**                      | 6                                                | 6                                               | 4                                                      |
| **Carga útil nominal / máxima**             | 6 kg                                             | 6 kg                                            | 1 kg nominal, 3 kg máxima                              |
| **Alcance máximo horizontal**               | ≈ 1377 mm (1,38 m)                               | 800 mm (0,8 m)                                   | 400 mm (225 mm brazo 1 + 175 mm brazo 2)              |
| **Carrera eje vertical (Z)**                | No aplica como “stroke” especificado (articulado)| No aplica como “stroke” especificado (articulado) | 150 mm en el eje 3 (Z)                                |
| **Repetibilidad (posición)**                | ±0,08 mm aprox.                                  | ±0,03 mm aprox.                                  | ±0,02 mm en ejes 1–3, ±0,02° en eje 4                 |
| **Velocidad máx. en ejes principales**      | Alta, según eje y trayectoria (orden de m/s)     | Alta, según configuración y carga                | J1–J2: hasta 3700 mm/s; J3: 1000 mm/s; J4: 2600 °/s   |
| **Rango típico de movimiento**              | Rotaciones amplias en todos los ejes             | Envolvente compacto con gran rango angular       | J1: ±132°; J2: ±141°; Z: 150 mm; J4: ±360°            |
| **Peso del manipulador**                    | ≈ 130 kg                                         | ≈ 100 kg                                         | 16 kg (sin cables)                                    |
| **Modos de montaje típicos**                | Suelo, pared, techo                              | Suelo, pared, vertical/suspendido               | Montaje sobre bancada o mesa                         |
| **Aplicaciones típicas**                    | Manipulación, soldadura, carga/descarga de máquina, empaquetado | Manipulación, ensamblaje, cuidado de máquina, paletizado ligero | Pick & place de alta velocidad, ensamblaje ligero, manipulación de piezas pequeñas |
| **Entorno y protección**                    | Uso industrial general (según variante)          | Robot IP67; controlador IP54                     | Uso en celdas de automatización ligera, 5–40 °C       |


### Configuraciones *Home* del EPSON T3-401S

En el laboratorio se trabaja con la idea de que el EPSON T3-401S tenga una posición de referencia o **posición Home**, a la cual el robot puede regresar de forma segura antes y después de ejecutar trayectorias.

El T3-401S es un robot SCARA de **4 grados de libertad (J1, J2, J3, J4)**:

- **J1 – Eje de base (rotacional)**  
- **J2 – Eje de brazo (rotacional)**  
- **J3 – Eje vertical Z (lineal)**  
- **J4 – Eje de rotación de herramienta (rotacional)**  

A partir de esto, se suelen manejar dos niveles de referencia:

#### 1. Home de sistema / referencia del manipulador

Corresponde a la configuración de referencia del robot después de la calibración de motores (**MCal**) y del procedimiento de *homing*. En esta configuración, cada eje se ubica en su posición de referencia de fábrica.

De forma general, la posición de cada articulación en este **Home de sistema** se describe así:

- **J1 (base):**  
  - Ángulo cercano a **0°**.  
  - El primer brazo del SCARA queda alineado aproximadamente con el eje **X+** del sistema de coordenadas base del robot.

- **J2 (brazo):**  
  - Ángulo cercano a **0°**.  
  - El segundo brazo se alinea con el primero, de modo que el brazo completo queda totalmente extendido hacia adelante.

- **J3 (eje vertical Z):**  
  - Valor de referencia de **Z = 0** (o la cota definida como referencia vertical en el controlador).  
  - El efector final se sitúa a una **altura segura** sobre la mesa de trabajo, evitando contacto con la superficie.

- **J4 (rotación de herramienta):**  
  - Ángulo cercano a **0°**.  
  - La brida de montaje y la herramienta quedan alineadas con el eje **X** del brazo, es decir, sin giro adicional respecto al brazo.

Esta configuración es la que el controlador considera como **posición de referencia** del manipulador y sirve como base para los movimientos posteriores en coordenadas articulares y cartesianas.

#### 2. Home de trabajo / Home de usuario en EPSON RC+ 7.0

Además del Home de sistema, en el laboratorio se define normalmente una **posición Home de trabajo**, registrada en EPSON RC+ 7.0 como un punto seguro dentro del espacio de trabajo (por ejemplo,


### Procedimiento para realizar movimientos manuales del EPSON T3-401S en EPSON RC+ 7.0

A continuación se describe el procedimiento paso a paso para mover manualmente el manipulador EPSON T3-401S usando el **Robot Manager** de EPSON RC+ 7.0, cambiando entre modos por **articulaciones (Joint)** y **cartesiano (World)**, y realizando **traslaciones y rotaciones en X, Y, Z**.

---

#### 1. Acceso al Robot Manager y habilitación del robot

1. Abrir **EPSON RC+ 7.0**.
2. Crear o abrir un proyecto que tenga configurado el robot T3-401S.
3. Ir al menú **Tools → Robot Manager**.  
4. En la ventana de Robot Manager, seleccionar el robot correspondiente (real o virtual) y verificar que esté **conectado**.
5. Desde la pestaña **Panel de Control**, encender el robot:
   - Liberar paros de emergencia.
   - Activar servomotores / potencia del robot (MOTOR ON).
6. Verificar que no haya errores activos antes de pasar a **Jog & Teach**.

---

#### 2. Ingreso al modo de movimientos manuales (Jog & Teach)

1. En la parte superior de Robot Manager, seleccionar la pestaña **Jog & Teach**.
2. En esta pestaña se muestran:
   - Las coordenadas actuales del robot.
   - Los **pulsadores de JOG** (botones de movimiento).
   - El selector de **modo de movimiento** (World / Joint).
   - Opciones para registrar puntos (*Teach*).

---

#### 3. Cambio entre modos de operación: Joint vs World

En la sección **JOG & TEACH** existen al menos dos modos de movimiento principales:

- **Modo Joint (articulaciones)**  
  - Cada pulsador actúa sobre una única articulación (J1, J2, J3, J4).  
  - Permite girar o desplazar cada eje de forma independiente.  
  - Es útil para aproximar el robot a una postura deseada o para movimientos de ajuste fino de cada articulación.

- **Modo World (cartesiano)**  
  - El software calcula los movimientos de las articulaciones necesarios para que el efector final se mueva **paralelo a los ejes X, Y o Z** del sistema de coordenadas del robot.  
  - Permite realizar movimientos intuitivos de **traslación y rotación** en el espacio cartesiano.

**Para cambiar de modo:**

1. En la pestaña **Jog & Teach**, localizar el selector de modo (normalmente un cuadro de lista o botones etiquetados como **World** y **Joint**).
2. Seleccionar:
   - **Joint** para movimientos por articulaciones.
   - **World** para movimientos cartesianos.
3. Confirmar visualmente que el modo activo cambió (el modo seleccionado queda resaltado en la interfaz).

---

#### 4. Movimientos manuales por articulaciones (modo Joint)

Con el modo Joint activo:

1. En la zona de pulsadores de JOG, seleccionar la articulación que se desea mover:
   - J1 (giro de base).
   - J2 (giro del segundo brazo).
   - J3 (eje vertical Z).
   - J4 (rotación de herramienta).
2. Para cada articulación se disponen dos pulsadores:
   - **+**: movimiento en el sentido positivo del eje.
   - **−**: movimiento en el sentido negativo del eje.
3. Pulsar y mantener presionado el botón **+** o **−** para desplazar la articulación mientras se observa el robot físico o el modelo en el simulador.
4. Soltar el pulsador cuando se alcance la postura deseada.

Este modo es especialmente útil para:
- Llevar el robot a su **posición Home**.
- Evitar zonas de posible colisión ajustando uno a uno los ejes.

---

#### 5. Movimientos cartesianos: traslación en X, Y, Z (modo World)

Con el modo World activo, los pulsadores de JOG se agrupan por ejes cartesianos:

1. Identificar los botones de traslación:
   - **X+ / X−**: desplazan el efector final en la dirección del eje X (adelante/atrás respe



