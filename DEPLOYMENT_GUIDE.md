# RoadGuard Deployment Guide

## Project Overview
RoadGuard is a self-contained HTML web application for environmental compliance monitoring of construction sites. It's a single-file application with embedded CSS and JavaScript.

---

## ✅ Deployment Options (Ranked by Ease)

### **1. EASIEST: GitHub Pages (Free, Fast, No Setup)**
**Best for:** Quick demos, portfolios, public sharing

**Steps:**
1. Create a GitHub repository
2. Upload `RoadGuard_updated__1_.html` 
3. Rename it to `index.html`
4. Go to **Settings > Pages > Build from main branch**
5. Deploy link: `https://yourusername.github.io/roadguard`

**Pros:** Free, automatic HTTPS, one-click deploy
**Cons:** Public by default (unless you make repo private)

---

### **2. SIMPLE: Netlify (Free, Drag & Drop)**
**Best for:** Quick deployment with custom domain options

**Steps:**
1. Go to [netlify.com](https://netlify.com)
2. Sign up (free tier available)
3. Drag & drop `RoadGuard_updated__1_.html` into Netlify
4. Rename to `index.html` before upload
5. Instant deploy URL assigned

**Pros:** Super easy, free SSL, custom domains available
**Cons:** Free tier has storage limits

---

### **3. STANDARD: Vercel (Free, Optimized for Web Apps)**
**Best for:** Production-grade deployment

**Steps:**
1. Go to [vercel.com](https://vercel.com)
2. Import project or upload files
3. Vercel auto-deploys
4. Instant URL like: `roadguard.vercel.app`

**Pros:** Fast CDN, serverless support, free tier generous
**Cons:** Requires account setup

---

### **4. TRADITIONAL: Self-Hosted (VPS/Server)**
**Best for:** Maximum control, sensitive data

**Requirements:**
- VPS (AWS, DigitalOcean, Linode, etc.)
- Nginx or Apache webserver
- Domain name (optional)

**Basic Setup (Nginx on Ubuntu):**
```bash
# SSH into server
ssh user@your_server_ip

# Install Nginx
sudo apt update && sudo apt install nginx -y

# Create directory
sudo mkdir -p /var/www/roadguard

# Upload your file
scp RoadGuard_updated__1_.html user@your_server_ip:/var/www/roadguard/index.html

# Configure Nginx
sudo nano /etc/nginx/sites-available/default
```

**Nginx config block:**
```nginx
server {
    listen 80;
    server_name yourdomain.com;
    
    root /var/www/roadguard;
    index index.html;
    
    location / {
        try_files $uri $uri/ =404;
    }
}
```

**Pros:** Full control, can handle databases (MongoDB/MySQL integration ready)
**Cons:** Requires server management, costs $4–20/month

---

### **5. DOCKER (Containerized)**
**Best for:** Scalable deployment, DevOps workflows

**Dockerfile:**
```dockerfile
FROM nginx:alpine
COPY RoadGuard_updated__1_.html /usr/share/nginx/html/index.html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

**Build & Run:**
```bash
docker build -t roadguard .
docker run -p 8080:80 roadguard
```

**Deploy to:**
- **Docker Hub** (registry)
- **AWS ECS / EKS** (container orchestration)
- **Google Cloud Run** (serverless)
- **Heroku** (with docker.json)

---

### **6. ENTERPRISE: AWS S3 + CloudFront**
**Best for:** High-traffic, mission-critical apps

**Steps:**
1. Upload HTML to AWS S3
2. Enable static website hosting
3. Use CloudFront CDN for distribution
4. Custom SSL certificate
5. Route53 for DNS

**Estimated Cost:** $0.50–5/month

---

## 🚀 Quick Start Recommendation

### For Development/Demo:
```bash
# Option A: Local Python Server
python -m http.server 8000
# Then open http://localhost:8000/RoadGuard_updated__1_.html

# Option B: Local Node.js Server
npx http-server
```

### For Production:
**Use Netlify or Vercel** — 5 minutes, zero config, production-ready.

---

## 📋 Important Notes

### API Integration (Claude API)
Your app calls the Claude API (`api.anthropic.com`). To use AI analysis:
1. **Add API key to environment** (don't expose in HTML)
2. Create a **backend proxy** to handle requests securely:
   - Node.js/Express server
   - AWS Lambda
   - Or add CORS-enabled endpoint

### Current API Call (Line 1719):
```javascript
fetch('https://api.anthropic.com/v1/messages', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ model: 'claude-sonnet-4-20250514', max_tokens: 1000, ... })
})
```

**⚠️ This will fail** because you need an API key. Solution:

**Secure Backend (Node.js/Express):**
```javascript
app.post('/api/analyze', async (req, res) => {
  const response = await fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: { 
      'Content-Type': 'application/json',
      'x-api-key': process.env.CLAUDE_API_KEY  // ← Secure
    },
    body: JSON.stringify(req.body)
  });
  res.json(await response.json());
});
```

Then in HTML, call your endpoint:
```javascript
const response = await fetch('/api/analyze', { ... });
```

---

## 📦 Deployment Checklist

- [ ] Rename to `index.html`
- [ ] Test locally first
- [ ] Add CORS headers if calling APIs
- [ ] Set up Claude API key securely (backend only)
- [ ] Enable HTTPS (all platforms provide this)
- [ ] Test on mobile devices
- [ ] Set up custom domain (optional)
- [ ] Configure environment variables
- [ ] Add monitoring/analytics

---

## 🔗 Recommended Deployment Platform Comparison

| Platform | Cost | Setup Time | Best For | URL Format |
|----------|------|-----------|----------|-----------|
| **GitHub Pages** | Free | 5 min | Public demos | `user.github.io/repo` |
| **Netlify** | Free-$99 | 2 min | Quickest deploy | `sitename.netlify.app` |
| **Vercel** | Free-$20 | 3 min | Next.js/React | `app.vercel.app` |
| **AWS S3+CloudFront** | $0.50-5 | 15 min | High traffic | Custom domain |
| **DigitalOcean** | $4-24 | 20 min | Full control | Custom domain |
| **Heroku** | Free (ending) | 10 min | Hobby projects | `app.herokuapp.com` |

---

## 🎯 Recommended Stack for Production

```
RoadGuard (Frontend)
    ↓
Backend (Node.js/Python)
    ↓
Claude API (AI Analysis)
    ↓
MongoDB (Environmental Readings)
MySQL (Compliance Limits)
```

**Host on:** Vercel (frontend) + AWS Lambda/Railway (backend)

---

## Need Help?

- **Local testing:** Open `RoadGuard_updated__1_.html` directly in browser
- **Domain setup:** Use Namecheap or GoDaddy ($1–3/year)
- **SSL cert:** Automatically included with Netlify/Vercel
- **Backups:** Git repository ensures version control

**Next step:** Choose your platform above and deploy! 🚀
