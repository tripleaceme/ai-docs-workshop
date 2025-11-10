## 🧩 **Case Study: Automating Documentation for a Retail Analytics Project**

### **Problem Context**

A mid-sized retail company, **ShopSmart**, manages millions of daily transactions across multiple sales channels such as online, in-store, and third-party marketplaces.
Their data team built a dbt project to transform raw sales, customer, and inventory data into analytics-ready models for dashboards and performance reports.

However, there’s a major challenge:
While their dbt project runs smoothly, **their documentation is always outdated**.

### **The Core Problems**

1. **Manual Documentation Overload**
   Each time a model changes, analysts have to manually update descriptions for new columns, metrics, and transformations. Over time, this became inconsistent and error-prone.

2. **Lack of Context for Stakeholders**
   Business users often ask questions like:

   * “What does the *average_order_value* metric actually mean?”
   * “Where does this *customer_segment* field come from?”
   * “Which data source feeds into the *sales_summary* model?”

   The answers were often buried deep inside SQL files or scattered across Slack messages.

3. **Slow Onboarding for New Team Members**
   New data engineers or analysts struggled to understand model dependencies and transformation logic because the **dbt docs were incomplete** and **no conceptual documentation existed** to explain *why* certain data flows were built the way they were.

---

### **Proposed Solution**

So, the company wants to **automate its documentation process**, integrating **dbt**, **AI (Gemini API)** to ensure that both the *technical* and *business* sides of their data project stay aligned.

<img width="1280" height="389" alt="Documentation Pipeline" src="https://github.com/user-attachments/assets/998ccdfe-f663-487c-b5ee-9d59ac230c97" />


