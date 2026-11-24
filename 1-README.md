# AI Lead Intake (n8n + Groq + Gmail + Google Sheets)

An n8n workflow that captures project inquiries from a web form, validates them, has an AI model write a short first reply, and logs every lead. Replies are saved as Gmail drafts for review. Nothing is sent automatically.

## Flow
Form > Normalize > Valid Lead? > Groq (first reply) > Build Row > Google Sheets, then a Gmail draft and an owner notification for valid leads.

## What it does
1. A form collects name, email, service and budget.
2. A code step trims and lowercases the input and checks it.
3. Valid leads go to Groq, which writes a reply under 70 words. It does not invent prices or dates and asks one follow-up question.
4. Every lead is logged with a `lead_status`: `New`, `Needs Review` (invalid input) or `AI Error` (model failed).
5. For valid leads, the reply is saved as a Gmail draft and the owner gets an email.

## Setup
1. Create a Google Sheet with a tab named `Leads` and headers in row 1: `lead_id, submitted_at, full_name, email, service, budget, ai_message, lead_status, validation_errors, source, is_valid, error_details`.
2. Import `workflow.json` into n8n.
3. Add a Groq credential (free API key from console.groq.com) and select it on the Groq node.
4. Connect Google Sheets and pick your sheet in the three Save Lead nodes.
5. Connect Gmail for the draft and notify nodes. Set the Notify node's To address (placeholder: `your-email@example.com`).
6. Open the form URL, submit a test lead, check the sheet row, the draft and the notification.

## Status
Tested on n8n cloud in a throwaway copy with a dummy lead: Form, Normalize, Valid Lead?, Groq and Build Row ran correctly and the AI reply was good. The Google Sheets and Gmail nodes were disabled in that test, so they are untested here. The invalid-lead and AI-error branches were not run. Credentials, sheet IDs and emails are removed. The model name in the Groq node may need updating if Groq retires it.

## Author
Osama Atef, AI and automation developer. Open to small freelance projects.
