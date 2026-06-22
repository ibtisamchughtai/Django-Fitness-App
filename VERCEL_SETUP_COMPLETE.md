# Vercel Deployment Setup Guide

## ✅ Completed Steps

1. **vercel.json Configuration**: Created with Python runtime and proper routing
2. **Static Files**: Collected and committed to repository (130 files)
3. **Git Repository**: All changes pushed to GitHub

## 🔧 Required: Vercel Dashboard Configuration

### Step 1: Add Vercel Postgres Database

**Why needed**: SQLite (db.sqlite3) doesn't persist on Vercel's serverless platform. You need a PostgreSQL database.

1. Go to your Vercel project dashboard
2. Click on the **Storage** tab
3. Click **Create Database**
4. Select **Postgres**
5. Choose **Database Name**: `fitness-tracker-db` (or any name)
6. Select **Region**: Choose closest to your users
7. Click **Create**

**What happens**: Vercel will automatically add these environment variables to your project:
- `POSTGRES_URL`
- `POSTGRES_PRISMA_URL`
- `POSTGRES_URL_NON_POOLING`
- `POSTGRES_USER`
- `POSTGRES_HOST`
- `POSTGRES_PASSWORD`
- `POSTGRES_DATABASE`

### Step 2: Configure Environment Variables

1. Go to your project **Settings** → **Environment Variables**
2. Add these required variables:

```
SECRET_KEY = production-secret-key-generate-a-random-string-here
```
**Generate SECRET_KEY**: Run this command locally:
```bash
python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"
```

```
DEBUG = False
```

```
DATABASE_URL = ${POSTGRES_URL}
```
(Use the existing POSTGRES_URL that was auto-created)

```
ALLOWED_HOSTS = .vercel.app
```

3. Set **Environment** for each variable:
   - Select: **Production**, **Preview**, and **Development**

### Step 3: Update Django Settings for Postgres

Your settings.py is already configured to use `dj_database_url`, but you need to ensure migrations run.

After deploying, you'll need to run migrations. Vercel doesn't support this automatically, so you have two options:

**Option A: Run migrations locally against production database**
1. Copy the `POSTGRES_URL` from Vercel environment variables
2. Run locally:
```bash
DATABASE_URL="your-postgres-url-here" python manage.py migrate
```

**Option B: Create a temporary script**
Add this to a file called `migrate.py` in your project root:
```python
import os
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'fitness_project.settings')
import django
django.setup()
from django.core.management import call_command
call_command('migrate')
```

Then run: `vercel env pull` and `python migrate.py`

### Step 4: Redeploy

After adding the database and environment variables:

1. Vercel will automatically redeploy when you pushed to GitHub
2. Or manually: Click **Deployments** → **Redeploy**

### Step 5: Create Superuser (Admin Access)

To access Django admin, you need to create a superuser in production:

1. Install Vercel CLI: `npm install -g vercel`
2. Link your project: `vercel link`
3. Run command in production:
```bash
vercel exec -- python manage.py createsuperuser
```

Or use Option A from Step 3 with:
```bash
DATABASE_URL="your-postgres-url-here" python manage.py createsuperuser
```

## 📋 Deployment Checklist

- [ ] Vercel Postgres database created
- [ ] SECRET_KEY environment variable set
- [ ] DEBUG=False environment variable set
- [ ] DATABASE_URL environment variable set
- [ ] ALLOWED_HOSTS environment variable set
- [ ] Project redeployed
- [ ] Migrations run on production database
- [ ] Superuser created
- [ ] Test login at your-app.vercel.app

## 🚨 Important Notes

1. **Database**: Your local SQLite data will NOT transfer to production. You'll start with an empty database.

2. **Static Files**: Now served directly from your repository (that's why we committed staticfiles/)

3. **Migrations**: Unlike Render, Vercel doesn't auto-run migrations. You must run them manually.

4. **Logs**: View deployment logs in Vercel dashboard under **Deployments** → Click deployment → **View Logs**

5. **Cold Starts**: First request after inactivity may take 5-10 seconds (serverless limitation)

## 🐛 Troubleshooting

If deployment fails:
1. Check **Deployments** → Latest deployment → **View Logs**
2. Verify all environment variables are set correctly
3. Ensure DATABASE_URL is properly configured

Common issues:
- "Secret key not set": Add SECRET_KEY environment variable
- "Database error": Ensure Postgres database is created and DATABASE_URL is set
- "Static files 404": Check that staticfiles/ directory is in your git repository

## 📱 Testing Your Deployment

Once deployed:
1. Visit `https://your-project.vercel.app/login/`
2. Try registering a new account (database should work)
3. Check that preloader shows on login page
4. Verify navigation menu works
5. Test dashboard, workouts, and other features

Your app should now be live on Vercel! 🚀
