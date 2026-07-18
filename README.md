<div align="center">

# Wraithbane — Showcase Técnico

![Estado del Proyecto](https://img.shields.io/badge/Estado-Beta%20Patreon--Gated-F96854?style=for-the-badge&logo=patreon&logoColor=white)
![Plataforma](https://img.shields.io/badge/Plataforma-Navegador%20Web-000000?style=for-the-badge&logo=googlechrome&logoColor=white)
![Stack](https://img.shields.io/badge/Stack-JavaScript%20Vanilla-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

</div>

Este es mi primer juego completo como proyecto de desarrollo. Se trata de un *platformer* en 2D con temática de fantasía oscura y combate contra jefes, construido íntegramente por mí con **JavaScript Vanilla** y la **API de Canvas 2D**, sin recurrir a motores comerciales.

Lo que me motivó fue la posibilidad de combinar una arquitectura desacoplada de red con un motor gráfico optimizado para navegadores, manteniendo un control total sobre la lógica del juego.

[Sitio Web](https://wraithbane.pro/) | [Mi Perfil](http://localhost:3000/autor)

---

## Topología de la Arquitectura

El sistema se fundamenta en tres componentes independientes que garantizan modularidad y aislamiento de responsabilidades:

### Cliente del Juego (Frontend)
El núcleo gráfico se ejecuta en el hilo principal del navegador. Sus responsabilidades son el procesamiento de físicas, el renderizado, la inteligencia artificial de los jefes y la gestión de estado. Se compila como una SPA estática optimizada con Vite para tiempos de carga mínimos, y se comunica mediante un cliente Socket.IO.

### Puente de Transmisión (Bridge)
Middleware de alta disponibilidad para la integración con feeds de datos externos. Captura, normaliza y difunde eventos de streaming en tiempo real, traduce los payloads externos en eventos de red consumibles por el motor, y opera como un servidor Node.js optimizado para conexiones simultáneas de baja latencia.

### Servidor de Control y Telemetría
Backend administrativo enfocado en la gestión de acceso y persistencia. Implementa autenticación OAuth2, persistencia de perfiles y recolección de telemetría de rendimiento. El almacenamiento opera sobre una arquitectura servida mediante Supabase para sincronización global.

---

## Motor Gráfico y Núcleo de Ingeniería

Diseñé cada subsistema del motor con un enfoque específico de optimización:

### Ciclo de Ejecución y Sincronización
El bucle principal se basa en `requestAnimationFrame` con cálculo dinámico de tiempo delta, lo que asegura una velocidad de simulación constante de 60 FPS. Incorporé un sistema de **Hitstop** que congela selectivamente los frames tras impactos críticos para acentuar la respuesta sensorial del combate sin interrumpir la lógica de fondo.

### Gestión Avanzada de Recursos (AtlasCache)
*   **Texture Packing:** Atlas de texturas en formato WebP con soporte para **Trimmed Rects** (eliminación de píxeles transparentes redundantes) para optimizar el uso de VRAM.
*   **Compensación Dinámica:** Algoritmo que revierte el recorte en tiempo real durante el renderizado, garantizando una posición pixel-perfect idéntica a la imagen original.
*   **Asynchronous Decoding:** Carga perezosa de recursos que permite el inicio de la simulación mientras las páginas del atlas se decodifican en hilos secundarios.

### Físicas y Resolución de Colisiones
*   **Motor AABB Propietario:** Detección de colisiones basada en cajas delimitadoras alineadas a los ejes con resolución de penetración de baja sobrecarga computacional.
*   **Plataformas Unidireccionales:** Lógica personalizada para el manejo de plataformas semi-sólidas y sistemas de descenso intencional (Gravity Slam).
*   **Manejo de Lag Spikes:** Lógica de compensación de posición segura que previene que los objetos atraviesen muros ante fluctuaciones en la tasa de frames.

---

## Sistemas de Estado y Persistencia

El sistema de jefes utiliza una lógica de estados inmutable gestionada por un almacén de fase dedicado (`BossPhaseStore`). Esto permite que el jefe retenga su salud y comportamiento exacto entre intentos del jugador, optimizando la experiencia de juego sin recargas de página.

En el apartado gráfico, dispongo de un motor de partículas basado en buffers locales para humo, chispas y explosiones, además de generadores procedurales para efectos climáticos como lluvia y niebla.

---

## Integración con Streaming en Tiempo Real

Una parte esencial del proyecto es la capacidad de interactuar con la audiencia durante mis transmisiones en vivo. El puente normaliza sockets externos, mapea atributos para vincularlos con funciones del juego (curación, invocación o proyectiles), y renderiza overlays dinámicos descargando recursos desde CDN externos de forma asíncrona con transiciones suaves de opacidad.

---

## Flujo de Trabajo Asistido por IA

El desarrollo de Wraithbane integra activamente el uso de inteligencia artificial de última generación como herramienta de aprendizaje y construcción.

### Modelos de IA

| Modelo | Implementación |
| --- | --- |
| ![Claude](https://img.shields.io/badge/Claude-Sonnet%20%7C%20Opus%20%7C%20Fable-D97757?style=for-the-badge&logo=anthropic&logoColor=white) | Modelos avanzados de Anthropic empleados para razonamiento crítico y arquitectura. |
| ![MiniMax](https://img.shields.io/badge/MiniMax-M3-FF6B35?style=for-the-badge&logo=openai&logoColor=white) | Modelo **M3** mediante plan de tokens y API de NVIDIA. |
| ![GLM](https://img.shields.io/badge/GLM-5.2-4D8EFF?style=for-the-badge&logo=zhipu&logoColor=white) | Modelo **5.2** integrado a través de la API de NVIDIA. |
| ![Gemini](https://img.shields.io/badge/Gemini-3.5%20Vertex-4285F4?style=for-the-badge&logo=google&logoColor=white) | Modelo **3.5** de Google mediante la plataforma Vertex de mis padres. |

### Entornos de Desarrollo (IDEs)

| Herramienta | Detalle |
| --- | --- |
| ![Kiro](https://img.shields.io/badge/IDE-Kiro-7E47E0?style=for-the-badge) | IDE principal para la generación y edición de código. |
| ![Antigravity](https://img.shields.io/badge/IDE-Antigravity-000000?style=for-the-badge) | Entorno secundario para tareas de implementación. |

### Terminales y Frameworks CLI

| Herramienta | Detalle |
| --- | --- |
| ![OpenCode](https://img.shields.io/badge/CLI-OpenCode-000000?style=for-the-badge) | Interfaz principal de comandos. |
| ![Claude Code](https://img.shields.io/badge/CLI-Claude%20Code-D97757?style=for-the-badge&logo=anthropic&logoColor=white) | Interacción complementaria con Claude. |

### Framework SDD

![Gentle-AI](https://img.shields.io/badge/SDD-Gentle_AI-FF8C00?style=for-the-badge&logo=openai&logoColor=white)

Empleo el framework **[Gentle-AI](https://github.com/Gentleman-Programming/gentle-ai)** para el ciclo de desarrollo basado en especificaciones (Spec-Driven Development). Es una herramienta **excelente** para proyectos de esta naturaleza, gracias a su integración nativa con múltiples modelos de IA, gestión de contexto avanzado y automatización de fases de planificación e implementación.

---

Este proyecto representa una demostración de ingeniería web aplicada, donde el control total sobre el pipeline de renderizado y la arquitectura de red permite construir experiencias de juego de nivel profesional en el navegador.