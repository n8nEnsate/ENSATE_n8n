# ENSATE Workflow Agents

This repo provides 4 n8n agents for automating ENSATE workflows developed using **n8n** for ENSATE workflows. Each agent handles a specific part of the candidate management and examination process.

---

## 📥 Agent 1: Validator

### Description

Validates candidate data submitted via webhook submitted via a webhook. It ensures fields like **CIN** and **email** meet formatting standards and logs validation results.

### Features

* Validates candidate CIN and email.
* Logs validation results to Google Sheets.
* Forwards valid data to Agent 2.
* Sends email notifications for invalid submissions.

### Inputs

* Candidate data (JSON) via webhook.

### Outputs

* Forwards valid data to the next agent.
* Logs all validation attempts with timestamps.

---

## 🧠 Agent 2: AI Verifier

### Description

Agent 2 performs biometric verification by comparing the candidate's selfie with their stored ID photo.

### Features

* Decodes base64 selfies.
* Downloads candidate photos from the database.
* Uses Face++ API to compare faces.
* Returns a confidence score and match status.

### Inputs

* Selfie (base64-encoded).
* Candidate room ID.

### Outputs

* JSON response with `confidence` and `match` fields.

> ⚠ **Note:** Azure Face API is disabled due to access restrictions. Face++ API is used instead.

---

## 🪑 Agent 3: Room and Seat Assignment

### Description

Agent 3 automates the assignment of rooms and seats to candidates.

### Features

* Reads candidate and room data from Google Sheets.
* Assigns a room and seat to each candidate.
* Updates the Google Sheet with the new assignments.

### Inputs

* Google Sheets with student and room data.

### Outputs

* Updated Google Sheets with room and seat assignments.

---

## 📧 Agent 4: Invitation Sender

### Description

Agent 4 generates and sends email invitations to candidates for their examinations.

### Features

* Uses AI to generate personalized invitation emails.
* Sends emails using Gmail.
* Maintains conversation memory and context.

### Inputs

* Candidate name, email, date, hour, and room.

### Outputs

* Sends email invitations directly to candidates.

---

## 📊 Agents Interaction Diagram

![Agents Interaction Diagram](A_flowchart_diagram_illustrates_four_interconnecte.png)

This diagram illustrates the flow of data and tasks between the four agents in the workflow.

---

## 🛠 Setup and Deployment

* **n8n** self-hosted or cloud instance.
* Credentials for:

  * Google Sheets API
  * Face++ API
  * SMTP or Gmail
  * (Optional) Azure Face API

## 🚀 Deployment

1. Import the JSON files for each agent into your n8n instance.
2. Set up credentials for each integration.
3. Activate the workflows in the proper sequence:

   1. Agent 1: Validator
   2. Agent 2: AI Verifier
   3. Agent 3: Room and Seat Assignment
   4. Agent 4: Invitation Sender

## 🏷 Tags

`Validation` `Biometric Verification` `Automation` `ENSATE` `n8n`

---

## 👥 Contributors

* Salaheddine KAYOUH
* Anas EL HASSOUNI
* Abdoul Latif KINDA

## 📄 License

This project is for ENSATE internal use and academic purposes.
 
