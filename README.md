# 🤖 AI News to LinkedIn Automation (n8n + Groq API)

## 📘 Overview
This project is an **end-to-end n8n automation** that automatically fetches the latest **AI-related news**, summarizes each article using a **Groq LLM (via Groq API)**, and posts the final content to **LinkedIn** using the **UGC API**.

It’s designed to eliminate manual effort in:
- Collecting AI/ML/Tech news
- Summarizing using an AI model
- Formatting posts for LinkedIn engagement
- Publishing automatically on schedule

---

## 🧠 Key Features
- ⚡ **Groq API–powered summarization** (fast inference, low latency)
- 📰 Fetches news via **NewsAPI**
- ✍️ Auto-generates engaging LinkedIn posts (with hashtags & CTAs)
- 🔄 Fully automated through **n8n**
- 🚀 Posts directly via **LinkedIn UGC API**
- 🧩 Automatically avoids duplicate posts

---

## ⚙️ Workflow Architecture

### 🧩 Step-by-Step Process

1. **🔔 Schedule Trigger**  
   Runs automatically every morning (configurable) to start the flow.

2. **📰 Fetch News**  
   - Fetches top AI-related articles using **NewsAPI**:  
     `https://newsapi.org/v2/everything?q=artificial+intelligence+OR+machine+learning&sortBy=publishedAt&language=en`
   - Extracts: title, description, source, URL, and image.

3. **🧠 Summarization using Groq API**  
   - Sends article title + description to **Groq LLM**.  
   - Model: `mixtral-8x7b` or `llama3-70b` (depending on your node setup).  
   - Returns structured JSON output:
     ```json
     {
       "hook": "AI takes center stage again!",
       "summary": "OpenAI unveils GPT-5 — a new multimodal LLM that merges reasoning, language, and perception.",
       "hashtags": "#AI #OpenAI #Innovation",
       "link_cta": "Read more: https://example.com/article"
     }
     ```

4. **✍️ Post Formatting**  
   Combines the Groq output into a LinkedIn-friendly message:
   ```json{
    {{ $json.output.hook }}
    {{ $json.output.summary }}
    {{ $json.output.link_cta }}
    
    {{ $json.output.hashtags }}
    (Post ID: {{Date.now()}})


5. **📤 Post to LinkedIn**  
- Uses LinkedIn **UGC API**:  
  `https://api.linkedin.com/v2/ugcPosts`
- Sends authorized POST request with headers:
  ```
  Authorization: Bearer <YOUR_ACCESS_TOKEN>
  Content-Type: application/json
  X-Restli-Protocol-Version: 2.0.0
  ```
- Posts under your profile (`urn:li:person:W-4upPaK9W`).

6. **🧩 Optional Nodes**
- Filters to skip already-posted articles.
- Error handlers for API rate limits.
- Logging success/failure in n8n UI.

---

## 🧰 Tech Stack

| Tool / API | Purpose |
|-------------|----------|
| **n8n** | Workflow orchestration |
| **Groq API** | LLM inference (text summarization & content generation) |
| **NewsAPI** | Fetching latest AI/Tech articles |
| **LinkedIn UGC API** | Publishing posts |
| **LangChain JSON parsers** | Structuring Groq outputs |

---

## 🖼️ Screenshots

| Step | Screenshot |
|------|-------------|
| Workflow Overview | ![workflow](./screenshots/workflow_overview.png) |
| Groq Output | ![groq_output](./screenshots/groq_output.png) |
| LinkedIn Post | ![linkedin_post](./screenshots/linkedin_post.png) |
| Execution Log | ![log](./screenshots/execution_log.png) |

All screenshots are included inside the `/screenshots` folder.

---

## 🧩 Setup Instructions

### 1️⃣ Import the Workflow
- Open n8n → *Workflows → Import from File*
- Upload: `AI_News_to_LinkedIn_Workflow.json`

### 2️⃣ Add Credentials
- **NewsAPI Key** → for fetching articles  
- **Groq API Key** → for summarization  
- **LinkedIn Access Token** → for posting  

### 3️⃣ Run the Flow
- Test manually once
- Then enable **Schedule Trigger** for daily automation

---

## 🧪 Example Output

> 🤖 AI takes center stage again!  
> OpenAI unveils GPT-5 — a new multimodal LLM that merges reasoning, language, and perception.  
> Read more: https://techcrunch.com/2025/10/30/openai-gpt5-launch  
>  
> #ArtificialIntelligence #OpenAI #Innovation  
> _(Post ID: 1730489271123)_

---

## 👤 Author

**Bondalapati Bhargava Sai Abhinay**  
🎓 B.Tech (AIML), Final Year  
📍 Hyderabad, Telangana  
📧 bhargavasaiabhinay.b@gmail.com  
📞 +91 7989104567  
🔗 [LinkedIn Profile](https://www.linkedin.com/in/bhargavasaiabhinay)

---

## 🔗 Resources
- [Groq API Documentation](https://console.groq.com/docs)
- [n8n Docs](https://docs.n8n.io)
- [NewsAPI Docs](https://newsapi.org/docs)
- [LinkedIn UGC API Reference](https://learn.microsoft.com/en-us/linkedin/marketing/integrations/community-management/shares/ugc-post-api)

---

## ✅ Future Improvements
- Add **image upload** with each post (LinkedIn asset upload API)
- Store posted article URLs to prevent duplicates
- Generate AI-based image thumbnails via DALL·E or Replicate
- Add Slack/Email notifications after posting

---

⭐ **This automation demonstrates Groq-powered AI integration with real-world content pipelines (LinkedIn).**
