# Laboratorio 2
1)cuadro comparativo especificaciones técnicas del Motoman MH6 y el IRB140

| Ítem                                      | **Motoman MH6 (DX100/DX200)**                                                | **ABB IRB 140 (IRC5)**                                                                                                   |
| ----------------------------------------- | ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| **Grados de libertad / ejes**             | 6                                                                            | 6                                                                                                                        |
| **Carga máxima (payload)**                | **6 kg**                                                                     | **6 kg** ([library.e.abb.com][1])                                                                                        |
| **Alcance horizontal**                    | **1 422 mm**                                                                 | **810 mm (0.8 m)** ([library.e.abb.com][1])                                                                              |
| **Repetibilidad (ISO 9283)**              | **±0.08 mm**                                                                 | **±0.03 mm** (RP); **AP ≈ 0.02 mm** ([library.e.abb.com][1])                                                             |
| **Masa del manipulador**                  | **≈130 kg**                                                                  | **≈98 kg** ([library.e.abb.com][1])                                                                                      |
| **Montaje**                               | Piso, pared, techo (invertido)                                               | Piso, pared/invertido, ángulo; IRB 140T alta velocidad disponible ([library.e.abb.com][1])                               |
| **Rango de rotación por articulación**    | **J1 ±170°, J2 +155/−90°, J3 +250/−175°, J4 ±180°, J5 +225/−45°, J6 ±360°**  | **J1 ±180°, J2 +110/−90°, J3 +50/−230°, J4 ±200° (ampliable), J5 ±115°, J6 ±400° (ampliable)** ([library.e.abb.com][1])  |
| **Velocidad máx. por eje (°/s)**          | **J1 220, J2 200, J3 220, J4 410, J5 410, J6 610**                           | **J1 200, J2 200, J3 260, J4 360, J5 360, J6 450** (3-fase) ([library.e.abb.com][1])                                     |
| **Torque máx. de muñeca**                 | **J4 11.8 Nm, J5 9.8 Nm, J6 5.9 Nm**                                         | **J4 8.58 Nm, J5 8.58 Nm, J6 4.91 Nm** ([library.e.abb.com][1])                                                          |
| **Momento de inercia admisible (muñeca)** | **J4 0.27, J5 0.27, J6 0.06 kg·m²**                                          | **J5 ≤0.42 kg·m², J6 ≤0.30 kg·m²** (cargas/inercia según diagrama) ([library.e.abb.com][1])                              |
| **Protección / variantes**                | Estándar industrial (IP típico)                                              | Variantes **Foundry Plus 2** (IP67 cuerpo-muñeca) y **Clean Room ISO 6** disponibles ([library.e.abb.com][1])            |
| **Controlador**                           | **DX100/DX200** (multi-robot hasta 8/72 ejes)                                | **IRC5 + RobotWare**, opciones Absolute Accuracy, etc. ([library.e.abb.com][1])                                          |
| **Potencia / consumo**                    | **~1.5 kVA** (ficha MH6)                                                     | **Consumo típico**: **0.34–0.44 kW** según velocidad (trayecto ISO) ([library.e.abb.com][1])                             |
| **Aplicaciones típicas**                  | Manipulación, tending, empaque, dosificado, soldadura, etc.                  | Manipulación/ensamble; variantes para fundición y sala limpia; versión 140T para más velocidad. ([library.e.abb.com][1]) |

[1]: https://library.e.abb.com/public/a7121292272d40a9992a50745fdaa3b2/3HAC041346%20PS%20IRB%20140-en.pdf "Product specification - IRB 140"


2) Diferencias entre el home1 y el home2 del Motoman MH6:
   Es una postura mecánicamente estable pensada para cuando el robot está inactivo. Minimiza el par requerido en las articulaciones (cercano a cero), por lo que en el teach pendant se observa corriente prácticamente nula en los ejes. Esto reduce desgaste de frenos y motores y disminuye el consumo energético durante los periodos de espera o apagado seguro.
![her1](Imagenes/HOME_1.jpg)
![her2](Imagenes/HOME_2_2.jpg)

Es una postura de trabajo previa al ciclo. Evita singularidades y límites articulares y deja el TCP bien orientado y elevado sobre el área de operación, mejorando la alcanzabilidad de los distintos objetivos con trayectorias más cortas y seguras. En esta posición las articulaciones sí aplican par para mantenerse, por lo que el teach pendant muestra corriente en los ejes.

![her1](Imagenes/HOME_2.jpg)
![her2](Imagenes/HOME_1_1.jpg)

3) Procedimiento para realizar el movimiento manual del manipulador Motoman por articulaciones, cambiar a movimientos cartesianos y realizar movimientos de traslación y rotación en los ejes X, Y, Z.

Iniciamos  poniendo la llave de modos de funcionamiento en TEACH y ver que el E-Stop esté liberado y el área despejada.

Procedemos a encender servos. para eso pulsamos SERVO ON READY hasta que aparezca “Servo ON”.

Luego elegimos cómo mover. Presiona COORD para alternar entre JOINT (articular) o BASE/ROBOT, TOOL o USER (cartesiano) — se ve en la barra del display.

luego ejecutamos el jog. para eso mantenemos presionado el boton de hombre muerto (enabling switch) y SHIFT/INTERLOCK; luego usa las teclas:

    X±, Y±, Z± = traslaciones; Rx±, Ry±, Rz± = rotaciones (en cartesiano).

    En JOINT, esas mismas teclas actúan sobre J1–J6 (S,L,U,R,B,T).

Ajustar velocidad. Cambia con FAST/SLOW (empieza lento).

Para detener con seguridad. Se duelta el boton de hombre muerto o se pulsa la parada de emergencia; el robot se detiene de inmediato.


4) Explicación completa sobre los niveles de velocidad para movimientos manuales, el proceso para cambiar entre
niveles y cómo identificar el nivel establecido en la interfaz del robo:

El manipulador permite cuatro niveles de velocidad para movimientos manuales:
| Nivel       | Uso típico                                  | Qué hace distinto                                                             |
| ----------- | ------------------------------------------- | ----------------------------------------------------------------------------- |
| **INCHING** | Ajustes finos cerca de blancos u obstáculos | Avances muy pequeños por pulsos; máxima precisión.                            |
| **LOW**     | Enseñar puntos con seguridad                | Movimiento lento y controlado.                                                |
| **MEDIUM**  | Desplazamientos moderados en zona despejada | Compensa distancia con control aceptable.                                     |
| **HIGH**    | Traslados largos y despejados               | Velocidad más alta permitida en TEACH (siempre con visibilidad y área libre). |


5) Descripción de las principales funcionalidades de RoboDK, explicando cómo se comunica con el manipulador
Motoman y qué procesos realiza para ejecutar movimientos

¿Qué es?

RoboDK es una plataforma de simulación y programación offline para robots industriales. Permite construir la celda, definir herramientas (TCP), crear trayectorias y generar programas nativos del robot. También ofrece API en Python para automatizar tareas y trayectorias paramétricas.

Funcionalidades clave

Modelado de celda: importación CAD (piezas, mesas, útiles), definición de frames.

Herramientas/TCP: creación y calibración del TCP, cambios de herramienta.

Programación gráfica: targets, MoveJ/MoveL, velocidades, aceleraciones y blending.

Verificación: chequeo de colisiones, límites articulares, singularidades y estimación de tiempo de ciclo.

Post-procesado: generación de código nativo del controlador (p. ej., Yaskawa INFORM).

API Python: scripts para generar curvas (espiral, rosa polar, etc.), controlar I/O y parametrizar procesos.

Procesos integrados: módulos para corte, pulido, mecanizado, seguimiento de curvas y visión.

Cómo se comunica con el Motoman MH6

Dos formas de trabajo (elige según objetivo):

1) Programación offline (recomendada para entrega de programas)

Flujo: simular → verificar → post-procesar → obtener archivo INFORM (.JBI) → transferir al DX100/DX200 (USB/FTP) → ejecutar desde el pendant.

Ventaja: el controlador ejecuta su propio programa, con tiempos e I/O nativas.

2) Conexión online (ejecución directa desde la PC)

Flujo: RoboDK se conecta por red al controlador (driver) y envía comandos MoveJ/MoveL en tiempo real.

Requisitos: robot en modo remoto, servos ON y permisos activos.

Ventaja: iteración rápida de pruebas sin exportar/cargar archivos cada vez.

En ambos casos, RoboDK sincroniza frame activo, TCP, velocidades y aceleraciones para que la ejecución coincida con la simulación (dentro de los límites del controlador).

Qué ocurre al ejecutar un movimiento

Secuencia de usuario: targets + MoveJ/MoveL + velocidades + blending.

Cinemática inversa: cálculo de J1–J6 evitando límites y singularidades.

Planificación: aplicación de blending, perfiles de velocidad/aceleración y verificación de colisiones.

Traducción:

Offline: generación de instrucciones INFORM (DX100/DX200).

Online: envío de comandos al controlador por la conexión activa.

Ejecución en el DX100/DX200: el controlador interpola y mueve los ejes con sus perfiles internos y seguridades.















1) Cuadro comparativo: especificaciones técnicas Motoman MH6 vs ABB IRB 140
Ítem	Motoman MH6 (DX100/DX200)	ABB IRB 140 (IRC5)
Grados de libertad / ejes	6	6
Carga máxima (payload)	6 kg	6 kg (library.e.abb.com
)
Alcance horizontal	1 422 mm	810 mm (0.8 m) (library.e.abb.com
)
Repetibilidad (ISO 9283)	±0.08 mm	±0.03 mm (RP); AP ≈ 0.02 mm (library.e.abb.com
)
Masa del manipulador	≈130 kg	≈98 kg (library.e.abb.com
)
Montaje	Piso, pared, techo (invertido)	Piso, pared/invertido, ángulo; versión IRB 140T de alta velocidad (library.e.abb.com
)
Rango de rotación por articulación	J1 ±170°, J2 +155/−90°, J3 +250/−175°, J4 ±180°, J5 +225/−45°, J6 ±360°	J1 ±180°, J2 +110/−90°, J3 +50/−230°, J4 ±200° (ampliable), J5 ±115°, J6 ±400° (ampliable) (library.e.abb.com
)
Velocidad máx. por eje (°/s)	J1 220, J2 200, J3 220, J4 410, J5 410, J6 610	J1 200, J2 200, J3 260, J4 360, J5 360, J6 450 (3-fase) (library.e.abb.com
)
Torque máx. de muñeca	J4 11.8 Nm, J5 9.8 Nm, J6 5.9 Nm	J4 8.58 Nm, J5 8.58 Nm, J6 4.91 Nm (library.e.abb.com
)
Momento de inercia admisible (muñeca)	J4 0.27, J5 0.27, J6 0.06 kg·m²	J5 ≤0.42 kg·m², J6 ≤0.30 kg·m² (según diagrama de cargas) (library.e.abb.com
)
Protección / variantes	Estándar industrial	Foundry Plus 2 (IP67) y Clean Room ISO 6 disponibles (library.e.abb.com
)
Controlador	DX100/DX200 (multi-robot)	IRC5 + RobotWare, opciones como Absolute Accuracy (library.e.abb.com
)
Potencia / consumo	~1.5 kVA (dato de placa)	0.34–0.44 kW típicos según velocidad (perfil ISO) (library.e.abb.com
)
Aplicaciones típicas	Manipulación, tending, empaque, dosificado, soldadura	Manipulación/ensamble; variantes para fundición y sala limpia; 140T para mayor velocidad (library.e.abb.com
)

Nota: kVA (MH6) es dato de placa; kW (IRB 140) es consumo medido. No son comparables 1:1.

2) Diferencias entre HOME1 y HOME2 del Motoman MH6

HOME1 — Reposo/estacionamiento.
Postura mecánicamente estable para inactividad. Minimiza el par en las articulaciones (corriente casi nula en el pendant), reduce desgaste de frenos/motores y baja el consumo durante espera o apagado seguro.




HOME2 — Preparado/ready.
Postura previa al ciclo. Evita singularidades y límites, deja el TCP alto y bien orientado sobre el área de trabajo, mejorando alcanzabilidad y rutas. Requiere par para mantenerse (se observa corriente en ejes).




3) Procedimiento de jog: articular ↔ cartesiano; traslación y rotación en X/Y/Z

Preparar
Llave en TEACH. E-Stop liberado y zona despejada.

Activar servos
Pulsa SERVO ON READY hasta ver “Servo ON”.

Elegir el modo
Presiona COORD y selecciona:

JOINT (articular: J1–J6).

BASE/ROBOT, TOOL o USER (cartesiano: X, Y, Z, Rx, Ry, Rz).
La selección aparece en la barra superior del display.

Mover
Mantén el hombre muerto en posición media y SHIFT/INTERLOCK.
Usa las teclas: X±, Y±, Z± (traslación) y Rx±, Ry±, Rz± (rotación) en cartesiano.
En JOINT, esas teclas actúan sobre J1–J6 (S, L, U, R, B, T).

Velocidad / parada
Ajusta con FAST/SLOW (empieza lento).
Para detener: suelta el hombre muerto o pulsa HOLD.

4) Niveles de velocidad en movimientos manuales (Teach)

El robot ofrece cuatro niveles para jog:

Nivel	Uso típico	Qué cambia
INCHING	Ajustes finos junto a piezas u obstáculos	Avances por pulsos muy pequeños; máxima precisión.
LOW	Enseñar puntos con seguridad	Movimiento lento y controlado.
MEDIUM	Desplazamientos moderados con área despejada	Mayor rapidez manteniendo control aceptable.
HIGH	Traslados largos y despejados	Velocidad más alta permitida en TEACH (siempre con visibilidad).

Cómo cambiar: teclas SLOW (↓) y FAST (↑).
Cómo identificar: el nivel activo (INCH / LOW / MED / HIGH) se muestra en la barra de estado del pendant y cambia al instante cuando presionas SLOW/FAST.

5) RoboDK: funcionalidades, comunicación con Motoman y ejecución de movimientos

Qué es.
RoboDK es una plataforma de simulación y programación offline. Permite construir la celda, definir herramientas (TCP), crear trayectorias y generar programas nativos. También ofrece API en Python para automatizar procesos y trayectorias paramétricas.

Funcionalidades clave.

Modelado de celda (importación CAD), definición de frames.

Herramientas/TCP: creación y calibración.

Programación gráfica: targets, MoveJ/MoveL, velocidades, aceleraciones, blending.

Verificación: colisiones, límites, singularidades, estimación de tiempo de ciclo.

Post-proceso: generación de código INFORM (Yaskawa) u otros.

API Python: curvas (espiral, rosa), control de I/O, parametrización.

Módulos de proceso: corte, pulido, mecanizado, seguimiento de curvas, visión.

Cómo se comunica con el MH6.

Offline (recomendado para entrega): simular → verificar → post-procesar → obtener .JBI → transferir al DX100/DX200 (USB/FTP) → ejecutar desde pendant.

Online (directo desde la PC): conexión por red (driver), envío de comandos MoveJ/MoveL en tiempo real. Requiere modo remoto y servos ON.

Qué ocurre al mover.

Secuencia (targets + MoveJ/MoveL + velocidades + blending).

Cinemática inversa → juntas J1–J6 válidas (evita límites/singularidades).

Planificación → aplica blending, perfiles de velocidad/aceleración y verifica colisiones.

Traducción → INFORM .JBI (offline) o comandos por red (online).

Ejecución en el DX100/DX200 con perfiles internos y seguridades.
