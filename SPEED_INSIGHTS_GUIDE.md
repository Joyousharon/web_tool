# Vercel Speed Insights Integration Guide

This document explains how Vercel Speed Insights has been integrated into the Web Tool project and provides guidance for deployment and monitoring.

## What is Vercel Speed Insights?

Vercel Speed Insights is a performance monitoring tool that provides real user monitoring (RUM) data. It helps you track and analyze the performance metrics of your deployed application from real users' browsers.

## Prerequisites

Before integrating Speed Insights, ensure you have:

1. **A Vercel account** - If you don't have one, [sign up for free](https://vercel.com/signup)
2. **A Vercel project** - If you don't have one, [create a new project](https://vercel.com/new)
3. **The Vercel CLI** installed (optional, but recommended):
   ```bash
   npm install -g vercel
   # or
   pnpm add -g vercel
   # or
   yarn global add vercel
   # or
   bun add -g vercel
   ```

## Integration for HTML Projects

This project is a pure HTML/CSS/JavaScript application. The Speed Insights integration is straightforward and requires no additional packages or build tools.

### What Was Implemented

The Vercel Speed Insights script has been added to `index.html` just before the closing `</body>` tag:

```html
<!-- Vercel Speed Insights -->
<script>
  window.si = window.si || function () { (window.siq = window.siq || []).push(arguments); };
</script>
<script defer src="/_vercel/speed-insights/script.js"></script>
</body>
```

**Note:** The `/_vercel/speed-insights/script.js` route becomes available after enabling Speed Insights on your Vercel project and deploying your application.

## Setup Instructions

### Step 1: Enable Speed Insights on Vercel Dashboard

1. Go to your [Vercel Dashboard](https://vercel.com/dashboard)
2. Select your project
3. Navigate to the **Speed Insights** tab
4. Click the **Enable** button in the dialog

> **Note:** Enabling Speed Insights will add new routes (scoped at `/_vercel/speed-insights/*`) after your next deployment.

### Step 2: Deploy to Vercel

Deploy your application to Vercel using one of these methods:

**Using Vercel CLI:**
```bash
vercel deploy
```

**Using Git Integration:**
1. Connect your GitHub repository to Vercel
2. Vercel will automatically deploy on every push to your main branch

**Using Vercel Web Interface:**
1. Go to your Vercel Dashboard
2. Click "Add New..." > "Project"
3. Import your Git repository
4. Select deployment options and deploy

### Step 3: Verify the Integration

After deployment:

1. Visit your deployed application URL
2. Open the browser's Developer Tools (F12 or right-click → Inspect)
3. Go to the **Network** tab
4. Search for `script.js` - you should see a request to `/_vercel/speed-insights/script.js`
5. You should also see `/_vercel/speed-insights/record` POST requests as the page loads

This confirms that Speed Insights is properly installed and sending data to Vercel.

### Step 4: View Your Data

Once your application is deployed and users have visited your site:

1. Go to your [Vercel Dashboard](https://vercel.com/dashboard)
2. Select your project
3. Click the **Speed Insights** tab
4. After a few days of visitor traffic, you'll see metrics including:
   - Page load times
   - Core Web Vitals (LCP, FID, CLS)
   - First Contentful Paint (FCP)
   - Largest Contentful Paint (LCP)
   - First Input Delay (FID)
   - Cumulative Layout Shift (CLS)
   - Resource timing data

## Monitoring Performance

### Key Metrics to Track

Speed Insights provides several important performance metrics:

- **LCP (Largest Contentful Paint)**: Measures loading performance. Aim for < 2.5s
- **FID (First Input Delay)**: Measures responsiveness. Aim for < 100ms
- **CLS (Cumulative Layout Shift)**: Measures visual stability. Aim for < 0.1
- **FCP (First Contentful Paint)**: Measures perceived load speed
- **TTFB (Time to First Byte)**: Measures server response time

### Understanding the Dashboard

The Speed Insights dashboard shows:

- Real-time and historical performance data
- Distribution of metrics across visits
- Performance trends over time
- Regional performance breakdowns
- Device and browser information

## Customization Options

### Adding beforeSend Hook (Optional)

For other frameworks that support it, you can customize what data is sent by adding a `beforeSend` function. However, for pure HTML projects, this is typically not necessary.

If needed in the future, you could add:

```html
<script>
  window.speedInsightsBeforeSend = function(data) {
    // Customize data before sending to Vercel
    // Remove sensitive information from URLs, etc.
    return data;
  };
</script>
```

## Deployment Considerations

### For This Static HTML Project

1. **No build step required** - The HTML file is served as-is
2. **Lightweight** - Speed Insights script adds minimal overhead (< 5KB)
3. **Privacy-friendly** - No sensitive data is collected by default
4. **No configuration needed** - Works automatically after enabling on Vercel

### For GitHub Pages or Other Hosting

Speed Insights only works when deployed to Vercel. If you deploy to other platforms like GitHub Pages or Cloudflare Pages, the Speed Insights route won't be available. The script will still load but won't send data.

## Troubleshooting

### Speed Insights Data Not Showing

1. **Verify deployment** - Ensure your app is deployed to Vercel, not just running locally
2. **Check enablement** - Go to Vercel dashboard and verify Speed Insights is enabled for your project
3. **Wait for data** - It can take a few minutes to an hour for data to appear in the dashboard
4. **Check network** - Open DevTools Network tab and verify `/_vercel/speed-insights/script.js` loads successfully
5. **Check visitors** - Ensure real users have visited your site (localhost doesn't count)

### Script Fails to Load

- This is normal if Speed Insights isn't enabled yet - the script will fail gracefully
- Once enabled on Vercel and redeployed, it should load successfully
- The application will continue to function normally even if the script fails

## Privacy and Compliance

Vercel Speed Insights:

- Collects performance data only from real users
- Does not track personally identifiable information (PII) by default
- Complies with GDPR, CCPA, and other privacy regulations
- Allows you to customize what data is collected
- Does not use cookies for tracking

For more details, see [Vercel Speed Insights Privacy Policy](/docs/speed-insights/privacy-policy).

## Next Steps

1. Deploy your application to Vercel
2. Enable Speed Insights in your Vercel dashboard
3. Monitor your application's performance over time
4. Use insights to optimize performance
5. Check out [Using Speed Insights](/docs/speed-insights/using-speed-insights) for detailed metric explanations

## Additional Resources

- [Learn how to use the @vercel/speed-insights package](https://vercel.com/docs/speed-insights/package)
- [Learn about metrics](https://vercel.com/docs/speed-insights/metrics)
- [Read about privacy and compliance](https://vercel.com/docs/speed-insights/privacy-policy)
- [Explore pricing](https://vercel.com/docs/speed-insights/limits-and-pricing)
- [Troubleshooting guide](https://vercel.com/docs/speed-insights/troubleshooting)

## Quick Reference

### Enable Speed Insights
1. Vercel Dashboard → Project → Speed Insights tab → Enable

### Deploy
```bash
vercel deploy
```

### View Data
Vercel Dashboard → Project → Speed Insights tab

### Verify Installation
Check browser DevTools Network tab for `/_vercel/speed-insights/script.js` requests
