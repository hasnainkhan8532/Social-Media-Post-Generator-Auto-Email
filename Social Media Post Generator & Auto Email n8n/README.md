<p align="center">
  <img src="./Workflow.png" alt="n8n Workflow Overview" width="900" />
</p>

## Social Media Post Generator & Auto Email (n8n)

Automated LinkedIn-style post generation with AI + scheduled delivery via email.

- Generates content topics aligned to brand pillars
- Writes a polished post (title + body + image prompt)
- Produces relevant, mixed-level hashtags for reach and SEO
- Optionally generates an image prompt using Google Gemini image model
- Emails the result on a schedule (every 6 hours by default)

### What’s inside

Nodes used (high level):

- Schedule Trigger: runs every 6 hours
- Content topic generate: agent that proposes topic, rationale, and hook
- Content creator: LLM chain that writes the post text from the chosen topic
- Hashtag & SEO: agent that generates hashtag sets
- Generate an image: Google Gemini image generation (model: `models/gemini-2.0-flash-preview-image-generation`)
- Structured Output Parsers: ensure consistent fields (title, content, image description, hashtags)
- Merge: merges image + hashtag streams
- Send email: sends the final result via SMTP

### Prerequisites

- n8n running (self-hosted or desktop)
- n8n AI nodes available (LangChain-based nodes bundled with n8n)
- Credentials configured in n8n:
  - Google Gemini (PaLM) API account
  - SMTP account for sending email

### Import the workflow

1. Open n8n → Workflows → Import from file
2. Select the JSON file in this repo:
   - `Social Media Post Generator & Auto Email.json`
3. Save the workflow

### Configure credentials and settings

1. Google Gemini credentials
   - Open nodes:
     - `Google Gemini Chat Model`
     - `Google Gemini Chat Model1`
     - `Google Gemini Chat Model2`
     - `Generate an image`
   - Assign your Google Gemini (PaLM) API credential to each.
   - Ensure the image node model is set to `models/gemini-2.0-flash-preview-image-generation`.

2. SMTP (email) credential
   - Open the `Send email` node
   - Select your SMTP credential
   - Update `fromEmail` and `toEmail` as desired

3. Schedule
   - Open the `Schedule Trigger` node
   - Adjust the interval (default: every 6 hours)

### How it works (flow)

1. Schedule Trigger fires
2. Content topic generate proposes a topic, rationale, and hook
3. Content creator writes a full post using the proposed topic
4. Hashtag & SEO generates mixed-level hashtags
5. Generate an image produces image output (metadata)
6. Merge combines image and hashtag branches
7. Send email composes and sends the email with:
   - Post title
   - Post content
   - Hashtags (comma-separated)
   - Image metadata (MIME type as currently configured)

Note: The current template includes the image MIME type in the email. If you want to embed or attach the generated image, add handling to store the image output and attach/embed it in the `Send email` node.

### Customization tips

- Tune the brand pillars and instructions in the `Content topic generate` node text
- Adjust the formatting in `Send email` to match your brand voice (e.g., HTML template)
- Increase/decrease frequency via `Schedule Trigger`
- Swap models or temperature settings in Gemini chat nodes to change tone

### Files in this repo

- `Social Media Post Generator & Auto Email.json`: n8n workflow export
- `README.md`: this guide
- `Workflow.png`: visual overview (top)

### Attribution

- Built with n8n and Google Gemini models
