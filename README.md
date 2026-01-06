# GhostWriter-Bot

## 🤖 DevOps-GhostWriter - Multi-Agent PR Review System

A production-grade GitHub App that automatically reviews Pull Requests using three specialized AI agents:
- **Security Auditor**: Scans for security vulnerabilities
- **Runtime Validator**: Detects runtime logic flaws
- **Ghostwriter**: Synthesizes findings into professional PR comments

## 🚀 Deployment on Render

### Prerequisites
1. A GitHub App created at https://github.com/settings/apps
2. A Render account at https://render.com
3. A Groq API key (if using Groq for agents)

### Step-by-Step Deployment Guide

#### 1. Prepare Your Repository
Ensure all changes are committed and pushed to GitHub:
```bash
git add .
git commit -m "Prepare for Render deployment"
git push origin main
```

#### 2. Create Web Service on Render
1. Log in to [Render Dashboard](https://dashboard.render.com/)
2. Click **"New +"** → **"Web Service"**
3. Connect your GitHub repository: `Bikram-Mondal3/GhostWriter-Bot`
4. Configure the service:
   - **Name**: `ghostwriter-bot` (or your preferred name)
   - **Region**: Choose closest to your users
   - **Branch**: `main`
   - **Runtime**: `Python 3`
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `uvicorn main:app --host 0.0.0.0 --port $PORT`
   - **Instance Type**: `Free`

#### 3. Configure Environment Variables
In the Render dashboard, add these environment variables:

| Variable Name | Value | Notes |
|--------------|-------|-------|
| `GITHUB_APP_ID` | Your GitHub App ID | Found in GitHub App settings |
| `GITHUB_WEBHOOK_SECRET` | Your webhook secret | Created when setting up GitHub App |
| `GITHUB_PRIVATE_KEY` | Full private key content | **Include BEGIN/END lines** |
| `GROQ_API_KEY` | Your Groq API key | Optional, if using Groq |

**Important for `GITHUB_PRIVATE_KEY`:**
- Copy the **entire** content from `private-key.pem`
- Include the `-----BEGIN RSA PRIVATE KEY-----` and `-----END RSA PRIVATE KEY-----` lines
- Keep all line breaks intact
- In Render, paste it as a multi-line value

#### 4. Deploy
1. Click **"Create Web Service"**
2. Render will automatically build and deploy your app
3. Wait for deployment to complete (usually 2-3 minutes)
4. Copy your deployment URL: `https://ghostwriter-bot.onrender.com`

#### 5. Update GitHub App Webhook
1. Go to your GitHub App settings: https://github.com/settings/apps
2. Click on your app
3. Scroll to **"Webhook"** section
4. Update **Webhook URL**: `https://your-app-name.onrender.com/webhook`
5. Ensure **Webhook secret** matches `GITHUB_WEBHOOK_SECRET`
6. Enable webhook
7. Set permissions:
   - **Pull requests**: Read & Write
   - **Contents**: Read-only
   - **Metadata**: Read-only
8. Subscribe to events:
   - Pull request (opened, synchronize, reopened)

#### 6. Test Your Deployment
1. Create a test Pull Request in a repository where the app is installed
2. Check Render logs to see if webhook is received
3. Verify the bot posts a review comment

### 🔧 Monitoring and Logs
- View logs in Render Dashboard → Your Service → Logs
- Monitor performance and uptime
- Check for any errors or warnings

### ⚠️ Important Notes
- **Free tier limitations**:
  - Service spins down after 15 minutes of inactivity
  - First request after sleep may take 30-60 seconds (cold start)
  - 750 hours/month limit
- **Keep secrets safe**: Never commit `.env` or `private-key.pem` to Git
- **Private key format**: Must be multi-line with proper line breaks

### 🆘 Troubleshooting

**Service won't start:**
- Check environment variables are set correctly
- Verify `GITHUB_PRIVATE_KEY` includes BEGIN/END lines
- Review build logs for missing dependencies

**Webhook not working:**
- Verify webhook URL in GitHub App settings
- Check webhook secret matches
- Review Render logs for incoming requests

**Authentication errors:**
- Ensure GitHub App ID is correct
- Verify private key format (must be multi-line)
- Check app permissions and installation

### 📝 Local Development
```bash
# Install dependencies
pip install -r requirements.txt

# Copy environment template
cp .env.example .env

# Edit .env with your credentials
# Run locally
uvicorn main:app --reload
```

### 🔗 Useful Links
- [Render Documentation](https://render.com/docs)
- [GitHub Apps Documentation](https://docs.github.com/apps)
- [FastAPI Documentation](https://fastapi.tiangolo.com/)

---

## 📄 License
MIT License