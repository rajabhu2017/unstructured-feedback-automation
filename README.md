# Unstructured Customer Feedback Automation

An n8n workflow that collects customer feedback in text, image, or audio
form, uses OpenAI models to analyze the available feedback, classifies
the issue, determines severity and escalation, sends email
notifications, generates a customer response, and stores the result in
Google Sheets.

## Workflow

``` text
Customer Feedback Form
        │
        ▼
Detect Input Type
   ┌────┼─────┐
   │    │     │
 Text Image  Audio
   │    │     │
   │    ▼     ▼
   │ Image   Transcription
   │ Analysis   │
   └────┬───────┘
        ▼
   Classify Feedback
        │
        ▼
    If Escalated
      ┌───┴───┐
     Yes     No
      │       │
      ▼       ▼
 Company   Customer
 Alert     Response
      │       │
      └───┬───┘
          ▼
     Google Sheets
```

## What it does

-   Collects:
    -   Customer name
    -   Email
    -   Written feedback
    -   Optional image, video, or voice-note attachment
-   Detects the attachment type.
-   Analyzes uploaded images for visible vehicle/service-related
    evidence.
-   Transcribes uploaded audio.
-   Combines available text, image analysis, and audio transcription.
-   Uses OpenAI to classify:
    -   Language
    -   Sentiment
    -   Issue type
    -   Severity
    -   Customer intent
    -   Escalation requirement
    -   Escalation reason
    -   Summary
-   Routes escalated feedback to a company notification email.
-   Generates a customer-facing response for non-escalated feedback.
-   Stores the processed feedback and AI-generated fields in Google
    Sheets.

## Tech Stack

-   n8n
-   OpenAI
-   Gmail
-   Google Sheets

## Setup

1.  Import `Unstructured_Feedback_Workflow.json` into n8n.
2.  Configure the required OpenAI credentials.
3.  Configure the Gmail credential used for sending emails.
4.  Configure the Google Sheets credential.
5.  Update the destination email address used for escalation
    notifications.
6.  Connect the workflow to the required Google Sheet and verify the
    expected column names.
7.  Activate the workflow after testing.

## Google Sheets Output

The workflow stores fields including:

-   Name
-   Email
-   Input Type
-   Language
-   Feedback / Transcription
-   Visual Summary
-   Sentiment
-   Issue Type
-   Severity
-   Customer Intent
-   Escalated
-   Escalation Reason
-   AI Summary
-   Customer Notified
-   Submitted At

## Input Handling

### Text

Written feedback is passed directly to the classification step.

### Image

The image is analyzed for visible evidence such as vehicle damage,
dents, scratches, broken parts, collision damage, and potential safety
concerns.

### Audio

The uploaded audio is transcribed before classification.

### Video

The workflow contains an input-type branch for video, but the current
workflow does not contain a video analysis/transcription step. Video
handling would need to be added separately.

## AI Classification

The classification step returns structured JSON with:

``` json
{
  "language": "",
  "sentiment": "Positive | Negative | Neutral",
  "issue_type": "",
  "severity": "Low | Medium | High | Critical",
  "customer_intent": "Praise | Complaint | Suggestion | Information | Other",
  "escalate": true,
  "escalation_reason": "",
  "summary": ""
}
```

The classification uses the available feedback evidence rather than
requiring the customer to select a category.

## Repository Structure

Recommended structure:

``` text
.
├── README.md
├── Unstructured_Feedback_Workflow.json
└── screenshots/
    └── workflow-overview.png
```

The JSON workflow is the essential file required to share the
automation.

## Important

Before uploading the workflow to a public GitHub repository:

-   Remove or replace personal email addresses.
-   Remove or replace any private Google Sheets references.
-   Review exported credential references and other instance-specific
    metadata.
-   Do not commit API keys, OAuth tokens, passwords, or other secrets.
-   Test the sanitized workflow before publishing it.

## Current Scope

This repository contains the n8n workflow export. External services such
as OpenAI, Gmail, and Google Sheets require their own credentials and
configuration.

