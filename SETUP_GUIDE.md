# 🚀 Sahayak AI Assistant - Complete Setup & Deployment Guide

## 📖 Table of Contents
1. [Local Development Setup](#local-development-setup)
2. [Phase-by-Phase Implementation](#phase-by-phase-implementation)
3. [Deployment Guide](#deployment-guide)
4. [Testing](#testing)
5. [Troubleshooting](#troubleshooting)

---

## 🏠 Local Development Setup

### System Requirements
- Python 3.8 or higher
- Node.js 16 or higher
- npm or yarn
- Tesseract OCR
- Git

### Install Tesseract OCR

**Ubuntu/Debian:**
```bash
sudo apt-get update
sudo apt-get install tesseract-ocr
```

**MacOS:**
```bash
brew install tesseract
```

**Windows:**
Download and install from: https://github.com/UB-Mannheim/tesseract/wiki

### Backend Setup

1. **Navigate to backend directory:**
```bash
cd backend
```

2. **Create virtual environment (recommended):**
```bash
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate
```

3. **Install dependencies:**
```bash
pip install -r requirements.txt
```

4. **Install Playwright browsers:**
```bash
playwright install
playwright install-deps
```

5. **Configure environment:**
```bash
cp .env.example .env
# Edit .env and add your Hugging Face API key
```

6. **Run the backend:**
```bash
python main.py
```

Backend should now be running on `http://localhost:8000`

Test it: `curl http://localhost:8000`

### Frontend Setup

1. **Navigate to frontend directory:**
```bash
cd frontend
```

2. **Install dependencies:**
```bash
npm install
```

3. **Configure environment:**
```bash
cp .env.example .env
# Edit .env if needed (defaults should work for local development)
```

4. **Start development server:**
```bash
npm start
```

Frontend should now be running on `http://localhost:3000`

---

## 📊 Phase-by-Phase Implementation

### Phase 1: Project Foundation ✅
**What it does:** Basic web app with React frontend and FastAPI backend

**Test it:**
1. Start backend: `python main.py`
2. Start frontend: `npm start`
3. Open http://localhost:3000
4. You should see the Sahayak interface

### Phase 2: AI Brain Integration ✅
**What it does:** Converts user commands into actionable plans using LLM

**Test it:**
```bash
curl -X POST http://localhost:8000/api/execute \
  -H "Content-Type: application/json" \
  -d '{"command": "search for AI news on Google"}'
```

### Phase 3: Browser Control Automation ✅
**What it does:** Uses Playwright to control real browser

**Test it:**
1. Enter command: "Go to google.com"
2. Watch the backend logs for browser actions
3. Check screenshots in response

### Phase 4: Vision Capabilities ✅
**What it does:** OCR and vision model analyze screenshots

**Test it:**
1. Execute any command that opens a website
2. Check the response for `vision_analysis` data
3. Should include extracted text and description

### Phase 5: Autonomous Operation Loop ✅
**What it does:** AI observes, decides, acts, and verifies in a loop

**Test it:**
1. Command: "Search for Python tutorials on Google"
2. Watch it execute multiple steps automatically
3. Each step should verify success before continuing

### Phase 6: Self-Healing Selectors ✅
**What it does:** Finds alternative selectors when primary fails

**Test it:**
1. Modify a selector in ai_brain.py to be incorrect
2. Run a command
3. System should automatically try alternative selectors

### Phase 7: Memory System ✅
**What it does:** Stores user preferences and learns patterns

**Test it:**
```bash
# Save memory
curl -X POST http://localhost:8000/api/memory/save?user_id=test \
  -H "Content-Type: application/json" \
  -d '{"key": "favorite_search", "value": "AI news", "category": "preferences"}'

# Retrieve memory
curl http://localhost:8000/api/memory/test
```

Check `sahayak_memory.db` file is created

### Phase 8: Human-Like Behavior ✅
**What it does:** Random delays and mouse movements

**Test it:**
1. Execute any command
2. Watch timing between actions (should vary)
3. Check logs for delay information

### Phase 9: Privacy Layer ✅
**What it does:** Blurs sensitive information in screenshots

**Test it:**
1. Navigate to a login page
2. Check screenshot in response
3. Password fields should be blurred

### Phase 10: Deployment ✅
**What it does:** Production-ready deployment configuration

**See deployment section below**

---

## 🌐 Deployment Guide

### Deploy Backend to Railway

1. **Install Railway CLI:**
```bash
npm install -g @railway/cli
```

2. **Login to Railway:**
```bash
railway login
```

3. **Initialize project:**
```bash
cd backend
railway init
```

4. **Add environment variables:**
```bash
railway variables set HUGGINGFACE_API_KEY=your_key_here
```

5. **Deploy:**
```bash
railway up
```

6. **Get your backend URL:**
```bash
railway status
# Copy the URL (e.g., https://your-app.railway.app)
```

### Deploy Frontend to Vercel

1. **Install Vercel CLI:**
```bash
npm install -g vercel
```

2. **Login to Vercel:**
```bash
vercel login
```

3. **Deploy:**
```bash
cd frontend
vercel
```

4. **Configure environment variables:**
```bash
# In Vercel dashboard, add:
# REACT_APP_API_URL=https://your-backend.railway.app
# REACT_APP_WS_URL=wss://your-backend.railway.app/ws/live
```

5. **Redeploy with environment variables:**
```bash
vercel --prod
```

### Database Setup (Supabase - Optional)

1. Create account at https://supabase.com
2. Create new project
3. Get connection string
4. Update `memory_manager.py` to use Supabase instead of SQLite
5. Add Supabase credentials to Railway environment variables

---

## 🧪 Testing

### Backend API Tests

**Health Check:**
```bash
curl http://localhost:8000/
```

**Execute Command:**
```bash
curl -X POST http://localhost:8000/api/execute \
  -H "Content-Type: application/json" \
  -d '{
    "command": "go to google.com",
    "user_id": "test_user"
  }'
```

**Save Memory:**
```bash
curl -X POST http://localhost:8000/api/memory/save?user_id=test \
  -H "Content-Type: application/json" \
  -d '{
    "key": "email",
    "value": "test@example.com",
    "category": "autofill"
  }'
```

**Get Memory:**
```bash
curl http://localhost:8000/api/memory/test
```

### Frontend Tests

1. Open http://localhost:3000
2. Try example commands:
   - "Search for AI news on Google"
   - "Go to youtube.com"
   - "Open github.com"
3. Check for:
   - Commands appearing in chat
   - Step-by-step execution
   - Screenshots displaying
   - Success/failure indicators

---

## 🔧 Troubleshooting

### Backend Issues

**Issue: ImportError for modules**
```bash
# Solution: Reinstall dependencies
pip install -r requirements.txt --force-reinstall
```

**Issue: Playwright browser not found**
```bash
# Solution: Install browsers
playwright install chromium
playwright install-deps
```

**Issue: Tesseract not found**
```bash
# Ubuntu/Debian:
sudo apt-get install tesseract-ocr

# Mac:
brew install tesseract

# Then verify:
tesseract --version
```

**Issue: Database locked error**
```bash
# Solution: Close existing connections
rm sahayak_memory.db  # Delete and recreate
python main.py
```

**Issue: Hugging Face API errors**
```bash
# Check API key is valid
# Verify key in .env file
# Try different model or increase timeout
```

### Frontend Issues

**Issue: Cannot connect to backend**
```bash
# Check .env file has correct API URL
# Verify backend is running
# Check CORS settings in backend main.py
```

**Issue: WebSocket connection failed**
```bash
# Update WS_URL in .env
# Check firewall settings
# Verify WebSocket support on hosting platform
```

**Issue: Build errors**
```bash
# Clear cache and reinstall
rm -rf node_modules package-lock.json
npm install
npm run build
```

### Common Errors

**"Module not found"**
- Run `pip install -r requirements.txt` in backend
- Run `npm install` in frontend

**"Port already in use"**
- Kill process using port 8000: `lsof -ti:8000 | xargs kill -9`
- Change port in main.py

**"Vision model timeout"**
- Increase timeout in vision_processor.py
- Use lighter vision model
- Check internet connection

---

## 📚 Additional Resources

- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [React Documentation](https://react.dev/)
- [Playwright Documentation](https://playwright.dev/)
- [Hugging Face Models](https://huggingface.co/models)
- [Railway Deployment](https://docs.railway.app/)
- [Vercel Deployment](https://vercel.com/docs)

---

## 🎯 Next Steps

After successful setup:
1. Test all example commands
2. Try custom commands
3. Check memory functionality
4. Review screenshots for privacy layer
5. Monitor performance and logs
6. Deploy to production

---

**Need help? Open an issue on GitHub or refer to the troubleshooting section above.**
