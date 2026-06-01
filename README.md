# Menu-Promotion-Content-Generator-AI-Agent
This project automates weekly restaurant menu promotion content generation using n8n, Google Sheets, Gmail, and Generative AI. It analyzes menu data, bestsellers, margins, and seasonal ingredients to generate Instagram captions, WhatsApp messages, Zomato/Swiggy descriptions, hashtags, and approval emails.
1. Agent Name

Menu Promotion Content Generator AI Agent

2. Tools Needed

Use:

Frontend Form → n8n Webhook → Set Node → Claude/Gemini AI → Google Sheets → Respond to Webhook

Optional advanced tools:

Scheduler, Canva API, Zomato/Swiggy Partner API

Step-by-Step n8n Workflow
Step 1: Create Workflow in n8n

Open n8n → click Create Workflow

Name it:

Menu Promotion Content Generator

Step 2: Add Webhook Node

Add node:

Webhook

Set:

HTTP Method: POST
Path: menu-promotion-agent
Response Mode: Using Respond to Webhook node

Your test URL will look like:

https://your-n8n-url/webhook-test/menu-promotion-agent
Step 3: Input Fields From Website/Form

Your form should send this data:

{
  "menu": "Malabar Fish Curry - ₹320 - Margin 45%, Chicken Biryani - ₹250 - Margin 35%",
  "bestsellers": "Malabar Fish Curry, Chicken Biryani",
  "seasonalIngredients": "Mango, Coconut, Curry leaves",
  "platform": "Instagram",
  "restaurantName": "Kozhikodan Kitchen",
  "swiggyLink": "https://swiggy.com/example"
}
Step 4: Add Set Node

Add node:

Set

Create these fields:

menu
bestsellers
seasonalIngredients
platform
restaurantName
swiggyLink

Use expressions like:

{{ $json.body.menu }}

For each field:

{{ $json.body.bestsellers }}
{{ $json.body.seasonalIngredients }}
{{ $json.body.platform }}
{{ $json.body.restaurantName }}
{{ $json.body.swiggyLink }}
Step 5: Add AI Model Node

Use:

Claude
or
Google Gemini → Message a Model

Choose model:

Gemini 1.5 Flash / Gemini 2.0 Flash / Claude Sonnet

Step 6: Paste This Master Prompt

Paste this in AI prompt box:

You are a senior restaurant marketing strategist with 20+ years of experience.

Create high-converting menu promotion content for the given restaurant.

Restaurant Name:
{{ $json.restaurantName }}

Current Menu with Price and Margin:
{{ $json.menu }}

Bestseller Data:
{{ $json.bestsellers }}

Seasonal Ingredients Available:
{{ $json.seasonalIngredients }}

Target Platform:
{{ $json.platform }}

Ordering Link:
{{ $json.swiggyLink }}

Your task:
1. Identify 3 hero dishes based on bestseller and high-margin logic.
2. Write captions for each dish in 3 lengths:
   - Short caption
   - Medium caption
   - Long storytelling caption
3. Create Instagram story script.
4. Create Zomato/Swiggy listing description update.
5. Create WhatsApp broadcast message for weekly special.
6. Suggest hashtags and keywords.
7. Add urgency, scarcity, emotional appeal, local taste, and CTA.
8. Make the content sound natural, premium, and restaurant-friendly.

Output format:

MENU PROMOTION REPORT

1. Featured Hero Dishes
- Dish 1:
- Reason:
- Promotion Angle:

2. Captions
Dish Name:
Short:
Medium:
Long:

3. Instagram Story Script
Slide 1:
Slide 2:
Slide 3:
CTA:

4. Zomato/Swiggy Description Update

5. WhatsApp Broadcast Message

6. Hashtags

7. Keywords

8. Weekly Marketing Tip
Step 7: Add Google Sheets Node

This is for saving reports.

Add:

Google Sheets → Append Row

Create columns:

Date
Restaurant Name
Platform
Menu
Bestsellers
Seasonal Ingredients
AI Output

For AI Output use:

{{ $('Message a model').item.json.content.parts[0].text }}

If your node name is different, select it from expression editor.

Step 8: Add Respond to Webhook Node

Add:

Respond to Webhook

Response Body should be valid JSON:

{
  "success": true,
  "result": "{{ $('Message a model').item.json.content.parts[0].text }}"
}

If red error comes, use this instead in expression mode:

{{
{
  success: true,
  result: $('Message a model').item.json.content.parts[0].text
}
}}
Final Workflow Structure
Webhook
   ↓
Set
   ↓
Gemini / Claude AI
   ↓
Google Sheets
   ↓
Respond to Webhook
