> La práctica de la ingeniería de software tiene un solo objetivo general: entregar a tiempo software operativo de alta calidad que contenga funciones y características que satisfagan las necesidades de todos los participantes. Para lograrlo, debe adoptarse un conjunto de principios fundamentales que guíen el trabajo técnico. Estos principios tienen mérito sin que importen los métodos de análisis y diseño que se apliquen, ni las técnicas de construcción (por ejemplo, el lenguaje de programación o las herramientas automatizadas) que se usen o el enfoque de verificación y validación que se elija.
> 
> *Esta unidad es crítica porque es donde dejamos de pensar solo en "qué hace el software" para definir "cómo va a estar construido globalmente".*

# 📘 Unidad 1: Arquitectura y Diseño

## 🎯 1. Objetivo Principal de la Fase
El propósito central de esta unidad es establecer la estructura global del sistema antes de escribir código detallado. Esta etapa resuelve el problema de cómo satisfacer los requerimientos no funcionales del sistema, tales como el rendimiento, la seguridad (protección), la disponibilidad y la mantenibilidad. Su objetivo es tomar decisiones fundamentales sobre la organización del sistema, identificando los subsistemas principales y cómo se comunican entre sí, ya que estas decisiones afectarán la calidad del software durante toda su vida útil.

---

## 🔗 2. Trazabilidad (El Puente)

- **📥 Entradas (Inputs):**
  - **Requerimientos no funcionales:** Información crítica sobre necesidades de rendimiento, seguridad y disponibilidad que dictan el estilo arquitectónico a elegir (OnLine u OffLine).
  - **Conocimiento y experiencia del arquitecto:** Se asume que se cuenta con el "know-how" para responder preguntas sobre distribución de procesadores y estilos arquitectónicos.
  - **Conceptos del dominio:** Comprensión de arquitecturas genéricas o similares en el mismo dominio de aplicación (por ejemplo, saber cómo funciona habitualmente un sistema de compras o el sistema que se va a desarrollar).

- **📤 Salidas (Outputs/Entregables):**
  - **Documento de Diseño Arquitectónico:** Es el entregable principal. Incluye texto descriptivo y representaciones gráficas del sistema.
  - **Modelos Arquitectónicos Específicos:**
    - Modelo estructural estático (subsistemas).
    - Modelo de proceso dinámico (tiempo de ejecución).
    - Modelo de interfaz y distribución (cómo se reparten en computadoras).

---

## 👣 3. Flujo de Trabajo Teórico (Paso a Paso)

### 3.1. Requerimientos
1. **El trabajo de análisis debe avanzar de la información esencial hacia la implementación en detalle**
   Comenzar describiendo la "esencia" del problema desde la perspectiva del usuario final, sin considerar todavía de qué forma técnica se implementará la solución. La implementación detallada debe posponerse para la fase de diseño.
   - *Ejemplo fácil:* En la etapa de requerimientos de tu Sitio Web, la esencia es simplemente: "El usuario debe poder filtrar las cartas por color o rareza". En este punto no debes preocuparte por detalles de implementación técnica como: "¿Usaré un query `SELECT * FROM cartas WHERE color =...` o usaré un framework de Laravel con ORM Eloquent?". Primero defines el qué necesita el sistema (la esencia) y luego, en el diseño, decides el cómo (la tecnología).

2. **Debe representarse y entenderse el dominio de información de un problema**
   Debemos comprender y mapear claramente qué datos fluyen hacia el sistema (entradas de usuarios o dispositivos externos), qué datos fluyen hacia afuera (salidas como reportes, gráficos o interfaces) y qué datos se conservan de forma permanente (almacenamientos de datos).
   - *Ejemplo fácil:* En tu Sitio Web de Compras, el dominio de información consiste en:
     - **Entradas:** El cliente ingresa sus credenciales de login y los datos de su tarjeta de crédito.
     - **Almacenamiento:** La base de datos en PostgreSQL que guarda el stock actualizado de los productos.
     - **Salidas:** El recibo de compra en PDF o el catálogo de productos que se muestra en la pantalla del navegador.

3. **Deben definirse las funciones que realizará el software**
   Son las que otorgan un beneficio directo al usuario final. Estas funciones se encargan de transformar los datos que entran al sistema.
   - *Ejemplo fácil:* En tu Gestor de Torneos de Cartas, una función esencial es "Generar emparejamientos". Esta función toma la lista de jugadores (datos de entrada), aplica un algoritmo con las reglas del torneo (procesamiento interno, emparejar únicamente de a 2 o de a 4 jugadores) y devuelve a la interfaz qué jugador se enfrenta a quién en la mesa (salida transformada).

4. **Debe representarse el comportamiento del software (como consecuencia de eventos externos)**
   Entradas de usuarios, control de datos por otros sistemas externos o la red, actúan como "eventos" que provocan que el software reaccione y se comporte de una forma específica.
   - *Ejemplo fácil:* En tu Sistema de Listas de Compras, ¿qué pasa si un usuario hace clic en el botón "Borrar Lista" (evento externo)? El comportamiento del software debe reaccionar mostrando una alerta de confirmación (estado de advertencia) antes de eliminar los datos de la base de datos para evitar accidentes.

5. **Los modelos que representen información, función y comportamiento deben dividirse de manera que revelen los detalles en forma estratificada (o jerárquica)**
   Aplicar la estrategia de "divide y vencerás", consiste en dividir un problema grande y complejo en subproblemas hasta que cada uno sea fácil de entender.
   - *Ejemplo fácil:* Si estás modelando la arquitectura de tu Gestor de Torneos, no modeles "El Torneo" como un solo bloque gigante. Divídelo jerárquicamente: un gran bloque es la "Gestión de Jugadores", otro la "Gestión de Rondas" y otro el "Ranking". Luego, toma la "Gestión de Rondas" y subdivídela en tareas más pequeñas como "Asignar mesas", "Cargar resultados" y "Calcular puntajes".

El ingeniero debe seguir una secuencia de decisiones críticas para estructurar el sistema:

### 3.2. Toma de Decisiones Arquitectónicas
El primer paso es cuestionarse la estructura fundamental. Debes decidir cómo distribuir el sistema y qué estilo usar.
- **Rendimiento:** Debes diseñar subsistemas con pocas comunicaciones entre sí (minimizar el tráfico en la red o entre procesos). Cuantas más veces se pasen mensajes entre sí los módulos, más lento será el sistema en general. Para enfocarse en el rendimiento, debes agrupar las funciones de manera inteligente para que cada subsistema sea lo más independiente posible y solo cruce información con los demás cuando sea estrictamente indispensable.
- **Seguridad:** Se debe optar una arquitectura en capas, protegiendo los recursos más valiosos en las capas internas. Garantizar que la información esté protegida y que solo las personas autorizadas puedan verla o modificarla. (Ejemplo: garantiza que el "Usuario A" jamás pueda ver, editar o borrar la lista del "Usuario B").
- **Disponibilidad:** La garantía de que el sistema estará encendido, accesible y listo para usarse en el momento exacto en que el usuario lo necesite.
- **Mantenibilidad:** Se deben usar componentes independientes de grano fino.



### 3.3. Selección del Modelo de Organización del Sistema
Debes elegir una estrategia básica para estructurar el sistema.

1. **Modelo de Repositorio:**
   - **Concepto:** Todos los subsistemas comparten una base de datos central.
   - **Ventaja:** Eficiente para compartir grandes cantidades de datos sin transmitirlos explícitamente.
   - **Desventaja:** Si cambia el modelo de datos, afecta a todos los subsistemas. Es difícil integrar sistemas con esquemas diferentes.

2. **Modelo Cliente-Servidor:**
   - **Concepto:** Un conjunto de servidores ofrece servicios y los clientes acceden a ellos a través de una red.
   - **Ventaja:** Arquitectura distribuida real. Es fácil añadir nuevos servidores o actualizarlos sin afectar al resto.
   - **Contexto:** Ideal para sistemas multiusuario web, como bibliotecas de medios.

3. **Modelo por Capas (Máquina Abstracta):**
   - **Concepto:** Organiza el sistema en capas donde cada una ofrece servicios a la superior (ej. Modelo OSI).
   - **Ventaja:** Permite cambiar una capa (ej. interfaz) sin afectar a las otras (ej. base de datos).
   - **Desventaja:** Puede ser difícil estructurar un sistema puramente en capas y a veces afecta el rendimiento.

### 3.4. Estilos de Descomposición Modular
Una vez definidos los subsistemas, se descomponen en módulos:
- **Orientada a Objetos:** Módulos como objetos con estado privado y operaciones. Ideal para sistemas interactivos donde se mantiene estado.
- **Flujo de Funciones (Pipeline):** Módulos como transformaciones funcionales (Entrada -> Proceso -> Salida). Ideal para procesamiento de datos por lotes donde no se guarda estado entre operaciones.

### 3.5. Definición del Estilo de Control
¿Cómo se gobierna la ejecución?
- **Control Centralizado:**
  - *Lllamada-retorno:* La clásica subrutina secuencial (top-down).
  - *Modelo del gestor:* Para sistemas concurrentes, un componente coordina a los demás.
- **Control Basado en Eventos:**
  - *Broadcast:* Un evento se transmite y quien sepa manejarlo responde.
  - *Interrupciones:* Usado en sistemas de tiempo real para respuestas inmediatas externas.

---

## 🛠️ 4. Herramientas, Modelos y Patrones
Según los documentos de la unidad, estas son las herramientas conceptuales a utilizar:
- **Diagramas de Bloques (Cajas y Líneas):** Aunque simples y a veces criticados por falta de detalle técnico, son efectivos para comunicarse con los stakeholders (interesados) y planificar al inicio.
- **Modelos de Referencia (Genéricos):** Usar plantillas preexistentes del dominio (ej. modelo de compilador o modelo OSI) para no reinventar la rueda.
- **Patrones de Arquitectura de Aplicaciones:**
  - *Procesamiento de Transacciones:* Estructura: E/S -> Lógica -> Gestor de Transacciones -> BD. Garantiza atomicidad (ACID).
  - *Procesamiento de Eventos:* Estructura de bucle de eventos (Pantalla -> Evento -> Comando -> Editor de Datos). Clave para editores y GUIs.
  - *Procesamiento de Lenguajes:* Traductores y compiladores (Léxico -> Sintáctico -> Semántico -> Generador de Código).
- **Arquitecturas Distribuidas:** Cliente-Servidor (2 niveles, 3 niveles, multi-niveles), Peer-to-Peer (P2P) y Proxy referencia visual en slides finales.

---



## 💡 5. Aplicación Práctica (Llevándolo a mi código)
Dado que usas Django/Laravel (Web) y Java/C# (Escritorio), aquí es donde la teoría se vuelve código:

**Caso de uso personal: "Sitio Web de Compras" vs. "Gestor de Torneos (Desktop)"**

1. **Para tu Sitio Web de Compras (Django/Laravel/PostgreSQL):**
   - **Arquitectura:** Debes aplicar la Arquitectura de Procesamiento de Transacciones (Cliente/Servidor).
   - **Decisión de Ingeniería:** Tu sistema debe garantizar que si un usuario compra una carta, se descuente del stock y se cobre el dinero como una unidad atómica.
   - **En tu código:** Esto se refleja usando el modelo Cliente-Servidor. El navegador (Cliente) hace peticiones HTTP al servidor (Django). Django actúa como el "Gestor de Transacciones" hablando con PostgreSQL (Repositorio).
   - **Estructura:** Usarías una arquitectura en capas (MVC/MVT):
     - *Capa Datos:* Modelos de Django (ORM).
     - *Capa Lógica:* Vistas/Controladores (donde validas la compra).
     - *Capa Presentación:* Templates HTML/JSON.

2. **Para tu Gestor de Torneos de Cartas (C#/Java GUI):**
   - **Arquitectura:** Debes aplicar Sistemas de Procesamiento de Eventos.
   - **Decisión de Ingeniería:** A diferencia de la web, aquí el flujo no es lineal. El usuario puede hacer clic en "Registrar Jugador", "Iniciar Ronda" o "Ver Ranking" en cualquier orden.
   - **En tu código:** Implementarás un bucle de eventos (el MainLoop de Windows Forms o Swing).
   - **Patrón:**
     - *Objeto Pantalla:* Detecta el clic del mouse.
     - *Objeto Evento:* Interpreta que el clic fue en el botón "Iniciar Ronda".
     - *Objeto Comando:* Ejecuta la lógica del torneo (Command Pattern).
     - *Visualización:* Refresca la lista de emparejamientos en pantalla sin recargar todo el programa.

### 🚨 Prevención de Errores (Consejos para Junior)
Basado en la teoría de la Unidad 1, evitarás estos errores comunes:

1. **El error del "Monolito Rígido":**
   - **Error:** Conectar tu lógica de negocio de torneos directamente a la interfaz gráfica (ej. poner código SQL dentro del evento del botón).
   - **Solución Teórica:** Aplica la Independencia de Componentes y Capas. Si mañana quieres pasar tu torneo de Escritorio a Web, si separaste la lógica de la "Pantalla", podrás reutilizar todo el código del torneo.
2. **El error de la "Base de Datos Cuello de Botella":**
   - **Error:** Diseñar tu web de compras asumiendo que la base de datos lo aguanta todo, creando consultas complejas entre subsistemas que no deberían conocerse.
   - **Solución Teórica:** Entender el Modelo de Repositorio. Si ves que el rendimiento cae, la teoría sugiere identificar operaciones críticas y aislarlas, o moverte a un modelo distribuido donde cada subsistema tenga sus datos si es necesario.
3. **El error de "Reinventar la Rueda":**
   - **Error:** Intentar crear tu propia forma de procesar pedidos desde cero sin estructura.
   - **Solución Teórica:** Usa Modelos de Referencia. Para compras, usa el modelo estándar de Procesamiento de Transacciones (Entrada -> Lógica -> Gestor -> BD). No inventes flujos extraños; usa el estándar que garantiza la integridad de los datos.