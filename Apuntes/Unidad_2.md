📘 Unidad 2: Repaso de Diseño Estructurado
🎯 1. Objetivo Principal de la Fase
El propósito central de esta etapa es realizar la transición del Análisis (el ¿Qué? hará el sistema) al Diseño (el ¿Cómo? se implementará). Esta fase busca establecer la organización interna del software para producir sistemas que sean fáciles de entender, construir y mantener a lo largo de su vida útil, detallando los procesos funcionales y cómo los módulos se comunican entre sí.

--------------------------------------------------------------------------------
🔗 2. Trazabilidad (El Puente)
• 📥 Entradas (Inputs): Para comenzar el diseño, se asume que ya contamos con las salidas del Análisis Estructurado: Diagramas de Flujo de Datos (DFDs), el Diccionario de Datos, las Mini-Especificaciones de los procesos, el Modelo Lógico de Datos (Modelo E-R) y el catálogo de requisitos.
• 📤 Salidas (Outputs/Entregables): Al finalizar esta unidad obtendremos:
    ◦ El Diagrama de Estructura (arquitectura de módulos).
    ◦ La Especificación de Módulos (pseudo-código de cada uno).
    ◦ El Diseño Físico de Datos (el "paso a tablas" en SGBD relacionales como PostgreSQL/MariaDB).
    ◦ Las especificaciones del entorno de construcción y el Plan de Pruebas técnicas.

--------------------------------------------------------------------------------
👣 3. Flujo de Trabajo Teórico (Paso a Paso)
3.1. Identificación de la Estrategia de Diseño
El ingeniero debe revisar el DFD (Diagrama de Flujo de Datos) de los procesos primitivos (el nivel más bajo) y determinar qué tipo de flujo domina el sistema. Existen dos estrategias principales:
• Análisis de Transformación: Se usa cuando los datos entran, se transforman en el sistema y salen. Se debe identificar el flujo de llegada, el "centro de la transformación" y el flujo de salida.
• Análisis de Transacción: Se usa cuando el sistema evalúa una entrada y, basándose en ella, elige un camino de acción entre varios posibles (como un menú de opciones). Se debe identificar el "centro de la transacción" y los caminos de acción.
3.2. Descomposición y Creación del Diagrama de Estructura
Una vez elegida la estrategia, se procede a descomponer lógicamente el sistema en módulos jerárquicos:
• Separar Administración de Trabajo: Los módulos de nivel superior son "gerentes" (toman decisiones, coordinan, llaman a otros), y los módulos de nivel inferior son los "trabajadores" (hacen cálculos, lecturas, escrituras).
• Descomposición (Factoring): Se dividen los módulos grandes en submódulos más pequeños. Esto se hace hasta que el módulo resultante quepa en media página (unas 30 a 60 líneas), tenga una única función clara, y su interfaz sea sencilla de comprender.
3.3. Evaluación y Refinamiento (Criterios de Calidad)
Un diseño no está listo en su primer boceto; se debe evaluar con métricas de calidad:
• Acoplamiento (Reducirlo): Mide la dependencia entre módulos. Hay que buscar módulos independientes (Cajas Negras). Se logra eliminando envíos de datos innecesarios y debilitando la dependencia para que un módulo no necesite saber cómo está programado internamente el otro.
• Cohesión (Aumentarla): Mide qué tan relacionadas están las instrucciones dentro de un mismo módulo. Un módulo altamente cohesivo ejecuta una sola función bien definida y sus partes no tienen relación fuerte con partes de otros módulos.
• Fan-Out (Límite de Complejidad): Es la cantidad de submódulos que un módulo invoca directamente. Para evitar una complejidad excesiva, el "Fan-Out" no debe superar el límite de 7±2 módulos subordinados.
• Fan-In (Maximizar Reusabilidad): Es la cantidad de módulos "jefes" que invocan a un mismo submódulo. Un alto Fan-In es excelente porque indica que el código se reutiliza (ej. una función de validación que se llama desde 10 lugares distintos), pero requiere que dicho módulo tenga alta cohesión y una interfaz muy estricta.
3.4. Especificación y Documentación
Finalmente, se documenta la lógica de cada módulo usando interfaces-función, pseudocódigo (que le da libertad técnica al programador) y tablas de decisión, para que el equipo de construcción sepa exactamente qué codificar.

--------------------------------------------------------------------------------
🛠️ 4. Herramientas, Modelos y Patrones
• Diagrama de Estructura de Cuadros de Constantine (DEC): Es la herramienta principal. Representa la jerarquía en forma de árbol. Sus elementos son:
    ◦ Módulos: Rectángulos.
    ◦ Conexiones: Flechas que implican una invocación (CALL) de un módulo superior a uno inferior (devolviendo el control al terminar).
    ◦ Datos (Data): Flechas con un círculo vacío en la base. Representan información del dominio (como el nombre de un jugador) que se pasa para procesar.
    ◦ Controles (Flags): Flechas con un círculo lleno (negro) en la base. Comunican condiciones (como "Error", "Fin de archivo - EOF", "Campo válido"). Suelen ir de abajo hacia arriba.
• Tablas de Interfaz: Tablas que listan cada módulo, sus parámetros formales, e indican claramente si es de "Entrada", de "Salida", y si el dato será Procesado o Modificado.
• Pseudocódigo / Diagramas Nassi-Schneiderman / Árboles de decisión: Herramientas para escribir la lógica interna del módulo como paso previo a programarlo en Java/C#/PHP.

--------------------------------------------------------------------------------
💡 5. Aplicación Práctica (Llevándolo a mi código)
Caso de uso personal: "Centro de Transacción en App de Compras y Torneos"
Imagina que estás trabajando en tu Sitio Web de Compras (Laravel/Django).
• Teoría aplicada: El ruteador (urls.py o web.php) junto con tu Controlador principal actúan como el Centro de Transacción.
• Estructura DEC: El usuario pulsa un botón. El módulo "Administrador de Pedidos" (nivel superior) no hace cálculos; delega. Tiene un Fan-Out de 3: Llama a ValidarStock(), luego a CobrarTarjeta(), y finalmente a GenerarRecibo().
• Acoplamiento y Datos vs Flags: Cuando llamas a ValidarStock(), le pasas un Dato (círculo vacío) que es el ID_Carta. ValidarStock() va a la base de datos (PostgreSQL), revisa, y te devuelve un Flag/Control (círculo lleno) que dice True/False (Hay/No hay). Fíjate cómo mantenemos el Acoplamiento bajo: No le pasas el objeto "Usuario" entero a la función de stock si solo necesita el ID_Carta.
Imagina tu Gestor de Torneos en C#/Java (Desktop).
• Teoría aplicada: Quieres guardar los resultados de una ronda y mostrar un mensaje si hubo un error.
• Fan-In: En lugar de escribir el código de "Mostrar Alerta Visual" en cada botón, creas un módulo llamado MostrarMensajeError(string texto). Ahora, el botón "Registrar Jugador" y el botón "Guardar Resultado" llaman a este mismo módulo. Esto genera un Fan-In alto (mucha reusabilidad, menos código duplicado).
Prevención de Errores (Consejos para Junior)
1. Evitar el "Código Espagueti" o Módulos Kilométricos:
    ◦ Error Junior: Hacer una función procesar_todo() de 500 líneas que lee la base de datos, valida tarjetas, y manda un email.
    ◦ Solución Teórica: Aplica el Factoring (Descomposición). La teoría dice que un módulo debe medir "media página" (30-60 líneas). Separa esa gran función en "Trabajadores" pequeños y deja a procesar_todo() solo como el gerente que invoca a los demás.
2. Evitar el Acoplamiento Excesivo (Pasar "el mundo" por parámetro):
    ◦ Error Junior: En C# o Python, pasarle a una función auxiliar todo el Formulario (this) o todo el objeto Request de Django, solo para extraer un triste campo de texto.
    ◦ Solución Teórica: Minimizar el Acoplamiento. Envía estrictamente el dato primitivo que la función necesita (Ej. envíale solo el string correo, no todo el objeto).
3. Evitar Funciones que hacen "Múltiples Cosas" (Baja Cohesión):
    ◦ Error Junior: Crear un método validarJugador_Y_calcularPuntos().
    ◦ Solución Teórica: Aplicar la Cohesión. Un módulo debe ejecutar una función única y fuertemente genuina. Divídelo en dos módulos separados, lo que mejorará tu Fan-In futuro (reusabilidad).