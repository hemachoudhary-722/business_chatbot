# Business FAQ Chatbot — MySQL Version

Same chatbot concept as the basic version, but upgraded to store FAQ
answers and conversation history in a **MySQL database** instead of a
Python dictionary and text file. The script sets up its own database
and tables the first time it runs — no separate SQL file needed.

## What's different from the basic version

| Basic version | MySQL version |
|---|---|
| FAQ answers stored in a Python dictionary | FAQ answers stored in a `faqs` table |
| Conversation saved to a `.txt` file | Conversation saved to a `conversation_log` table |
| No setup needed | Needs MySQL server running locally |

## Setup (run these once)

1. **Install the MySQL Python connector:**
   ```bash
   pip install mysql-connector-python
   ```

2. **Make sure MySQL server is running** on your machine (the one you
   already use with PyCharm/IDLE).

3. **Add your MySQL username/password.** Open `chatbot_mysql.py` and
   update the `DB_CONFIG` dictionary near the top:
   ```python
   DB_CONFIG = {
       "host": "localhost",
       "user": "root",
       "password": "your_mysql_password_here",
   }
   ```

That's it — no manual SQL step. The script handles the rest.

## Usage

```bash
python chatbot_mysql.py
```

The first time you run it, `setup_database()`:
1. Connects to your MySQL server
2. Creates the `business_chatbot` database if it doesn't exist
3. Creates the `faqs` and `conversation_log` tables if they don't exist
4. Fills the `faqs` table with sample data — but only if it's empty,
   so running the script again won't duplicate data or wipe anything

By default it runs a demo with sample questions. To chat with it
yourself, change the last line of the script from `run_demo()` to
`run_chat()`.

## Checking the logged conversations

After running it, you can see everything the chatbot logged by querying
MySQL directly:

```sql
USE business_chatbot;
SELECT * FROM conversation_log ORDER BY logged_at DESC;
```

This is the real advantage of using a database instead of a text file —
a business could easily search, filter, or analyze customer questions
over time (e.g., "what are the top 5 most common questions this month?").

## Why this project

Shows practical use of Python + SQL together — connecting to a database,
creating schema programmatically, reading configuration data at runtime,
and writing application data back to it — which is core to most real
business software, including AI-powered ones.

## Possible extensions

- Add an admin function to add/edit FAQ entries from within Python
- Query `conversation_log` to find the most frequently asked questions
- Combine with the Claude API: if no FAQ keyword matches, send the
  question to an LLM and log both the question and the AI-generated answer
