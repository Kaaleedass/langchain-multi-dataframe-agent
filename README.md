# LangChain Multi-DataFrame Agent 🤖

An AI-powered data analysis agent that uses **LangChain, OpenAI, and Pandas** to answer natural-language questions across multiple CSV datasets.

Instead of manually searching different CSV files, users can simply ask questions in plain English. The AI agent determines which DataFrame is relevant, generates Pandas operations, analyzes the data, and returns the answer.

## 🚀 Project Overview

This project demonstrates how to build an **AI Data Analysis Agent** using LangChain's `create_pandas_dataframe_agent`.

The agent works with four different datasets:

- **SaaS Documentation** — Technical documentation and API information
- **Credit Card Terms** — APR, fees, rates, and financial terms
- **Hospital Policy** — Visiting rules and administrative information
- **Ecommerce FAQs** — Shipping, returns, and product information

The user does not need to know which CSV contains the answer. The agent determines the appropriate DataFrame automatically.

## 🏗️ Architecture

```text
                    User Question
                          │
                          ▼
                   OpenAI LLM
                          │
                          ▼
              LangChain DataFrame Agent
                          │
             ┌────────────┼────────────┐
             ▼            ▼            ▼
            df1          df2          df3          df4
           SaaS        Credit       Hospital    Ecommerce
                       Card
             │            │            │            │
             └────────────┴────────────┴────────────┘
                          │
                          ▼
                    Pandas Analysis
                          │
                          ▼
                   Analysis Result
                          │
                          ▼
                  Natural Language Answer
```

## ✨ Features

- Natural-language data analysis
- Multiple CSV/DataFrame support
- Automatic DataFrame selection
- Pandas-based data analysis
- OpenAI tool calling
- Flexible string matching
- Natural-language responses
- Interactive command-line interface
- Google Drive CSV downloading

## 🛠️ Technologies Used

- **Python**
- **LangChain**
- **LangChain Experimental**
- **LangChain OpenAI**
- **OpenAI**
- **Pandas**
- **gdown**

## 📂 Datasets

### 1. SaaS Documentation

Contains technical documentation and API-related information.

```text
df1 → SaaS Documentation
```

### 2. Credit Card Terms

Contains credit card information such as:

```text
Term ID
Card Type
Category
Detail Text
Variable Rate
Fee Amount
```

Example:

```text
Card Type: Platinum Rewards
Category: APR
Variable Rate: Yes
```

### 3. Hospital Policy

Contains hospital visiting rules and administrative information.

```text
df3 → Hospital Policy
```

### 4. Ecommerce FAQs

Contains information related to:

- Shipping
- Returns
- Products
- Frequently asked questions

```text
df4 → Ecommerce FAQs
```

## ⚙️ How It Works

### Step 1 — Download the datasets

The application downloads the required CSV files from Google Drive using `gdown`.

```python
for filename, file_id in files_to_download.items():
    if not os.path.exists(filename):
        gdown.download(
            id=file_id,
            output=filename,
            quiet=False
        )
```

### Step 2 — Load CSV files

Each CSV is loaded into a Pandas DataFrame.

```python
dataframes = []

for filename in files_to_download.keys():
    df = pd.read_csv(filename)
    dataframes.append(df)
```

### Step 3 — Initialize the LLM

The project uses OpenAI through LangChain.

```python
llm = ChatOpenAI(
    temperature=0.0,
    model="gpt-4o-mini",
    openai_api_key=api_key
)
```

### Step 4 — Create the DataFrame Agent

LangChain provides the ready-made Pandas DataFrame Agent.

```python
agent = create_pandas_dataframe_agent(
    llm,
    dataframes,
    verbose=True,
    agent_type="openai-tools",
    prefix=custom_prefix,
    include_df_in_prompt=True,
    allow_dangerous_code=True
)
```

### Step 5 — Ask questions

Users can ask questions in natural language.

Example:

```text
What is the visiting hour in the hospital?
```

or:

```text
What is the API limit?
```

or:

```text
What is the APR for Platinum Rewards?
```

The agent determines which DataFrame is relevant and performs the required Pandas analysis.

## 🧠 Agent Workflow

The agent follows a workflow similar to:

```text
User Question
      ↓
Understand Question
      ↓
Select Relevant DataFrame
      ↓
Generate Pandas Operation
      ↓
Execute Python/Pandas
      ↓
Observe Result
      ↓
Generate Final Answer
```

For example:

```text
"What is the APR for Platinum Rewards?"
                    ↓
          Identify financial question
                    ↓
                   df2
                    ↓
       Search Card Type / Category
                    ↓
              Find matching row
                    ↓
          Return APR information
```

## 🔍 Flexible String Matching

The agent is instructed to use flexible string matching instead of relying only on exact matches.

For example:

```python
df["Detail Text"].str.contains(
    "interest",
    case=False
)
```

`case=False` allows matching regardless of uppercase/lowercase differences.

This makes natural-language queries more flexible.

## 🔐 Environment Setup

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/langchain-multi-dataframe-agent.git
```

```bash
cd langchain-multi-dataframe-agent
```

### 2. Install dependencies

```bash
pip install pandas gdown langchain langchain-openai langchain-experimental tabulate
```

### 3. Configure the OpenAI API key

Set your API key as an environment variable.

Windows PowerShell:

```powershell
$env:OPENAI_API_KEY="your-api-key"
```

Or use a `.env` file if your project is configured to load environment variables.

**Never commit your API key to GitHub.**

## ▶️ Run the Project

Run:

```bash
python your_script_name.py
```

You should see:

```text
AI Agent is ready! You can ask questions across ALL files.
```

Then enter your question:

```text
You: What is the APR for Platinum Rewards?
```

The agent will analyze the appropriate DataFrame and return the answer.

## ⚠️ Security Note

This project uses:

```python
allow_dangerous_code=True
```

This allows the Pandas agent to execute Python code generated during the agent workflow.

Use this option only in a **trusted and controlled environment**.

Do not expose this implementation directly to untrusted users without appropriate sandboxing and security controls.

## 🎯 Learning Objectives

This project demonstrates the following concepts:

- Large Language Models (LLMs)
- LangChain
- AI Agents
- Tool Calling
- Pandas DataFrame Agents
- Natural-language data analysis
- Multi-dataset routing
- Prompt engineering
- Python code execution by agents
- OpenAI integrations

## 🔮 Future Improvements

Possible improvements include:

- Add more datasets
- Add SQL database support
- Add charts and visualizations
- Add Streamlit UI
- Add conversation memory
- Add authentication
- Add safer Python execution using sandboxing
- Add database/RAG integration
- Add agent tracing and monitoring

## 👨‍💻 Author

**Kaaleedass Manoharan**

Focused on **Data Analytics, Automation, Generative AI, and Agentic AI**.

---

⭐ If you find this project useful, consider giving the repository a star.
