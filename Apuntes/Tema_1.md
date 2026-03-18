# Material de Estudio: Tema 1 - Arquitectura de Software

---

## 1. El Propósito de la Arquitectura
Para empezar, debes saber **por qué importa la arquitectura**. La arquitectura de un sistema es la estructura base que afectará directamente cómo se comporta el software en la realidad. 

El estilo elegido dependerá de los **requerimientos no funcionales críticos** que el cliente priorice:
*   **Rendimiento:** Requiere pocas comunicaciones entre pocos subsistemas.
*   **Seguridad / Protección:** Requiere una arquitectura en capas (protegiendo lo más interno) o aislar las operaciones críticas en un solo subsistema.
*   **Disponibilidad:** Exige componentes redundantes que puedan actualizarse sin detener el sistema.
*   **Mantenibilidad:** Requiere componentes independientes y pequeños (grano fino) fáciles de modificar.

## 2. El Proceso de Diseño y sus Modelos
Una vez entendidos los requerimientos, el arquitecto debe tomar **decisiones fundamentales** (respondiendo preguntas como: ¿cómo se distribuirá el sistema?, ¿qué estilos se usarán?, ¿cómo se estructurará y descompondrá?).

El resultado de estas decisiones se documenta usando representaciones gráficas. Aunque los **diagramas de bloques** son útiles para planificar y hablar con los clientes (stakeholders) porque son sencillos, no sirven como representación arquitectónica técnica profunda porque omiten los detalles de las relaciones. 
Por ello, se utilizan **cinco modelos arquitectónicos más específicos**:
1.  **Estructural estático:** Muestra los componentes separados.
2.  **Proceso dinámico:** Muestra la organización en tiempo de ejecución.
3.  **De interfaz:** Define los servicios públicos de cada subsistema.
4.  **De relaciones:** Muestra flujos de datos.
5.  **De distribución:** Muestra cómo se reparten en las computadoras físicas.

> *Conector lógico para el examen:* "Sabiendo qué debemos diseñar y cómo documentarlo, el siguiente paso es decidir **la estrategia general de organización del sistema**."

## 3. Organización del Sistema (El nivel macro)
Las arquitecturas de sistemas grandes rara vez usan un solo estilo puro, pero se basan en tres estilos organizacionales principales para estructurar el sistema:

1.  **Modelo de Repositorio:** Todos los datos compartidos están en una base de datos central.
    *   *Ventaja:* Comparte grandes volúmenes de datos eficientemente sin que los subsistemas necesiten conocerse entre sí.
    *   *Desventaja:* Es rígido. Si un subsistema nuevo no se ajusta al esquema de datos, es muy difícil de integrar o evolucionar.
2.  **Modelo Cliente-Servidor:** Es una arquitectura distribuida donde servidores ofrecen servicios a clientes a través de una red (ej. un catálogo de películas web).
    *   *Ventaja principal:* Es fácil añadir o actualizar servidores de forma transparente, aprovechando sistemas en red.
3.  **Modelo de Capas (Máquina Abstracta):** Se organiza el sistema en capas funcionales secuenciales, como el modelo de red OSI. Cada capa da servicios a la superior. Su desventaja es que estructurarlo adecuadamente puede ser muy complejo.

> *Conector lógico para el examen:* "Una vez que elegimos la organización global (ej. Cliente-Servidor), debemos tomar cada bloque o subsistema y **descomponerlo internamente** en módulos más pequeños."

## 4. Estilos de Descomposición Modular (El nivel micro)
Es importante diferenciar: un **subsistema** funciona de forma independiente, mientras que un **módulo** provee servicios a otros módulos y no suele considerarse un sistema por sí solo.
Para dividir un subsistema en módulos, se usan dos grandes estrategias:
*   **Orientada a Objetos:** El sistema se descompone en objetos que se comunican, cada uno con su estado privado y operaciones propias.
*   **Orientada a flujos de funciones:** El sistema se descompone en módulos que actúan como transformaciones funcionales (toman datos de entrada y generan salidas).

> *Conector lógico para el examen:* "Ya tenemos el sistema organizado y dividido en módulos. Ahora necesitamos establecer **quién da las órdenes**, es decir, cómo fluye el control entre ellos."

## 5. Estilos de Control
Define cómo se inicia y detiene a los distintos subsistemas.
1.  **Control Centralizado:** Un componente tiene toda la responsabilidad. Puede ser de dos tipos:
    *   *Llamada-retorno:* Subrutinas típicas, solo aplicable a sistemas secuenciales.
    *   *Modelo del gestor:* Un componente coordina los procesos, aplicable a sistemas concurrentes o paralelos.
2.  **Control Dirigido por Eventos:** No hay un jefe central; las decisiones se toman según eventos externos. Puede ser mediante:
    *   *Transmisión (Broadcast):* Un evento se transmite a todos los subsistemas y responde el que esté programado para ello.
    *   *Interrupciones:* Exclusivo de sistemas de tiempo real, donde las interrupciones externas requieren respuesta inmediata.

> *Conector lógico para el examen:* "En la vida real, los arquitectos no inventan esto desde cero cada vez. Utilizan plantillas de dominios específicos y configuran la **arquitectura según el tipo de aplicación comercial** que están construyendo."

## 6. Arquitecturas de Aplicaciones y de Referencia
Existen **modelos de referencia** (arquitecturas ideales abstractas) y **modelos genéricos** (abstracciones de sistemas reales). Basados en ellos, los sistemas se agrupan en cuatro grandes familias:

1.  **Procesamiento de Datos (Por Lotes):** No requieren intervención del usuario. Se operan lotes de datos secuencialmente usando el clásico modelo *Entrada-Proceso-Salida* (ej. nóminas o facturación). Son orientados a funciones, ya que no mantienen estado entre transacciones.
2.  **Procesamiento de Transacciones:** Sistemas de negocio interactivos y centrados en una base de datos. Aseguran que múltiples usuarios no interfieran entre sí mediante "unidades atómicas" (todas las operaciones se completan juntas o se rechazan). Su estructura es E/S -> Lógica -> Gestor de Transacciones -> Base de datos.
3.  **Procesamiento de Eventos:** Suelen ser gobernados por la interfaz de usuario o entornos impredecibles (sistemas en tiempo real o editores de texto). Responden rápidamente a estímulos externos (ej. ratón o teclado) comunicando la pantalla con objetos de eventos, órdenes y datos almacenados mayormente en memoria volátil para brindar respuestas rápidas.
4.  **Procesamiento de Lenguajes:** Toman un lenguaje natural o artificial (ej. Java, XML) y lo traducen o ejecutan. El mejor ejemplo son los compiladores, que usan análisis léxico, sintáctico y semántico para finalmente generar un código ejecutable.

### Aclaración sobre Sistemas Distribuidos (Variantes Cliente-Servidor)
Dado que los sistemas actuales rara vez corren en un solo equipo, el modelo Cliente-Servidor evoluciona en diversas formas: de múltiples servidores (ej. la web), servidores Proxy (con caché de recursos), sistemas de múltiples niveles (ej. buscadores de internet organizados en nivel de interfaz, proceso y datos), o incluso modelos de Igual a Igual (P2P) para alta escalabilidad. Pueden funcionar bajo interacción asíncrona (como internet, sin límites de velocidad estrictos) o síncrona (con tiempos muy estrictos y limitados).

---

### Resumen Final (Para englobar el temario)
**La Arquitectura del Software** es la fase de toma de decisiones estratégicas donde definimos cómo cumplir con los atributos de calidad (rendimiento, seguridad, etc.) del sistema a construir. Para lograrlo, el arquitecto atraviesa un proceso mental: primero define la **organización general** (usando modelos como Repositorio, Cliente-Servidor o Capas); luego **descompone** esos macro-bloques en módulos pequeños (vía objetos o funciones); y finalmente establece las **reglas de control** y comunicación (centralizadas o por eventos). En la práctica industrial, no partimos de cero, sino que adaptamos **arquitecturas de aplicaciones genéricas** preexistentes (ya sean para procesar lotes de datos, transacciones interactivas, eventos en tiempo real o interpretación de lenguajes), resultando en sistemas modernos que, en su inmensa mayoría, acaban siendo sistemas distribuidos e híbridos.

---
### Conceptos Adicionales a tener en cuenta: Funcional vs. No Funcional

*   **Requerimiento Funcional:** Es **qué hace** la aplicación.
*   **Requerimiento No Funcional:** Es **cómo de bien** lo hace. La elección del estilo y estructura de la arquitectura del software dependerá directamente de estos requerimientos no funcionales. Durante el diseño, los arquitectos evalúan aspectos críticos como el rendimiento, la protección, la seguridad, la disponibilidad y la mantenibilidad.

**Ejemplo práctico: Transferencias bancarias**
*   **El requerimiento funcional:** Es que la aplicación debe mover dinero de la cuenta A a la cuenta B.
*   **El requerimiento no funcional:** Dicta que esa transferencia debe estar encriptada (asociado a los requerimientos de *protección y seguridad*), debe procesarse en menos de 100 milisegundos (asociado a los requerimientos de *rendimiento*), que tiene que soportar a un millón de usuarios simultáneos, y que el sistema debe tener un tiempo de inactividad de 0 segundos al año (asociado a los requerimientos de *disponibilidad*).