# Quick Troubleshooting Guide

## "Won't let me connect Google Calendar"

### Step 1: Open Browser Console
**This is the most important step!**

1. **Right-click** anywhere on the page
2. Select **"Inspect"** or **"Inspect Element"**
3. Click the **"Console"** tab
4. Look for messages starting with ✅ ❌ 🔄 or ⚠️

### Step 2: Check What You See

Take a screenshot of the console and look for these specific messages:

---

## Common Issues & Solutions:

### Issue 1: "No Client ID found" or Client ID format invalid
**Problem:** Your Client ID wasn't entered correctly

**Solution:**
1. Make sure you copied the ENTIRE Client ID
2. It should look like: `123456789-abc123def456.apps.googleusercontent.com`
3. No extra spaces before or after
4. Clear your browser data and re-enter it

---

### Issue 2: "redirect_uri_mismatch" or "Error 400"
**Problem:** Your Netlify URL doesn't match what's in Google Cloud Console

**Solution:**
1. Copy your EXACT Netlify URL (e.g., `https://your-app.netlify.app`)
2. Go to [Google Cloud Console](https://console.cloud.google.com)
3. Go to: APIs & Services → Credentials
4. Click on your OAuth 2.0 Client ID
5. Under "Authorized JavaScript origins" add your Netlify URL
6. Under "Authorized redirect URIs" add your Netlify URL
7. Click **SAVE**
8. Wait 5 minutes for changes to take effect
9. Try again

**Important:** The URLs must match EXACTLY:
- ✅ `https://my-app.netlify.app` (correct)
- ❌ `https://my-app.netlify.app/` (wrong - has trailing slash)
- ❌ `http://my-app.netlify.app` (wrong - http instead of https)

---

### Issue 3: "access_denied" or "This app is blocked"
**Problem:** You're not added as a test user

**Solution:**
1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Go to: APIs & Services → OAuth consent screen
3. Scroll down to "Test users"
4. Click **"+ ADD USERS"**
5. Enter the email address you're trying to sign in with
6. Click **ADD**
7. Try signing in again

---

### Issue 4: Button does nothing when clicked
**Problem:** JavaScript isn't loading or Client ID is wrong

**Solution:**
1. Check the browser console for red error messages
2. Make sure you're accessing via `https://` not opening the file directly
3. Try a different browser (Chrome works best)
4. Clear cache: Ctrl+Shift+Delete (Windows) or Cmd+Shift+Delete (Mac)

---

### Issue 5: "Google Calendar API has not been used in project..."
**Problem:** The API isn't enabled

**Solution:**
1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Make sure the correct project is selected (top dropdown)
3. Go to: APIs & Services → Library
4. Search for "Google Calendar API"
5. Click on it
6. Click **ENABLE**
7. Wait 2-3 minutes
8. Try again

---

## Testing Checklist:

Before asking for help, verify:

- [ ] Client ID ends with `.apps.googleusercontent.com`
- [ ] Google Calendar API is **enabled** in your project
- [ ] You're a **test user** in OAuth consent screen
- [ ] Your Netlify URL is in **Authorized JavaScript origins**
- [ ] Your Netlify URL is in **Authorized redirect URIs**
- [ ] You've waited 5 minutes after making changes in Google Cloud
- [ ] You're accessing via https:// (not opening file directly)
- [ ] Browser console shows no red errors
- [ ] You've cleared browser cache

---

## Still Not Working?

1. **Take a screenshot** of your browser console (with errors visible)
2. **Take a screenshot** of your Google Cloud Console credentials page
3. **Copy the exact error message** you see
4. Share these with support

---

## Quick Reset:

If nothing works, try this complete reset:

1. Go to Google Cloud Console → Credentials
2. **Delete** your existing OAuth client
3. **Create a new one** following the setup guide exactly
4. Get the new Client ID
5. Clear browser data (Ctrl+Shift+Delete)
6. Close and reopen browser
7. Go to your Netlify URL
8. Enter the NEW Client ID
9. Try connecting again

---

## Need More Help?

The browser console will tell you exactly what's wrong. Look for:
- ✅ = Success
- ❌ = Error (this is what we need to see!)
- 🔄 = Processing
- ⚠️  = Warning

Copy the error messages and we can solve it quickly!
