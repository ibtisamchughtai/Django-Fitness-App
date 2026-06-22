# Vercel Deployment Guide for Fitness Tracker

## Prerequisites
- GitHub account
- Vercel account (sign up at https://vercel.com)
- Git installed on your computer

## Step 1: Prepare Your Repository

1. Initialize git (if not already done):
   ```bash
   git init
   ```

2. Add all files to git:
   ```bash
   git add .
   git commit -m "Initial commit for Vercel deployment"
   ```

3. Create a new repository on GitHub and push your code:
   ```bash
   git remote add origin https://github.com/YOUR_USERNAME/fitness-tracker.git
   git branch -M main
   git push -u origin main
   ```

## Step 2: Deploy to Vercel

### Option A: Deploy via Vercel Dashboard (Recommended)

1. Go to https://vercel.com and sign in with GitHub
2. Click "Add New" → "Project"
3. Import your GitHub repository
4. Configure project:
   - **Framework Preset:** Other
   - **Root Directory:** ./
   - **Build Command:** (leave empty)
   - **Output Directory:** staticfiles

5. Add Environment Variables:
   Click "Environment Variables" and add:
   ```
   SECRET_KEY = your-secret-key-here (generate a new one)
   DEBUG = False
   ALLOWED_HOSTS = .vercel.app
   ```

6. Click "Deploy"

### Option B: Deploy via Vercel CLI

1. Install Vercel CLI:
   ```bash
   npm install -g vercel
   ```

2. Login to Vercel:
   ```bash
   vercel login
   ```

3. Deploy:
   ```bash
   vercel
   ```

4. Follow the prompts and set environment variables when asked

## Step 3: Configure Environment Variables in Vercel

Go to your project settings in Vercel dashboard:
1. Navigate to: Project → Settings → Environment Variables
2. Add these variables:

   **SECRET_KEY**
   ```
   Generate a new Django secret key:
   python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"
   ```

   **DEBUG**
   ```
   False
   ```

   **DATABASE_URL** (Optional - if using external database)
   ```
   Your PostgreSQL connection string
   ```

## Step 4: Important Notes

### Database Limitation
⚠️ **Vercel's serverless environment doesn't support SQLite for production.**

You have 3 options:

1. **Use as Demo (SQLite):** The app will work but data won't persist between deployments
2. **Add External Database:** Use Vercel Postgres, Neon, or Supabase
3. **Use for Static Demo:** Great for showcasing the UI/UX

### Adding Vercel Postgres (Recommended for Production):

1. In Vercel Dashboard → Storage → Create Database → Postgres
2. Follow the setup wizard
3. Environment variables will be automatically added
4. Redeploy your application

## Step 5: Run Migrations (If using Postgres)

After adding a database, you need to run migrations. Add this to your `build_files.sh`:

```bash
#!/bin/bash
pip install -r requirements.txt
python3.9 manage.py collectstatic --noinput --clear
python3.9 manage.py migrate --noinput
```

## Troubleshooting

### Build Fails
- Check that all dependencies are in `requirements.txt`
- Verify Python version compatibility (3.9)
- Check build logs in Vercel dashboard

### Static Files Not Loading
- Ensure `collectstatic` runs in build script
- Verify `STATIC_ROOT` and `STATIC_URL` in settings.py
- Check that WhiteNoise is installed and configured

### 500 Internal Server Error
- Check environment variables are set correctly
- Review function logs in Vercel dashboard
- Ensure `DEBUG=False` for production

### Database Connection Issues
- Verify `DATABASE_URL` environment variable
- Check database credentials
- Ensure database is accessible from Vercel's region

## Post-Deployment

### Create Superuser (If using Postgres)
You'll need to use Vercel's serverless function console or connect directly to your database to create a superuser.

### Custom Domain (Optional)
1. Go to Project → Settings → Domains
2. Add your custom domain
3. Follow DNS configuration instructions

## Useful Commands

**Redeploy:**
```bash
vercel --prod
```

**View Logs:**
```bash
vercel logs
```

**View Deployments:**
```bash
vercel ls
```

## Support

- Vercel Documentation: https://vercel.com/docs
- Django on Vercel: https://vercel.com/guides/deploying-django-with-vercel

## Current Config Files

✅ `vercel.json` - Vercel configuration
✅ `build_files.sh` - Build script
✅ `.vercelignore` - Files to exclude
✅ `requirements.txt` - Python dependencies
✅ `settings.py` - Updated with Vercel hosts
