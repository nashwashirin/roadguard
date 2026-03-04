# RoadGuard Quick Deploy Cheatsheet

## 🚀 Deploy in 2 Minutes

### Option 1: GitHub Pages (Easiest)
```bash
# 1. Create repo on github.com
# 2. Clone it locally
git clone https://github.com/yourusername/roadguard.git
cd roadguard

# 3. Copy your file
cp RoadGuard_updated__1_.html index.html

# 4. Push to GitHub
git add .
git commit -m "Deploy RoadGuard"
git push origin main

# 5. Enable Pages: Settings > Pages > Deploy from main branch
# Your site: https://yourusername.github.io/roadguard
```

---

### Option 2: Netlify (No Command Needed)
1. Go to [netlify.com](https://netlify.com)
2. Sign up (free)
3. Drag & drop `index.html` file
4. **Done!** Live in 30 seconds

---

### Option 3: Vercel CLI
```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
vercel

# Live immediately with URL like: roadguard.vercel.app
```

---

### Option 4: Local Testing (No Deploy)
```bash
# Python 3
python -m http.server 8000

# Then open:
# http://localhost:8000/RoadGuard_updated__1_.html
```

---

### Option 5: AWS S3 + CloudFront
```bash
# Install AWS CLI
aws configure  # Add your credentials

# Create S3 bucket
aws s3 mb s3://roadguard-app

# Upload file
aws s3 cp index.html s3://roadguard-app/

# Enable website hosting
aws s3 website s3://roadguard-app/ \
  --index-document index.html

# URL: http://roadguard-app.s3-website-us-east-1.amazonaws.com
```

---

### Option 6: Docker
```bash
# Create Dockerfile (see full guide)
docker build -t roadguard .

# Run locally
docker run -p 8080:80 roadguard

# Push to Docker Hub
docker push yourusername/roadguard:latest

# Deploy to any cloud with docker
```

---

### Option 7: Traditional VPS (DigitalOcean/AWS EC2)
```bash
# SSH into your server
ssh user@your_server_ip

# Install web server
sudo apt update
sudo apt install nginx -y

# Create web directory
sudo mkdir -p /var/www/roadguard

# Upload file (from your local machine)
scp index.html user@your_server_ip:/var/www/roadguard/

# Start Nginx
sudo systemctl start nginx
sudo systemctl enable nginx

# Visit: http://your_server_ip
```

---

## ✅ Pre-Deployment Checklist

- [ ] Rename file to `index.html`
- [ ] Test locally first
- [ ] Check Claude API key handling (requires backend)
- [ ] Verify all fonts load from Google Fonts CDN
- [ ] Test responsive design on mobile

---

## 🔑 API Key Security

### DON'T DO THIS (Exposed in Frontend):
```javascript
// ❌ WRONG - Your API key is visible to everyone!
const response = await fetch('https://api.anthropic.com/v1/messages', {
  headers: { 'x-api-key': 'sk-ant-...' }  // EXPOSED!
});
```

### DO THIS INSTEAD (Backend):
```javascript
// ✅ RIGHT - Call your own backend
const response = await fetch('/api/analyze', {
  method: 'POST',
  body: JSON.stringify({ data: 'safe' })
});
```

**Backend handler (Node.js):**
```javascript
app.post('/api/analyze', async (req, res) => {
  const response = await fetch('https://api.anthropic.com/v1/messages', {
    headers: { 'x-api-key': process.env.CLAUDE_API_KEY }  // ✅ SAFE
  });
  res.json(await response.json());
});
```

---

## 📊 Platform Comparison

| Feature | GitHub Pages | Netlify | Vercel | AWS | VPS |
|---------|---|---|---|---|---|
| **Cost** | Free | Free | Free | $0.50-5 | $4-20/mo |
| **Setup** | 5 min | 2 min | 3 min | 15 min | 20 min |
| **SSL** | ✅ Auto | ✅ Auto | ✅ Auto | ✅ Need | ⚠️ Manual |
| **Uptime** | 99.9% | 99.95% | 99.95% | 99.99% | Varies |
| **Custom Domain** | ✅ Easy | ✅ Easy | ✅ Easy | ✅ Need Route53 | ✅ Easy |
| **Best For** | Public repos | Quickest | Production | High-traffic | Control |

---

## 🎯 Recommended Deployment Path

1. **Development:** Run locally with `python -m http.server 8000`
2. **Testing:** Deploy to Netlify (drop file, 30 seconds)
3. **Production:** Use Vercel + Node.js backend for API key security
4. **Scale:** Move to AWS Lambda + S3 + CloudFront

---

## Troubleshooting

### File Not Loading
- Ensure filename is `index.html`
- Check file is in root directory
- Clear browser cache (Ctrl+Shift+Del)

### Styles Not Showing
- Google Fonts CDN should work from anywhere
- Check browser console for CORS errors
- Fonts load from `fonts.googleapis.com`

### Claude API Errors
- You must implement a backend proxy
- Frontend cannot safely store API keys
- See "API Key Security" section above

### Custom Domain Not Working
- DNS changes take 24-48 hours
- Check you're using `https://`
- Verify domain registration active

---

## 🚀 Deploy Now!

**Fastest option:** Drag & drop to [Netlify.com](https://netlify.com)
**Most control:** Self-hosted on [DigitalOcean](https://digitalocean.com)
**Production-grade:** [Vercel](https://vercel.com) + Node backend

---

## Need the Full Backend?

For API integration, create a simple Node.js server:

```javascript
const express = require('express');
const app = express();

app.use(express.static('.'));  // Serve HTML files

app.post('/api/analyze', async (req, res) => {
  try {
    const response = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'x-api-key': process.env.CLAUDE_API_KEY
      },
      body: JSON.stringify(req.body)
    });
    const data = await response.json();
    res.json(data);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

app.listen(3000);
```

Deploy this to Railway or Render (free tier).

---

Questions? Start with Netlify — it's the easiest! 🎯
