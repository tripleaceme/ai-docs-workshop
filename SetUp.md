# 🧰 Workshop Setup Guide

### **Leveraging AI for Data-Driven Documentation: Capturing & Scaling Domain Knowledge**

*Facilitated by [Ayoade Adegbite](https://www.linkedin.com/in/tripleaceme/)*

---

## 🪜 Step 1: Prerequisites

Before we begin, make sure you have the following installed:

✅ **Python 3.9+**
✅ **pip** (comes with Python)
✅ **git**
✅ **PostgreSQL** (you’ll need it running locally or use an online instance)
✅ **VS Code or any text editor**

---

## 🧱 Step 2: Create and Activate a Virtual Environment

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

## ⚙️ Step 3: Install dbt

Once your virtual environment is active, install dbt core and the Postgres adapter.

```bash
pip install dbt-core dbt-postgres
```

Confirm installation:

```bash
dbt --version
```

---

## 🗄️ Step 4: Set Up PostgreSQL

Next, let’s prepare your database before initializing dbt.

1. Open **pgAdmin** or your preferred SQL client.
2. Create a **new database** named `workshop_db`.
3. Inside `workshop_db`, create a **schema** called `raw_data`.

Your credentials will look like this:

* **Host:** `localhost`
* **User:** `postgres`
* **Password:** your chosen password
* **Port:** `5432`
* **Database:** `workshop_db`
* **Schema:** `raw_data`

Keep these details handy — you’ll use them when setting up your dbt profile.

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
mkdir seeds
mv ../ai-docs-workshop/data/*.csv seeds/
```

Now your dbt project has a `seeds/` directory containing your raw data files.

---

## 🌾 Step 10: Load Data Using dbt Seeds

With your database connection ready and seed files in place, let’s load the data:

```bash
dbt seed
```

✅ You should see an output like:

```
Running 1 seed for ai_docs
Completed successfully
```

This means dbt has loaded your CSV data into the database under the `raw_data` schema.

---

## 🧰 Step 11: 



---

## 🚀 You’re All Set!

You’ve now completed all environment setup steps:

1. Installed dbt
2. Configured Postgres
3. Initialized a dbt project
4. Loaded your first dataset

During the workshop, we’ll build dbt models, explore documentation generation, and automate it with AI.

---

### 💬 Need Help?

If you encounter any issues during setup, reach out to me on LinkedIn:
👉 [Ayoade Adegbite](https://www.linkedin.com/in/tripleaceme/)

---

Would you like me to now format this as a **clean `SETUP.md` file** (with Markdown formatting, emojis, and code blocks) so you can add it directly to your GitHub repo for participants to view online?
