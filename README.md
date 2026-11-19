## Trabajo juego organizacion de computadores.

### Profesor: Jose Luis Montoya.

### Integrantes:
- Samuel Quintero Quintero.
- Ashly Sofia Robayo Parra.

---
## Descripción
Videojuego hecho en lenguaje `Jack` con el objetivo de comprender el funcionamiento de la maquina virtual y el lenguaje mencionado como finalización de los contenidos del curso de `nandtotetris.org`. Es un juego de carreras de tres carriles que procesa el teclado y dibuja en la memoria de video, con una rutina gráfica escrita a mano en lenguaje VM.

## ¿Qué hace el juego?

* **Menú y selección de distancia:** Al iniciar, muestra las reglas y controles en pantalla y permite elegir entre tres longitudes de recorrido (`1`: 50 pasos, `2`: 100 pasos, `3`: 200 pasos) o salir (`ESC`).
* **Mecánica de carrera en 3 carriles:**
  * **Vehículo del jugador:** Se desplaza verticalmente entre los tres carriles (`Flecha Arriba` / `Flecha Abajo`) manteniendo una columna fija en pantalla.
  * **Obstáculos y recolectables:** Genera dinámicamente elementos con distintos comportamientos:
    * **Cono (Tipo 0):** Obstáculo letal de 1 celda de ancho y velocidad estándar.
    * **Camión (Tipo 1):** Obstáculo letal largo que ocupa 3 celdas horizontales consecutivas.
    * **Moto (Tipo 2):** Obstáculo letal rápido que avanza 2 columnas por ciclo.
    * **Moneda (Tipo 3):** Elemento no letal que suma **+10 puntos** al ser recogido.
* **Control de partida:** Soporta pausa en tiempo real (`P`), reinicio inmediato de la carrera (`R`) y salida del programa (`ESC`), además de detectar condiciones de victoria (llegar a la meta) o derrota (colisión con un obstáculo letal).

---

## Arquitectura del Proyecto

El sistema sigue el patrón Modelo-Vista-Controlador (MVC) e implementa una **arquitectura híbrida** que combina código de alto nivel en Jack con rutinas gráficas escritas directamente en lenguaje de máquina virtual (VM):

* `Main.jack`: Punto de entrada que instancia, ejecuta y libera el controlador principal.
* `Juego.jack`: Controlador del ciclo de vida del programa, lectura de teclado sin repetición accidental, menú, pausa y bucle principal.
* `Carrera.jack`: Lógica del estado del juego, generación pseudoaleatoria de entidades, avance de pasos y detección de colisiones.
* `Obstaculo.jack`: Modelo de cada entidad en pista (posición, longitud, velocidad, letalidad y puntaje).
* `Sprites.jack`: Definición en mapas de bits de `16 × 16` píxeles para el auto, cono, camión, moto, moneda y celda vacía.
* `Vista.jack`: Gestión de la interfaz visual, dibujo de la pista, marcadores y actualización de celdas.
* `Grafico.vm`: Módulo escrito manualmente en lenguaje VM encargado de renderizar cada sprite de `16 × 16` píxeles directamente en la memoria de pantalla de Hack (`RAM[16384..24575]`), sin depender de `Grafico.jack` ni de `Memory.poke`/`Math.multiply`.

---

## Requisitos y Ejecución

Debido al enlace directo entre clases compiladas desde `.jack` y el módulo manual `Grafico.vm`, el proyecto debe ejecutarse utilizando la **suite oficial de escritorio de Nand2Tetris**:

1. Compilar los archivos `.jack` desde la carpeta del proyecto:
   ```bash
   JackCompiler.bat .
   ```
2. Abrir el VMemulator.bat parte del software descargable de Nand2tetris y subir la carpeta con los .vm

---
## Limitaciones Conocidas
- Incompatibilidad con el Web IDE de Nand2Tetris: El compilador en línea exige que toda clase referenciada exista como archivo .jack y presenta restricciones al enlazar archivos .vm externos escritos a mano, por lo que requiere ejecutarse en el JackCompiler y VMEmulator locales.

- Resolución basada en grilla discreta: El movimiento vertical y horizontal ocurre por celdas de 16 × 16 píxeles (grilla de 16 × 32 en la pantalla de 256 × 512 píxeles de Hack), sin desplazamientos sub-píxel.

- Temporización dependiente del hardware/emulador: La velocidad de la partida está regulada mediante Sys.wait(100) y el refresco de toda la grilla en cada ciclo, lo cual puede variar ligeramente según la velocidad de ejecución configurada en el emulador.

- Generación pseudoaleatoria simple: La semilla se incrementa según la interacción inicial en el menú y utiliza aritmética entera básica en Jack, por lo que los patrones pueden repetirse si se inicia inmediatamente con la misma temporización.

---

## Declaración de Uso de Inteligencia Artificial
En cumplimiento con las buenas prácticas académicas y de transparencia en el desarrollo de software, se declara el uso de Gemini (Google) como herramienta de apoyo durante la construcción y depuración de este proyecto. Su intervención se limitó a las siguientes tareas de asistencia técnica:

- Depuración y compatibilidad con el compilador Jack: Identificación y corrección de errores de sintaxis propios de la gramática estricta de Jack (como llamadas a métodos sobre elementos de arreglos, agrupamiento explícito de precedencia de operadores y prevención de división por cero).

- Diagnóstico del entorno de ejecución: Orientación en el uso de las herramientas de escritorio (JackCompiler.bat y VMEmulator.bat) para soportar la integración entre clases compiladas y el módulo Grafico.vm escrito a mano.

- Gestión de control de versiones (Git/GitHub).

Y de Claude(Sonnet 5.5) para preguntas de escritura en lenguaje Jack

