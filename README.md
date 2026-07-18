# Wraithbane - Showcase Tecnico

🎮 Videojuego inmersivo de plataformas, accion y combate contra jefes en 2D con tematica de fantasia oscura, desarrollado integramente desde cero sin motores comerciales (como Unity o Unreal) ni plantillas prefabricadas. El proyecto demuestra como estructurar una arquitectura desacoplada uniendo un motor grafico de alto rendimiento en el navegador con integraciones en tiempo real para transmisiones en vivo.

🌐 **Prueba de la beta privada:** https://wraithbane.pro/  
👤 **Sobre el autor:** https://wraithbane.pro/autor

---

## 🏗️ Topologia de la Arquitectura

El ecosistema de Wraithbane esta compuesto por tres sub-sistemas independientes que se comunican mediante protocolos web estandares, asegurando aislamiento de responsabilidades y portabilidad:

### 🖥️ Cliente del Juego (Frontend)
Motor grafico y logica de juego que se ejecuta en el navegador del usuario.
*   **Funcion:** Renderizado de graficos, fisicas de colision personalizadas, IA de enemigos, control de estados de combate y HUD.
*   **Pipeline:** Compilado como una aplicacion de pagina unica (SPA) estatica ultra liviana mediante Vite.
*   **Conectividad:** Cliente Socket.IO que se suscribe a los eventos del bridge de transmision en vivo y APIs REST de telemetria.

### 🔌 Puente de Transmision (Bridge)
Servidor de integracion bidireccional en tiempo real.
*   **Funcion:** Conectarse al feed de eventos de transmisiones en directo de TikTok mediante web scraping y conexion de sockets inversa. Captura comentarios, me gustas, nuevos seguidores y regalos enviados por la audiencia.
*   **Normalizacion:** Traduce los payloads variables de los servicios externos a un esquema de eventos unificado para el motor del juego.
*   **Distribucion:** Sostiene salas de Socket.IO que difunden los eventos filtrados de manera inmediata a las instancias activas del cliente de juego.

### 🛡️ Servidor de Control y Telemetria (Auth y Telemetria)
Backend administrativo, portal de acceso y pasarela de QA.
*   **Funcion:** Control de acceso de patrocinadores mediante OAuth2 conectado a la API de Patreon (Patreon Gated), almacenamiento de partidas salvadas, tabla de clasificacion global e inyeccion de scripts de QA para diagnostico de rendimiento en tiempo real.
*   **Persistencia:** Gestion de perfiles de usuario, sincronizacion de progreso y analisis a traves de Supabase.

---

## 🛠️ Stack Tecnico Detallado

### Motor y Cliente de Juego
*   **Lenguaje:** JavaScript Vanilla (Modulos de ECMAScript nativos, con integracion progresiva de TypeScript en entorno estricto).
*   **Renderizado:** API de Canvas 2D de HTML5. Resolucion logica de 1280x720 pixeles, auto-escalada mediante transformaciones matriciales de CSS para adaptarse a pantallas retina y monitores de alta resolucion sin deformacion de pixeles (Pixel-Perfect rendering).
*   **Empaquetador de Recursos:** Vite para resolucion rapida de modulos, optimizacion de assets en compilacion de produccion y hot module replacement en desarrollo.
*   **Entorno de Pruebas:** Vitest para la ejecucion de pruebas de regresion e integracion sobre los sistemas de fisica, calculo de dano y persistencia.
*   **Gestion de Dependencias:** pnpm (arquitectura monorrepo con enlaces de alta eficiencia).

### Servidores Backend
*   **Entorno de Ejecucion:** Node.js en todas las capas del servidor.
*   **Librerias Core de Red:** Express para servicios de API y ruteo, Socket.IO para el canal persistente de baja latencia (WebSockets con fallback HTTP long-polling).
*   **Conexiones Externas:** Librerias de conexion reversa para feed de streaming (tiktok-live-connector) y SDK oficial de Supabase para integraciones con bases de datos PostgreSQL en la nube.

### Despliegue e Infraestructura (Produccion)
*   **Alojamiento:** Servidor de Produccion (VPS dedicado Linux).
*   **Aislamiento:** Contenedores Docker coordinados con Docker Compose bajo una red virtualizada privada y segura.
*   **Proxy Inverso y Seguridad:** Servidor Caddy actuando como terminador TLS (certificados automaticos via Let's Encrypt), ruteando trafico HTTPS a archivos estaticos, APIs del servidor y endpoints Socket.IO de forma eficiente.

---

## ⚙️ Arquitectura del Motor Grafico y Fisicas

El motor de Wraithbane fue programado de forma nativa sin frameworks de terceros. Sus modulos clave incluyen:

### ⏱️ Frame Loop y Temporizacion
Implementacion basada en `requestAnimationFrame` sincronizada con un delta time estricto. Esto previene desfaces de velocidad de juego en monitores con tasas de refresco de alta frecuencia (como 144Hz o 240Hz), manteniendo una velocidad de simulacion constante equivalente a 60 frames por segundo.

### 📐 Sistema de Fisicas y Colisiones
*   **Caja delimitadora alineada a los ejes (AABB):** Calculos optimizados de colision entre el jugador, proyectiles y elementos del entorno.
*   **Deteccion de plataformas semi-solidas:** Permite al personaje subir a traves de plataformas saltando desde abajo y descender de forma intencional pulsando abajo y espacio (Gravity Slam).
*   **Ajuste de posicion seguro:** Algoritmo de des-penetracion de colisiones que previene que los personajes queden atrapados dentro de muros o suelos ante caidas de frames.

### 💾 Checkpoints Intra-nivel Inteligentes
*   El mapa se divide proceduralmente en zonas de progreso.
*   Al cruzar estos umbrales invisibles, el juego toma un snapshot en memoria del estado de vida, proyectiles y progreso del jugador.
*   **Sistema de caidas (Pit-fall):** Caer al vacio no descuenta vida; reubica al jugador en el ultimo suelo seguro guardado para evitar frustracion innecesaria y mantener la fluidez.

---

## 👾 Arquitectura del Sistema de Jefes (El Rey Espectro)

El combate final contra el Rey Espectro cuenta con un diseno estructurado de alta complejidad tecnica:

### 🧠 Persistencia del Estado de Combate
Modulo de estado persistente que mantiene el estado actual de la barra de vida del jefe e hitboxes activas de forma inmutable durante la sesion. Si el jugador muere dentro de la arena del jefe, al reiniciar el nivel, el jefe retiene la fase y la barra de salud que tenia en el momento de la derrota, evitando la repeticion de fases ya superadas y optimizando la reentrada del jugador.

### 🔄 Maquina de Estados de Ataque Multifase
El jefe muta a traves de 5 fases diferentes controladas por umbrales precisos de salud:
*   **Fase 1 (Acecho - 100% a 80% HP):** Patron defensivo con proyectiles parryables de rango largo.
*   **Fase 2 (Colera - 80% a 55% HP):** Patrones de area amplia en tierra y lluvia vertical aleatoria.
*   **Fase 3 (Disolucion - 55% a 30% HP):** Comportamiento de flotacion lateral persistente en el eje Y, alternando rafagas de proyectiles rapidos.
*   **Fase 4 (Umbra - 30% a 10% HP):** Teletransportaciones aleatorias basadas en temporizadores e inmunidad temporal tras realizar ataques.
*   **Fase 5 (Extincion - 10% a 0% HP):** Comportamiento frenetico que combina multiples clases de proyectiles simultaneamente e invocacion de esbirros de rango melee.

### 🛡️ Capa de Blindaje (HUD Dual)
El jefe cuenta con una barra de escudo que absorbe el 30% del dano total recibido. La renderizacion de esta capa en el canvas utiliza una barra secundaria cyan en paralelo con la barra de salud real.

---

## 💬 Arquitectura del Puente TikTok Live

La conexion de eventos de redes sociales con la logica del juego funciona de la siguiente manera:

1.  **Captura de Sockets:** El bridge se conecta a las APIs de TikTok simulando una sesion de espectador sobre el identificador de transmision provisto.
2.  **Mapeo de Regalos:** Un catalogo de referencias mapea los identificadores de regalos de TikTok a estructuras de datos in-game. Por ejemplo, el envio de un regalo especifico puede disparar una curacion al jefe, o un regalo de menor valor puede invocar proyectiles adicionales en pantalla.
3.  **Renderizacion Dinamica en Canvas:** Cuando el bridge emite un evento de regalo al juego, el overlay de la partida captura el payload que contiene el nombre del remitente y la URL de la imagen del regalo. La imagen se descarga de forma asincrona desde el CDN de TikTok y se dibuja directamente en el canvas con efectos de transiciones fluidas de opacidad de forma dinamica, usando fallbacks graficos locales si la conexion al CDN falla.

---

## 🚀 Pipeline de Despliegue y Mantenimiento

Para facilitar el desarrollo seguro y despliegues rapidos sin interrumpir las pruebas activas en el VPS, se utiliza el siguiente flujo de trabajo:

1.  **Desarrollo Local:** Ejecucion de servidores en entorno local con recarga en vivo de codigo. Las credenciales de integraciones de bases de datos y tokens se leen localmente desde archivos de entorno controlados y no incluidos en el repositorio (.env).
2.  **Pruebas Unitarias:** Ejecucion de suites de pruebas con Vitest antes de cada integracion de codigo para certificar estabilidad en las fisicas del jugador.
3.  **Compilacion y Empaquetado:** Generacion de archivos distribuidos optimizados utilizando minificacion de codigo estatico via Vite.
4.  **Despliegue Contenerizado:** El juego compilado se monta en los volumenes de produccion del contenedor web, mientras que los servidores Node.js (Bridge y control) se reconstruyen dentro de la red privada aislada de Docker en el servidor para reflejar los ultimos cambios inmediatamente.
