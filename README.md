<p align="center">
  <img src="assets/logo.png" alt="Cheque Logo" width="180"/>
</p>

<h1 align="center">Cheque</h1>
<h3 align="center">Bilingual, Offline-Ready Programming Education Platform</h3>

---

## 🌍 Overview

**Cheque** is an open-source platform for learning programming with a **bilingual (English/Spanish)** interface, a **block-to-text hybrid editor**, and robust **offline support**.  
It is specifically designed for students in regions with:

- Limited or unreliable internet  
- Limited access to computer science education  
- A need for Spanish-first learning materials  
- Low-resource devices (old PCs, shared tablets, Android phones)

Cheque provides:

- A drag-and-drop editor built with **Blockly**  
- Real editable code (JavaScript/Python) synchronized with blocks  
- One-click **natural language switching** (English ↔ Spanish)  
- **Offline-first architecture** for low-connectivity environments  
- **Teacher dashboards**: classes, assignments, and performance tracking  
- **Localized curriculum** designed for Latin American learners  

Cheque aims to give underserved communities meaningful access to programming education — not just block coding, but real-world coding skills.

---

## 🚀 What Makes Cheque Different

### 🔹 Bilingual UI (English ↔ Spanish)
All UI elements, block labels, tooltips, and instructions change instantly between languages.

### 🔹 Blocks + Real Code Together
Students can see and edit real code at any moment, enabling a smooth transition from visual coding → text coding.

### 🔹 Offline-First
Cheque is designed to continue working even when internet access drops.

Key offline features:
- Cached lessons  
- Local sandbox execution  
- Local progress storage  
- Syncing when connection returns  

### 🔹 Curriculum Relevant to Latin America
Lessons will be culturally contextualized, helping students relate programming to real problems in their own environment.

### 🔹 Open Source & Extensible
Unlike closed educational platforms:

- Anyone can add lessons  
- Anyone can improve translation  
- Anyone can create new block sets  
- Anyone can contribute to the teacher dashboard  
- Anyone can adapt Cheque for their region  

---

## 🧩 Why Cheque Is Necessary

### ❗1. No existing platform bridges blocks → real code well
Students learn Scratch/Code.org (blocks)  
then are expected to jump into Python or JavaScript.

Most fail at that transition.

Cheque provides the missing bridge:
> **Blocks ↔ Text ↔ Real Projects**, in English and Spanish.

---

### ❗2. Current tools assume strong internet
In many Latin American schools:
- WiFi drops in and out  
- Bandwidth is extremely limited  
- Students rely on cheap Android devices  

Code.org, Khan Academy, and MakeCode all fail in these conditions.

Cheque works **offline-first**.

---

### ❗3. Lack of relevant, Spanish-first programming curriculum
Most CS education tools:
- Are written in English  
- Use American examples  
- Assume American school environments  

Cheque is built from the ground up for **Spanish-speaking students**.

---

### ❗4. Teachers need simpler, more practical tools
Cheque will offer:
- Simple class administration  
- Low-bandwidth dashboards  
- Offline progress viewing  
- Optional WhatsApp notifications  
- Printable worksheets  

---

### ❗5. No open-source project combines these strengths
There is currently *no free tool* that is:

- Bilingual  
- Block-to-text hybrid  
- Offline-ready  
- Teacher-friendly  
- Fully open source  

Cheque fills that gap.

---

## 🌟 Expected Impact

### Students:
- Learn in Spanish  
- Edit real JavaScript/Python code  
- Progress naturally from blocks to text  
- Keep learning even when offline  
- Build skills useful for future employment  

### Teachers:
- Use a free and simple tool  
- Assign lessons even with low bandwidth  
- Track student progress  
- Teach programming without needing specialized training  

### Schools:
- No licensing fees  
- Works with older technology  
- Provides long-term progression beyond block coding  

---

## 🛠 Technical Overview

Cheque will be built on:

- **Blockly** for visual programming  
- A custom **AST synchronization layer** (blocks ↔ JS/Python)  
- **Service Workers + IndexedDB** for offline modes  
- **Pyodide** for local Python execution  
- Built-in JavaScript VM for JS sandboxing  
- **React/Next.js** (or SvelteKit) for the frontend  
- Optional backend (Convex / Supabase) for teacher dashboards  
- Modular lesson format for easy contributions  

---

## 🤝 How to Contribute

We welcome developers with experience in:

- TypeScript / React  
- Blockly  
- Offline-first app architecture  
- AST transformations / compilers  
- Python/JavaScript runtimes  
- UI/UX for educational apps  
- Spanish/English localization  
- Writing curriculum  

All contributions are welcome — Cheque is **community-driven**.

---

## 📣 Mission Statement

> Cheque exists to bring real programming education to students who currently lack access —  
> not because they lack talent, but because they lack the infrastructure, tools, or Spanish-first curriculum needed to succeed.

Together, we can empower a new generation of programmers across Latin America and beyond.

---

## 📬 Get Involved

If you want to help build Cheque, contribute ideas, or join the development, open an issue or reach out!

Let’s build something that truly expands access to programming education.
