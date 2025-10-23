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


4) Explicación completa sobre los niveles de velocidad para movimientos manuales, el proceso para cambiar entre
niveles y cómo identificar el nivel establecido en la interfaz del robo:

El manipulador permite cuatro niveles de velocidad para movimientos manuales:
| Nivel       | Uso típico                                  | Qué hace distinto                                                             |
| ----------- | ------------------------------------------- | ----------------------------------------------------------------------------- |
| **INCHING** | Ajustes finos cerca de blancos u obstáculos | Avances muy pequeños por pulsos; máxima precisión.                            |
| **LOW**     | Enseñar puntos con seguridad                | Movimiento lento y controlado.                                                |
| **MEDIUM**  | Desplazamientos moderados en zona despejada | Compensa distancia con control aceptable.                                     |
| **HIGH**    | Traslados largos y despejados               | Velocidad más alta permitida en TEACH (siempre con visibilidad y área libre). |


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

6) Análisis comparativo: RoboDK vs RobotStudio:

| Criterio                     | **RoboDK**                                                                                               | **RobotStudio (ABB)**                                                                                       |
| ---------------------------- | -------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| **Compatibilidad**           | Multimarca (ABB, Yaskawa/Motoman, KUKA, Fanuc, UR, etc.)                                                 | Exclusivo ABB (IRB, GoFa, SWIFTI, etc.)                                                                     |
| **Modelo de control**        | Simulador propio + drivers para conexión online; genera código nativo vía **post-procesadores**          | **Controlador Virtual (VC)** que replica el firmware del IRC5/OmniCore; corre **RAPID** real                |
| **Programación**             | Gráfica (targets, MoveJ/MoveL) + **API Python** (trayectorias paramétricas, automatización)              | Gráfica + edición completa de **RAPID**, *PickMaster*, *PowerPac* (Path, Arc, Cut, Machine Tending, etc.)   |
| **Generación de código**     | Sí (INFORM para Yaskawa, RAPID para ABB, KRL, TP, etc.)                                                  | Sí (RAPID nativo, carga directa al VC o al controlador)                                                     |
| **Calibración/Exactitud**    | Calibración de TCP/frames; exactitud depende del post y del robot                                        | Soporta **Absolute Accuracy**, *Tune Master*, parámetros de robot reales; alta fidelidad de trayectoria     |
| **Colisiones/Límites**       | Chequeo de colisiones, límites, singularidades; estimación de tiempo de ciclo                            | Chequeo de colisiones y límites con el **VC**; tiempos muy cercanos al robot real                           |
| **Integraciones de proceso** | Módulos para mecanizado, corte, pulido, seguimiento de curvas, visión; importación CAD                   | *PowerPacs* específicos ABB (Arc, Spot, Cut, Bin Picking, etc.); *Smart Components* y señales I/O virtuales |
| **Conexión online**          | Posible (drivers); envía comandos en tiempo real a varios controladores                                  | Posible (ABB PC SDK/Online); descarga y depuración directa de RAPID                                         |
| **Curva de aprendizaje**     | Baja–media (especialmente con la API Python)                                                             | Media–alta si se explota RAPID y los PowerPacs                                                              |
| **Casos de uso típicos**     | Laboratorios académicos multimarca, células mixtas, generación rápida de programas para distintos robots | Producción y *commissioning* **ABB**, validación fiel de lógicas RAPID/I/O, depuración previa a planta      |

RoboDK — Ventajas

Multimarca: una misma estación sirve para distintos fabricantes.

API Python: rápido para generar trayectorias paramétricas (espirales, rosas polares, patrones) y automatizar flujos.

Post-procesadores: exportas código nativo para muchos controladores (útil en entornos mixtos).

Sencillez: modelado y programación visual directos; buena para docencia e I+D.

RoboDK — Limitaciones

La fidelidad de ejecución depende del post y de los parámetros del robot real (no hay “firmware virtual” del fabricante).

Menos herramientas específicas de proceso que los PowerPacs nativos de ABB para ciertas aplicaciones.

RobotStudio — Ventajas

Controlador Virtual: ejecuta RAPID real con cinemática, aceleraciones y límites del robot ABB → fidelidad alta.

Ecosistema ABB: PowerPacs, Smart Components, I/O virtual, paths avanzados, PickMaster, etc.

Soporte a Absolute Accuracy y utilidades de ajuste/cálculo de tiempos muy cercanas a la celda real.

RobotStudio — Limitaciones

Monomarca (ABB): no sirve para programar robots de otros fabricantes.

Requiere manejar RAPID y la lógica ABB para sacarle todo el jugo (curva de aprendizaje mayor).

