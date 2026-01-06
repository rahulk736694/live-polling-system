# Deployment Guide for Live Polling System

## Quick Deployment Steps

### 1. Deploy MongoDB Database (Free)

1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Create a free account
3. Create a free cluster (M0 Sandbox)
4. Go to "Database Access" → Add Database User
   - Username: `pollinguser`
   - Password: Generate a secure password
5. Go to "Network Access" → Add IP Address
   - Add `0.0.0.0/0` (Allow access from anywhere)
6. Go to "Database" → Click "Connect" → "Connect your application"
7. Copy the connection string (looks like):
   ```
   mongodb+srv://pollinguser:<password>@cluster0.xxxxx.mongodb.net/polling?retryWrites=true&w=majority
   ```
8. Replace `<password>` with your actual password

---

### 2. Deploy Backend on Render (Free)

1. Go to [Render.com](https://render.com) and sign in with GitHub
2. Click **"New +"** → **"Web Service"**
3. Connect your repository: `https://github.com/rahulk736694/live-polling-system`
4. Configure the service:
   - **Name:** `live-polling-backend`
   - **Root Directory:** `backend`
   - **Environment:** `Node`
   - **Build Command:** `npm install`
   - **Start Command:** `node index.js`
   - **Instance Type:** `Free`

5. **Add Environment Variables:**
   - Click "Advanced" → "Add Environment Variable"
   - Add these variables:
     ```
     MONGODB_URI=mongodb+srv://pollinguser:<password>@cluster0.xxxxx.mongodb.net/polling
     PORT=5000
     NODE_ENV=production
     ```

6. Click **"Create Web Service"**
7. Wait for deployment (takes 2-3 minutes)
8. **Copy your backend URL** (e.g., `https://live-polling-backend.onrender.com`)

---

### 3. Deploy Frontend on Vercel (Free)

1. Go to [Vercel.com](https://vercel.com) and sign in with GitHub
2. Click **"Add New"** → **"Project"**
3. Import your repository: `rahulk736694/live-polling-system`
4. Configure the project:
   - **Framework Preset:** Vite
   - **Root Directory:** `frontend`
   - **Build Command:** `npm run build`
   - **Output Directory:** `dist`

5. **Add Environment Variables:**
   - Click "Environment Variables"
   - Add:
     ```
     VITE_BACKEND_URI=https://your-backend-url.onrender.com
     ```
   - Replace with your actual Render backend URL

6. Click **"Deploy"**
7. Wait for deployment (takes 1-2 minutes)
8. **Your app is live!** (e.g., `https://live-polling-system.vercel.app`)

---

### 4. Update Backend CORS Settings

After deployment, you need to update the backend to allow requests from your Vercel frontend URL.

The backend already has CORS enabled for all origins (`*`), which works for development.
For production, you might want to restrict it to your frontend URL only.

---

## Testing Your Deployment

1. Open your Vercel URL (e.g., `https://live-polling-system.vercel.app`)
2. Try creating a teacher session
3. Open another browser/tab as a student
4. Test the polling functionality

---

## Important Notes

- **Free Tier Limitations:**
  - Render: Backend goes to sleep after 15 minutes of inactivity
  - MongoDB Atlas: 512MB storage limit
  - Vercel: Unlimited bandwidth for hobby projects

- **First Request Delay:** If backend is asleep, first request takes 30-60 seconds

- **Custom Domain:** You can add custom domains in both Vercel and Render settings

---

## Troubleshooting

### Backend Issues:
- Check Render logs: Dashboard → Your Service → Logs
- Verify environment variables are set correctly
- Ensure MongoDB connection string is correct

### Frontend Issues:
- Check Vercel deployment logs
- Verify `VITE_BACKEND_URI` points to your Render URL
- Check browser console for errors (F12)

### Connection Issues:
- Ensure MongoDB Atlas allows connections from `0.0.0.0/0`
- Verify backend CORS settings
- Check that WebSocket connections work (Socket.IO)

---

## Alternative Deployment Options

### Backend Alternatives:
- **Railway.app** (easier, but limited free tier)
- **Fly.io** (more flexible, Docker-based)
- **Heroku** (paid only now)

### Frontend Alternatives:
- **Netlify** (similar to Vercel)
- **GitHub Pages** (requires some configuration)
- **Cloudflare Pages**

---

## Next Steps After Deployment

1. Test all features thoroughly
2. Monitor usage on free tiers
3. Consider upgrading if you need:
   - No sleep time on backend
   - More database storage
   - Custom domains
   - Better performance

---

## Support

If you encounter issues:
1. Check deployment logs
2. Verify all environment variables
3. Test MongoDB connection separately
4. Check browser console for frontend errors

Good luck with your deployment! 🚀
