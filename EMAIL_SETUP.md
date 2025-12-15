# Email Configuration for AgriCure Node Backend

## Issue: SMTP Connection Timeout on Render

Render blocks outgoing SMTP connections on ports 25, 465, and 587 by default to prevent spam. You need to use alternative email services.

## Recommended Solutions

### Option 1: Use SendGrid (Recommended for Production)

SendGrid offers 100 free emails per day and works reliably on Render.

1. **Sign up for SendGrid**: https://sendgrid.com/
2. **Create an API key**: 
   - Go to Settings > API Keys
   - Create API Key with "Mail Send" permission
   - Copy the API key
3. **Install SendGrid package**:
   ```bash
   npm install @sendgrid/mail
   ```
4. **Set environment variables in Render**:
   ```
   EMAIL_SERVICE=sendgrid
   SENDGRID_API_KEY=your_api_key_here
   EMAIL_USER=noreply@yourdomain.com
   ```

### Option 2: Use Resend (Modern Alternative)

Resend offers 100 free emails per day with a developer-friendly API.

1. **Sign up for Resend**: https://resend.com/
2. **Get API key from dashboard**
3. **Install Resend**:
   ```bash
   npm install resend
   ```
4. **Set environment variables in Render**:
   ```
   EMAIL_SERVICE=resend
   RESEND_API_KEY=re_your_api_key_here
   EMAIL_USER=noreply@yourdomain.com
   ```

### Option 3: Use Gmail with App Password

If you want to use Gmail (not recommended for production):

1. **Enable 2-Step Verification** on your Google account
2. **Create an App Password**:
   - Go to Google Account > Security
   - Under "2-Step Verification", find "App passwords"
   - Generate a password for "Mail"
3. **Set environment variables in Render**:
   ```
   EMAIL_SERVICE=gmail
   EMAIL_USER=your-email@gmail.com
   EMAIL_PASSWORD=your_app_password_here
   ```

**Note**: Gmail has daily sending limits (500 emails/day) and may not be reliable for production.

### Option 4: Use Mailgun

Mailgun offers 5,000 free emails per month.

1. **Sign up for Mailgun**: https://www.mailgun.com/
2. **Verify your domain or use sandbox domain**
3. **Get SMTP credentials from dashboard**
4. **Set environment variables in Render**:
   ```
   EMAIL_SERVICE=smtp
   SMTP_HOST=smtp.mailgun.org
   SMTP_PORT=587
   SMTP_SECURE=false
   EMAIL_USER=your-mailgun-username
   EMAIL_PASSWORD=your-mailgun-password
   ```

## Current Implementation

The email service now:
- ✅ Has increased timeout settings (10 seconds)
- ✅ Includes better TLS configuration
- ✅ Logs OTP to console if email credentials are missing
- ✅ Won't crash the server if email fails
- ✅ Supports multiple email services

## Environment Variables Needed

Add these to your Render dashboard (Environment tab):

```env
# Choose one email service
EMAIL_SERVICE=gmail  # or 'sendgrid', 'resend', 'smtp'

# For Gmail
EMAIL_USER=your-email@gmail.com
EMAIL_PASSWORD=your_app_password

# For SendGrid
SENDGRID_API_KEY=your_sendgrid_api_key

# For Resend
RESEND_API_KEY=your_resend_api_key

# For custom SMTP
SMTP_HOST=smtp.example.com
SMTP_PORT=587
SMTP_SECURE=false
```

## Testing Locally

To test email locally without sending real emails:
1. Don't set EMAIL_USER and EMAIL_PASSWORD
2. The OTP will be logged to the console instead
3. You can copy it from the terminal for testing

## Troubleshooting

- **Connection timeout**: Use SendGrid or Resend instead of Gmail/SMTP
- **Authentication failed**: Check your API key or app password
- **Domain not verified**: Some services require domain verification
- **Rate limiting**: Upgrade your plan or use a different service

## Next Steps

1. Choose an email service provider
2. Sign up and get credentials
3. Add environment variables to Render
4. Redeploy your service
5. Test registration with a real email
