# Wraithbane — Showcase Técnico

![Estado del Proyecto](https://img.shields.io/badge/Estado-Beta%20Patreon--Gated-F96854?style=for-the-badge&logo=patreon&logoColor=white)
![Plataforma](https://img.shields.io/badge/Plataforma-Navegador%20Web-000000?style=for-the-badge&logo=googlechrome&logoColor=white)
![Stack](https://img.shields.io/badge/Stack-JavaScript%20Vanilla-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

Videojuego de plataformas y combate contra jefes en 2D con temática de fantasía oscura. El proyecto se caracteriza por haber sido desarrollado de forma nativa, prescindiendo de motores comerciales o plantillas prefabricadas, integrando una arquitectura desacoplada para interacciones en tiempo real.

[Sitio Web](https://wraithbane.pro/) | [Perfil del Autor](https://wraithbane.pro/autor)

---

## Topología de la Arquitectura

El sistema se fundamenta en tres componentes independientes que garantizan la modularidad y el aislamiento de responsabilidades:

### Cliente del Juego (Frontend)
Núcleo gráfico y lógico ejecutado directamente en el entorno del navegador.
*   **Responsabilidad:** Procesamiento de físicas de colisión AABB, renderizado de alta fidelidad, IA de jefes y gestión del estado del jugador.
*   **Infraestructura:** Aplicación estática optimizada mediante Vite para una carga inmediata.
*   **Comunicación:** Conexión persistente mediante Socket.IO para eventos dinámicos.

### Puente de Transmisión (Bridge)
Middleware encargado de la integración con plataformas externas de streaming.
*   **Responsabilidad:** Captura y normalización de eventos de TikTok Live (regalos, comentarios y seguidores).
*   **Traducción:** Conversión de metadatos externos en comandos ejecutables por el motor del juego en tiempo real.
*   **Distribución:** Servidor Node.js que difunde eventos a múltiples clientes de forma concurrente.

### Servidor de Control y Telemetría
Backend administrativo enfocado en la seguridad y gestión de datos.
*   **Responsabilidad:** Control de acceso mediante OAuth2 (Patreon), persistencia de perfiles y captura de telemetría de rendimiento para control de calidad.
*   **Almacenamiento:** Integración con Supabase para la sincronización global de progreso y tablas de clasificación.

---

## Especificaciones Técnicas

### Núcleo de Desarrollo
*   **Lenguaje de Programación:** JavaScript Vanilla con módulos nativos y entorno estricto.
*   **Renderizado:** API de Canvas 2D de HTML5 con resolución lógica de 1280x720. Sistema de escalado pixel-perfect mediante transformaciones CSS.
*   **Entorno de Pruebas:** Vitest para la validación automática de mecánicas de combate y sistemas de físicas.

### Infraestructura y Despliegue
*   **Contenerización:** Uso extensivo de Docker y Docker Compose para el aislamiento de servicios.
*   **Servidor Web y Proxy:** Caddy configurado para la terminación automática de TLS y enrutamiento inteligente de tráfico hacia los contenedores internos.
*   **Mantenimiento:** Pipeline de despliegue automatizado que garantiza la actualización del juego sin interrumpir la disponibilidad de los servicios de backend.

---

## Arquitectura del Motor de Juego

### Gestión del Ciclo de Ejecución
Implementación de un bucle de juego basado en `requestAnimationFrame` con sincronización de tiempo delta. Este enfoque asegura una velocidad de simulación constante de 60 FPS, independientemente de la frecuencia de refresco del monitor del usuario.

### Sistema de Colisiones y Físicas
Utilización de algoritmos de cajas delimitadoras alineadas a los ejes (AABB) optimizados para el rendimiento en dispositivos web. Se incluye soporte para plataformas semi-sólidas y sistemas de resolución de penetración de colisiones.

### Sistema de Checkpoints Inteligente
El motor realiza capturas de estado automáticas al alcanzar hitos de progreso dentro de cada nivel. El sistema de caídas (Pit-fall) utiliza estos datos para reposicionar al jugador de forma segura, eliminando la frustración por errores de precisión en plataformas.

---

## Integración Dinámica con Streaming

La arquitectura permite que la audiencia influya directamente en la partida mediante el envío de regalos virtuales:
1.  **Captura de Señal:** El puente procesa el flujo de datos de la API de streaming.
2.  **Ejecución de Efectos:** Los regalos se mapean a acciones específicas, como la invocación de proyectiles adicionales o la curación del jefe final.
3.  **Renderizado Asíncrono:** Uso de overlays dinámicos que descargan y dibujan recursos visuales desde redes de entrega de contenido (CDN) externas de forma eficiente.

---

Este proyecto representa un esfuerzo de ingeniería por demostrar que las tecnologías web modernas son capaces de sostener experiencias de juego complejas y profesionales.
