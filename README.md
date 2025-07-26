## <a name="_v931u9sb352g"></a>**1. Definición del concepto y análisis de mercado**
1. Objetivo del juego
   1. Juego ultra sencillo: mecánica intuitiva, curva de aprendizaje mínima.
   1. Experiencia idéntica al anuncio: evita falsedad publicitaria.
   1. Enfoque en retención y monetización in-game (microtransacciones + anuncios).
1. Investigación de competencia
   1. Identifica títulos top “hyper-casual” en ambas tiendas.
   1. Analiza métricas clave: descargas, reseñas, ARPU (ingreso medio por usuario).
   1. Estudia formatos de anuncios más rentables (rewarded video, intersticiales).
1. Público objetivo
   1. Edad: 18–45 años.
   1. Horario de juego: momentos de ocio breve (transporte, descansos).
   1. Regiones: mercados con CPM/CPM alto (EEUU, Europa, Japón, Corea).
## <a name="_g0vhbpdgo3l"></a>**2. Planificación y documentación**
1. Documento de diseño (Game Design Document)
   1. Descripción de la mecánica principal y secundarias.
   1. Flujos de juego (niveles, reinicios, menús).
   1. Árbol de monetización: precios de ítems, packs de skins, boosts.
1. Especificaciones técnicas
   1. Plataformas objetivo: iOS (iPhone/iPad) y Android.
   1. Resoluciones soportadas, rango mínimo de dispositivo.
   1. Integraciones: SDK de anuncios (AdMob, Unity Ads), sistema de pagos (IAP nativo).
1. Cronograma de desarrollo
   1. Preproducción: 1–2 semanas (prototipo en papel).
   1. Producción: 4–6 semanas (programación y assets).
   1. Testeo y QA: 2 semanas.
   1. Publicación y marketing: 1 semana.

## <a name="_eg5gfmybn88w"></a>**3. Selección de tecnología y herramientas**
### <a name="_54irem261qx7"></a>**3.1 Motores de juego comparados**

|**Característica**|**Unity**|**Godot**|**Unreal Engine**|
| :-: | :-: | :-: | :-: |
|Curva de aprendizaje|Moderada|Baja|Alta|
|Multiplataforma|iOS, Android, Web|iOS, Android, Desktop|iOS, Android, Consolas|
|Presupuesto|Gratis (con revenue cap)|100% gratis|Gratis (royalties 5%)|
|Comunidad & recursos|Muy amplia|Creciente|Amplia|
|Integración de Ads & IAP|Plugins oficiales y terceros|Módulos de terceros|Plugins oficiales|
### <a name="_h5wvytgryuoy"></a>**3.2 Herramientas adicionales**
- Editor de gráficos: Aseprite, Photoshop, GIMP.
- Sonido: Audacity, Bfxr, FreeSound.org..
- Control de versiones: Git + GitHub/GitLab.
## <a name="_3ftekfpzya0r"></a>**4. Desarrollo (Unity)**
A partir de este punto, toda la implementación se enfoca en Unity 2021 LTS (o superior) con los módulos de iOS y Android instalados.
### <a name="_9o40v799xg7a"></a>**4.1 Configuración del proyecto en Unity**
Antes de escribir una sola línea de código, prepara tu entorno de trabajo.

- Instala Unity Hub y crea un proyecto nuevo usando la plantilla **2D** (o **3D**, según tu mecánica).
- En Unity Hub, asegúrate de incluir los módulos de **iOS Build Support** y **Android Build Support**.
- Abre **Edit → Project Settings** y, en **Player**:
  - Define el **Company Name** y **Product Name**.
  - Ajusta la **Bundle Identifier** (com.tuempresa.tujuego) para iOS y Android.
  - Selecciona **IL2CPP** como Scripting Backend y **.NET 4.x** como API Compatibility Level para mejor rendimiento y compatibilidad con SDKs.
  - En **Resolution and Presentation**, marca **Portrait** o **Landscape** según tu diseño.
### <a name="_pdpgdk328sx6"></a>**4.2 Prototipado rápido**
Construye un “vertical slice” de la mecánica principal con assets temporales.

- Utiliza sprites cuadrados o círculos para representar jugadores y obstáculos con GameObjects sencillos.
- Emplea el nuevo **Input System** o el **Input Manager** clásico, según tu preferencia.
- Añade **Cinemachine** para gestionar cámaras suaves y responsive.
- Itera en la jugabilidad: valida controles, tiempos de respuesta (input lag) y feedback visual acústico.
### <a name="_ye9jhpv2xb2t"></a>**4.3 Integración de monetización**
Unity facilita la inclusión de anuncios y compras in-app a través de sus paquetes oficiales.
#### <a name="_yg0vvuow9s28"></a>**4.3.1 Unity Ads**
1. Abre **Window → Package Manager**, busca “Advertisement” e instala el paquete oficial.
1. Ve a **Window → Services**, activa el servicio **Ads** y conecta tu proyecto al **Unity Project ID**.
1. En Dashboard de Unity Ads, crea **Ad Units** para:
   1. Intersticiales (por nivel).
   1. Rewarded Video (para ofrecer vidas extras o monedas).
   1. Banner (opcional, en menú principal).

Inicializa el SDK en tu código:\
csharp\
using UnityEngine.Advertisements;

void Start() {

`  `Advertisement.Initialize("UNITY\_GAME\_ID", testMode: true);

}

void ShowRewarded() {

`  `if (Advertisement.IsReady("rewardedVideo")) {

`    `Advertisement.Show("rewardedVideo");

`  `}

}

#### <a name="_w2p066yxdb27"></a>**4.3.2 Unity IAP**
1. En **Package Manager**, instala **In-App Purchasing**.
1. Activa **Purchasing** dentro de **Services** y configura tus productos (consumibles y no consumibles).
1. Define los **Product IDs** iguales a los de App Store Connect y Google Play Console.

Implementa la lógica de compra:\
csharp\
using UnityEngine.Purchasing;

public class IAPManager : IStoreListener {

`  `private static IStoreController controller;

`  `void Start() {

`    `var builder = ConfigurationBuilder.Instance(StandardPurchasingModule.Instance());

`    `builder.AddProduct("com.tuempresa.tujuego.coinpack1", ProductType.Consumable, new IDs{{"coinpack1", AppleAppStore.Name}, {"coinpack1", GooglePlay.Name}});

`    `UnityPurchasing.Initialize(this, builder);

`  `}

`  `public void OnInitialized(IStoreController ctrl, IExtensionProvider ext) {

`    `controller = ctrl;

`  `}

`  `public void BuyCoins() {

`    `controller.InitiatePurchase("com.tuempresa.tujuego.coinpack1");

`  `}

`  `// Maneja callbacks de éxito y fallo...

}

### <a name="_epbevmz3vce9"></a>**4.4 Guardado y sincronización ligera**
Para empezar, usa **PlayerPrefs** y, si necesitas nube:

- **PlayerPrefs** para progreso local (niveles desbloqueados, monedas).
- **Unity Cloud Save** (paquete de Unity Gaming Services) para sincronizar datos entre dispositivos sin montar tu propio servidor.
- Alternativa: integra **Firebase Realtime Database** usando el SDK de Firebase para Unity, si requieres más flexibilidad.
### <a name="_1ryg4337remf"></a>**4.5 Optimización para móviles**
Asegura una experiencia suave, incluso en dispositivos de gama baja.

- Reduce **Draw Calls** usando **Sprite Atlases**.
- Comprime texturas en **ASTC** (Android) y **PVRTC** (iOS).
- Ajusta el **Quality Level** para que no supere los 30 FPS en hardware mínimo.
- Usa **Addressables** para cargar assets en tiempo de ejecución y reducir el tamaño inicial del APK/AAB.

## <a name="_jb1rleuj9jp8"></a>**5. Pruebas y aseguramiento de calidad**
1. Test funcional
   1. Verifica mecánicas, IAP y anuncios en dispositivos reales (Android 6.0+ e iOS 12+).
   1. Prueba rutas de compra y cancelación.
1. Test de rendimiento
   1. Mide FPS en terminales de gama baja.
   1. Optimiza sprites, reduce draw calls, comprime texturas.
1. Test de cumplimiento de políticas
   1. Apple Review Guidelines (publicidad, privacidad, datos).
   1. Google Play Developer Policy (experiencia, contenido, pagos).
1. Beta testing
   1. TestFlight para iOS, beta interna de Google Play.
   1. Recoge feedback sobre bugs, usabilidad y monetización.
## <a name="_uxt9ae5nc5et"></a>**6. Publicación en App Store y Play Store**
### <a name="_glroxx1oh2k3"></a>**6.1 Requisitos y costos**

|**Plataforma**|**Cuota única**|**Cuenta de desarrollador**|
| :-: | :-: | :-: |
|App Store (iOS)|US$ 99 / año|Apple Developer Program|
|Play Store (Android)|US$ 25 (único)|Google Play Console|
### <a name="_a536ifpq41i5"></a>**6.2 Procedimiento**
1. Prepara assets: iconos (1024×1024), capturas de pantalla, vídeo promocional (opcional).
1. Completa metadatos: título, descripción breve y completa, palabras clave.
1. Configura precios in-app en consola de cada tienda.
1. Sube archivo binario (IPA en App Store Connect, APK/AAB en Google Play Console).
1. Envía revisión:
   1. iOS: suele tardar 1–3 días.
   1. Android: publicada casi al instante.
## <a name="_b49685z3puzr"></a>**7. Estrategia de marketing y ASO**
1. Optimización en tiendas (App Store Optimization)
   1. Título claro + palabra clave principal.
   1. Descripción atractiva con bullet points destacados.
   1. Capturas que muestren jugabilidad real.
1. Campañas pagadas
   1. Facebook Ads, Unity Ads “Playable Ads”.
   1. Google UAC (Universal App Campaigns).
1. Medición y análisis
   1. Integra Google Analytics for Firebase o similar.
   1. KPI: installs, DAU, ARPU, retención D1/D7/D30.
1. Ajustes continuos
   1. A/B testing de iconos y screenshots.
   1. Revisión de precios y frecuencia de anuncios.
## <a name="_cx2jyklyp1qd"></a>**8. Mantenimiento y escalado**
- Actualizaciones periódicas de contenido (nuevos niveles, skins).
- Monitoreo de reseñas y atención al usuario.
- Mejora continua de rendimiento y corrección de bugs.
- Expansión a otros mercados o idiomas.
## <a name="_pklcw9mkynzt"></a>**9. Recomendaciones finales**
- Sé transparente: ofrece la misma experiencia que prometes en tu anuncio.
- Prioriza la jugabilidad suave y sin frustraciones.
- Ajusta la densidad de anuncios para no ahuyentar usuarios.
- Planifica reinversiones de ingresos en marketing y desarrollo de contenido.

