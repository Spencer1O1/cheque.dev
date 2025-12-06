<p align="center">
  <img src="assets/logo.png" alt="Logo de Cheque" width="180"/>
</p>

<h1 align="center">Cheque</h1>
<h3 align="center">Plataforma Bilingüe y Offline para Aprender Programación</h3>

---

## 🌍 Descripción General

**Cheque** es una plataforma de código abierto para aprender programación con una interfaz **bilingüe (inglés/español)**, un editor híbrido de **bloques y código real**, y soporte robusto **offline**.  
Está diseñada especialmente para estudiantes en regiones con:

- Internet limitado o inestable  
- Poco acceso a educación formal en ciencias de la computación  
- Necesidad de materiales educativos en español  
- Dispositivos de bajos recursos (PCs antiguas, tabletas, teléfonos Android)

Cheque ofrece:

- Un editor visual basado en **Blockly**  
- Código real editable (JavaScript/Python) sincronizado con los bloques  
- Cambio de idioma **instantáneo** (inglés ↔ español)  
- Arquitectura **offline-first**  
- Paneles para docentes (clases, tareas, seguimiento de progreso)  
- Currículo contextualizado para estudiantes hispanohablantes  

El objetivo es abrir la puerta a una educación en programación accesible, útil y adaptable en regiones donde hoy no existe una solución adecuada.

---

## 🚀 Qué Hace Único a Cheque

### 🔹 Interfaz Bilingüe (Inglés ↔ Español)
Todos los textos, etiquetas, descripciones y mensajes cambian de idioma con un solo clic.

### 🔹 Bloques + Código Real
Los estudiantes pueden ver y editar código **real** en todo momento, permitiendo una transición suave de programación visual → programación profesional.

### 🔹 Funciona Sin Conexión
Cheque está diseñado para seguir funcionando aun cuando el internet falle.

Incluye:
- Lecciones descargables  
- Cacheo local  
- Ejecución local de código en sandbox  
- Sincronización automática cuando regrese la conexión  

### 🔹 Currículo Relevante para Latinoamérica
Ejemplos y actividades diseñados para estudiantes hispanohablantes, con un enfoque culturalmente apropiado y cercano a su realidad.

### 🔹 Código Abierto y Extensible
La comunidad puede:
- Crear nuevas lecciones  
- Añadir bloques personalizados  
- Mejorar la traducción  
- Extender el soporte a nuevos lenguajes  
- Adaptar Cheque a distintas regiones  

---

## 🧩 Por Qué Cheque Es Necesario

### ❗1. No existe una transición efectiva entre bloques → código real
Los estudiantes usan Scratch/Code.org (bloques),  
pero cuando pasan a Python o JavaScript, la mayoría se queda atrás.

Cheque crea el puente que falta:
> **Bloques ↔ Texto ↔ Proyectos Reales**, en inglés y español.

---

### ❗2. Las plataformas actuales requieren internet estable
En muchas escuelas de Latinoamérica:
- El internet es lento  
- Se comparte un solo hotspot  
- Los dispositivos son económicos  
- El servicio falla constantemente  

Code.org, Khan Academy y MakeCode simplemente no funcionan bien en esas condiciones.

Cheque está diseñado **offline-first**.

---

### ❗3. No hay currículo de programación en español diseñado para la región
El contenido existente:
- Está en inglés  
- Usa ejemplos poco relevantes  
- Asume escuelas con muchos recursos  

Cheque se enfoca desde el inicio en **pedagogía en español**.

---

### ❗4. Los docentes necesitan herramientas más simples
Cheque ofrecerá:
- Paneles de clase de bajo ancho de banda  
- Tareas simples de asignar  
- Progreso visible aun sin conexión  
- Hojas imprimibles  
- Notificaciones opcionales por WhatsApp  

---

### ❗5. No hay una alternativa open-source con estas capacidades
No existe ninguna herramienta gratuita que sea:

- Bilingüe  
- Bloques ↔ texto sincronizados  
- Funcional sin internet  
- Amigable para docentes  
- Basada en lenguajes de programación reales  
- 100% abierta  

Cheque llena ese hueco.

---

## 🌟 Impacto Esperado

### Para Estudiantes:
- Aprender programación en español  
- Ver y modificar código real  
- Transición suave de bloques hacia texto  
- Continuar aprendiendo sin internet  
- Adquirir habilidades para el futuro laboral  

### Para Docentes:
- Herramienta gratuita y sencilla  
- Seguimiento de progreso incluso sin conexión  
- Asignación de tareas de bajo consumo de datos  
- Contenido adaptado a su contexto  

### Para Escuelas:
- Cero licencias  
- Funciona en dispositivos antiguos  
- Va más allá de la programación básica por bloques  

---

## 🛠 Resumen Técnico

Cheque se construirá utilizando:

- **Blockly** como editor visual  
- Una capa de sincronización **AST** (bloques ↔ Python/JS)  
- **Service Workers + IndexedDB** para modo offline  
- **Pyodide** para ejecutar Python localmente  
- Motor de JavaScript en sandbox para ejecución local  
- **React/Next.js** o SvelteKit como frontend  
- Backend opcional (Convex / Supabase) para docentes  
- Formato modular de lecciones para colaboraciones  

---

## 🤝 Cómo Contribuir

Buscamos desarrolladores con experiencia en:

- TypeScript / React  
- Blockly  
- Arquitecturas offline-first  
- AST / compiladores  
- Runtimes de Python/JS  
- Diseño UX/UI educativo  
- Localización español/inglés  
- Creación de contenido educativo  

Cualquier colaboración es bienvenida. Cheque es un proyecto comunitario.

---

## 📣 Misión

> _Cheque existe para llevar educación en programación a estudiantes que hoy no pueden acceder a ella — no por falta de capacidad, sino por falta de infraestructura, materiales en su idioma y herramientas adecuadas._

Construyamos juntos algo que pueda cambiar vidas.

---

## 📬 Participa

Si deseas colaborar, contribuir ideas o unirte al desarrollo, abre un issue o contáctanos.

Hagamos de Cheque una plataforma que amplíe el acceso a la educación en programación en todo el mundo hispanohablante.
