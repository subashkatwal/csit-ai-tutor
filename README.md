# CSIT AI Tutor

An AI-powered study helper for TU CSIT 7th semester students. The app detects the subject and topic of a question, generates a step-by-step answer, checks exam relevance, and creates practice questions.

Live app: https://csit-ai-tutor.onrender.com/

## Features

- Solves CSIT 7th semester academic questions.
- Detects subject, topic, question type, and difficulty.
- Generates step-by-step explanations.
- Provides TU exam pattern guidance.
- Creates two related practice problems with answers.
- Streams progress updates while the answer is being generated.
- Limits each user to 3 messages per day based on IP address.

## Supported Subjects

- Advanced Java Programming
- Data Warehouse and Data Mining
- Software Project Management
- Principles of Management

## Tech Stack

- Frontend: HTML, CSS, JavaScript
- Backend: FastAPI
- Server: Uvicorn
- AI/LangChain: LangChain, LangChain Groq
- Model Provider: Groq
- Deployment: Render

## Project Structure

```text
.
├── csitseventh_agent/
│   ├── index.html       # Frontend UI
│   ├── main.py          # FastAPI backend and routes
│   ├── chains.py        # LangChain model and chains
│   └── prompts.py       # Prompt templates
├── requirements.txt     # Python dependencies
├── .gitignore           # Ignored local/private files
└── README.md
```

## How It Works

The project is deployed as a single FastAPI application.

The backend serves the frontend:

```text
GET /
```

The frontend sends questions to:

```text
POST /solve
```

The backend streams response events back to the browser using `text/event-stream`.

## API Endpoints

### Frontend

```text
GET /
```

Returns the main `index.html` page.

### Health Check

```text
GET /health
```

Example response:

```json
{
  "status": "ok"
}
```

### Solve Question

```text
POST /solve
```

Request body:

```json
{
  "problem": "Explain K-Means clustering with an example."
}
```

The response is streamed as server-sent events.

## Daily Usage Limit

The backend limits each user to 3 questions per day.

Currently, users are identified by IP address because the app does not have login/authentication yet.

If the daily limit is reached, the backend returns:

```text
429 Too Many Requests
```

Note: The current limit is stored in memory, so it can reset when the server restarts. For production use, this should be moved to a database, Redis, Supabase, or Firebase which will be implemented later as a future work.

## Local Setup

### 1. Clone The Repository

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
cd YOUR_REPO_NAME
```

### 2. Create A Virtual Environment

On Windows:

```powershell
python -m venv myenv
myenv\Scripts\activate
```

On macOS/Linux:

```bash
python3 -m venv myenv
source myenv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Create `.env`

Create a `.env` file in the project root:

```env
GROQ_API_KEY=your_groq_api_key_here
```

Do not commit `.env` to GitHub.

### 5. Run Locally

Use port `8001` locally:

```powershell
python -m uvicorn csitseventh_agent.main:app --reload --port 8001
```

Open:

```text
http://127.0.0.1:8001
```

Health check:

```text
http://127.0.0.1:8001/health
```

## Deployment On Render

This project hosts both frontend and backend together on Render.

### Render Settings

Create a new Render Web Service and use:

```text
Runtime: Python
Build Command: pip install -r requirements.txt
Start Command: python -m uvicorn csitseventh_agent.main:app --host 0.0.0.0 --port $PORT
```

### Environment Variables

Add this environment variable in Render:

```text
GROQ_API_KEY=your_groq_api_key_here
```

Render provides the `$PORT` variable automatically.

## GitHub Push Workflow

After making changes:

```bash
git status
git add .
git commit -m "Describe your change"
git push
```

Render will redeploy automatically after the push if auto-deploy is enabled.

## Security Notes

- Never commit `.env`.
- Keep `GROQ_API_KEY` private.
- If an API key is accidentally pushed to GitHub, rotate/regenerate it immediately.
- The frontend should not contain API keys.
- API keys should only be stored as backend environment variables.

## Future Improvements

- Add user authentication.
- Store daily usage limits in a database.
- Add persistent chat history.
- Add admin controls for usage limits.
- Improve mobile UI polish.
- Add support for PDF/image question extraction.

## License

This project is for educational use.
