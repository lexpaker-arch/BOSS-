# Mapa de Arquitetura — ExoDash

## Visão Geral
App Android nativo (Kotlin) de HUD automotivo para veículo (foco: Peugeot 2001). Combina telemetria OBD-II via USB, voz (assistente "Boss"), diagnóstico DTC, media/GPS e UI estilo HUD sci-fi. Build via Gradle KTS.

## Estrutura de Diretórios

### Raiz / Build
- `build.gradle.kts`, `settings.gradle.kts`, `gradle.properties` — config Gradle raiz.
- `gradlew`, `gradle/wrapper/` — wrapper Gradle.
- `app/build.gradle.kts` — módulo app (dependências, versão, assinatura).
- `.github/workflows/release.yml` — CI de release (APK).
- `.devcontainer/` — ambiente de dev (post-create.sh instala toolchain).
- `scan.sh`, `trocar-carro.sh` — scripts auxiliares (scan de dispositivos / troca de perfil de veículo).
- `app-debug-vigilante.apk` — artefato de build local (não versionar idealmente).

### Configuração de Veículo
- `custom/focus_2001/vehicle.json` — perfil de veículo (PID map, limites). Lido por `VehicleConfig.kt`.

## Código-Fonte (`app/src/main/java/com/exodash/`)

### Entrada / Ciclo de Vida
- `SplashActivity.kt` — splash com vídeo (`res/raw/splash.mp4`).
- `MainActivity.kt` — tela principal HUD (dashboard de telemetria).
- `AppDrawerActivity.kt` — gaveta de apps.
- `SettingsActivity.kt` — preferências.
- `LogsActivity.kt` — visualização de logs.
- `DtcActivity.kt` — tela de códigos de falha (DTC).
- `BootReceiver.kt` — auto-start no boot.

### Núcleo de Telemetria / OBD
- `ObdUsbService.kt` — serviço USB serial OBD-II (foreground service).
- `UsbSerialStreams.kt` — streams de I/O sobre USB serial.
- `ObdBus.kt` — barramento/roteamento de mensagens OBD.
- `TelemetryData.kt` / `TelemetryState.kt` — modelos e estado reativo de telemetria.
- `VehicleConfig.kt` — carrega `custom/*/vehicle.json`.
- `DtcHistory.kt` — persistência de histórico de DTCs.
- `NetworkMonitor.kt` — monitor de conectividade.

### Voz / Assistente "Boss"
- `BossService.kt` — serviço do assistente.
- `BossBrain.kt` — lógica de decisão/NLU do assistente.
- `MainVoiceInteractionService.kt` — integração com VoiceInteractionService do Android (`res/xml/voice_interaction_service.xml`).
- `GroqClient.kt` — cliente HTTP para API Groq (LLM).

### Media / Sistema
- `MediaListenerService.kt` — NotificationListener para controle de mídia.
- `Updater.kt` — verificação de atualização (via GitHub releases).
- `CrashHandler.kt` — captura de crashes.
- `Breadcrumbs.kt` — rastreamento leve de eventos.
- `LogEventos.kt` — logging de eventos.
- `Prefs.kt` — wrapper de SharedPreferences.

## Recursos (`app/src/main/res/`)

### Layouts (`layout/`)
- `activity_main.xml` — HUD principal.
- `activity_splash.xml`, `activity_settings.xml`, `activity_logs.xml`, `activity_dtc.xml`, `activity_app_drawer.xml`.
- `item_app.xml`, `item_dtc.xml`, `item_log.xml` — itens de lista.

### Drawables (`drawable/`)
- HUD/estética: `hud_corner_*.xml`, `frame_silver.xml`, `bg_metallic.xml`, `scanline_line.xml`, `divider_gradient.xml`, `clock_ring.xml`.
- Barras: `barra_progress.xml`, `barra_trilha.xml`, `barra_fill_ciano.xml`, `barra_fill_ambar.xml`.
- Boss: `boss_button_bg.xml`, `boss_button_listening.xml`, `boss_glow.xml`.
- Mídia: `music_frame_bg.xml`, `music_progress_bar.xml`, `music_progress_bg.xml`.
- Ícones: `ic_apps`, `ic_gps`, `ic_music`, `ic_phone`, `ic_settings`, `ic_launcher_exodash`, `error_triangle`.
- Botões/ripple: `exo_button_bg.xml`, `shortcut_bg.xml`, `shortcut_ripple.xml`, `card_bg.xml`.

### Outros Recursos
- `font/` — Inter (light/regular), Roboto Mono (regular/bold).
- `values/` — `colors.xml`, `dimens.xml`, `strings.xml`, `themes.xml`; `values-sw600dp/dimens.xml` (tablet).
- `raw/splash.mp4` — vídeo de splash.
- `xml/` — `device_filter.xml` (USB VID/PID OBD), `file_paths.xml` (FileProvider), `voice_interaction_service.xml`.
- `assets/dtc_codes.json` — base de códigos DTC.

## Manifesto
- `app/src/main/AndroidManifest.xml` — declara Activities, Services (OBD, Boss, Media, Voice), BootReceiver, permissões USB/foreground/notification listener.

## Prototipagem / Design
- `preview/*.html` — protótipos de UI (evolução do design: v1→v23, laboratórios, splash, formatos). Referência visual, não compilado.
- `preview/frame4_novo.png` — mockup.

## Documentação
- `README.md` — visão geral.
- `WORKFLOW.md` — fluxo de desenvolvimento/release.

## Convenções-Chave
- **Pacote único**: `com.exodash` (flat, sem subpacotes).
- **Padrão de nomes**: `*Activity` (UI), `*Service` (background), `*Data`/`*State` (modelos), `*Client` (rede), `*Handler`/`*Listener` (callbacks).
- **Estado de telemetria**: centralizado em `TelemetryState`/`TelemetryData`, alimentado por `ObdUsbService` via `ObdBus`.
- **Config de veículo**: JSON externo em `custom/<modelo>/vehicle.json`, carregado por `VehicleConfig`.
- **UI**: XML clássico (sem Compose), tema HUD escuro com acentos ciano/âmbar, fontes Inter + Roboto Mono.