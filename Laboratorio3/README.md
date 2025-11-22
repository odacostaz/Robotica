### Cuadro comparativo de características técnicas

| Característica                     | Motoman MH6 (Yaskawa)                                         | ABB IRB 140                                                   | EPSON T3-401S (SCARA)                                                                 |
|------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------------------------------|
| Tipo de robot                      | Articulado, 6 ejes                                             | Articulado, 6 ejes                                             | SCARA, 4 ejes                                                                          |
| Nº de grados de libertad (GDL)     | 6                                                              | 6                                                              | 4                                                                                      |
| Carga útil máxima                  | 6 kg :contentReference[oaicite:0]{index=0}                                       | 6 kg :contentReference[oaicite:1]{index=1}                          | 3 kg :contentReference[oaicite:2]{index=2}                                  |
| Alcance máximo (horizontal)        | ≈ 1422 mm :contentReference[oaicite:3]{index=3}                    | 810 mm :contentReference[oaicite:4]{index=4}                        | 400 mm :contentReference[oaicite:5]{index=5}                                |
| Carrera eje vertical (Z)           | Articulado; no se especifica como “stroke” en mm              | Articulado; no se especifica como “stroke” en mm              | 150 mm de carrera en Z :contentReference[oaicite:6]{index=6}                                |
| Repetibilidad (ISO 9283)           | ±0,08 mm :contentReference[oaicite:7]{index=7}                      | ±0,03 mm :contentReference[oaicite:8]{index=8}                      | ±0,02 mm :contentReference[oaicite:9]{index=9}                                |
| Peso del manipulador               | ≈ 130 kg :contentReference[oaicite:10]{index=10}                      | ≈ 98 kg :contentReference[oaicite:11]{index=11}                       | ≈ 16 kg :contentReference[oaicite:12]{index=12}                                             |
| Velocidad / desempeño típico       | Velocidades articulares hasta ~220–610 °/s según eje :contentReference[oaicite:13]{index=13} | Velocidad TCP máx. ≈ 2,5 m/s; ejes hasta 200–450 °/s :contentReference[oaicite:14]{index=14} | Tiempo de ciclo típico ≈ 0,52–0,54 s (trayecto 25–300–25 mm) :contentReference[oaicite:15]{index=15} |
| Tipo de montaje                    | Piso, pared o pedestal (según configuración) :contentReference[oaicite:16]{index=16} | Piso, pared, invertido, en casi cualquier ángulo :contentReference[oaicite:17]{index=17} | Montaje en mesa / celda de trabajo (sobremesa) :contentReference[oaicite:18]{index=18}    |
| Controlador habitual               | DX100 (u otros de la familia Yaskawa) :contentReference[oaicite:19]{index=19} | IRC5 Compact / Single Cabinet :contentReference[oaicite:20]{index=20} | Controlador integrado + software EPSON RC+ 7.0 :contentReference[oaicite:21]{index=21} |
| Clase de protección / ambiente     | Uso general, libre de polvo/agua excesivos (según serie) :contentReference[oaicite:22]{index=22} | IP67 en versiones Foundry, Clean Room y Wash :contentReference[oaicite:23]{index=23} | Uso en ambientes industriales limpios, con protección básica (serie T) :contentReference[oaicite:24]{index=24} |
| Aplicaciones típicas               | Manipulación de materiales, soldadura, ensamblaje, dispensado, manufactura aditiva :contentReference[oaicite:25]{index=25} | Soldadura por arco, ensamblaje, tendido de máquinas, manipulación, empaquetado, desbarbado :contentReference[oaicite:26]{index=26} | Pick & place, alimentación de piezas, posicionamiento, inspección, ensamblaje ligero :contentReference[oaicite:27]{index=27} |
| Nivel de carga / tipo de tarea     | Baja carga (6 kg), tareas de precisión con mayor alcance       | Baja carga (6 kg), tareas de precisión en espacio compacto     | Muy baja carga (3 kg), tareas rápidas y repetitivas en plano horizontal                 |


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



