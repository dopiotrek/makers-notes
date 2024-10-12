# Setup Google Social Login in the Local Environment

- Google Social Login
- Supabase Local Environment (Docker)

## Step 1: Configure Google OAuth in config.toml

[auth.external.google]
enabled = true
client_id = "env(GOOGLE_CLIENT_ID)"
secret = "env(GOOGLE_CLIENT_SECRET)"
redirect_uri = "http://localhost:54321/auth/v1/callback"
url = ""

## Step 2: Stop and start Supabase

- supabase stop
- supabase start

## Configure Google Oath 2.0 in Google Cloud Console

- Authorized JavaScript origins:
  - http://localhost:5173
  - https://yourcustomdomain.com
- Authorized redirect URIs:
  - https://<project_id>.supabase.co/auth/v1/callback
  - http://localhost:54321/auth/v1/callback
  - http://localhost:5173/auth/callback
  - http://localhost:54321/auth/v1/callback
