# Product Requirements Document (PRD)
## Calculee — Tocris Dilution Calculator & Lab Solution Suite

### 1. Document Overview
* **Product Name:** Tocris Dilution Calculator (Calculee)
* **Website URL:** [https://calculee.io/](https://calculee.io/)
* **Document Version:** 2.0
* **Target Audience:** Biomedical researchers, pharmacologists, biochemists, cell biologists, laboratory technicians, graduate students, and academic institutions.
* **Core Value Proposition:** Fast, zero-friction, privacy-first laboratory dilution calculations with mathematical validation, automated unit normalization, live visual liquid representations, serial dilution generators, and copyable bench protocols.

---

### 2. Executive Summary & Goals
Calculee provides an intuitive, high-precision laboratory calculation tool built to eliminate human mathematical errors in pharmacology, molecular biology, and biochemistry research. The tool specializes in:
1. **Algebraic $C_1V_1 = C_2V_2$ Single Dilutions:** Solving for any unknown variable (stock volume $V_1$, stock concentration $C_1$, final volume $V_2$, final concentration $C_2$) with instant cross-unit normalization.
2. **Serial Dilution Generators:** Generating multi-tube or multi-well plate preparation schemes for dose-response curves ($IC_{50}$ / $EC_{50}$ assays) with Excel-exportable tabular data.
3. **Powder Reconstitution & Molecular Weight (MW) Conversions:** Enabling mass-to-volume and molarity interconversions with vehicle safety sanity checks (e.g. DMSO threshold $< 0.1\%$ v/v).
4. **Zero-Latency Client-Side Computation:** 100% in-browser processing ensuring zero proprietary formula leakage and HIPAA/GDPR laboratory data privacy.

---

### 3. User Personas

| Persona | Role | Key Pain Points | Desired Outcome |
|---|---|---|---|
| **Dr. Elena Chen** | Senior Pharmacologist | Small-volume pipetting errors (<0.5 µL) leading to assay variance; DMSO cytotoxicity artifacts. | Fast 2-step intermediate dilution workflows, exact solvent volume calculation, and vehicle % sanity checks. |
| **Marcus Vance** | High-Throughput Screening Tech | Manually typing 10-point serial dilution tables into Excel takes too much time and risks typos. | One-click serial dilution table generation with instant copy-to-clipboard (TSV / Excel ready). |
| **Amina Patel** | Graduate Student / Lab Trainee | Confused by unit conversions between $\mu\text{M}$, $\text{mM}$, $\text{mg/mL}$, and percentage solutions. | Clear step-by-step bench protocols with automated unit handling and graphical beaker visualizers. |

---

### 4. Technical Architecture & Design System

* **Frontend Architecture:** Clean semantic HTML5, Vanilla JavaScript (ES6+ modular closures), Responsive CSS3.
* **Design Language:** Apple (España) Minimalist Design System:
  * **Typography:** SF Pro Display (Headings, 600-700 weight), SF Pro Text (Body, 400-500 weight), tabular numbers.
  * **Color Palette:**
    * Canvas Paper: `#ffffff`
    * Canvas Section Alternate: `#f5f5f7`
    * Primary Ink: `#1d1d1f`
    * Secondary Mid-Gray: `#707070`
    * Primary Accent: Electric Blue `#0071e3` / Hover `#0077ed`
    * Warning/Notice: Ember `#b64400` / Success `#065f46` (Bg: `#d1fae5`)
  * **Border Radii:** 28px cards, 980px pill buttons, borderless flat surfaces with alternating background rhythm.
* **SEO & Metadata:** JSON-LD WebApplication schema, FAQPage schema, OpenGraph, Twitter Cards, canonical tags, XML sitemap, and robots.txt.

---

### 5. Functional Requirements & Feature Specifications

#### 5.1 Single Dilution ($C_1V_1 = C_2V_2$) Algebraic Solver
* **FR-1.1 Variable Solving:** The system must allow users to designate any of the 4 parameters ($V_1$, $C_1$, $V_2$, $C_2$) as the target variable to be computed.
* **FR-1.2 Multi-Unit Concentration Normalization:**
  * Supported Concentration Units: $\text{M}$ (Molar), $\text{mM}$ (Millimolar), $\mu\text{M}$ (Micromolar), $\text{nM}$ (Nanomolar), $\text{pM}$ (Picomolar), $\text{mg/mL}$, $\mu\text{g/mL}$, $\text{ng/mL}$, and $\%$ (w/v).
* **FR-1.3 Multi-Unit Volume Normalization:**
  * Supported Volume Units: $\text{L}$ (Liters), $\text{mL}$ (Milliliters), $\mu\text{L}$ (Microliters), $\text{nL}$ (Nanoliters).
* **FR-1.4 Molecular Weight (MW) Interconversion:**
  * When converting between molarity and mass/volume units, the system must allow input of solute Molecular Weight ($\text{g/mol}$) and convert seamlessly.
* **FR-1.5 Real-Time Validation & Alerts:**
  * Infeasible scenarios (e.g. $V_1 > V_2$ or $C_2 > C_1$ or division by zero) must trigger instant amber warning alerts with clear explanations without breaking the app state.
* **FR-1.6 Output & Recipe Generation:**
  * Calculates exact required Stock Solute Volume ($V_1$), Diluent / Buffer Volume ($V_{\text{diluent}} = V_2 - V_1$), Dilution Factor (DF / $X$-fold), and Total Volume ($V_2$).
  * Generates a 3-step actionable laboratory bench protocol.
  * Live updates a graphical beaker visualizer showing dynamic liquid height and solute-to-solvent percentage ratio.
* **FR-1.7 Export & Clipboard Actions:**
  * "Copy Protocol" button formats recipe into clear text and copies to system clipboard with toast confirmation.
  * "Print Protocol" triggers clean browser print view.

#### 5.2 Serial Dilution Series Generator
* **FR-2.1 Flexible Input Parameters:**
  * Master Stock Concentration ($C_{\text{stock}}$) & unit.
  * Dilution Step Factor ($DF$, e.g., 2-fold, 3-fold, 5-fold, 10-fold).
  * Target Working Volume per Tube ($V_{\text{target}}$) & unit.
  * Total Steps / Wells ($N$, from 2 up to 24 tubes).
* **FR-2.2 Mathematical Engine:**
  * Step Transfer Volume: $V_{\text{transfer}} = \frac{V_{\text{target}}}{DF - 1}$
  * Diluent Volume: $V_{\text{diluent}} = V_{\text{target}}$
  * Step Concentration: $C_n = \frac{C_{n-1}}{DF}$
  * Cumulative Dilution: $1 : (DF^{n-1})$
* **FR-2.3 Tabular Results Display:**
  * Shows Tube #, Transfer Source & Volume, Diluent Volume, Total Mix Volume, Resulting Concentration, and Cumulative Dilution Ratio.
* **FR-2.4 TSV / Excel Copy Export:**
  * Single-click copy formatted tab-separated values (TSV) ready to paste into Microsoft Excel, Google Sheets, or Prism GraphPad.

#### 5.3 One-Click Lab Solution Presets
* **FR-3.1 Pre-built Configurations:**
  * 10 mM DMSO Stock $\rightarrow$ 10 µM Assay Working Solution (1:1,000 dilution).
  * 100 mM Standard $\rightarrow$ 1 mM Working Standard (1:100 dilution).
  * 100 µM Oligo/Primer Stock $\rightarrow$ 10 µM PCR Working Solution (1:10 dilution).
* **FR-3.2 Preset Activation:**
  * Clicking any preset card populates all fields, selects the correct units, computes results, and smoothly scrolls to the active calculator.

#### 5.4 Educational & Protocol Guidelines
* **FR-4.1 Worked Examples:** Detailed 2-phase dilution cascade diagrams for small molecule handling (master stock $\rightarrow$ intermediate stock $\rightarrow$ assay plate).
* **FR-4.2 DMSO Cytotoxicity Threshold Guide:** Bench decision limits ensuring $<0.1\%$ v/v vehicle concentration.
* **FR-4.3 Interactive FAQ Accordion:**
  * Includes questions on $C_1V_1=C_2V_2$, dry powder reconstitution, intermediate dilutions, DMSO thresholds, serial dilution geometry, and frozen stock storage.
  * Includes "Expand All" and "Collapse All" master toggles.

#### 5.5 Static Informational & Compliance Pages
* **FR-5.1 About Page (`about.html`):** Mission, scientific methodology, calculation integrity.
* **FR-5.2 Contact Page (`contact.html`):** Scientific inquiry form and support details.
* **FR-5.3 Legal Pages:** Privacy Policy (`privacy.html`), Terms of Service (`terms.html`), and Scientific Disclaimer (`disclaimer.html`).

---

### 6. Non-Functional Requirements (NFRs)

1. **Performance & Speed:** Zero network latency for calculations; sub-second page load times ($< 0.5\text{s}$ First Contentful Paint).
2. **Privacy & Security:** 100% client-side execution. No proprietary reagent names, molecular weights, or compound concentrations are transmitted to any remote servers.
3. **Cross-Platform Responsiveness:** Full viewport adaptation across mobile ($320\text{px}+$), tablet ($768\text{px}$), laptop ($1024\text{px}$), and wide desktop displays ($1200\text{px}+$) with minimum 42px touch targets on mobile.
4. **Accessibility (a11y):** High contrast typography, clear ARIA states for tabs (`aria-selected`), label-input associations, and keyboard navigability.
5. **Browser Compatibility:** Support for Chrome, Safari, Edge, Firefox, iOS Safari, and Android Chrome.

---

### 7. Success Metrics & KPIs
* **Calculation Accuracy:** 100% algebraic fidelity verified across float boundaries.
* **User Engagement:** High protocol copy/print rate and low bounce rate on mobile and desktop.
* **Organic Search Visibility:** Page 1 ranking for primary lab keywords (*"Tocris dilution calculator"*, *"C1V1 calculator"*, *"serial dilution calculator"*).
