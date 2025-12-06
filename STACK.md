# STACK.md
✅ CHEQUE Technical Stack  
Local-first, offline-capable, and fully functional in a standard web browser.

This document describes the complete system architecture for Cheque.dev, including the student app, teacher app, offline runtime, web runtime, local database, syncing, course packs, backend (optional), and development infrastructure.

---

# 1. Applications

## 1.1 Student App (Cheque Student)
Runs everywhere:
- Offline-first native apps (with optional cloud sync)
- Works as an installable PWA on Chromebooks (required for many schools)
- Full in-browser web version (online only)

### Technologies
- React + TypeScript  
- Next.js App Router  
- Capacitor (Native + PWA builds)  
- Tailwind CSS  
- i18next  
- Blockly (block-based coding)  
- Monaco Editor (JS/Python)  
- Pyodide (Python execution via Web Worker)  
- JavaScript sandbox (Web Worker)  
- SQLite  
  - Native SQLite (mobile/desktop via Capacitor)
  - SQLite WASM (web browser)  
- Drizzle ORM  
- Service Worker + Workbox (PWA)  
- Zustand or Jotai (UI state)  

### Capabilities
- Runs fully offline (native/PWA)
- Runs fully online with feature parity (browser)
- Local lesson engine
- Local code execution (JS, Python)
- Local progress tracking
- LAN/USB sync (native/PWA)
- Cloud sync (browser/native) via optional Convex layer

---

## 1.2 Teacher App (Cheque Teacher)
Acts as the "local server" in offline schools.

### Technologies
- React + TypeScript  
- Next.js + Capacitor  
- Tailwind CSS  
- i18next  
- SQLite + Drizzle ORM  
- WebRTC/WebSocket LAN sync server (native apps only) 
- Optional Convex sync adapter  

### Responsibilities
- Manage students and classes  
- Import and update course packs  
- Perform offline sync with student devices  
- Merge progress  
- Sync to cloud when online  
- Provide dashboards (local or cloud-connected)  

---

## 1.3 Admin / Authoring Dashboard
Runs online only.

### Technologies
- Next.js (web)  
- React + TypeScript  
- Tailwind CSS  
- Convex backend  
- Monaco Editor (authoring autograder code)  
- Course pack builder and publisher  

---

# 2. Shared Libraries (Monorepo Packages)

- packages/
- @cheque/ui
- @cheque/i18n
- @cheque/lesson-engine
- @cheque/runtime-js
- @cheque/runtime-python
- @cheque/local-db
- @cheque/web-db
- @cheque/sync
- @cheque/blocks
- @cheque/content-schema
- @cheque/auth-local
- @cheque/web-course-pack-loader
- @cheque/web-projects  

## 2.1 @cheque/ui
Shared components, layout primitives, and styling.

## 2.2 @cheque/i18n
Shared localization setup using i18next.

## 2.3 @cheque/blocks
Custom Blockly blocks and JS/Python generators.
Supports custom block categories for generating HTML and CSS for 
student web projects. HTML/CSS is compiled into project files that 
can be previewed locally and published online.

## 2.4 @cheque/lesson-engine
Loads, evaluates, and navigates lessons. Works offline.

## 2.5 @cheque/runtime-js
Web Worker sandbox for JavaScript with:
- timeout protection  
- no global network access  
- autograder support  

## 2.6 @cheque/runtime-python
Pyodide runtime:
- Worker-based  
- restricted environment  
- custom autograder integration  

## 2.7 @cheque/local-db
SQLite (native) wrapper using Drizzle ORM.

## 2.8 @cheque/web-db
SQLite WASM + OPFS or IndexedDB handling for the browser.

## 2.9 @cheque/sync
Implements:
- student → teacher offline sync  
- teacher → cloud sync  
- timestamp-based conflict resolution  
- sync pack creation and merging  

## 2.10 @cheque/content-schema
Validates `.cheque` course packs against strict schemas.

## 2.11 @cheque/auth-local
PIN-based local authentication system for offline schools.

## 2.12 @cheque/web-course-pack-loader
Allows `.cheque` course packs to be loaded directly in the browser.

## 2.13 @cheque/web-projects
Handles HTML/CSS/JS student projects, Blockly → code generation, 
browser previews, syncing, and publishing artifacts.

Responsibilities:
- Download pack from Convex/CDN
- Decompress (JSZip/fflate)
- Store JSON into SQLite WASM
- Store assets into OPFS/IndexedDB as Blobs
- Provide asset URLs via createObjectURL

This enables a full in-browser Cheque version.

---

# 3. Local-First Architecture

## 3.1 Local Database Options

### Native / PWA
- SQLite via Capacitor SQLite plugin
- Direct filesystem access for storing course packs

### Browser (Full Web Version)
- SQLite WASM
- Storage via OPFS (preferred) or IndexedDB
- Assets stored as Blobs  
- No direct filesystem access  
- Course packs loaded through @cheque/web-course-pack-loader  

All database queries share the same Drizzle ORM schema.

---

## 3.2 Execution Engine

### Python
- Pyodide loaded in a worker  
- Cached via service worker (PWA)  
- Loaded on demand for browser version  

### JavaScript
- Worker sandbox  
- No access to global network APIs  
- Execution timeout traps  

---

## 3.3 Lesson Engine
Shared across all platforms:
- Loads lessons from SQLite  
- Tracks student attempts  
- Validates answers locally  
- Operates fully offline  

---

## 3.4 Web Runtime Considerations (Full Browser Version)

The web version supports all features with these differences:

- No LAN/WebRTC server mode (browser security restriction)  
- Sync only through Convex (teacher dashboards, progress reporting)  
- Course packs must be loaded through the Web Course Pack Loader  
- SQLite is WASM-based and stored in OPFS  

Everything else works identically to native/PWA versions.

---

# 4. Sync Architecture

Cheque supports three modes:

## 4.1 Fully Offline Schools
- Student → Teacher sync only  
- LAN/WebRTC/PWA-native sync  
- No cloud involvement  

## 4.2 Occasionally Connected Schools
- Teacher syncs with Convex when possible  
- Offline-first locally, cloud-optimal when online  

## 4.3 Fully Online (Web Browser)
- Browser communicates only with Convex  
- Sync occurs via Convex queries/mutations  
- SQLite WASM acts as optional cache  

### Note
The browser cannot act as a WebRTC/WebSocket server, so LAN sync does not exist online.

---

# 5. Backend (Optional Cloud Layer)

Convex acts as the optional online backend.

## Responsibilities
- Online teacher/admin accounts  
- Hosting course pack metadata and downloads  
- Assignments  
- Student progress aggregation  
- Real-time dashboards  
- Version distribution
- Student project storage for public webpages (HTML/CSS/JS)
- Management of public student handles (e.g., /spencer-smith)
- Providing project data to Next.js for rendering public webpages

Convex is not required for offline-only deployments.

---

# 6. Content Pipeline (.cheque Course Packs)

## 6.1 Pack Format

- course-name-vX.cheque
- manifest.json
- lessons.json
- exercises.json
- assets/
- ...
- blocks/custom_blocks.json
- runtime/python_tests.py
- runtime/js_tests.js


## 6.2 Native/PWA Loading
- File imported via filesystem  
- Unpacked locally  
- Stored in SQLite + device storage  

## 6.3 Web Loading
Handled entirely by @cheque/web-course-pack-loader:

- Download via HTTPS  
- Decompress in memory  
- Insert JSON into SQLite WASM  
- Save assets to OPFS/IndexedDB  
- Provide URLs to runtime  
- No installation required  

---

# 7. Platform Feature Parity

| Feature | Native App | PWA | Web Browser |
|--------|------------|-----|--------------|
| Blockly | Yes | Yes | Yes |
| Monaco Editor | Yes | Yes | Yes |
| Python Execution | Yes | Yes | Yes |
| JS Sandbox | Yes | Yes | Yes |
| SQLite | Native | Native | WASM |
| Course Pack Import | Yes | Yes | Yes (via loader) |
| Offline Support | Yes | Yes | Limited (requires PWA install) |
| LAN Sync | Yes | No | No |
| Cloud Sync | Optional | Optional | Yes |
| Teacher Dashboard | Local or Cloud | Local or Cloud | Cloud |

---

# 8. Deployment Models

## 8.1 Browser Version (Online Only)
- Full feature set  
- No installation required  
- Cloud sync only  

## 8.2 PWA Version (Critical for Chromebook Support)
The PWA exists primarily to support schools that use Chromebooks.

ChromeOS does not allow installing native apps, but PWAs:
- can be installed as standalone applications
- work offline using service workers
- store data in SQLite WASM + OPFS
- support the full Cheque experience in a browser-controlled environment

Limitations:
- No LAN sync (browsers cannot run native Capacitor plugins)
- No native SQLite engine (uses SQLite WASM instead)
- No access to native filesystem or network APIs

The PWA is ideal for Chromebook-heavy deployments, while native apps provide the full offline + LAN sync experience. 

## 8.3 Native App Version
- Full offline-first  
- Largest storage support  
- LAN sync  
- Course packs import via filesystem  

---

# 9. Summary

Cheque runs in:
- Full browser environments  
- PWAs  
- Native apps  
- Offline schools  
- Online schools  
- Mixed environments  

Core components:
- React + Next.js  
- Blockly + Monaco  
- Pyodide + JS Sandbox  
- SQLite (native or WASM)  
- Web Course Pack Loader  
- Local lesson engine  
- Offline or cloud sync  
- Optional Convex online backend  

# 10. Student Webpages (Publishing System)

Cheque supports optional public-facing student webpages such as:

  username.cheque.dev

These pages are generated from projects created inside the Cheque environment.

## Project Flow
1. Student builds project using custom HTML/CSS blocks and/or Monaco.
2. Project files are stored locally in SQLite.
3. If online, project files sync to Convex.
4. Next.js renders webpages dynamically under the route /[studentHandle].

## Rendering
- Dynamic SSR via Next.js
- Optional static generation for performance
- Asset hosting via Convex file storage

## Offline Behavior
- Students may edit projects offline.
- Publishing requires an online session.

This architecture ensures Cheque works everywhere:  
from offline village schools to fully connected web deployments.

# Architecture Diagram

``` mermaid

flowchart TB
    %% ========================
    %% COLOR DEFINITIONS
    %% ========================
    classDef app fill:#2d3748,stroke:#e2e8f0,color:white
    classDef runtime fill:#4a5568,stroke:#e2e8f0,color:white
    classDef db fill:#2b6cb0,stroke:#bee3f8,color:white
    classDef sync fill:#6b46c1,stroke:#d6bcfa,color:white
    classDef cloud fill:#2f855a,stroke:#9ae6b4,color:white
    classDef public fill:#b7791f,stroke:#fbd38d,color:white
    classDef pack fill:#805ad5,stroke:#d6bcfa,color:white
    classDef auth fill:#b83280,stroke:#fbb6ce,color:white

    %% ========================
    %% CLIENT APPLICATIONS
    %% ========================

    subgraph StudentApp["Student App"]
        SA1["Native App\n(Android / iOS / Desktop)"]
        SA2["PWA\n(Chromebooks)"]
        SA3["Web Browser\n(Online Only)"]
    end
    class StudentApp app

    subgraph TeacherApp["Teacher App"]
        TA1["Native App\n(LAN Sync Host)"]
        TA2["PWA\n(Limited)"]
    end
    class TeacherApp app

    subgraph AdminDashboard["Admin / Authoring Dashboard"]
        AD1["Next.js Web App"]
    end
    class AdminDashboard app

    %% ========================
    %% AUTH
    %% ========================

    subgraph Auth["Local Authentication"]
        AUTH1["PIN-based Login\n(@cheque/auth-local)"]
    end
    class Auth auth

    %% ========================
    %% LOCAL RUNTIME
    %% ========================

    subgraph LocalEngines["Local Runtime Layer"]
        LE1["Lesson Engine"]
        LE2["JS Runtime\n(Web Worker Sandbox)"]
        LE3["Python Runtime\n(Pyodide Worker)"]
        LE4["HTML/CSS Project Renderer\n(iframe sandbox)"]
    end
    class LocalEngines runtime

    %% ========================
    %% DATABASES
    %% ========================

    subgraph LocalDB["Local Database Layer"]
        DB1["SQLite Native\nvia Capacitor"]
        DB2["SQLite WASM\nvia OPFS / IndexedDB"]
    end
    class LocalDB db

    %% ========================
    %% COURSE PACKS
    %% ========================

    subgraph CoursePacks["Course Packs (.cheque)"]
        CP1["Native File Import"]
        CP2["Web Loader\n(JSZip + OPFS)"]
        CPV["Content Schema Validator\n(@cheque/content-schema)"]
    end
    class CoursePacks pack

    %% ========================
    %% SYNC SYSTEM
    %% ========================

    subgraph SyncSystem["Sync System"]
        SY1["Student → Teacher\nLAN / WebRTC Sync"]
        SY2["Teacher → Cloud\nSync Adapter"]
        SY3["Browser → Cloud\nConvex API"]
        SY4["Sync Merge Engine\n(Conflict Resolution)"]
    end
    class SyncSystem sync

    %% ========================
    %% BACKEND
    %% ========================

    subgraph ConvexCloud["Convex Cloud (Optional)"]
        C1["User Accounts\nTeacher / Admin"]
        C2["Course Pack Metadata\n+ Downloads"]
        C3["Assignments & Classes"]
        C4["Realtime Dashboards"]
        C5["Student Progress Store"]
        C6["Student Webpages Data\n(HTML/CSS/JS)"]
    end
    class ConvexCloud cloud

    %% ========================
    %% PUBLIC WEBPAGES
    %% ========================

    subgraph PublicWebpages["Public Student Webpages"]
        PW1["Next.js SSR / ISR Renderer"]
        PW2["username.cheque.dev"]
    end
    class PublicWebpages public

    %% ========================
    %% CONNECTIONS
    %% ========================

    %% Student App → Auth
    SA1 --> AUTH1
    SA2 --> AUTH1
    SA3 --> AUTH1

    %% Teacher App → Auth
    TA1 --> AUTH1
    TA2 --> AUTH1

    %% Student App → DB
    SA1 --> DB1
    SA2 --> DB2
    SA3 --> DB2

    %% Student App → Course Packs
    SA1 --> CP1
    SA2 --> CP2
    SA3 --> CP2

    %% Course Packs Validator
    CP1 --> CPV
    CP2 --> CPV
    CPV --> LocalDB
    CPV --> LocalEngines

    %% Student App → Engines
    SA1 --> LE1
    SA1 --> LE2
    SA1 --> LE3
    SA1 --> LE4
    SA2 --> LE1
    SA2 --> LE2
    SA2 --> LE3
    SA2 --> LE4
    SA3 --> LE1
    SA3 --> LE2
    SA3 --> LE3
    SA3 --> LE4

    %% Student LAN Sync
    SA1 --> SY1
    SA2 -. No LAN Sync .- SY1
    SA3 -. Browser Cannot Sync LAN .- SY1

    %% Student Cloud Sync
    SA1 --> SY2
    SA2 --> SY2
    SA3 --> SY3

    %% Teacher App
    TA1 --> DB1
    TA2 --> DB2
    TA1 --> SY1
    TA1 --> SY2
    TA2 --> SY2

    %% Sync Merge Engine
    SY1 --> SY4
    SY2 --> SY4
    SY3 --> SY4

    %% Cloud connections
    SY2 --> ConvexCloud
    SY3 --> ConvexCloud

    %% Admin Dashboard → Cloud
    AD1 --> ConvexCloud

    %% Cloud → Public Webpages
    ConvexCloud --> PW1
    PW1 --> PW2
