# AI-Marketing-Asisstent
AI Marketing AssetThe AI agent takes a basic input (like a single photo) of any product, such as a necklace , perfume or anything and uses advanced generative AI to instantly transform it into a high-quality, professional advertisement image or realistic demo.

Complete Setup Guide: AI Marketing Asset Generator with n8n
This workflow automatically generates three types of AI-powered marketing visuals (Instagram Post, Instagram Story, and Ad Creative) for product launches using a form submission trigger, OpenAI's image editing API, and Google Drive storage.
Prerequisites
Before starting, ensure you have:
•	An active n8n instance (cloud or self-hosted)
•	OpenAI API account with credits
•	Google Cloud Platform account
•	Google Drive folder access


Step 1: Set Up Google Drive API
Create Google Cloud Project
Navigate to Google Cloud Console and create a new project with a meaningful name like "n8n-marketing-automation". Go to the API Library, search for "Google Drive API" and click Enable to activate it for your project.
Generate OAuth Credentials
Access the Credentials section and select "Create Credentials" to generate an OAuth 2.0 Client ID. Configure the OAuth consent screen with your application details, then choose "OAuth client ID" as the credential type. Add authorized redirect URIs that n8n will provide in the credential setup screen.
Connect to n8n
In your n8n instance, add a Google Drive node and input your Client ID and Client Secret from Google Cloud. Complete the OAuth authorization flow to grant n8n access to your Google Drive, then create a parent folder in your Google Drive where all generated creatives will be stored.


Step 2: Configure OpenAI API Access
Obtain API Key
Visit the OpenAI platform and generate an API key from your account settings. Ensure your account has sufficient credits for image generation operations, as the workflow uses the image edit endpoint which requires credits per generation.
Note API Details
The workflow uses the OpenAI Images Edit API endpoint at https://api.openai.com/v1/images/edits. This endpoint accepts multipart form data with parameters including model, prompt, image file, and output format.


Step 3: Import and Configure the Workflow
Import JSON File
Open your n8n instance and create a new workflow, then import the provided JSON file using the workflow import function.
Update Credentials
Replace the placeholder Google Drive credentials with your configured OAuth connection in all Google Drive nodes (Create folder, Upload file, Upload file1, Upload file2). Add your OpenAI API key to all three HTTP Request nodes by replacing <Enter Open AI Token> with Bearer YOUR_API_KEY in the Authorization headers.
Configure Google Drive Folder
In the "Create folder" node, update the parent folder ID to match your designated storage location in Google Drive.


Step 4: Customize Brand Guidelines
Edit Brand Settings
Open the "Edit Fields" node which contains your brand identity in JSON format. Customize the following brand parameters to match your specific brand:
•	brandName: Your company or product brand name
•	brandTone: Describe your brand's visual and emotional style
•	colorTheme: Primary color palette for creatives
•	backgroundStyle: Preferred background textures and materials
•	lightingStyle: Lighting preferences for product photography
•	productPlacement: Rules for how products should be positioned
•	typographyStyle: Font styles and text formatting preferences
•	compositionGuidelines: Layout and spatial arrangement rules
The default configuration is set for "Minimalist" brand with luxury skincare aesthetics, black and white color theme, and soft elegant lighting.


Step 5: Configure the Form Trigger
Set Form Details
Open the "On form submission" node to configure your input form. The form collects five required fields: Product Name, Product Tagline, Product Image upload (PNG/JPG), Product Category, and Highlighted Benefit.
Customize Form Content
Update the "Form Title" to match your branding (default: "AI Marketing Asset Generator") and modify the "Form Description" to explain the value proposition to users. Set a custom "Form Path" slug for a branded URL if desired.
Get Form URLs
After saving the node, you'll receive two URLs: a Test URL for development testing where you can see submissions in the editor, and a Production URL for live use that triggers the workflow automatically.

Step 6: Understand the AI Agent Configuration
AI Agent Role
The workflow includes a sophisticated AI Agent powered by OpenAI's GPT-4.1-mini model that acts as a "Luxury Creative Director". This agent generates three distinct visual concepts based on your product inputs and brand guidelines.
System Prompt
The AI Agent uses an extensive system message defining its role as an elite creative director specializing in beauty and skincare visual strategy. It combines artistic direction with consumer psychology and conversion-focused copywriting.
Structured Output
The "Structured Output Parser" node ensures the AI generates consistent JSON output containing three asset variations, each with specific properties like background tone, surface type, accent props, lighting, camera angle, and overlay text.


Step 7: Configure Image Generation Nodes
Three Parallel Generators
The workflow splits into three parallel paths using a Switch node that routes to Instagram Post Generator, Instagram Story Generator, and Ad Creative Generator.
Instagram Post Generator
This creates square 1:1 ratio images designed as hero assets with bold, polished, brand-first composition. The prompt instructs the AI to create photorealistic visuals using provided product images integrated into stylized scenes with specific backgrounds, lighting, and brand guidelines.
Instagram Story Generator
This generates vertical 9:16 ratio images optimized for mobile scrolling with intimate, immersive aesthetics. The creative feels lighter and more tactile than the hero post, designed specifically for vertical flow on mobile devices.
Ad Creative Generator
This produces square 1:1 scroll-stopping ads with high-impact, dramatic visuals featuring bold lighting and confident angles. These differ from organic feed content by being more visually punchy and conversion-focused.


Step 8: Test the Workflow
Execute Test Run
Click the "Test URL" in the Form Trigger node and fill out the form with sample product information. Upload a product image (ensure it's PNG or JPG format) and submit the form to trigger the workflow.
Monitor Execution
Watch the workflow execution in the n8n editor to ensure each node processes successfully. Check that the AI Agent generates three distinct creative concepts with unique visual elements.
Verify Outputs
Confirm that three images are generated and uploaded to Google Drive in a timestamped folder named with your product name and submission date. Download the generated images and review quality, brand consistency, and adherence to your guidelines.


Step 9: Optimize and Iterate
Refine Brand Guidelines
Based on test results, adjust the brand parameters in the "Edit Fields" node to better match your desired aesthetic. Experiment with different lighting styles, background tones, and composition guidelines to achieve optimal results.
Adjust AI Prompts
Modify the prompts in the three HTTP Request nodes if the generated images don't meet expectations. You can emphasize specific elements, add more constraints, or request different stylistic approaches.
Fine-tune Output Parser
Update the JSON schema in the "Structured Output Parser" if you need additional parameters or want to modify the asset structure.


Step 10: Deploy to Production
Activate Workflow
Save all changes and activate the workflow using the toggle switch in the top right corner of the n8n editor. Switch to using the "Production URL" from the Form Trigger node for live submissions.
Share Form URL
Distribute the production form URL to your team members, clients, or integrate it into your website or marketing tools. When users submit the form, the workflow will automatically generate creatives without requiring manual intervention.
Monitor Performance
Regularly check your Google Drive folder for new creative outputs and review execution logs in n8n for any errors. Monitor your OpenAI API usage and costs to ensure you stay within budget.
Workflow Architecture Overview
The complete flow follows this sequence: Form submission triggers the workflow → Brand guidelines are applied → Google Drive folder is created → AI Agent generates three creative concepts → Product image binary data is prepared → Switch node routes to three parallel paths → Each path calls OpenAI Image Edit API with specific prompts → Generated images are converted to files → Files are uploaded to Google Drive with descriptive names.
Cost Considerations
Each workflow execution generates three images using OpenAI's image edit API, which incurs charges based on your OpenAI pricing plan. Google Drive API usage is typically free within standard quotas, but check Google Cloud Platform pricing if you exceed limits. Consider implementing rate limiting or approval workflows if deploying to a large user base to manage costs effectively.
Troubleshooting Tips
If the AI Agent fails to generate concepts, verify your OpenAI credentials in the "OpenAI Chat Model" node. When image generation fails, check that the Authorization header in HTTP Request nodes contains a valid Bearer token. For Google Drive upload errors, ensure OAuth credentials have proper permissions and the parent folder ID is correct. If form submissions don't trigger the workflow, confirm the workflow is activated and you're using the Production URL.


