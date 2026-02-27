# 🩺 Clinical Summary Assistant

A mini **clinical copilot** web app that helps clinicians quickly review a patient’s **labs, vitals, medications, triage status, and next orders** for a small inpatient list (p1–p6). Built with **Next.js** and **Tailwind CSS**, it behaves like a lightweight EMR summary dashboard.

---

## 🌐 App Preview

![Clinical Summary Assistant UI](medical.png)

---

## ✨ Features

- 👥 **Multi‑patient support (p1–p6)**
  - Dropdown to switch between six mock patients.
  - Each patient has structured data: demographics, diagnosis, vitals, labs, and medications.

- 📋 **Patient summary card (left panel)**
  - Shows name, age, sex, diagnosis, status (Stable / Observation / Critical).
  - Key vitals and brief problem summary at a glance.

- 💬 **Assistant chat (right panel)**
  - Type questions in natural language like:
    - “Give a clinical snapshot for p3 with labs, vitals, and medications.”
    - “Show medications for p4 and mention any high‑risk drugs.”
    - “How is p2 doing right now? Include labs and vitals.”
  - The app:
    - Detects which **patient** you mean (p1–p6 / “patient 3”).
    - Infers the **intent** (snapshot, meds, summary, triage, orders).
    - Generates a concise, rules‑based answer from structured data.

- 🧪 **Labs + vitals snapshot card**
  - For “snapshot / status / labs / vitals” questions, the assistant:
    - Shows a **LabsCard** with:
      - Key labs (e.g., Hemoglobin, Creatinine).
      - Latest vitals (HR, BP, RR, SpO₂).
      - Current medications list.
    - Provides a short text summary of the patient’s status.

- 💊 **Medication card with high‑risk flags**
  - For medication‑focused questions (“meds / medications / drugs”), the assistant:
    - Shows a **MedicationsCard** with name, dose, route, frequency.
    - Highlights **high‑risk medications** (e.g., chemo, heparin, steroids, oxygen support).

- 🚦 **Quick actions (one‑click prompts)**
  - Buttons above the chat:
    - **Summarize** – short clinical summary of the current patient.
    - **Triage** – risk level, recommended care setting (OPD/Ward/ICU), and one monitoring caution.
    - **Next 2 orders** – two sensible next investigations/orders based on diagnosis.
  - These actions auto‑fill the chat, so you can demo the app quickly.

- 🔄 **Chat‑driven patient switching**
  - When you ask about a specific patient (e.g., “snapshot for p3”):
    - The app automatically switches the **left summary card** to that patient.
    - The **right panel** shows the matching Labs/Medications card and text.

- 🧱 **No real EMR / PHI**
  - All patient data is **mock** and stored locally in `patientData.ts`.
  - Safe for demos and academic use.

---

## 🧠 How it works (logic overview)

This project does **not** rely on a remote LLM at runtime.  
Instead, it uses a **local “mini‑copilot” engine**:

1. **Intent detection**
   - Simple keyword‑based function classifies each user message into:
     - `snapshot`, `meds`, `summary`, `triage`, or `orders`.
   - Snapshot keywords: `snapshot`, `labs`, `vitals`, `status`.
   - Meds keywords: `meds`, `medications`, `drug`, `medicine`.

2. **Patient resolution**
   - Parses the text for:
     - `p1`–`p6`, or
     - “patient 1”–“patient 6”.
   - If none found, defaults to the **currently active** patient from the dropdown.

3. **Clinical response generation**
   - Uses the selected patient’s structured data to build:
     - A **summary string** (demographics, diagnosis, vitals, key meds).
     - A **triage string** (low / moderate / high risk, setting, caution).
     - **Next orders** suggestions based on diagnosis (COPD, pneumonia, NSTEMI, diabetes, etc.).
   - No hallucinations: it only uses what’s present in the data.

4. **UI rendering**
   - The chat stores an array of messages with:
     - `role` (`user` / `assistant`),
     - `text`,
     - `intent`,
     - `patientId`.
   - `MessagesList`:
     - Shows the latest user + assistant messages.
     - For `intent === "snapshot"` → renders `<LabsCard />`.
     - For `intent === "meds"` → renders `<MedicationsCard />`.
   - `PatientSummaryCard` always shows the **currently active** patient; the active ID is updated whenever you ask about a new patient.

---

## 🏗️ Tech stack

- ⚛️ **React** (via Next.js App Router)  
- ▲ **Next.js** 16  
- 🎨 **Tailwind CSS** for styling  
- 🧩 Local state with `useState` and a simple `PatientContext`  
- 🧪 TypeScript types for patients, labs, vitals, and meds

---

## 🚀 Getting started

### 1. Clone the repository

```bash
git clone https://github.com/ezhilarasi0509/clinical-summary-assistant.git
cd clinical-summary-assistant
