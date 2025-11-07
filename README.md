# 🔬 LLM Security Research Paper Monitor

An automated system using CrewAI and Gemini API to fetch, summarize, and email recent research papers on LLM jailbreaks and red teaming from multiple academic sources.

## 📋 Features

- **Multi-Source Paper Fetching**: Collects papers from ArXiv, ViXra, and SSRN
- **Intelligent Filtering**: Focuses on LLM jailbreaks and red teaming papers from the last 5 days
- **AI-Powered Summaries**: Uses Gemini AI to generate concise summaries
- **Beautiful Email Digests**: Color-coded HTML emails with organized sections
- **CrewAI Integration**: Five specialized agents working together seamlessly

## 🏗️ Architecture

```
5 Agents System:
├── Agent 1: ArXiv Fetcher (3 jailbreak + 2 red team papers)
├── Agent 2: ViXra Fetcher (3 jailbreak + 2 red team papers)
├── Agent 3: SSRN Fetcher (3 jailbreak + 2 red team papers)
├── Agent 4: Summarizer (Uses Gemini AI)
└── Agent 5: Email Sender (Color-coded digest)
```

## 📁 Project Structure

```
D:.
│   .env                      # Environment variables (create from .env.example)
│   .env.example              # Template for environment variables
│   main.py                   # Main execution script
│   README.md                 # This file
│   requirements.txt          # Python dependencies
│   test_agents.py           # Testing individual agents
│
├───agents/                   # Agent implementations
│       arxiv_agent.py
│       vixra_agent.py
│       ssrn_agent.py
│       summarizer_agent.py
│       email_agent.py
│
├───config/                   # Configuration
│       settings.py
│
├───crew_config/             # CrewAI configuration
│       agents.yaml
│       tasks.yaml
│
├───logs/                    # Execution logs
│
├───models/                  # Data models
│       paper.py
│
├───output/                  # Output files
│
├───templates/               # Email templates
│       email_template.html
│
└───utils/                   # Utility functions
        date_utils.py
        email_builder.py
        gemini_client.py
        paper_fetcher.py
```

## 🚀 Setup Instructions

### Step 1: Clone and Navigate

```bash
cd your-project-directory
```

### Step 2: Create Virtual Environment

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# Linux/Mac
python3 -m venv venv
source venv/bin/activate
```

### Step 3: Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 4: Configure Environment Variables

1. Copy `.env.example` to `.env`:
```bash
cp .env.example .env
```

2. Edit `.env` and fill in your credentials:

```env
# Gemini API Key (Get from: https://makersuite.google.com/app/apikey)
GEMINI_API_KEY=your_actual_gemini_api_key

# Email Configuration
SENDER_EMAIL=zzzzzzzz@gmail.com
SENDER_PASSWORD=your_gmail_app_password
RECIPIENT_EMAILS=xxxxxxxxx@gmail.com,yyyyyyyyyyyy@gmail.com

# SMTP Settings (Gmail default)
SMTP_SERVER=smtp.gmail.com
SMTP_PORT=587
```

### Step 5: Set Up Gmail App Password

For Gmail, you need an App Password (not your regular password):

1. Go to your Google Account: https://myaccount.google.com/
2. Select "Security"
3. Under "How you sign in to Google," select "2-Step Verification"
4. At the bottom, select "App passwords"
5. Select "Mail" and your device
6. Copy the generated 16-character password
7. Use this as your `SENDER_PASSWORD` in `.env`

### Step 6: Get Gemini API Key

1. Visit: https://makersuite.google.com/app/apikey
2. Click "Create API Key"
3. Copy the key and add it to `.env` as `GEMINI_API_KEY`

## 🎯 Usage

### Run the Complete System

```bash
python main.py
```

This will:
1. ✅ Fetch 5 papers from ArXiv (3 jailbreak + 2 red team)
2. ✅ Fetch 5 papers from ViXra (3 jailbreak + 2 red team)
3. ✅ Fetch 5 papers from SSRN (3 jailbreak + 2 red team)
4. ✅ Generate AI summaries for all papers using Gemini
5. ✅ Send a beautifully formatted email digest

### Test Individual Agents

```bash
# Test ArXiv agent only
python test_agents.py arxiv

# Test ViXra agent only
python test_agents.py vixra

# Test SSRN agent only
python test_agents.py ssrn

# Test Summarizer with ArXiv papers
python test_agents.py summarizer

# Test all agents
python test_agents.py all
```

## 📧 Email Output

The email digest includes:

### 📚 ArXiv Section (Green Theme)
- Color: Green background with green accent
- Contains ArXiv papers with summaries

### 📘 ViXra Section (Blue Theme)
- Color: Blue background with blue accent
- Contains ViXra papers with summaries

### 📙 SSRN Section (Orange Theme)
- Color: Orange background with orange accent
- Contains SSRN papers with summaries

Each paper includes:
- ✅ Title with category badge (Jailbreak/RedTeam)
- ✅ Author names
- ✅ Publication date
- ✅ AI-generated summary
- ✅ PDF download link

## 🔧 Configuration Options

Edit `config/settings.py` to customize:

```python
DAYS_BACK = 5                    # Look back N days for papers
ARXIV_JAILBREAK_PAPERS = 3      # Number of jailbreak papers
ARXIV_REDTEAM_PAPERS = 2        # Number of red team papers
# Similar settings for ViXra and SSRN
```

## 📝 Logging

All execution logs are saved in the `logs/` directory with timestamps:
```
logs/run_20250930_143052.log
```

## 🐛 Troubleshooting

### Issue: "No module named 'crewai'"
**Solution**: Make sure you've activated the virtual environment and installed dependencies:
```bash
pip install -r requirements.txt
```

### Issue: "GEMINI_API_KEY not found"
**Solution**: Create `.env` file from `.env.example` and add your API key

### Issue: Email sending fails
**Solution**: 
- Verify Gmail App Password is correct (not regular password)
- Enable 2-Step Verification on your Google Account
- Check SMTP settings in `.env`

### Issue: "SMTPAuthenticationError"
**Solution**: 
- Use App Password, not regular Gmail password
- Make sure "Less secure app access" is not blocking (use App Passwords instead)

### Issue: No papers found
**Solution**:
- The search terms might be too specific
- Try increasing `DAYS_BACK` to 7 or 10 days
- Check internet connection
- Some sources might be temporarily unavailable

### Issue: Rate limiting from APIs
**Solution**:
- Add delays between requests
- Reduce the number of papers fetched
- Check API quotas for Gemini

## 🔒 Security Notes

- Never commit `.env` file to version control
- Keep your API keys and passwords secure
- Use App Passwords for Gmail (never use your main password)
- The `.env` file is already in `.gitignore`

## 📚 Dependencies

Key libraries used:
- `crewai`: Multi-agent orchestration
- `google-generativeai`: Gemini API integration
- `arxiv`: ArXiv API wrapper
- `feedparser`: RSS feed parsing for ViXra
- `beautifulsoup4`: Web scraping for SSRN
- `requests`: HTTP requests
- `python-dotenv`: Environment variable management

## 🎨 Customization

### Modify Email Template

Edit `utils/email_builder.py` to customize:
- Colors and styling
- Section layouts
- Content formatting

### Change Search Queries

Edit the fetch methods in:
- `agents/arxiv_agent.py`
- `agents/vixra_agent.py`
- `agents/ssrn_agent.py`

### Adjust Summary Format

Edit `utils/gemini_client.py` to modify the summary prompt

## 🔄 Scheduling (Optional)

### Windows Task Scheduler

1. Open Task Scheduler
2. Create Basic Task
3. Set trigger (e.g., daily at 9 AM)
4. Action: Start a program
5. Program: `C:\path\to\venv\Scripts\python.exe`
6. Arguments: `C:\path\to\main.py`
7. Start in: `C:\path\to\project`

### Linux Cron Job

```bash
# Edit crontab
crontab -e

# Run daily at 9 AM
0 9 * * * cd /path/to/project && /path/to/venv/bin/python main.py >> /path/to/logs/cron.log 2>&1
```

### macOS Launchd

Create `~/Library/LaunchAgents/com.llm.papermonitor.plist`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.llm.papermonitor</string>
    <key>ProgramArguments</key>
    <array>
        <string>/path/to/venv/bin/python</string>
        <string>/path/to/main.py</string>
    </array>
    <key>StartCalendarInterval</key>
    <dict>
        <key>Hour</key>
        <integer>9</integer>
        <key>Minute</key>
        <integer>0</integer>
    </dict>
</dict>
</plist>
```

## 📊 Output Examples

### Console Output
```
======================================================================
🚀 Starting LLM Security Research Paper Monitor
======================================================================

📦 Initializing agents...

======================================================================
STEP 1: Fetching papers from ArXiv
======================================================================
🔍 Fetching papers from ArXiv...
✅ Found 5 papers from ArXiv (3 jailbreak, 2 red team)

======================================================================
STEP 2: Fetching papers from ViXra
======================================================================
🔍 Fetching papers from ViXra...
✅ Found 5 papers from ViXra (3 jailbreak, 2 red team)

...

✅ PROCESS COMPLETED SUCCESSFULLY!
📧 Email sent to: xxxxxxxxx@gmail.com, yyyyyyyyyyyy@gmail.com
📚 Total papers: 15
```

## 🤝 Contributing

Feel free to submit issues, fork the repository, and create pull requests for any improvements.

## 📄 License

This project is open source and available under the MIT License.

## 🙏 Acknowledgments

- CrewAI for the multi-agent framework
- Google Gemini for AI summarization
- ArXiv, ViXra, and SSRN for providing research papers

## 📞 Support

If you encounter any issues:
1. Check the troubleshooting section
2. Review the logs in the `logs/` directory
3. Ensure all API keys are valid
4. Verify internet connectivity

---

**Happy Research! 🎓🔬**# Crew_agents
