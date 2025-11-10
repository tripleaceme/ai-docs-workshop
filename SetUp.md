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
4. Inside `workshop_db`, create a **schema** called `raw_data` and `prod`. Ensure to select the `dbt_user` as the `owner` of the database and table


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
git clone https://github.com/<your-org-or-username>/ai-docs-workshop.git
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
Move those CSV files into your dbt project’s `seeds/` folder.

Example:

```bash
# Assuming you're inside the ai_docs project
mv ../ai-docs-workshop/data/*.csv seeds/
```

Now your dbt project has a `seeds/` directory containing your raw data files.

---

## 🌾 Step 10: Load Data Using dbt Seeds

Now that your connection is configured and your CSV files are in the `seeds/` folder, it’s time to load those files into your database using dbt’s **seed** feature.

The `dbt seed` command looks for all `.csv` files inside your project’s `seeds/` directory and loads them into your target database (in our case, Postgres).

Each CSV file name automatically becomes the **table name** in your database.
For example:

```
seeds/
 └── customers.csv
```

will create a table called **`raw_data.customers`** in your database schema defined in your `profiles.yml` (that’s `raw_data` in this workshop).

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
3. You should now see tables with names matching your CSV files (e.g., `customers`, `orders`, etc.).
4. Run a quick query to confirm data is there:

```sql
SELECT * FROM raw_data.customers LIMIT 5;
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

During the workshop, we’ll build dbt models, explore documentation generation, and automate it with AI.

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
