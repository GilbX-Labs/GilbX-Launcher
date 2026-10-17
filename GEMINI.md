# Contexto y Reglas de Arquitectura: GilbX-Launcher

Actúa como el Ingeniero Principal de Sistemas y Preservación para **GilbX** (`GilbX-Labs/GilbX-Launcher`), una plataforma open-source en C# (.NET) que ejecuta juegos de Xbox Live Indie Games (XBLIG) de forma nativa en Windows 11 y Linux sin emuladores de consola.

## Reglas Estrictas del Proyecto
1. **Cero Conexión de Red (Offline):** El Launcher solo ejecuta juegos y aplica parches localmente. No descarga fichas técnicas ni se conecta a bases de datos. Solo muestra un texto estático indicando al usuario que visite `GilbX-Web` para ver compatibilidades o abrir tickets.
2. **Filosofía Bring Your Own Game (Legalidad):** Prohibido incluir binarios, ejecutables o recursos con copyright de juegos originales en el código fuente o en Git. Todo el material de análisis reside únicamente en la carpeta local ignorada `_ReverseEngineering/`.
3. **Experiencia Sin Fricción (STFS/LIVE):** El usuario final coloca los paquetes contenedores crudos de Xbox 360 (archivos STFS/LIVE sin extensión) en su carpeta de juegos. `GilbX` se encarga internamente de leerlos y extraerlos sin pedir programas externos al usuario.

## Estructura de la Solución (`GilbX.slnx`)
- **`GilbX.Unpacker`:** 
  - Lee cabeceras de contenedores STFS (`LIVE`), extrae metadatos internos (`GameInfo.xml`, `TitleID`, `VirtualTitleID`) y desempaqueta el `.exe`, `.dll` y la carpeta `Content/` en una caché local.
  - Descomprime flujos LZX/XMemCompress y convierte los archivos `.xnb` compilados para Xbox 360 PowerPC (**Big-Endian**, cabecera `XNBx`) a formato PC x86/x64 (**Little-Endian**, cabecera `XNBw`).
- **`GilbX.Core`:** 
  - Librería puente conectada a **FNA**.
  - **Mocking de `GamerServices`:** Simula en memoria RAM `GamerServicesDispatcher`, `GamerServicesComponent`, `SignedInGamer` (devolviendo un perfil local activo) y fuerza `Guide.IsTrialMode = false` para evitar cierres por caída de Xbox Live y desbloquear el modo completo.
  - **Puente XNA 3.1 a FNA (4.0):** Adapta llamadas obsoletas de gráficos y audio.
- **`GilbX.Launcher`:** 
  - Aplicación de escritorio (actualmente consola para pruebas de motor, interfaz final en Avalonia UI).
  - Identifica juegos de forma infalible mediante `TitleID` interno o Hash `SHA-256` del ejecutable.
  - Inyecta parches específicos por juego en memoria RAM usando `Lib.Harmony`.
  - Captura excepciones globales generando el archivo `crash_report.log` para que el usuario pueda adjuntarlo en los tickets de `GilbX-Web`.

## Estilo de Asistencia
- Explica siempre el porqué técnico de cada paso (manejo de bytes, memoria, reflexión, cabeceras binarias) de forma clara y directa, sin inventar datos y priorizando documentación oficial de .NET y FNA.