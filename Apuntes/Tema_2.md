# Material de Estudio: Tema 2 - Diseño de Software e Interfaz de Usuario
---

## 1. El Concepto: Del Análisis al Diseño
El diseño de software es el proceso iterativo donde aplicamos técnicas y principios para traducir los requisitos del sistema (obtenidos en la etapa de análisis) en una representación detallada que permita su posterior construcción física.

Para lograr esto, el diseño aborda cuatro áreas principales:
*   **Diseño de datos:** Transforma el modelo de información (análisis) en estructuras de datos lógicas, como el "Modelo Relacional".
*   **Diseño arquitectónico:** Define la estructura modular general del programa o aplicación.
*   **Diseño de interfaz:** Establece cómo el software se comunicará con otros sistemas y diseña detalladamente las pantallas e impresiones para los usuarios. **Importante:** La interfaz de usuario debe corresponderse y ser coherente con la estructura modular.
*   **Diseño procedimental:** Describe detalladamente los algoritmos y procedimientos de cada componente de software.

> *Conector lógico para el examen:* "Sabiendo qué áreas abarca el diseño, pasamos a estructurar el sistema dividiéndolo de lo macro a lo micro."

## 2. Definición de la Arquitectura y Módulos
El diseño se construye en niveles descendentes:

1.  **Arquitectura del Sistema:** Se define la división global. Esto incluye particiones físicas, descomposición lógica en subsistemas, ubicación física de estos y la especificación de la infraestructura tecnológica.
2.  **Arquitectura de Módulos:** Para cada subsistema, se diseña la estructura interna de sus procesos utilizando herramientas como el *Diagrama de Clases de Diseño*.
3.  **Diseño Físico de Datos:** Se pasa del modelo lógico al modelo físico. Si se usa un motor de base de datos relacional (SGBDR), esto implica el "paso a tablas" y el análisis de los caminos de acceso para optimizar los tiempos de respuesta.

> *Conector lógico para el examen:* "Una vez que decidimos cómo dividir el sistema en módulos, necesitamos basarnos en principios y estrategias para que esa división sea correcta."

## 3. Principios y Estrategias de Diseño
El proceso de diseño se sustenta en principios fundamentales: **Abstracción, Modularidad, Encapsulamiento y Ocultamiento de información**.

Para derivar los módulos a partir de los requerimientos, existen dos estrategias principales:
*   **Análisis de transformaciones**.
*   **Análisis de transacciones**.

**La regla de oro de la jerarquía modular:** 
Al diseñar, se debe asegurar que los módulos de nivel superior actúen como "gerentes" (toman decisiones de ejecución y coordinan), mientras que los módulos de nivel inferior actúan como "trabajadores" (realizan las tareas pesadas de entrada, cálculo y salida). Esto hace que el sistema sea más fácil de implementar y mantener, separando la administración del trabajo real.

> *Conector lógico para el examen:* "Aunque dividamos el sistema siguiendo estos principios, debemos evaluar si la división es de calidad. Para ello utilizamos dos métricas críticas: el acoplamiento y la cohesión."

## 4. Evaluación de Calidad: Independencia Funcional
El objetivo es que los módulos sean lo más independientes posible y que cada uno ejecute una única función.

*   **Acoplamiento (Buscar el Mínimo):** Mide el grado de dependencia entre dos módulos. Un sistema bien particionado tiene un **bajo acoplamiento**. Se logra eliminando relaciones innecesarias, reduciendo la cantidad de conexiones y logrando que ningún módulo necesite conocer los detalles internos de implementación de otro (tratándolos como cajas negras).
*   **Cohesión (Buscar la Máxima):** Mide la intensidad con la que los elementos internos de un módulo (instrucciones, llamadas) están relacionados entre sí. El diseño estructurado busca módulos **altamente cohesivos**, donde todo su contenido esté fuerte y genuinamente enfocado en un solo propósito.

### La Descomposición (Factoring)
Si un módulo es demasiado grande, complejo o hace demasiadas cosas, aplicamos *Factoring* (separar una función en un módulo nuevo). Razones para hacerlo:
*   **Reducir tamaño:** Idealmente, un módulo debería ocupar entre media y una página (30 a 60 líneas).
*   **Reducir la complejidad de la interfaz:** Un módulo con una interfaz demasiado compleja es confuso.
*   **Claridad:** Para representar mejor las sub-funciones del sistema.
*   **Reutilización:** Minimizar la duplicación extrayendo funciones repetidas a un módulo separado.
*   **Separar coordinación de cálculo:** Para mantener la jerarquía de "módulos gerentes" y "módulos trabajadores".

> *Conector lógico para el examen:* "Finalmente, con un diseño modular de alta calidad, solo queda verificar, planificar las pruebas y dejar todo documentado para los programadores."

## 5. Verificación, Pruebas y Documentación Final
Antes de programar, el diseño debe ser aceptado:
*   **Verificación:** Garantiza la viabilidad, la calidad técnica y la coherencia de todos los modelos del diseño.
*   **Plan de Pruebas:** A partir del diseño se especifican los casos y el entorno para las pruebas unitarias, de integración, de implantación y de aceptación.
*   **Especificaciones de Construcción:** Se genera el pseudocódigo, la definición de herramientas (compiladores) y el código DDL para la base de datos.
*   **Documentación:** Es absolutamente vital. Permite detectar errores, sirve como especificación para la codificación y asegura que personas ajenas al diseño original puedan entenderlo y perfeccionarlo en el futuro. Se deben documentar procesos, almacenes de datos, flujos y terminadores.

---

### Resumen Breve del Temario (Tema 2)
El **Diseño de Software** es la fase puente entre los requerimientos (qué se necesita) y la construcción física (cómo se programará). El proceso arranca definiendo la arquitectura general y distribuyendo responsabilidades lógicas y físicas. Luego, a nivel micro, se diseña la **estructura de módulos y de datos**, asegurando que la interfaz de usuario refleje esta organización. 

Para que este diseño no sea un caos, nos basamos en principios como la modularidad y el ocultamiento de información, aplicando un esquema jerárquico donde los módulos superiores coordinan y los inferiores ejecutan. La calidad de esta estructura se mide buscando siempre **alta cohesión** (cada módulo hace una sola cosa bien) y **bajo acoplamiento** (los módulos son independientes entre sí). Si un módulo falla en esto, se aplica la técnica de **descomposición (factoring)** para refinarlo. El ciclo cierra cuando este modelo se verifica, se documenta de forma exhaustiva y se generan las especificaciones (como seudocódigo y planes de prueba) listas para ser entregadas a los desarrolladores.