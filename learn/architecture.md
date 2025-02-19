# Descripción y arquitectura

Codex está construyendo un motor de almacenamiento de datos duradero que es completamente descentralizado, proporcionando resistencia a la corrupción y censura para aplicaciones web3. Protege de forma innata a los participantes de la red dando a los hosts negación plausible sobre los datos que almacenan, y a los clientes garantías de durabilidad comprobables—hasta 99.99%—mientras mantiene la eficiencia en almacenamiento y ancho de banda.

Estas cuatro características clave se combinan para diferenciar a Codex de proyectos existentes en el nicho de almacenamiento descentralizado:

- **Codificación de borrado:** Proporciona redundancia de datos eficiente, lo que aumenta las garantías de durabilidad de datos.

- **Prueba de recuperabilidad basada en ZK:** Para garantías ligeras de durabilidad de datos.

- **Mecanismo de reparación perezosa:** Para reconstrucción eficiente de datos y prevención de pérdidas.

- **Incentivación:** Para fomentar el comportamiento racional, la participación generalizada en la red y la provisión eficiente de recursos finitos de la red.


### Descentralización incentivada

Los mecanismos de incentivación son una de las piezas clave que faltan en las redes tradicionales de compartición de archivos. Codex cree que una estructura de incentivos robusta basada en el mercado asegurará una amplia participación entre los tipos de nodos detallados a continuación.

El desarrollo de una estructura de incentivos adecuada está impulsado por los siguientes objetivos:

- Oferta y demanda para fomentar el uso óptimo de los recursos de la red.

- Aumentar la participación permitiendo que los nodos utilicen sus ventajas competitivas para maximizar beneficios.

- Prevenir el spam y desalentar la participación maliciosa.

Aunque aún está por finalizar, la estructura de incentivos de Codex involucrará un mercado de participantes que quieren almacenar datos, y aquellos que proveen almacenamiento publicando garantías, con estos últimos pujando por contratos de almacenamiento abiertos. Esta estructura busca asegurar que los incentivos de los participantes estén alineados, resultando en que Codex funcione según lo previsto.


### Arquitectura de la red

Codex está compuesto por múltiples tipos de nodos, cada uno tomando un rol diferente en la operación de la red. De manera similar, las demandas de hardware para cada tipo de nodo varían, permitiendo que aquellos que operan dispositivos con recursos restringidos participen.

**Nodos de almacenamiento**

Como proveedores de almacenamiento confiable a largo plazo de Codex, los nodos de almacenamiento depositan garantías basadas en las garantías publicadas en el lado de la solicitud de contratos, y el número de slots que tiene un contrato. Esto está vinculado a la durabilidad demandada por el usuario. El incumplimiento en proporcionar prueba periódica de posesión de datos resulta en penalizaciones de recorte.

**Nodo Agregador**

Un método para descargar la codificación de borrado, generación de pruebas y agregación de pruebas por un nodo cliente con bajos recursos, actualmente en desarrollo y será parte del siguiente lanzamiento de Codex Q2/Q4 del próximo año.

**Nodos cliente**

Los nodos cliente hacen solicitudes para que otros nodos almacenen, encuentren y recuperen datos. La mayoría de la red Codex serán nodos Cliente, y estos participantes pueden funcionar como nodos de caché para compensar el costo de los recursos de red que consumen.

Cuando un nodo se compromete con un contrato de almacenamiento y un usuario sube datos, la red verificará proactivamente que el nodo de almacenamiento esté en línea y que los datos sean recuperables. Los nodos de almacenamiento son entonces consultados aleatoriamente para transmitir pruebas de posesión de datos sobre un intervalo correspondiente a la duración del contrato y 9's de garantía de recuperabilidad que proporciona el protocolo.

Si el nodo de almacenamiento envía pruebas inválidas o falla en proporcionarlas a tiempo, la red expulsa al nodo de almacenamiento del slot, y el slot estará disponible para el primer nodo que genere una prueba válida para ese slot.

Cuando el contrato se vuelve a publicar, parte de la garantía del nodo defectuoso paga las tarifas de ancho de banda del nuevo nodo de almacenamiento. La codificación de borrado complementa el esquema de reparación permitiendo la reconstrucción de los fragmentos faltantes a partir de datos en otros slots dentro del mismo contrato de almacenamiento alojado por nodos sin fallas.


![arquitectura](/learn/architecture.png)

### Arquitectura del mercado ###

El mercado consiste en un contrato inteligente que se despliega en la cadena, y los módulos de compra y venta que son parte del software del nodo. El módulo de compra es responsable de publicar solicitudes de almacenamiento en el contrato inteligente. El módulo de ventas es su contraparte que los proveedores de almacenamiento usan para determinar en qué solicitudes de almacenamiento están interesados.

#### Contrato inteligente ####

El contrato inteligente facilita el emparejamiento entre proveedores de almacenamiento y clientes de almacenamiento. Un cliente de almacenamiento puede solicitar una cierta cantidad de almacenamiento por una cierta duración. Esta solicitud se publica entonces en la cadena, para que los proveedores de almacenamiento puedan verla y decidir si quieren llenar un slot en la solicitud.

Los principales parámetros de una solicitud de almacenamiento son:
- la cantidad de bytes de almacenamiento solicitados
- un identificador de contenido (CID) de los datos que deben almacenarse
- la duración durante la cual los datos deben almacenarse
- el número de slots (basado en los parámetros de codificación de borrado)
- una cantidad de tokens para pagar por el almacenamiento

A nivel de protocolo, un cliente de almacenamiento es libre de determinar estos parámetros como considere conveniente, para que pueda elegir un nivel de durabilidad que sea adecuado para los datos y ajustarse a los precios cambiantes del almacenamiento. Las aplicaciones construidas sobre Codex pueden proporcionar orientación a sus usuarios para elegir los parámetros correctos, análogo a cómo las carteras de Ethereum ayudan a determinar las tarifas de gas.

El contrato inteligente también verifica que los proveedores de almacenamiento mantengan sus promesas. Los proveedores de almacenamiento publican garantías cuando prometen llenar un slot de una solicitud de almacenamiento. Se espera que publiquen pruebas de almacenamiento periódicas al contrato, ya sea directamente o a través de un agregador. Si fallan en hacerlo repetidamente, entonces su garantía puede ser confiscada. Su slot se otorga entonces a otro proveedor de almacenamiento.

El contrato inteligente indica cuándo un determinado proveedor de almacenamiento debe proporcionar una prueba de almacenamiento. Esto no se hace en un intervalo de tiempo fijo, sino que se determina estocásticamente para asegurar que no sea posible que un proveedor de almacenamiento prediga cuándo debe proporcionar la siguiente prueba de almacenamiento.

#### Compras ####

El módulo de compra en el software del nodo interactúa con el contrato inteligente en nombre del operador del nodo. Publica solicitudes de almacenamiento y maneja cualquier otra interacción que se requiera durante la vida útil de la solicitud. Por ejemplo, cuando una solicitud se cancela porque no hay suficientes proveedores de almacenamiento interesados, entonces el módulo de compra puede retirar los tokens que estaban asociados con la solicitud.

#### Ventas ####

El módulo de ventas es la contraparte del módulo de ventas. Monitorea el contrato inteligente para ser notificado de las solicitudes de almacenamiento entrantes. Mantiene una lista de las solicitudes más prometedoras que puede cumplir. Favorecerá aquellas solicitudes que tengan una alta recompensa y baja garantía. Tan pronto como encuentra una solicitud adecuada, intentará primero reservar y luego llenar un slot descargando los datos asociados, creando una prueba de almacenamiento y publicándola en el contrato inteligente. Luego continuará monitoreando el contrato inteligente para proporcionarle pruebas de almacenamiento cuando sean requeridas.

El módulo de ventas contiene una estrategia de mejor esfuerzo para determinar en qué solicitudes de almacenamiento está interesado. Con el tiempo, esperamos que surjan estrategias más especializadas para atender las necesidades de, por ejemplo, grandes proveedores versus proveedores que ejecutan un nodo desde su casa.

### Libro Blanco ###

Lee el [libro blanco de Codex](/learn/whitepaper)
