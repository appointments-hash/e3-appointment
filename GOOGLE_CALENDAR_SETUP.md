# Google Calendar Integration Setup Guide

## Complete Setup Instructions for Appointment Manager Pro

This guide will walk you through setting up Google Calendar integration for your Appointment Manager Pro app.

---

## Prerequisites
- A Google account
- A modern web browser (Chrome, Firefox, Safari, or Edge)
- Basic understanding of web hosting (or willingness to learn!)

---

## Step 1: Create a Google Cloud Project

1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Click on the project dropdown at the top (it might say "Select a project")
3. Click "**NEW PROJECT**" in the top right
4. Enter a project name (e.g., "Appointment Manager")
5. Click "**CREATE**"
6. Wait for the project to be created (this takes a few seconds)
7. Make sure your new project is selected in the dropdown

---

## Step 2: Enable Google Calendar API

1. In the Google Cloud Console, open the navigation menu (☰) on the left
2. Go to "**APIs & Services**" → "**Library**"
3. In the search bar, type "**Google Calendar API**"
4. Click on "**Google Calendar API**" in the results
5. Click the blue "**ENABLE**" button
6. Wait for it to enable (takes a few seconds)

---

## Step 3: Configure OAuth Consent Screen

Before you can create credentials, you need to configure the OAuth consent screen:

1. Go to "**APIs & Services**" → "**OAuth consent screen**"
2. Select "**External**" as the user type
3. Click "**CREATE**"

### Fill out the App Information:
- **App name**: "Appointment Manager Pro" (or your preferred name)
- **User support email**: Your email address
- **App logo**: (Optional - you can skip this)
- **App domain**: Leave blank for now
- **Authorized domains**: Leave blank for local testing
- **Developer contact information**: Your email address

4. Click "**SAVE AND CONTINUE**"

### Scopes:
5. Click "**ADD OR REMOVE SCOPES**"
6. Filter for "calendar" in the search box
7. Select: `https://www.googleapis.com/auth/calendar` (Full access to Google Calendar)
8. Click "**UPDATE**"
9. Click "**SAVE AND CONTINUE**"

### Test Users:
10. Click "**ADD USERS**"
11. Enter your email address (the one you'll use to sign in)
12. Click "**ADD**"
13. Click "**SAVE AND CONTINUE**"

14. Review the summary and click "**BACK TO DASHBOARD**"

---

## Step 4: Create OAuth 2.0 Credentials

1. Go to "**APIs & Services**" → "**Credentials**"
2. Click "**+ CREATE CREDENTIALS**" at the top
3. Select "**OAuth client ID**"

### Configure the OAuth Client:
4. **Application type**: Select "**Web application**"
5. **Name**: "Appointment Manager Client" (or any name you prefer)

### Authorized JavaScript origins:
6. Click "**+ ADD URI**"
7. For **local testing**, add: `http://localhost:8000`
8. For **production hosting**, add your domain: `https://yourdomain.com`

### Authorized redirect URIs:
9. Click "**+ ADD URI**" 
10. For **local testing**, add: `http://localhost:8000`
11. For **production hosting**, add: `https://yourdomain.com`

12. Click "**CREATE**"

### Save Your Client ID:
13. A popup will appear showing your **Client ID** and **Client Secret**
14. **IMPORTANT**: Copy the **Client ID** - you'll need this!
    - It looks like: `123456789-abc123def456.apps.googleusercontent.com`
15. Click "**OK**"

---

## Step 5: Host Your Application

You cannot just open the HTML file directly in your browser due to security restrictions. You need to serve it through a web server.

### Option A: Local Testing (Python SimpleHTTPServer)

If you have Python installed:

**Python 3:**
```bash
cd /path/to/your/appointment-manager-folder
python -m http.server 8000
```

**Python 2:**
```bash
cd /path/to/your/appointment-manager-folder
python -m SimpleHTTPServer 8000
```

Then open your browser and go to: `http://localhost:8000/appointment-manager-gcal.html`

### Option B: Local Testing (VS Code Live Server)

1. Install VS Code
2. Install the "Live Server" extension
3. Right-click on the HTML file
4. Select "Open with Live Server"

### Option C: Production Hosting

For a live website that you can access from anywhere:

**Free Hosting Options:**
- **Netlify**: Easiest - just drag and drop your HTML file
- **Vercel**: Great for single-page apps
- **GitHub Pages**: Free hosting through GitHub
- **Google Firebase Hosting**: Fast and reliable

**Steps for Netlify (Recommended for beginners):**
1. Go to [netlify.com](https://netlify.com)
2. Sign up for a free account
3. Drag and drop your HTML file to deploy
4. Get a URL like: `https://your-app-name.netlify.app`
5. Go back to Google Cloud Console → Credentials
6. Update your OAuth client's Authorized JavaScript origins and Redirect URIs with your new Netlify URL

---

## Step 6: First-Time Setup in the App

1. Open your hosted application in a web browser
2. You'll see the setup screen
3. Paste your **Client ID** (from Step 4) into the input field
4. Click "**Save & Continue**"
5. Click "**Sign in with Google**"
6. Google will ask you to sign in and authorize the app
7. You might see a warning that "Google hasn't verified this app" - this is normal for apps in testing
8. Click "**Advanced**" → "**Go to Appointment Manager Pro (unsafe)**"
9. Review the permissions and click "**Allow**"
10. You're now connected!

---

## Step 7: Using the App

Once connected, you can:

✅ **Create appointments** that automatically sync to Google Calendar  
✅ **View all your Google Calendar events** in the app  
✅ **Edit and delete** your local appointments  
✅ **Filter** by source (local or Google)  
✅ **Sync anytime** with the "Sync Now" button  

### Features:
- **Dashboard**: Overview of your appointments
- **Calendar Widget**: Mini calendar showing days with appointments
- **Two-way sync**: Create in the app, see in Google Calendar
- **Status tracking**: Confirmed, Pending, Cancelled
- **Client information**: Store emails, phone numbers, notes

---

## Troubleshooting

### "Error 403: access_denied"
- Make sure you added your email as a test user in the OAuth consent screen
- Try signing out and signing back in

### "Error 400: redirect_uri_mismatch"
- Check that your Authorized JavaScript origins and Redirect URIs in Google Cloud Console match exactly where you're hosting the app
- For local testing, use `http://localhost:8000` (no trailing slash)

### "The app is blocked"
- You need to add yourself as a test user in the OAuth consent screen
- Or publish the app (though this requires verification for production use)

### Events not syncing
- Click the "Sync Now" button to manually sync
- Check that you're signed in to Google
- Make sure the Google Calendar API is enabled

### Can't sign in
- Clear your browser cache and cookies
- Try a different browser
- Check your Client ID is correct

---

## Security Notes

🔒 **Your data is secure:**
- The app only accesses YOUR Google Calendar (with your permission)
- The Client ID is safe to share publicly
- Never share your Client Secret (though we don't use it in this app)
- All communication happens directly between your browser and Google
- No data is stored on external servers (only in your browser's local storage)

---

## Moving to Production

When you're ready to use this professionally:

1. **Verify your app** through Google (required for > 100 users)
2. **Get a custom domain** for a professional look
3. **Set up proper hosting** (Netlify, Vercel, or your own server)
4. **Update OAuth settings** with production URLs
5. **Consider backup solutions** for your appointment data

---

## Support Resources

- [Google Calendar API Documentation](https://developers.google.com/calendar/api/quickstart/js)
- [OAuth 2.0 Guide](https://developers.google.com/identity/protocols/oauth2)
- [Google Cloud Console Help](https://console.cloud.google.com/support)

---

## What's Next?

Once you're up and running, you can:
- Customize the service types to match your business
- Adjust the color scheme to match your branding
- Add more features like email notifications
- Export appointment reports
- Integrate with other tools (CRM, email marketing, etc.)

---

**Need help?** The setup might seem complex at first, but follow each step carefully and you'll have it working in about 15 minutes!

Good luck with your Appointment Manager Pro! 🚀
