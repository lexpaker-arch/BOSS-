# Mapa de Arquitetura — ExoDash

## Visão Geral
App Android nativo (Kotlin) de HUD automotivo para veículo (foco: Focus 2001). Combina telemetria OBD-II via USB, voz (assistente "Boss"), monitoramento de mídia/rede, DTCs e atualização remota. UI em XML clássico (sem Compose).

## Build & Config
- **`build.gradle.kts` / `app/build.gradle.kts`** — configuração Gradle (raiz e módulo).
- **`settings.gradle.kts`**, **`gradle.properties`**, **`gradle/wrapper/*`**, **`gradlew(.bat)`** — wrapper e settings.
- **`custom/focus_2001/vehicle.json`** — perfil do veículo (config específica do carro).
- **`trocar-carro.sh`** — script para alternar perfil de veículo.
- **`scan.sh`** — utilitário de varredura (provavelmente OBD/portas).
- **`.github/workflows/release.yml`** — CI de release.
- **`.devcontainer/*`** — ambiente de desenvolvimento.

## Manifesto & Recursos Globais
- **`app/src/main/AndroidManifest.xml`** — declara Activities, Services, Receivers, permissões.
- **`res/values/`** — `colors.xml`, `dimens.xml` (+ `values-sw600dp/`), `strings.xml`, `themes.xml`.
- **`res/xml/`** — `device_filter.xml` (USB OBD), `file_paths.xml` (FileProvider), `voice_interaction_service.xml`.
- **`res/font/`** — Inter (light/regular) e Roboto Mono (regular/bold).
- **`res/raw/splash.mp4`** — vídeo de splash.

## Interface (UI)
Tudo em **`app/src/main/res/`**:
- **`layout/`** — telas: `activity_main.xml` (HUD principal), `activity_splash.xml`, `activity_settings.xml`, `activity_dtc.xml`, `activity_logs.xml`, `activity_app_drawer.xml`; itens de lista: `item_app.xml`, `item_dtc.xml`, `item_log.xml`.
- **`drawable/`** — estética HUD: molduras (`frame_silver`, `bg_metallic`, `hud_corner_*`), barras (`barra_*`), botões Boss (`boss_button_*`, `boss_glow`), cards, ícones (`ic_*`), progresso de música, scanlines, splash icon.

## Lógica de Apresentação (Activities)
Em **`app/src/main/java/com/exodash/`**:
- **`SplashActivity.kt`** — splash + vídeo.
- **`MainActivity.kt`** — HUD principal (velocidade, RPM, telemetria).
- **`AppDrawerActivity.kt`** — gaveta de apps.
- **`SettingsActivity.kt`** — configurações.
- **`DtcActivity.kt`** — diagnóstico de códigos de falha.
- **`LogsActivity.kt`** — visualização de logs.

## Serviços & Background
- **`BossService.kt`** — serviço do assistente de voz "Boss".
- **`MainVoiceInteractionService.kt`** — integração com VoiceInteractionService do Android.
- **`MediaListenerService.kt`** — NotificationListener para controle de mídia.
- **`ObdUsbService.kt`** — serviço de comunicação OBD via USB.
- **`BootReceiver.kt`** — inicialização no boot.
- **`NetworkMonitor.kt`** — monitor de conectividade.

## Núcleo de Domínio (lógica pura)
- **`BossBrain.kt`** — cérebro do assistente (NLU/roteamento de comandos).
- **`GroqClient.kt`** — cliente da API Groq (LLM).
- **`ObdBus.kt`** — barramento/abstração OBD.
- **`UsbSerialStreams.kt`** — streams seriais USB.
- **`TelemetryData.kt` / `TelemetryState.kt`** — modelos e estado de telemetria.
- **`VehicleConfig.kt`** — carregamento do perfil do veículo.
- **`DtcHistory.kt`** — histórico de DTCs.
- **`Breadcrumbs.kt`** — rastreamento de navegação/contexto.
- **`LogEventos.kt`** — logging de eventos.
- **`Prefs.kt`** — preferências (SharedPreferences).
- **`CrashHandler.kt`** — captura de crashes.
- **`Updater.kt`** — atualização in-app (via GitHub releases).

## Dados
- **`app/src/main/assets/dtc_codes.json`** — base de códigos DTC.

## Artefatos & Auxiliares
- **`app-debug-vigilante.apk`** — build de debug.
- **`.kotlin/errors/*.log`** — logs do compilador Kotlin (ignorar).
- **`preview/*.html`** — protótipos de UI (histórico de design, não usados em runtime).
- **`README.md`**, **`WORKFLOW.md`** — documentação.

## Convenções Rápidas
- Pacote único: `com.exodash`.
- UI = XML + Activities; sem Compose.
- Comunicação OBD: `ObdUsbService` → `ObdBus` → `TelemetryState` → `MainActivity`.
- Voz: `MainVoiceInteractionService` → `BossService` → `BossBrain` → `GroqClient`.
- Config de veículo isolada em `custom/<modelo>/vehicle.json` (troca via `trocar-carro.sh`).