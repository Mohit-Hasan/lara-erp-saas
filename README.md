# Portfolio Repository: Production-Grade Project Showcase

> **Professional Note:** Having built and deployed numerous commercial applications, I realized that much of my most impactful work remained locked in private enterprise repositories. To better demonstrate my technical depth and architectural thinking to potential partners and employers, I am publishing these curated portfolio repositories. I have worked on many projects, and I am now consolidating these representations to share my capabilities and open doors for new opportunities.

**Notice:** This repository represents a **Live Production System**. Due to commercial sensitivity and intellectual property agreements, the full source code is not public. However, I have included:
1.  **Architecture Overviews:** Deep dives into the "why" and "how" of the system logic.
2.  **Core Logic Snippets:** Selected high-impact code samples to demonstrate my standards.
3.  **Live Demos:** Fully functional environments to explore the system firsthand.

---

# **Lara ERP SaaS: Multi-Tenant**

An advanced evolution of the Lara ERP monolithic core, transformed into a high-performance **Software-as-a-Service (SaaS)** platform. This system utilizes a logical interceptor-based multi-tenancy architecture, enabling thousands of independent businesses to operate securely within a shared infrastructure with fully automated billing and tenant lifecycle management.

**Professional Note:** This repository showcases the transition from standalone enterprise software to a scalable SaaS model. It highlights my ability to handle complex architectural challenges such as tenant isolation, automated resource provisioning, and background job processing within a unified codebase.

**Evolution Note:** This project is the result of **transforming the standalone ERP engine** into a sophisticated multi-tenant ecosystem. By refactoring the core logic of the [Lara ERP Standalone Version](https://github.com/mohit-hasan/lara-erp-monolithic), **I successfully moved from a single-instance product to a scalable SaaS platform.**

**Notice:** This is a **Live Production-Grade SaaS Engine**. Due to commercial sensitivity, the repository includes architecture overviews and core logic snippets rather than the full source.

#### *Project Timeline: May 2024- October 2025*

![frontend_landing](https://raw.githubusercontent.com/Mohit-Hasan/lara-erp-saas/refs/heads/main/screenshots/1.png)

---

## **The SaaS Transformation**

The core technical achievement of this project was the **Architectural Shift** from a single-business entity into a **Parent-Child SaaS Ecosystem**.

* **Standalone to Multi-Tenant:** I refactored the original "Single-Company" ERP logic into a "Control Plane" (Parent) and "Tenant" (Child) relationship.  
* **Subdomain-Based Isolation:** Instead of physical separation, the system uses dynamic subdomain routing to differentiate between child entities while served by a single centralized logic engine.  
* **Logical Interception:** The transformation was achieved by implementing a global data-interceptor that wraps the entire application layer, ensuring that "Child" data never leaks across the "Parent" ecosystem.

## **SaaS Architectural Excellence**

While inheriting the "Zero-Dependency" financial engine of the standalone version, this SaaS edition introduces a sophisticated cloud-native layer:

### **The Interceptor Approach: Why Shared Schema?**

In designing this system, I evaluated the three primary multi-tenancy patterns: *Separate Databases*, *Separate Schemas*, and *Shared Schema with Row-Level Filtering (Interceptors)*.

I strategically chose the **Logical Interceptor approach** because:

* **Maintainability & Scalability:** Managing thousands of separate database migrations with limited human resources is an operational bottleneck. A single schema allows for atomic updates across all tenants simultaneously.  
* **Cost Efficiency:** Shared resource utilization reduces infrastructure overhead significantly while maintaining high performance through composite indexing on tenant columns.  
* **Rapid Provisioning:** New tenants can be onboarded instantly without the latency or complexity of physical database creation.

### **Wildcard Subdomain & Custom Domain Resolution**

The system implements a high-performance routing layer to resolve tenant identity:

1. **Wildcard DNS:** The server is configured to accept all \*.example.com requests.  
2. **Application Layer Interception:** Before the request reaches the controller, a custom middleware inspects the Host header.  
3. **Multi-Layer Lookup:** It checks the **Application Cache** (configured via Laravel's cache drivers) first for the tenant slug to ensure sub-millisecond resolution. If missing, it falls back to a database lookup.  
4. **Global Scoping:** Once identified, a global Eloquent scope is injected, ensuring every query (Select, Update, Delete) is automatically constrained by WHERE tenant\_id \= ?.  
   *Note: The architecture supports Custom Domains, though this is restricted in the demo environment due to hosting panel limitations.*

## **SaaS Workflows**

### **1\. Tenant Onboarding & Activation**

To ensure a "Zero-Wait" user experience, the onboarding process is entirely asynchronous:

* **Step 1: Sign Up & Wallet Funding:** The user registers and adds funds to their centralized wallet via integrated payment gateways.  
* **Step 2: Subscription Selection:** The user chooses a plan and confirms the purchase.  
* **Step 3: Background Provisioning:** The system pushes a "Tenant Setup" job to the **Application Queue** (configured via Laravel queue drivers).  
* **Step 4: Automated Seeding:** In the background, the worker handles subdomain mapping, generates default settings/ledgers, and seeds necessary lookup data.  
* **Step 5: Active Status:** Once the queue finishes processing all background tasks, the tenant instance is marked as "Active" and becomes immediately accessible.

### **2\. Automated Billing & Suspension**

* **Cron-Based Auditing:** A daily scheduled task calculates subscription validity for all tenants based on their selected plans.  
* **Automated Deductions:** Subscription fees are automatically debited from the tenant's centralized wallet balance (if auto-renew is enabled).  
* **Instant Suspension:** If the balance is insufficient for renewal, the interceptor layer immediately redirects all tenant traffic to our application billing domain page, locking access until the wallet is replenished.

## **Live SaaS Demonstration**

Explore the full SaaS ecosystem from three distinct perspectives:

* **SaaS Landing:** [https://laraerp.info](https://laraerp.info)

#### **1\. Control Plane (Super-Admin Access)**

*Monitor the entire ecosystem and manage all tenants.*
* **URL:** [https://laraerp.info/superadmin/login](https://laraerp.info/superadmin/login)

* **Email:** superadmin@domain.com | **Password:** password

#### **2\. Billing Plane (Tenant A)**

*Manage billings and subscriptions.*
* **URL:** [https://laraerp.info/tenant/login](https://laraerp.info/tenant/login)

* **Email:** tenant1@domain.com | **Password:** password

#### **3\. Tenant A: "TechFlow Solutions"**

* **URL:** [https://techflow.laraerp.info](https://techflow.laraerp.info)  
#### **Test Credentials**

| Account Type | Email Address | Password |
| :--- | :--- | :--- |
| **Administrator** | `01700000000` | `password` |
| **Staff** (All Permission) | `0190000001` | `password` |
| **Staff** (Limited Permission) | `0190000002` | `password` |
| **User / Customer** | `0180000001` | `password` |

#### **4\. Tenant B: "HyperLoop"**

* **URL:** [https://hyperloop.laraerp.info](https://hyperloop.laraerp.info)  
#### **Test Credentials**

| Account Type | Email Address | Password |
| :--- | :--- | :--- |
| **Administrator** | `01700000000` | `password` |

#### **4\. Tenant C: "Open For You" (User-Generated Demo)"**

* **Create your own tenant instantly!** Experience the automated provisioning workflow firsthand. Register, fund your wallet (manual funding via admin dashboard for demo security as payment credentials are disabled), and watch your business website go live within seconds via our background queue processing.

* **Get Started:** [https://laraerp.info/tenant/register](https://laraerp.info/tenant/register)


---

## **System Previews**

![UI_2](https://raw.githubusercontent.com/Mohit-Hasan/lara-erp-saas/refs/heads/main/screenshots/2.png)

![UI_3](https://raw.githubusercontent.com/Mohit-Hasan/lara-erp-saas/refs/heads/main/screenshots/3.png)

![UI_4](https://raw.githubusercontent.com/Mohit-Hasan/lara-erp-saas/refs/heads/main/screenshots/4.png)

![UI_5](https://raw.githubusercontent.com/Mohit-Hasan/lara-erp-saas/refs/heads/main/screenshots/5.png)

![UI_6](https://raw.githubusercontent.com/Mohit-Hasan/lara-erp-saas/refs/heads/main/screenshots/6.png)

---

## **Tech Stack & Infrastructure**

* **Architecture:** Multi-tenant Parent-Child Monolith (Single Codebase, Shared DB, Cache-First Resolution).  
* **Backend:** Laravel 10 (Custom Logic Implementation).  
* **Data Management:** MySQL (Optimized with Tenant-specific composite indexing).  
* **Infrastructure:** Wildcard DNS mapping and custom Middleware Interceptors.

### Repository Structure
This repository is a **Portfolio Representation**. To protect commercial intellectual property, the full source code is not public. Instead, core logic implementation is showcased via selected snippets.

```text
├── sample_code/
├── screenshots/
└── README.md
```

---

### Explore Other Projects
I have curated a selection of my work to showcase different architectural patterns and business solutions. Feel free to explore them:

[![GlobalShop](https://img.shields.io/badge/Project-GlobalShop-black?style=for-the-badge&logo=github)](https://github.com/mohit-hasan/globalshop-commerce-platform)
[![Classroom](https://img.shields.io/badge/Project-ClassFlow-black?style=for-the-badge&logo=github)](https://github.com/mohit-hasan/classroom-management-system)
[![WHMCSNexus](https://img.shields.io/badge/Project-WHMCSNexus-black?style=for-the-badge&logo=github)](https://github.com/mohit-hasan/whmcs-nexus-control-plane)
[![JournalAppSpring](https://img.shields.io/badge/Project-JournalApp-black?style=for-the-badge&logo=springboot&logoColor=white)](https://github.com/mohit-hasan/journal-app-spring-boot)
[![LaraERP](https://img.shields.io/badge/Project-Lara%20ERP-black?style=for-the-badge&logo=github)](https://github.com/mohit-hasan/lara-erp-monolithic)
[![LaraERP SaaS](https://img.shields.io/badge/Project-Lara%20ERP%20SaaS-6DB33F?style=for-the-badge&logo=github)](https://github.com/mohit-hasan/lara-erp-saas)