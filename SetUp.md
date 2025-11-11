# 🧰 **Workshop Setup Guide**
### **Leveraging AI for Data-Driven Documentation: Capturing & Scaling Domain Knowledge**

*Facilitated by [Ayoade Adegbite](https://www.linkedin.com/in/tripleaceme/) for Kwara Build Workshop 2025*

---

Welcome! 👋

In this workshop, we’ll be building an AI-powered documentation automation system for dbt models, the same kind that ensures every data model in your project is properly documented before deployment.

To get hands-on, we’ll go through **two setup parts**:

1. **Part 1:** Set up dbt, PostgreSQL connection, and prepare your seed data.
2. **Part 2:** Set up Gemini AI for documentation generation.

By the end of both parts, you’ll have a working environment ready for the workshop.

---

## ⚙️ **PART 1 — Setting Up dbt and Database**

This section will walk you through:

* Creating and activating a Python virtual environment
* Installing dbt and dependencies
* Setting up PostgreSQL and connection credentials
* Cloning the workshop repo
* Running dbt commands to seed and verify data

Let’s go step by step 👇

---


## 🪜 Step 1: Prerequisites

Before we begin, make sure you have the following installed:

- **Python 3.9+**
- **pip** (comes with Python)
- **git**
- **PostgreSQL** (you’ll need it running locally or use an online instance)
- **VS Code or any text editor**
---

## 🗄️ Step 2: Set Up PostgreSQL

Next, let’s prepare your database before initializing dbt.

1. Open **pgAdmin** or your preferred SQL client.
2. Create a **new database** named `workshop_db`.
3. Create a user called `dbt_user` with a password of your choice, e.g `dbt_user`.
4. Inside `workshop_db`, create a **schema** called `raw_data` and `prod`. Select the `dbt_user` as the `owner` of the database and table


Your credentials will look like this:

* **Host:** `localhost`
* **User:** `dbt_user`
* **Password:** your chosen password or `dbt_user`
* **Port:** `5432`
* **Database:** `workshop_db`
* **Schema:** `raw_data`

Keep these details handy — you’ll use them when setting up your dbt profile.

---

## 🧱 Step 3: Create and Activate a Virtual Environment

We’ll use a virtual environment to isolate our dependencies.

### Commands:

```bash
# Create a virtual environment
python -m venv venv

# Activate the environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate
```

You should now see `(venv)` at the start of your terminal line — that means your environment is active.

---

## ⚙️ Step 4: Install dbt

Once your virtual environment is active, install dbt core and the Postgres adapter.

```bash
pip install dbt-core dbt-postgres
```

Confirm installation:

```bash
dbt --version
```

---

## 🧬 Step 5: Clone the Workshop Repository

Now let’s get the starter repository that contains the data we’ll be using.

```bash
git clone https://github.com/tripleaceme/ai-docs-workshop.git
cd ai-docs-workshop
```

The repo contains a **data/** folder with the `.csv` files that we’ll load later using dbt seeds.

---

## 🧩 Step 6: Initialize a dbt Project

Now that dbt is installed and your repo is cloned, let’s create your dbt project.

Run:

```bash
dbt init ai_docs
```

When prompted:

* Enter project name → `ai_docs`
* Choose database type → `postgres`
* Enter your Postgres details when asked:

  * **Host:** `localhost`
  * **User:** `postgres`
  * **Password:** (your password)
  * **Port:** `5432`
  * **Database:** `workshop_db`
  * **Schema:** `raw_data`
    **Threads:** `5`

After initialization, dbt will create a folder called `ai_docs` with some starter files.

---

## 🧾 Step 7: Locate and Update Your `profiles.yml` File

dbt uses a file called `profiles.yml` to store connection settings.

Depending on your OS, it’s located here:

* macOS/Linux → `~/.dbt/profiles.yml`
* Windows → `C:\Users\<YourName>\.dbt\profiles.yml`

Open it and confirm it looks similar to this:

```yaml
ai_docs:
  target: dev
  outputs:
    dev:
      type: postgres
      host: localhost
      user: postgres
      password: your_password
      port: 5432
      dbname: workshop_db
      schema: raw_data
```

Save the file.

---

## 🧪 Step 8: Test Your Connection

Run this command to confirm dbt can connect to your database:

```bash
cd ai_docs
dbt debug
```

✅ If everything is correct, you’ll see:

```
Connection test: OK connection ok
All checks passed!
```

If not, double-check your `profiles.yml` credentials or ensure Postgres is running.

---

## 🌱 Step 9: Move Data Files to dbt’s Seed Folder

In your cloned repository (`ai-docs-workshop`), there’s a `data/` folder.

- Move those CSV files into your dbt project’s `seeds/` folder.
- Now your dbt project has a `seeds/` directory containing your raw data files.

---

## 🌾 Step 10: Load Data Using dbt Seeds

Now that your connection is configured and your CSV files are in the `seeds/` folder, it’s time to load those files into your database using dbt’s **seed** feature.

The `dbt seed` command looks for all `.csv` files inside your project’s `seeds/` directory and loads them into your target database (in our case, Postgres).

Each CSV file name automatically becomes the **table name** in your database.
For example:

```
seeds/
 └── orders.csv
```

will create a table called **`raw_data.orders`** in your database schema defined in your `profiles.yml` (that’s `raw_data` in this workshop).

### 🧠 Behind the scenes:

* dbt reads the CSV files in the `seeds` directory.
* It checks your connection details in `profiles.yml`.
* It creates new tables (or replaces existing ones) in the target schema.
* The table name matches the CSV file name (without `.csv`).

### 🧩 Command:

```bash
dbt seed
```

If successful, you’ll see output like this:

```
Running 1 seed for ai_docs
Completed successfully
```

✅ This means dbt successfully created your tables in the `raw_data` schema using the CSV data you provided.

---

## 🧾 Step 11: Verify That Data Was Loaded Correctly

Now let’s confirm that the tables actually exist and contain data in your Postgres database.

1. Open **pgAdmin** or your SQL client.
2. Navigate to your database → `workshop_db` → Schemas → `raw_data` → Tables.
3. You should now see tables with names matching your CSV files (e.g., `products`, `orders`, etc.).
4. Run a quick query to confirm data is there:

```sql
SELECT * FROM raw_data.orders LIMIT 5;
```

If you see rows returned — 🎉 congratulations, your seed step worked perfectly!
Your `raw_data` schema is now fully populated and ready for the dbt modeling and documentation automation phase.

---

## 🚀 You’re All Set!

You’ve now completed all environment setup steps:

1. Installed dbt
2. Configured Postgres
3. Initialized a dbt project
4. Loaded your first dataset

---
Let's set up our models, and during the workshop, we’ll build dbt models, explore documentation generation, and automate it with AI.

# Sources Configuration

Place this in:
`models/staging/schema/sources.yml`

```yaml
version: 2

sources:
  - name: workshop_db
    schema: raw_data
    database: workshop_db
    tables:
      - name: products
      - name: order_item_refunds
      - name: orders

```
#### **Order item refunds model**
---

Place this in:
`models/staging/stg_order_item_refunds.sql`

A simple staging model that renames columns and casts types where appropriate:

```sql
with source as (

    select *
    from {{ source('workshop_db', 'order_item_refunds') }}

),

renamed as (

    select
        order_item_refund_id,
        created_at::timestamp as created_at,
        order_item_id,
        order_id,
        refund_amount_usd::numeric(10,2) as refund_amount_usd
    from source

)

select * from renamed
```

---

### **model description for order item refunds**

Place this in:
`models/staging/schema/stg_order_item_refunds.yml`

```yaml
version: 2

models:
  - name: stg_order_item_refunds
    description: "Clean and standardized staging model for order item refunds."
    columns:
      - name: order_item_refund_id
        description: "Unique ID for each refund event on an order item."
        tests:
          - not_null
          - unique

      - name: created_at
        description: "Timestamp when the refund record was created."

      - name: order_item_id
        description: "ID of the specific order item that the refund applies to."

      - name: order_id
        description: "Order to which the refunded item belongs."

      - name: refund_amount_usd
        description: "Total refund amount in USD for the item."
```

---
#### **products model**

Place this in:
`models/staging/stg_products.sql`

```sql
with source as (

    select *
    from {{ source('workshop_db', 'products') }}

),

renamed as (

    select
        product_id,
        created_at,
        product_name
    from source

)

select * from renamed
```

---

### **model description for products**

Place this in:
`models/staging/schema/stg_products.yml`

```yaml
version: 2

models:
  - name: stg_products
    description: "Staging model containing product metadata."
    columns:
      - name: product_id
        description: "Unique identifier for each product."

      - name: created_at
        description: "Timestamp when the product record was added to the system."

      - name: product_name
        description: "Human-readable name of the product being sold."
```

---
#### **Order model**

Place this in:
`models/staging/orders/stg_orders.sql`

```sql
with source as (

    select *
    from {{ source('workshop_db', 'orders') }}

),

renamed as (

    select
        order_id,
        created_at,
        website_session_id,
        user_id,
        primary_product_id,
        items_purchased::int as items_purchased,
        price_usd::numeric(10,2) as price_usd,
        cogs_usd::numeric(10,2) as cogs_usd
    from source

)

select * from renamed
```

---

### **model description for order**

Place this in:
`models/staging/schema/stg_orders.yml`

```yaml
version: 2

models:
  - name: stg_orders
    description: "Staging model for customer orders including purchase details, pricing, and costs."
    columns:
      - name: order_id
        description: "Unique identifier for each order."

      - name: created_at
        description: "Timestamp when the order was created."

      - name: website_session_id
        description: "Session identifier representing the website visit that led to the order."

      - name: user_id
        description: "Unique customer identifier that placed the order."

      - name: primary_product_id
        description: "Identifier of the main product purchased in this order."

      - name: items_purchased
        description: "Number of items purchased in the order."

      - name: price_usd
        description: "Total price paid for the order in USD."

      - name: cogs_usd
        description: "Cost of goods sold associated with fulfilling this order."
```

---
### **model description for Intermediate order**

Place this in:
`models/intermediate/int_orders_enriched.sql`


```sql
with orders as (
    select * from {{ ref('stg_orders') }}
),

products as (
    select * from {{ ref('stg_products') }}
),

refunds as (
    select
        order_id,
        sum(refund_amount_usd) as total_refund_amount_usd
    from {{ ref('stg_order_item_refunds') }}
    group by order_id
),

joined as (

    select
        o.order_id,
        o.created_at,
        o.website_session_id,
        o.user_id,

        o.primary_product_id,
        p.product_name,

        o.items_purchased,
        o.price_usd,
        o.cogs_usd,

        coalesce(r.total_refund_amount_usd, 0) as refund_amount_usd,

        -- derived fields
        o.price_usd - o.cogs_usd as gross_margin_usd,
        (o.price_usd - o.cogs_usd) - coalesce(r.total_refund_amount_usd, 0) as net_margin_usd

    from orders o
    left join products p 
        on o.primary_product_id = p.product_id
    left join refunds r
        on o.order_id = r.order_id
)

select * from joined
```

---

### **model description for Fact order**

Place this in:
`models/marts/facts/fact_orders.sql`


```sql
select
    order_id,
    created_at,
    website_session_id,
    user_id,
    primary_product_id,
    product_name,

    items_purchased,
    price_usd,
    cogs_usd,
    refund_amount_usd,

    gross_margin_usd,
    net_margin_usd

from {{ ref('int_orders_enriched') }}
```

---
### **model description for Product Dimension**

Place this in:
`models/marts/dim/dim_products.sql`

```sql
select
    product_id,
    product_name,
    created_at
from {{ ref('stg_products') }}
```

---

### **model description for all marts table**

Place this in:
`models/marts/schema.yml`


```yaml
version: 2

models:
  - name: dim_products
    description: "Dimension table containing product attributes."
    columns:
      - name: product_id
        description: "Unique product identifier."
        tests:
          - unique
          - not_null

  - name: fact_orders
    description: "Fact table at the order grain, including financials and refund information."
    columns:
      - name: order_id
        description: "Unique identifier for each order."
        tests:
          - unique
          - not_null

      - name: primary_product_id
        description: "Main product purchased in the order."

      - name: price_usd
        description: "Total price of the order in USD."

      - name: cogs_usd
        description: "Cost of goods sold for the order."

      - name: refund_amount_usd
        description: "Total refund amount applied to the order."

      - name: gross_margin_usd
        description: "Gross margin before refunds."

      - name: net_margin_usd
        description: "Margin after refunds."
```


The last piece is to run the commands below:
- ``` dbt run ```
- ``` dbt docs generate ```

**Your complete setup should look something like this:**

<img width="295" height="425" alt="Screenshot 2025-11-11 at 18 20 09" src="https://github.com/user-attachments/assets/748e43b4-becc-45a4-b9ed-86ee3177f4ec" />

---

### **New Model Flow Diagram (Text Version)**

```
stg_orders  -----> 
                   \
                    → int_orders_enriched → fact_orders
                   /
stg_products -----
                
stg_order_item_refunds (aggregated at order level)
```

---

# 🤖 **PART 2 — Setting Up Gemini AI**

Now that your dbt project is ready and your data is loaded, it’s time to connect our AI engine (**Gemini AI**) which will help us automatically generate missing documentation.

We’ll go through 3 key steps:

1. Get your Gemini AI API key.
2. Store it securely in an environment file (`.env`).
3. Install dependencies so everything work as needed.
4. Test the connection to make sure everything works.

---

## 🔑 **Step 1: Get Your Gemini AI API Key**

1. Go to the Gemini AI API page:
   👉 [AI Google Studio](aistudio.google.com)
2. Log in with your Google account.
3. Click **“Create API key”**.
4. Copy your API key and keep it somewhere safe. We’ll need it soon.

<img width="1348" height="579" alt="Screenshot 2025-11-05 at 21 43 04" src="https://github.com/user-attachments/assets/5392e1dc-98fb-4e7b-a618-43dc8d162fa5" />


---

## 🧩 **Step 2: Create and Configure Your `.env` File**

You’ll need a place to store your API key securely so our Python scripts can access it.

1. In your **project root folder** (the same folder as your `dbt_project.yml` file), create a new file called:

   ```
   .env
   ```

2. Open the `.env` file and paste the following line inside it:

   ```
   GEMINI_API_KEY=your_api_key_here
   ```

   > 🔒 Replace `your_api_key_here` with the actual key you copied earlier.

3. Save and close the file.

---

## 🧠 **Step 3: Install Dependencies for AI Connection**

Before testing your AI connection, install the libraries needed to load environment variables and call Gemini AI:

```bash
pip install python-dotenv google-generativeai
```

---

## 🧪 **Step 4: Test Your Gemini AI Setup**

We’ll now confirm that your API key is working correctly.
In your VS Code, create a file called test_api.py and paste the script below. Click on the play button to run the script:

```python
import google.generativeai as genai
from dotenv import load_dotenv
import os

# Load environment variables
load_dotenv()

# Set up Gemini AI with your key
genai.configure(api_key=os.getenv("GEMINI_API_KEY"))

# Test the connection by asking a simple question
model = genai.GenerativeModel("gemini-2.5-flash")
response = model.generate_content("Say hello, Gemini!")
print(response.text)
```

✅ If everything is set up correctly, you should see a friendly response like:

```
Hello! It's great to hear from you. How can I help you today?
```

🎉 That means your Gemini API key is valid and connected — and your environment is fully ready for the automation part of the workshop!

But if you encounter any error like the one below:

```
google.api_core.exceptions.NotFound: 404 models/gemini-1.5-flash is not found for API version v1beta, or is not supported for generateContent. Call ListModels to see the list of available models and their supported methods.

```
Visit [Gemini API Docs](https://ai.google.dev/gemini-api/docs/text-generation) and check for the latest model that supports text generation.


---

### 💬 Need Help?

If you encounter any issues during setup, reach out to me on LinkedIn:
👉 [Ayoade Adegbite](https://www.linkedin.com/in/tripleaceme/)
