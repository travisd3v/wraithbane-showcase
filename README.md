# Wraithbane — Showcase Técnico

![Estado del Proyecto](https://img.shields.io/badge/Estado-Beta%20Patreon--Gated-F96854?style=for-the-badge&logo=patreon&logoColor=white)
![Plataforma](https://img.shields.io/badge/Plataforma-Navegador%20Web-000000?style=for-the-badge&logo=googlechrome&logoColor=white)
![Stack](https://img.shields.io/badge/Stack-JavaScript%20Vanilla-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

Videojuego de plataformas y combate contra jefes en 2D con temática de fantasía oscura. El proyecto se caracteriza por un motor propietario desarrollado de forma nativa, optimizado para ejecución en navegadores modernos mediante una arquitectura desacoplada y sistemas de renderizado personalizados.

[Sitio Web](https://wraithbane.pro/) | [Perfil del Autor](https://wraithbane.pro/autor)

---

## Topología de la Arquitectura

El sistema se fundamenta en tres componentes independientes que garantizan la modularidad y el aislamiento de responsabilidades:

### Cliente del Juego (Frontend)
Núcleo gráfico ejecutado en el hilo principal del navegador.
*   **Responsabilidad:** Procesamiento de físicas, renderizado, IA de jefes y gestión de estado.
*   **Infraestructura:** SPA estática optimizada con Vite para garantizar tiempos de carga mínimos.
*   **Comunicación:** Suscriptor Socket.IO para el procesamiento de eventos asíncronos externos.

### Puente de Transmisión (Bridge)
Middleware de alta disponibilidad para la integración con feeds de datos externos.
*   **Responsabilidad:** Captura, normalización y difusión de eventos de streaming en tiempo real.
*   **Traducción:** Esquematización de payloads externos en eventos de red consumibles por el motor.
*   **Distribución:** Servidor Node.js optimizado para conexiones simultáneas de baja latencia.

### Servidor de Control y Telemetría
Backend administrativo para la gestión de acceso y persistencia.
*   **Responsabilidad:** Autenticación OAuth2, persistencia de perfiles y recolección de telemetría de rendimiento.
*   **Almacenamiento:** Arquitectura de datos servida mediante Supabase para sincronización global.

---

## Motor Gráfico y Núcleo de Ingeniería

El motor de Wraithbane ha sido diseñado específicamente para maximizar el rendimiento de la API Canvas 2D sin frameworks externos:

### Ciclo de Ejecución y Sincronización
*   **Frame Loop:** Basado en `requestAnimationFrame` con cálculo dinámico de tiempo delta para asegurar una velocidad de juego constante a 60 FPS.
*   **Hitstop System:** Implementación de congelación selectiva de frames tras impactos críticos. Esta técnica acentúa la respuesta sensorial del combate sin interrumpir la lógica de fondo.

### Gestión Avanzada de Recursos (AtlasCache)
*   **Texture Packing:** Uso de atlas de texturas en formato WebP con soporte para **Trimmed Rects** (eliminación de píxeles transparentes redundantes) para optimizar el uso de VRAM.
*   **Compensación Dinámica:** Algoritmo integrado que revierte el recorte en tiempo real durante el renderizado, garantizando una posición pixel-perfect idéntica a la imagen original.
*   **Asynchronous Decoding:** Carga perezosa de recursos que permite el inicio de la simulación mientras las páginas del atlas se decodifican en hilos secundarios.

### Físicas y Resolución de Colisiones
*   **Motor AABB Propietario:** Detección de colisiones basada en cajas delimitadoras alineadas a los ejes con resolución de penetración de baja sobrecarga computacional.
*   **Plataformas Unidireccionales:** Lógica personalizada para el manejo de plataformas semi-sólidas y sistemas de descenso intencional (Gravity Slam).
*   **Manejo de Lag Spikes:** Lógica de compensación de posición segura que previene que los objetos atraviesen muros ante fluctuaciones en la tasa de frames.

---

## Sistemas de Estado y Persistencia

### Máquina de Estados Multifase
El sistema de jefes utiliza una lógica de estados inmutable gestionada por un almacén de fase dedicado (`BossPhaseStore`). Esto permite que el jefe retenga su salud y comportamiento exacto entre intentos del jugador, optimizando la experiencia de juego sin recargas de página.

### Generación de Efectos Procedurales
*   **Sistemas de Partículas:** Motor de partículas basado en buffers locales para la generación de humo, chispas y explosiones.
*   **Ambiente Dinámico:** Generadores procedurales para efectos climáticos (lluvia y niebla) que se ejecutan en paralelo con el renderizado de sprites.

---

## Integración con Streaming en Tiempo Real

La arquitectura permite una interacción bidireccional entre la audiencia y la partida de Travis:
1.  **Normalización de Sockets:** El puente traduce señales externas en estructuras de datos estandarizadas.
2.  **Mapeo de Atributos:** Catálogo dinámico que vincula identificadores externos con funciones de activación en el juego (curación, invocación o disparos de proyectiles).
3.  **Renderizado Dinámico de Overlays:** El motor descarga recursos desde CDN externos de forma asíncrona y los integra directamente en el ciclo de renderizado con transiciones suaves de opacidad.

---

## Atribuciones y Desarrollo Asistido por IA

Este proyecto fue creado en primera persona por **Travis**, contando con la guía y mentoría de sus padres programadores y desarrolladores. El desarrollo integra activamente flujos de trabajo asistidos por modelos de inteligencia artificial de última generación.

### Asistentes de IA Utilizados

| Modelo | Detalle de Uso |
| --- | --- |
| ![Claude](https://img.shields.io/badge/Claude-Sonnet%20%7C%20Opus%20%7C%20Fable-D97757?style=for-the-badge&logo=anthropic&logoColor=white) | Modelos avanzados de Anthropic empleados para razonamiento crítico y asistencia en arquitectura. |
| ![MiniMax](https://img.shields.io/badge/MiniMax-M3%20(Coding%20Plan)-FF6B35?style=for-the-badge&logo=openai&logoColor=white) | Modelo **M3** mediante plan de tokens y consumo puntual a través de la API de NVIDIA. |
| ![GLM](https://img.shields.io/badge/GLM-5.2%20(API%20NVIDIA)-4D8EFF?style=for-the-badge&logo=zhipu&logoColor=white) | Modelo **5.2** de Zhipu integrado vía API de NVIDIA. |
| ![Gemini](https://img.shields.io/badge/Gemini-3.5%20(Vertex)-4285F4?style=for-the-badge&logo=google&logoColor=white) | Modelo **3.5** de Google ejecutado a través de la plataforma Vertex de los padres. |

### Entornos de Desarrollo

| Herramienta | Detalle |
| --- | --- |
| ![Kiro](https://img.shields.io/badge/IDE-Kiro-7E47E0?style=for-the-badge) | Entorno integrado para la generación de código. |
| ![Antigravity](https://img.shields.io/badge/IDE-Antigravity-000000?style=for-the-badge) | Entorno secundario para tareas de implementación. |

### Terminales y Frameworks CLI

| Herramienta | Detalle |
| --- | --- |
| ![OpenCode](https://img.shields.io/badge/CLI-OpenCode-000000?style=for-the-badge) | Interfaz de comandos principal utilizada para la edición y ejecución de tareas. |
| ![Claude Code](https://img.shields.io/badge/CLI-Claude%20Code-D97757?style=for-the-badge&logo=anthropic&logoColor=white) | Herramienta complementaria para interacción con el modelo Claude. |

### Metodología de Especificación Dirigida (SDD)

![Gentle-AI](https://img.shields.io/badge/SDD-Gentle_AI-FF8C00?style=for-the-badge&logo=openai&logoColor=white)

Se hizo uso del framework **[Gentle-AI](https://github.com/Gentleman-Programming/gentle-ai)** para el ciclo de desarrollo basado en especificaciones (Spec-Driven Development). Esta herramienta resultó ser una elección **excelente** para trabajar en proyectos de esta naturaleza, gracias a su integración nativa con múltiples modelos de IA, gestión de contexto avanzado y automatización de fases de planificación e implementación.

---

Este proyecto representa una demostración de ingeniería web aplicada, donde el control total sobre el pipeline de renderizado y la arquitectura de red permite crear experiencias de juego de nivel profesional en el navegador.
