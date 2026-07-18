# Wraithbane — El Último Velador

**Videojuego inmersivo de plataformas, acción y combate contra jefes en 2D con temática de fantasía oscura.**

> 🎮 **Pruebalo gratis:** [https://wraithbane.pro/](https://wraithbane.pro/)
> 📖 **Conoce la historia:** [https://wraithbane.pro/historia](https://wraithbane.pro/historia)
> 👤 **Sobre el autor:** [https://wraithbane.pro/autor](https://wraithbane.pro/autor)

* * *

## Acerca del proyecto

**Wraithbane** es un juego de tipo *boss-rush* en 2D donde encarnas a **Aldric, el Último Velador**, y viajas a través de las tierras corruptas de **Veylan** para enfrentar al temible **Rey Espectro** durante la **Larga Noche**. Tu misión es romper el ciclo infinito de la oscuridad recuperando los siete Fragmentos de Alba.

El proyecto fue desarrollado íntegramente desde cero por un programador de **10 años**, sin utilizar plantillas prefabricadas, con el objetivo de demostrar que no hay una edad mínima para empezar a crear, programar y divertirse con la tecnología.

* * *

## Características principales

*   **Combate contra jefes en múltiples fases**: Enfrenta al Rey Espectro a lo largo de 5 desafiantes etapas de combate.
*   **Sistema de combate preciso**: Domina el bloqueo (*parry*) y el desplazamiento (*dash*) para sobrevivir a los patrones de ataque del enemigo.
*   **Integración con transmisiones en vivo**: Compatible con TikTok Live a través de un puente de Socket.IO, permitiendo que los regalos y eventos del chat activen efectos en tiempo real dentro del juego.
*   **Estética retro pixelada**: Renderizado mediante la API de Canvas 2D de HTML5 para gráficos nítidos y fluidos.

* * *

## Stack tecnológico

*   **Lenguaje**: JavaScript Vanilla (Módulos ES, `"type": "module"`)
*   **Renderizado**: API de Canvas 2D de HTML5 (resolución lógica 1280×720)
*   **Empaquetador**: Vite 8.1.3
*   **Pruebas unitarias**: Vitest 4.1.10
*   **Gestor de paquetes**: pnpm 8.15.9
*   **Backend en vivo**: Node.js + Socket.IO (TikTok Live Bridge)

* * *

## Cómo ejecutar el juego localmente

### Requisitos

*   Node.js versión 22 o superior
*   pnpm versión 8 o superior

### Pasos

1.  Clona el repositorio:
    ```bash
    git clone https://github.com/travisd3v/wraithbane.git
    cd wraithbane
    ```

2.  Instala las dependencias:
    ```bash
    pnpm install
    ```

3.  Compila el proyecto para producción:
    ```bash
    pnpm build
    ```

4.  Inicia el servidor local de pruebas:
    ```bash
    pnpm dev
    ```

* * *

## Licencia y uso

Este proyecto se distribuye de forma privada. El código fuente no está disponible públicamente. Si deseas colaborar, probarlo o apoyarlo, puedes hacerlo a través de los enlaces oficiales al inicio.

* * *

_Construyendo mundos digitales desde Colombia para todo el universo._
