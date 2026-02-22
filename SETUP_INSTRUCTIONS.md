# AI Resume Chatbot - Setup Instructions

## What You'll Need
- Mac/Linux computer
- GitHub account (free)
- Anthropic API account with $5 credits
- Render.com account (free)
- Your 7 resume CSV files from Claude

## Step 1: Get Your Resume Data
Download the 7 CSV files Claude created from your resume and place them in the `data/` folder:
1. work_experience.csv
2. achievements.csv
3. skills_matrix.csv
4. projects.csv
5. community_involvement.csv
6. education.csv
7. context_metadata.json

## Step 2: Set Up Anthropic API Key
1. Go to https://console.anthropic.com/settings/keys
2. Sign up and add $5 in credits
3. Click "Create Key"
4. Copy the key (starts with `sk-ant-`)
5. Copy `.env.template` to `.env`:
```bash
   cp .env.template .env
```
6. Edit `.env` and replace `sk-ant-your-key-here` with your actual key

## Step 3: Create GitHub Repository
1. Go to https://github.com
2. Click "New repository"
3. Name: `my-resume-chatbot` (or your choice)
4. Make it **PUBLIC** (required for free Render)
5. Do NOT add README, .gitignore, or license
6. Click "Create repository"

## Step 4: Push Code to GitHub
```bash
cd path/to/chatbot-template
git init
git add .
git commit -m "Initial commit - AI resume chatbot"
git remote add origin https://github.com/YOUR-USERNAME/my-resume-chatbot.git
git branch -M main
git push -u origin main
```

## Step 5: Deploy to Render
1. Go to https://render.com and sign up
2. Click "New +" → "Web Service"
3. Connect your GitHub repository
4. Configure:
   - **Name**: `your-name-resume` (becomes `your-name-resume.onrender.com`)
   - **Region**: Oregon (US West) or closest to you
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `gunicorn app:app`
   - **Instance Type**: Free
5. Add Environment Variable:
   - **Key**: `ANTHROPIC_API_KEY`
   - **Value**: Your API key (same one from `.env`)
6. Click "Create Web Service"
7. Wait 3-5 minutes for first deployment

## Step 6: Update Frontend for Production
Once deployed, get your Render URL (e.g., `your-name-resume.onrender.com`)

Edit `static/chatbot.html`:
```bash
nano static/chatbot.html
```

Find (around line 323):
```javascript
const API_URL = 'http://localhost:5000/api/chat';
```

Change to:
```javascript
const API_URL = 'https://your-name-resume.onrender.com/api/chat';
```

Save and push:
```bash
git add static/chatbot.html
git commit -m "Update API URL for production"
git push
```

Render auto-deploys in 2-3 minutes.

## Step 7: Customize the Chatbot
Edit `static/chatbot.html` to personalize:

**Line 12** - Update title:
```html
<title>Ask About Your Name's Qualifications</title>
```

**Line 14** - Update header:
```html
<h2>💬 Ask About Your Name's Qualifications</h2>
```

**Lines 200-215** - Customize suggestions:
```html
<div class="suggestion" onclick="askQuestion('What Snowflake experience do you have?')">
    What Snowflake experience do you have?
</div>
```

**Optional** - Change colors by searching for hex codes like `#667eea`

Commit and push changes:
```bash
git add static/chatbot.html
git commit -m "Customize chatbot for my resume"
git push
```

## Step 8: Test!
Visit: `https://your-name-resume.onrender.com`

Ask: "What's your background?"

**Done!** 🎉

## Updating Your Resume
When you get a new job, skill, or achievement:

1. Edit the CSV file in `data/` folder
2. Commit and push:
```bash
   git add data/
   git commit -m "Added new project"
   git push
```
3. Render auto-deploys in 2-3 minutes
4. Test on your live URL

## Troubleshooting
- **"Application error"**: Check Render logs for specific error
- **"Network error"**: Verify API_URL in chatbot.html matches your Render URL
- **Claude doesn't respond**: Check Anthropic API key is set in Render environment variables
- **Project count wrong**: Verify all CSV files are in `data/` folder and restart deployment

## Cost
- **Anthropic API**: ~$1-2/month for typical usage (~100 recruiter questions)
- **Render hosting**: Free tier (750 hours/month, spins down after 15 min inactivity)
- **Total**: ~$1-2/month

## Support
For detailed guidance, see: COMPLETE_CHATBOT_BUILD_GUIDE.md
