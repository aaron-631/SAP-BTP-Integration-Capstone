# SAP BTP Integration Suite — Employee Data Integration

**Capstone Project | SAP Industrial Elective – BTP Developer | KIIT University**

---

## 👤 Student Details

| Field | Details |
|-------|---------|
| **Name** | Aaron Chakraborty |
| **Roll Number** | 2305671 |
| **Email** | 2305671@kiit.ac.in |
| **Program** | SAP Industrial Elective – BTP Developer |
| **University** | KIIT University, Bhubaneswar |
| **Batch / Year** | 2025–26 |
| **Submitted** | April 2026 |
| **Guided By** | ExcelR Edtech – SAP Trainer |

---

## 📋 Problem Statement

In modern enterprises, HR systems must share employee data seamlessly across payroll engines, ERP platforms, and self-service portals. Manual transfers introduce inconsistencies, delays, and compliance risks. The **Hire-to-Retire (H2R)** process — spanning onboarding, role changes, transfers, and exit — demands real-time, automated synchronisation at every stage.

Without robust integration middleware, organisations face data silos and operational bottlenecks that directly impact workforce management and downstream system accuracy.

---

## 💡 Proposed Solution

This project implements a complete **end-to-end SAP BTP Integration Suite** scenario simulating an employee onboarding data flow from an HR source system to downstream enterprise platforms.

The iFlow **`Employee_Data_Fetch`** exposes a secure HTTPS endpoint, fetches live employee data from an external REST API, applies field-level transformation via Content Modifier and Groovy Script, and returns a structured JSON payload — with a built-in Exception Subprocess handling failures gracefully.

The architecture mirrors real **SAP BTP → SuccessFactors** and **S/4HANA** enterprise integration patterns.

---

## ⚙️ Key Features

| Feature | Description |
|---------|-------------|
| **HTTPS Trigger** | Secure inbound endpoint (`/employeedata`) accepts integration requests from upstream systems |
| **Real-time Data Fetch** | HTTP GET to JSONPlaceholder REST API returns live structured employee JSON payload |
| **Data Transformation** | Content Modifier structures payload; Groovy Script extracts key fields into clean output JSON |
| **Exception Handling** | Exception Subprocess returns structured error JSON with H2R-Onboarding process context on failure |
| **Cloud-Native Deployment** | Runs entirely on SAP BTP Cloud Foundry — no on-premise infrastructure required |
| **Request-Reply Pattern** | Industry-standard synchronous SAP integration pattern with guaranteed API response |

---

## 🏗️ Architecture

```
[External System / HR Platform]
        |
        | HTTPS POST → /employeedata
        ↓
┌────────────────────────────────────────────┐
│           SAP BTP Integration Suite         │
│                                             │
│  [Sender(HTTPS)] → [Start]                  │
│       ↓                                     │
│  [Fetch_Employee_API - Request Reply]        │
│       ↓  HTTP GET → JSONPlaceholder API     │
│  [Content Modifier 2]                       │
│       ↓                                     │
│  [Transform_Employee - Groovy Script]        │
│       ↓                                     │
│  [End] → [Receiver(HTTPS)]                  │
│                                             │
│  ── Exception Subprocess ──────────────     │
│  [Error Start] → [Content Modifier] → [End] │
│  Returns: { "status":"ERROR",               │
│    "process":"H2R-Onboarding" }             │
│                                             │
│  iFlow: Employee_Data_Fetch                 │
│  Package: KIIT_BTP_Capstone                 │
└────────────────────────────────────────────┘
        |
        ↓
https://jsonplaceholder.typicode.com/users/1
[JSONPlaceholder REST API — Employee JSON Response]
```

---

## 🔄 End-to-End Flow

**Step 1 – Trigger:** An HR system sends an HTTPS request to `/employeedata` on SAP BTP Cloud Foundry, simulating an H2R onboarding event.

**Step 2 – Fetch:** `Fetch_Employee_API` (Request Reply) calls JSONPlaceholder via HTTP Adapter; receives raw employee JSON with name, email, address, and company fields.

**Step 3 – Transform:** Content Modifier 2 structures the payload. Groovy Script (`Transform_Employee`) extracts required fields and formats a clean, standardised JSON aligned to downstream expectations.

**Step 4 – Response:** End Event returns the transformed, validated JSON to the calling system, completing the synchronous integration cycle.

**Step 5 – Error:** If any step fails, Exception Subprocess fires — Content Modifier returns:
```json
{
  "status": "ERROR",
  "message": "Employee data fetch failed",
  "process": "H2R-Onboarding",
  "handler": "SAP-BTP-Exception-Subprocess"
}
```

---

## 🛠️ Technology Stack

| Technology | Version | Role in Project |
|------------|---------|-----------------|
| SAP BTP | Trial | Cloud platform — subaccounts, services, runtime |
| SAP Integration Suite | Trial Plan | Integration middleware — iFlow design, deployment, runtime |
| Cloud Integration | Trial Tenant | Design, configure, deploy, and monitor integration flows |
| HTTPS Adapter | Built-in | Inbound HTTPS trigger endpoint |
| HTTP Adapter | Built-in | Outbound HTTP GET to external REST API |
| Content Modifier | Built-in | Structures and enriches the employee data payload mid-flow |
| Groovy Script | Built-in | Field-level data extraction and clean JSON output formatting |
| JSONPlaceholder API | Public REST | External employee data source — `GET /users/1` |
| SAP BTP Cockpit | Web Console | Subaccount management, role assignment, service subscriptions |
| SAP Business App Studio | Trial | Cloud IDE for BTP development and iFlow configuration |

---

## 📁 Repository Structure

```
SAP-BTP-Integration-Capstone/
│
├── README.md                                  ← This file
├── Aaron_Chakraborty_SAP_BTP_Capstone.pdf     ← Project documentation PDF
│
└── screenshots/
    ├── 01_btp_cockpit.png                     ← SAP BTP Trial Cockpit
    ├── 02_integration_suite.png               ← Integration Suite capabilities
    ├── 03_roles_assigned.png                  ← Role collections assigned
    ├── 04_iflow_canvas.png                    ← iFlow design canvas
    └── 05_monitor_deployment.png              ← Monitor deployment screen
```

---

## 🚀 Setup & Deployment Steps

### Prerequisites
- SAP BTP Trial account (free at [sap.com/trial](https://www.sap.com/trial))
- Integration Suite subscribed with Cloud Integration capability activated
- Role collections assigned: `Integration_Provisioner`, `PI_Administrator`, `PI_Integration_Developer`

### Step 1 — Create Integration Package
1. Open Integration Suite → Design → Integrations and APIs
2. Click **Create** → Fill Name: `KIIT_BTP_Capstone`
3. Click **Save**

### Step 2 — Create iFlow
1. Inside the package → Click **Edit** → **Add** → **Integration Flow**
2. Name: `Employee_Data_Fetch`
3. Click **OK** to open the canvas

### Step 3 — Configure HTTPS Sender
1. Hover over Sender → drag arrow to Start → select **HTTPS** adapter
2. Connection tab → Address: `/employeedata`
3. Uncheck **CSRF Protected** → Save

### Step 4 — Add Request Reply Step
1. Search "Request Reply" in step search → place between Start and End
2. Connect Request Reply → Receiver → select **HTTP** adapter
3. Connection tab settings:
   - Address: `https://jsonplaceholder.typicode.com/users/1`
   - Method: `GET`
   - Authentication: `None`

### Step 5 — Deploy
1. Click **Save** → Click **Deploy**
2. Monitor → All Integration Flows → wait for status: `Started`

---

## 📤 Sample API Response

```json
{
  "id": 1,
  "name": "Leanne Graham",
  "username": "Bret",
  "email": "Sincere@april.biz",
  "address": {
    "street": "Kulas Light",
    "city": "Gwenborough"
  },
  "phone": "1-770-736-8031 x56442",
  "company": {
    "name": "Romaguera-Crona"
  }
}
```

---

## ⭐ Unique Points

- **Cloud-Native, Zero On-Premise** — Entire solution on SAP BTP Cloud Foundry; no local SAP install, instantly scalable, fully managed.
- **Full Integration Stack** — Implements all enterprise layers in one iFlow: HTTPS trigger, Request-Reply fetch, Content Modifier + Groovy Script transformation, Exception Subprocess error handling.
- **Built-in Resilience** — Structured error JSON with explicit H2R-Onboarding process context enables graceful downstream failure handling without manual intervention.
- **Production-Ready Pattern** — Request-Reply + HTTPS/HTTP is the exact pattern used in live SAP BTP → S/4HANA and SuccessFactors production integrations.
- **Extensible Without Redesign** — OAuth 2.0, S/4HANA OData, and audit logging can be added as independent modular components; no core iFlow changes needed.

---

## 🔮 Future Improvements

| # | Improvement | Description |
|---|-------------|-------------|
| 1 | **OAuth 2.0 Security** | Secure HTTPS endpoint with client credentials for production-grade access control |
| 2 | **Message Mapping** | Transform JSON to SAP Doc/XML format for S/4HANA compatibility |
| 3 | **S/4HANA OData Connection** | Replace mock API with real SAP S/4HANA HR OData service for live enterprise data |
| 4 | **SuccessFactors Connect** | Connect to SAP SuccessFactors Employee Central for actual H2R lifecycle events |
| 5 | **Audit Logging** | Trace-level logging and monitoring dashboards for compliance and visibility |
| 6 | **Bulk / Batch Processing** | Paginated API calls to synchronise full employee datasets in batch mode |
| 7 | **Content-Based Routing** | Route data to different targets based on department, role, or employment type |

---

## 📄 Documentation

Full project documentation PDF is included in this repository:
📎 [`Aaron_Chakraborty_SAP_BTP_Capstone.pdf`](./Aaron_Chakraborty_SAP_BTP_Capstone.pdf)

---

*Submitted as part of KIIT University Capstone Project · ExcelR Edtech · SAP Industrial Elective – BTP Developer · April 2026*
