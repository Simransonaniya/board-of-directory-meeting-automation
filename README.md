# board-of-directory-meeting-automation
# Board of Directors Meeting Automation (n8n Workflow)

An AI-assisted workflow built with **n8n** that automates the full lifecycle of a Board of Directors meeting: drafting the formal notice and agenda, getting the Chairperson's approval, notifying and reminding directors, and producing the meeting minutes.

## Live Demo

| Item | Link |
|------|------|
| Live meeting request form | [https://simransonaniya77.app.n8n.cloud/form/board-meeting](https://simransonaniya77.app.n8n.cloud/form/board-meeting) |
| Demo video | [Watch the demo on Google Drive](https://drive.google.com/file/d/1hAjYGmTrMAPaiSt_0Vg1q8Q-JQCX8xdw/view?usp=sharing) |
| GitHub repository | [https://github.com/Simransonaniya/board-of-directory-meeting-automation](https://github.com/Simransonaniya/board-of-directory-meeting-automation) |

> The live form runs on an n8n cloud trial account, which is valid for a limited time. If the form link has expired, please watch the demo video. When testing, please enter your own email address in the chair, secretary and directors fields, because notices and approval emails are sent to the addresses you enter.

## Problem Statement

Organising a board meeting involves many manual steps: writing a formal notice and agenda, getting the Chairperson's sign-off, emailing every director, sending reminders, and writing the minutes afterwards. This is time-consuming and easy to get wrong. This workflow automates the whole process and keeps a human approval step in the loop.

## Workflow Overview

```
Form -> Prepare Details -> Compute Schedule -> AI Drafts Notice & Agenda
     -> Chair Approval -> Approved?
          |-- Yes -> Send Notice to Directors -> Wait Until 24h Before -> Send Reminder
          |         -> Wait Until Meeting Ends -> Collect Meeting Notes -> Produce Minutes
          |-- No / no reply in 2 days -> Notify Secretary of Rejection
```

The workflow is organised into four stages:

### 1. Draft meeting notice
| Node | What it does |
|------|--------------|
| Board Meeting Request Form | Form trigger. The Company Secretary enters the meeting details. |
| Prepare Meeting Details | Cleans and structures the submitted data. |
| Compute Schedule | Builds the meeting reference number (e.g. `BM-20261001-8`) and calculates the reminder time. |
| Draft Notice and Agenda | An AI agent (with a chat model) writes a formal HTML notice and agenda. |

### 2. Get chair sign-off
| Node | What it does |
|------|--------------|
| Request Chair Approval | Emails the draft to the Chairperson and waits for a response (send and wait). |
| Approved? | Routes the workflow based on the Chairperson's decision. |
| Notify Secretary of Rejection | If the notice is rejected, or there is no reply within 2 days, the Company Secretary is notified. |

### 3. Notify and remind board
| Node | What it does |
|------|--------------|
| Send Notice to Directors | Emails the approved notice to all directors. |
| Wait Until 24h Before | Pauses the workflow until 24 hours before the meeting. |
| Send Reminder to Directors | Sends a reminder email to the directors. |

### 4. Produce meeting minutes
| Node | What it does |
|------|--------------|
| Wait Until Meeting Ends | Pauses the workflow until the meeting is over. |
| Collect Meeting Notes | Emails the Secretary to submit the notes and action items (send and wait). |
| Minutes drafting nodes | AI drafts the meeting minutes from the collected notes. |

## Form Fields

| Field | Description |
|-------|-------------|
| `company_name` | Name of the company |
| `meeting_title` | Title of the meeting (e.g. Q3 Board Meeting) |
| `meeting_type` | Dropdown, e.g. Regular Board Meeting |
| `meeting_date` | Date of the meeting |
| `meeting_time` | Start time |
| `duration_hours` | Expected duration in hours |
| `venue` | Meeting location |
| `directors_emails` | Directors' email addresses, separated by `;` |
| `chair_email` | Chairperson's email |
| `secretary_email` | Company Secretary's email |
| `agenda_topics` | Topics to be discussed |
| `background_notes` | Optional background information |

## Key Features

- **AI-drafted notice and agenda** in a formal, professional format
- **Human-in-the-loop approval:** nothing is sent to directors until the Chairperson approves
- **Rejection and timeout handling:** the Secretary is informed if the notice is rejected or ignored
- **Automatic reminders** 24 hours before the meeting using Wait nodes
- **Automated minutes** drafting after the meeting
- **Unique meeting reference** generated for every meeting

## How to Import This Workflow

1. Download the `.json` file from this repository.
2. Open n8n and go to **Workflows**.
3. Click **Import from File** and select the downloaded `.json` file.
4. Connect your own **Gmail** credential in all Gmail nodes.
5. Connect your own **AI model credential** (e.g. OpenAI) in the Notice Model node (and the minutes model node).
6. Click **Execute workflow** to test, then **Publish** to make the form live.

> Credentials are not included in the exported file for security reasons. You must connect your own accounts after importing.

## Testing Tips

- Use your own email in `chair_email`, `secretary_email` and `directors_emails`.
- For quick testing, temporarily set the Wait nodes to a short interval (e.g. 1 minute) and restore them before going live.
- Use **Execute workflow** (not **Execute step** on the trigger) so pinned test data is not overwritten.

## Tech Stack

- n8n (workflow automation)
- n8n Form Trigger
- AI Agent with chat model
- Gmail (send, and send-and-wait for approvals)
- Wait nodes (time-based scheduling)

## Possible Improvements

- Store every meeting and its status in a Data Table or Google Sheet
- Add calendar invites (Google Calendar) for directors
- Track director attendance and RSVPs
- Attach the minutes as a PDF
- Validate email addresses and meeting dates on the form

## Author

**Simran Sonaniya**
Email: simransonaniya77@gmail.com
GitHub: [https://github.com/Simransonaniya](https://github.com/Simransonaniya)
