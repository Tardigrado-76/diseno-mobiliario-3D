# Tavilga 3D Mobile - Android Distribution & Runtime Architecture

[![Android](https://img.shields.io/badge/Platform-Android%2014%2B%20%28API%2034--36%29-3DDC84?logo=android&logoColor=white)]()
[![Engine](https://img.shields.io/badge/3D%20Engine-Three.js%20%2B%20WebGL%202.0-black?logo=threedotjs&logoColor=white)]()
[![Framework](https://img.shields.io/badge/Container-Capacitor%207%20Enterprise-119EFF?logo=capacitor&logoColor=white)]()
[![Security](https://img.shields.io/badge/Security-Hardened%20Native%20Sandbox-orange)]()
[![Cloud](https://img.shields.io/badge/Backend-Supabase%20Cloud%20Sync-3ECF8E?logo=supabase&logoColor=white)]()

## 📌 Resumen Ejecutivo
**Tavilga 3D Mobile** representa la extensión móvil nativa de alta fidelidad del configurador paramétrico de mobiliario. Empaqueta el motor de renderizado 3D reactivo (React Three Fiber / WebGL 2.0) dentro de un contenedor nativo ultraligero de **Capacitor 7**, optimizado con aceleración de hardware en GPU móvil y gestión eficiente de memoria RAM para entornos B2B y B2C en movilidad.

Este repositorio alberga la ingeniería de empaquetado, la arquitectura de ejecución y el pipeline de compilación de binarios Android (APKs / AABs) orientados a distribución empresarial.

---

## 🏗️ Arquitectura de Ejecución Móvil y Pipeline (Mermaid)

```mermaid
flowchart TD
    classDef mobile fill:#3b82f6,stroke:#fff,stroke-width:2px,color:#fff;
    classDef native fill:#10b981,stroke:#fff,stroke-width:2px,color:#fff;
    classDef engine fill:#8b5cf6,stroke:#fff,stroke-width:2px,color:#fff;
    classDef cloud fill:#f59e0b,stroke:#fff,stroke-width:2px,color:#fff;
    classDef ci fill:#6366f1,stroke:#fff,stroke-width:2px,color:#fff;

    subgraph CI_CD ["🛠️ Pipeline de Compilación Automatizada"]
        WebSource[Vite React Codebase]:::ci --> WebBuild[Optimized Web Bundle /dist]:::ci
        WebBuild --> CapSync[Capacitor Native Sync Engine]:::ci
        CapSync --> Gradle[Gradle 8.14 + OpenJDK 21]:::ci
        Gradle --> APK[Release Artifact: Tavilga3D.apk]:::ci
    end

    subgraph MobileRuntime ["📱 Runtime en Dispositivo Android"]
        APK --> Shell[Android Native App Shell - Java/Kotlin]:::native
        Shell --> WebView[Chromium Hardware-Accelerated WebView]:::mobile
        WebView --> Engine3D[Motor 3D: Three.js + WebGL 2.0]:::engine
        WebView --> State[Gestión de Estado & Cotizador Local]:::mobile
    end

    subgraph BackendSync ["☁️ Nube & Servicios Externos"]
        State <-->|TLS 1.3 / REST & WS| Supabase[(Supabase Cloud DB & Auth)]:::cloud
        State <-->|Presupuestos & Modelos| Storage[(Cloud 3D Assets CDN)]:::cloud
    end
```

---

## 📚 Estructura Estándar del Entorno de Distribución

```text
Diseno 3D APK/
├── dist/                   # Artefactos web compilados y cacheados
├── release/                # Metadatos de firma y verificación de integridad
├── .gitkeep                # Control de estructura de directorios
└── README.md               # Documentación ejecutiva de ingeniería y despliegue
```

---

## 🔐 Especificaciones Técnicas y de Seguridad

| Dimensión | Especificación Enterprise |
| :--- | :--- |
| **Arquitectura de Runtime** | Hybrid Native Container (Capacitor v7.x) |
| **API Level Objetivo** | Android 14+ (Target SDK 34 / Compatible SDK 36) |
| **Toolchain de Compilación** | Gradle 8.14.3 + Microsoft OpenJDK 21 (HotSpot) |
| **Aceleración Gráfica** | WebGL 2.0 con Context Loss Recovery y Mipmapping dinámico |
| **Seguridad de Red** | HTTPS forzado mediante esquema seguro nativo (`androidScheme: "https"`) |
| **Gestión de Memoria** | Texturas comprimidas y Garbage Collection optimizado para 3D en smartphones |