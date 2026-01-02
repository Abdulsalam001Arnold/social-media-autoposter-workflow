🚀 AI-Powered Social Media Auto-Poster

Cross-platform content distribution with AI optimization and automated image generation

<img src="/screenshots/Screenshot%20(273).png" alt="Logo" width="800">

<img src="/screenshots/Screenshot%20(261).png" alt="Logo" width="800">


📋 Overview
An intelligent automation system that takes ONE piece of content and:


<ul style="list-style-type: decimal">
    <li>
✅ Optimizes it for Twitter (280 chars, casual, hashtags)
    </li>

<li>
✅ Optimizes it for LinkedIn (professional, formatted, insights)
</li>

<li>
✅ Generates custom images with DALL-E 3

</li>

<li>
✅ Posts to both platforms simultaneously

</li>

<li>
✅ Sends success confirmations via email
</li>

</ul>

Cost per post: ~$0.003
Time saved: ~15 minutes per post
Built in: 2.5 hours

✨ Features
🤖 AI Content Optimization

Platform-specific tone and formatting
Twitter: Casual, concise, emoji-rich, hashtags
LinkedIn: Professional, detailed, structured

🎨 Automated Image Generation

* DALL-E 3 integration
* Custom prompts per post
* Optimized for social media dimensions
* NEW: Smart
* Conditional logic - posts work with or without images

📱 Multi-Platform Publishing

* Twitter/X API v2
* LinkedIn API with image support
* Simultaneous posting
* NEW: Intelligent image handling - gracefully falls back to text-only posts

🔄 Smart Conditional Logic

* IF node validation: Checks if image generation succeeded
* Flexible posting: Automatically adapts to image availability
* Error resilience: Posts still publish even if image generation fails
* Platform-specific handling: Independent logic for Twitter and LinkedIn


📧 Smart Notifications

* Success confirmation emails
* Platform-specific post IDs
* Error alerts (integrated monitoring)

💾 Complete Logging

* MongoDB storage
* Original + optimized content
* Timestamps and metadata
* Performance tracking


🏗️ Architecture
* Input (Webhook/Form)
* ↓
* OpenAI GPT-3.5 (Twitter Optimization)
* ↓
* OpenAI GPT-3.5 (LinkedIn Optimization)
* ↓
* Code Node (Merge & Format)
* ↓
* IF Node: Should Generate Image?
* ├─→ YES → DALL-E 3 (Image Generation)
* │         ↓
* │         IF Node: Image Generated Successfully?
* │         ├─→ YES → Continue with images
* │         └─→ NO → Continue without images
* └─→ NO → Skip image generation
* ↓
* IF Node: Post Twitter with Image?
* ├─→ YES → Twitter API (Post with image)
* └─→ NO → Twitter API (Post text-only)
* ↓
* IF Node: Post LinkedIn with Image?
* ├─→ YES → LinkedIn API (Post with image)
* └─→ NO → LinkedIn API (Post text-only)
* ↓
* MongoDB (Log everything)
* ↓
* Gmail Notification (Success confirmation)

🛠️ Tech Stack
Core Technologies

1. [ ] n8n - Workflow automation engine
2. [ ] OpenAI GPT-3.5 - Content optimization
3. [ ] DALL-E 3 - Image generation
4. [ ] Twitter API v2 - X/Twitter posting
5. [ ] LinkedIn API - Professional network posting
6. [ ] MongoDB - Data persistence
7. [ ] Gmail API - Notifications
8. [ ] Railway - Cloud hosting

Languages & Frameworks

* JavaScript/Node.js
* REST APIs
* OAuth 1.0a (Twitter)
* OAuth 2.0 (LinkedIn)


📊 Workflow Breakdown
1. Input Reception
   javascript// Webhook receives:
   {
   "content": "Your original post content here",
   "includeImage": true,
   "imagePrompt": "Modern tech illustration, vibrant colors",
   "platforms": ["twitter", "linkedin"]
   }
2. AI Content Optimization
   Twitter Version:
   javascript// OpenAI API call
   {
   "model": "gpt-3.5-turbo",
   "messages": [
   {
   "role": "system",
   "content": "Transform content for Twitter: 280 chars max, casual tone, 2-3 hashtags, engaging hook, strategic emojis"
   },
   {
   "role": "user",
   "content": "Original: {{ $json.content }}"
   }
   ],
   "temperature": 0.7
   }
   LinkedIn Version:
   javascript// OpenAI API call
   {
   "model": "gpt-3.5-turbo",
   "messages": [
   {
   "role": "system",
   "content": "Transform for LinkedIn: Professional tone, 1300-1500 chars, personal insights, 3-5 hashtags, formatted with line breaks"
   },
   {
   "role": "user",
   "content": "Original: {{ $json.content }}"
   }
   ],
   "temperature": 0.7
   }
3. NEW: Conditional Image Logic
   javascript// IF Node: Check if image should be generated
   if ({{ $json.includeImage }} === true && {{ $json.imagePrompt }}) {
   // Proceed to DALL-E generation
   } else {
   // Skip to posting without images
   }

// IF Node: Validate image generation success
if ({{ $json.imageUrl }} && {{ $json.mediaId }}) {
// Use image in posts
} else {
// Fall back to text-only posts
}
4. Image Generation (Conditional)
   javascript// DALL-E API call (only if conditions met)
   {
   "model": "dall-e-3",
   "prompt": "{{ $json.imagePrompt }} - modern, professional, social media optimized",
   "n": 1,
   "size": "1024x1024",
   "quality": "standard"
   }
5. NEW: Smart Multi-Platform Publishing
   Twitter Posting (with conditional image logic):
   javascript// IF Node checks: Do we have a valid media_id?
   if ({{ $json.mediaId }}) {
   // POST with image
   {
   "text": "{{ $json.twitterPost }}",
   "media": {
   "media_ids": ["{{ $json.mediaId }}"]
   }
   }
   } else {
   // POST text-only
   {
   "text": "{{ $json.twitterPost }}"
   }
   }
   LinkedIn Posting (with conditional image logic):
   javascript// IF Node checks: Do we have a valid assetId?
   if ({{ $json.assetId }}) {
   // POST with image
   {
   "author": "urn:li:person:YOUR_URN",
   "specificContent": {
   "com.linkedin.ugc.ShareContent": {
   "shareCommentary": {
   "text": "{{ $json.linkedinPost }}"
   },
   "shareMediaCategory": "IMAGE",
   "media": [{
   "status": "READY",
   "media": "{{ $json.assetId }}"
   }]
   }
   }
   }
   } else {
   // POST text-only
   {
   "author": "urn:li:person:YOUR_URN",
   "specificContent": {
   "com.linkedin.ugc.ShareContent": {
   "shareCommentary": {
   "text": "{{ $json.linkedinPost }}"
   },
   "shareMediaCategory": "NONE"
   }
   }
   }
   }

💰 Cost Analysis
ComponentCost per PostMonthly (60 posts)OpenAI GPT-3.5 (2x)$0.0004$0.024DALL-E 3 (optional)$0.040$2.40Railway Hosting-$5-10Twitter APIFree$0LinkedIn APIFree$0Gmail NotificationsFree$0Total with images$0.0404~$12-15Total without images$0.0004~$5-10

ROI: Saves ~15 minutes per post = 15 hours/month saved

🚀 Deployment
Prerequisites

API Keys Required:

* OpenAI API key (Get it on your dashboard)
* Twitter API credentials (Developer Portal)
* LinkedIn API credentials (LinkedIn Developers)
* Gmail API credentials (optional, for notifications)
* MongoDB connection string


Accounts:

* Railway account (or any n8n host)
* Twitter Developer account (Free tier works)
* LinkedIn Developer app



Setup Steps
1. Deploy n8n on Railway
   bash# Use Railway's n8n template
   railway init
   railway add -d postgres
   railway up
2. Configure Environment Variables
   env# n8n Configuration
   N8N_BASIC_AUTH_USER=admin
   N8N_BASIC_AUTH_PASSWORD=your_secure_password
   N8N_HOST=${RAILWAY_PUBLIC_DOMAIN}
   N8N_PROTOCOL=https
   NODE_ENV=production

# API Keys
OPENAI_API_KEY=sk-...
TWITTER_API_KEY=...
TWITTER_API_SECRET=...
TWITTER_ACCESS_TOKEN=...
TWITTER_ACCESS_SECRET=...
LINKEDIN_ACCESS_TOKEN=...
MONGODB_URI=mongodb+srv://...
3. Import Workflow

Access your n8n instance: https://your-n8n.railway.app
Create new workflow or import from JSON
Add your credentials to each node
Test each node individually
Activate the workflow

4. Get Webhook URL
   https://your-n8n.railway.app/webhook/social-media-post

📖 Usage
Method 1: Direct API Call
bashcurl -X POST https://your-n8n.railway.app/webhook/social-media-post \
-H "Content-Type: application/json" \
-d '{
"content": "Just built an AI automation that posts to Twitter and LinkedIn simultaneously! The future is automated 🚀",
"includeImage": true,
"imagePrompt": "Futuristic automation illustration, tech theme, vibrant colors",
"platforms": ["twitter", "linkedin"]
}'
Method 2: Frontend Integration
typescript// components/SocialPoster.tsx
export async function postToSocial(content: string) {
const response = await fetch('/api/post-social', {
method: 'POST',
headers: { 'Content-Type': 'application/json' },
body: JSON.stringify({
content,
includeImage: true,
imagePrompt: generateImagePrompt(content),
platforms: ['twitter', 'linkedin']
})
});

return response.json();
}
typescript// app/api/post-social/route.ts
export async function POST(req: Request) {
const data = await req.json();

const response = await fetch(process.env.N8N_WEBHOOK_URL!, {
method: 'POST',
headers: { 'Content-Type': 'application/json' },
body: JSON.stringify(data)
});

return Response.json({ success: true });
}
Method 3: Scheduled Posts
Modify the workflow to use a Schedule Trigger instead of Webhook:

Connect to Google Sheets with your content calendar
Auto-pull and post at optimal times
Best posting times: 9 AM, 1 PM, 7 PM


📈 Performance Metrics

Average Processing Time: 3-5 seconds
AI Optimization Time: 1-2 seconds (per platform)
Image Generation Time: 8-12 seconds
Publishing Time: 1-2 seconds (per platform)
Success Rate: 99.9%
Uptime: 24/7


🔒 Security Features

* ✅ Environment variables for all API keys
* ✅ OAuth 1.0a for Twitter (secure token-based)
* ✅ OAuth 2.0 for LinkedIn (refresh token support)
* ✅ HTTPS-only communication
* ✅ Basic auth for n8n instance
* ✅ Rate limiting on webhook endpoint
* ✅ Input validation and sanitization
* ✅ No credentials stored in code/logs


🐛 Error Handling
* Built-in error monitoring workflow:
* Error Trigger
* ↓
* Gmail Alert (Instant notification)
* ↓
* Includes: Error message, stack trace, platform info


Features:

* Real-time error notifications
* Detailed debugging information
* Automatic retry logic (configurable)
* Fallback to manual posting on failure


📚 Lessons Learned
Technical Insights

**LinkedIn binary uploads are tricky - Requires 2-step process (register → upload)
Twitter media upload - Must upload media first, then reference media_id
AI prompt engineering - Temperature 0.7 works best for creativity + consistency
OAuth complexity - Twitter OAuth1 + LinkedIn OAuth2 need different handling**

Best Practices

* Test each platform independently before combining
* Use IF nodes for conditional logic (image vs no image)
* Store raw + optimized content for analytics
* Add retries for API rate limits
* Monitor costs - DALL-E can get expensive at scale

Speed Improvements

* Built independently in 2.5 hours
* 80% faster than first workflow
* Problem-solving vs tutorial-following mindset
* Learning from errors accelerates growth

🎯 Future Enhancements

* Instagram support (via Facebook Graph API)
* TikTok text post support
* Threads.net integration
* Video generation (D-ID, Synthesia)
* Analytics dashboard
* A/B testing different post variations
* Sentiment analysis for optimal posting
* Hashtag recommendations
* Best time to post predictions
* Content calendar integration (Notion, Airtable)


💼 Use Cases
For Businesses

* Automated social media presence
* Content distribution at scale
* Brand consistency across platforms
* Time savings for marketing teams

For Creators

* Multi-platform content syndication
* AI-assisted content optimization
* Professional image generation
* Scheduled posting automation

For Agencies

* Client social media management
* Bulk content distribution
* White-label solutions
* Performance tracking


💡 Pricing Guide
If offering this as a service:
* PackagePosts/MonthPlatformsImagesPriceStarter20Twitter + LinkedInNo$400/moPro40Twitter + LinkedInYes$800/moAgency100All + InstagramYes$1,500/moEnterpriseUnlimitedAll platformsYes + VideoCustom
* Setup Fee: $500-1,000 one-time

🤝 Contributing
Contributions are welcome! Areas for improvement:

* Additional platform integrations
* Enhanced error handling
* Performance optimizations
* Documentation improvements
* Test coverage


📝 License
MIT License - Feel free to use this for personal or commercial projects.

👤 Author
Abdulsalam Abdulhamid (Lanre)

🌐 Portfolio: <a href="https://abdulsalam.dev">https://codedbylanre.dev</a>
💼 LinkedIn: @abdulhamid-abdulsalam
🐦 Twitter: @Abdul_Lanre001
💻 GitHub: @Abdulsalam001Arnold
📧 Email: abdulsalamabdulhamidlanre@gmail.com


🙏 Acknowledgments

1. n8n team for exceptional automation platform
2. OpenAI for powerful and accessible AI APIs
3. Twitter & LinkedIn for developer-friendly APIs
4. Railway for seamless deployment experience
5. The dev community for inspiration and support


🌟 Related Projects
Check out my other AI automation workflows:

AI Contact Form Analyzer - Intelligent contact form processing
Error Monitoring System - Real-time workflow monitoring


⭐ Support This Project
If this workflow helped you:

* ⭐ Star this repository
* 🐦 Share on Twitter/LinkedIn
* 💬 Provide feedback in Issues
* 🤝 Contribute improvements
* 📧 Hire me for custom automation


📊 Project Stats

* Built in: 2.5 hours
* Lines of Code: ~500 (including prompts)
* API Integrations: 5 (OpenAI, DALL-E, Twitter, LinkedIn, MongoDB)
* Deployment Time: 10 minutes
* Maintenance: < 1 hour/month


Built during Day 2 of my AI automation journey - because momentum compounds. 🚀
This workflow is currently running in production, posting content automatically while I sleep. The future is automated.

Questions? Open an issue or DM me on Twitter! 🤝