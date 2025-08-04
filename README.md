# Installation Guide for n8n Twilio-OpenAI-Google Sheets Chatbot Workflow

This guide outlines the steps to set up and deploy the provided n8n workflow, which integrates Twilio for messaging, OpenAI for generating responses, and Google Sheets for logging interactions.

## Prerequisites
- **n8n Account**: An active n8n cloud account or a self-hosted n8n instance (version compatible with the workflow, e.g., n8n 1.x or later).
- **Twilio Account**: A Twilio account with a phone number configured for SMS or WhatsApp messaging.
- **OpenAI Account**: An OpenAI account with an API key for accessing GPT models.
- **Google Account**: A Google account with access to Google Sheets and Google Drive.
- **Basic Knowledge**: Familiarity with n8n workflows, API credentials, and webhook configurations.

## Step-by-Step Installation

### 1. Set Up n8n
1. **Access n8n**:
   - For n8n cloud, log in to your n8n instance (e.g., `your-subdomain.app.n8n.cloud`).
   - For self-hosted, ensure n8n is installed and running (refer to [n8n documentation](https://docs.n8n.io/getting-started/installation/)).
2. **Create a New Workflow**:
   - In the n8n dashboard, click **New** to create a new workflow.
   - Import the provided JSON workflow:
     - Copy the JSON content from the provided workflow file (`twilio-openai-gsheets-chatbot.json`).
     - In n8n, click the three dots (⋮) in the top-right corner, select **Import Workflow**, and paste the JSON content.
     - Save the workflow.

### 2. Configure Credentials
The workflow requires credentials for Twilio, OpenAI, and Google Sheets. Replace placeholder credentials (`xxxxx`) in the workflow with your own.

#### Twilio Credentials
1. **Obtain Twilio Credentials**:
   - Log in to your Twilio Console (https://console.twilio.com).
   - Note your **Account SID** and **Auth Token**.
   - Ensure you have a Twilio phone number configured for SMS or WhatsApp.
2. **Add Twilio Credentials in n8n**:
   - In n8n, go to **Credentials** > **Add Credential** > Select **Twilio API**.
   - Enter:
     - **Name**: e.g., "Twilio account".
     - **Account SID**: Your Twilio Account SID (e.g., `xxxxx`).
     - **Auth Token**: Your Twilio Auth Token.
   - Save the credential.
3. **Update Workflow**:
   - Open the **HTTP Request** node in the workflow.
   - In the **Credentials** section, select the newly created Twilio credential (replacing `xxxxx`).

#### OpenAI Credentials
1. **Obtain OpenAI API Key**:
   - Log in to your OpenAI account (https://platform.openai.com).
   - Navigate to **API Keys** and generate a new API key.
2. **Add OpenAI Credentials in n8n**:
   - In n8n, go to **Credentials** > **Add Credential** > Select **OpenAI API**.
   - Enter:
     - **Name**: e.g., "OpenAI account".
     - **API Key**: Your OpenAI API key.
   - Save the credential.
3. **Update Workflow**:
   - Open the **Message a model** node.
   - In the **Credentials** section, select the newly created OpenAI credential (replacing `xxxxx`).

#### Google Sheets Credentials
1. **Set Up Google Cloud Project**:
   - Go to the Google Cloud Console (https://console.cloud.google.com).
   - Create a new project or select an existing one.
   - Enable the **Google Sheets API** and **Google Drive API**.
   - Create OAuth 2.0 credentials:
     - Go to **Credentials** > **Create Credentials** > **OAuth 2.0 Client IDs**.
     - Set **Application Type** to **Web Application**.
     - Add `https://your-n8n-instance.com/oauth2/callback` as an **Authorized redirect URI** (replace with your n8n instance URL).
     - Download the credentials JSON file.
2. **Add Google Sheets Credentials in n8n**:
   - In n8n, go to **Credentials** > **Add Credential** > Select **Google Sheets OAuth2 API**.
   - Enter:
     - **Name**: e.g., "Google Sheets account".
     - Upload the OAuth 2.0 credentials JSON or manually enter the **Client ID** and **Client Secret**.
     - Authenticate by following the OAuth flow.
   - Save the credential.
3. **Update Workflow**:
   - Open the **Append row in sheet** node.
   - In the **Credentials** section, select the newly created Google Sheets credential (replacing `xxxxx`).
   - Update the **Document ID** and **Sheet Name**:
     - Create a new Google Sheet or use an existing one.
     - Copy the **Document ID** from the sheet URL (e.g., `xxxxx` from `https://docs.google.com/spreadsheets/d/xxxxx`).
     - Replace the `documentId` and `sheetName` values in the **Append row in sheet** node.

### 3. Configure Webhook
1. **Set Up Webhook in n8n**:
   - Open the **Webhook** node in the workflow.
   - Copy the webhook URL (e.g., `https://your-subdomain.app.n8n.cloud/webhook-test/xxxxx`).
   - Ensure the webhook is set to **POST** and active.
2. **Configure Twilio Webhook**:
   - In the Twilio Console, go to your phone number’s configuration (under **Phone Numbers** > **Active Numbers**).
   - For SMS or WhatsApp, set the **Messaging** webhook to the n8n webhook URL copied above.
   - Save the configuration.

### 4. Test the Workflow
1. **Activate the Workflow**:
   - In n8n, toggle the **Active** switch to enable the workflow.
2. **Send a Test Message**:
   - Send a test SMS or WhatsApp message to your Twilio phone number.
   - Verify that:
     - The **Webhook** node receives the message.
     - The **Message a model** node processes it and generates a response using OpenAI.
     - The **HTTP Request** node sends a reply via Twilio.
     - The **Append row in sheet** node logs the interaction in the specified Google Sheet.
3. **Debugging**:
   - If the workflow fails, check the execution log in n8n for errors.
   - Ensure all credentials are correctly configured and the webhook URL is accessible.

### 5. Deploy and Monitor
1. **Deploy the Workflow**:
   - Save and keep the workflow active in n8n.
   - Ensure your n8n instance remains running (for self-hosted) or your cloud plan is active.
2. **Monitor Logs**:
   - Regularly check the Google Sheet for logged interactions.
   - Monitor n8n executions for any failures or issues.
3. **Scale (Optional)**:
   - For high message volumes, consider upgrading your n8n plan or optimizing your self-hosted instance.
   - Ensure your Twilio and OpenAI accounts have sufficient credits for API usage.

## Notes
- Replace all placeholder credentials (e.g., `xxxxx`) with your own.
- Ensure the Google Sheet has the correct column headers (`Timestamp`, `From (User)`, `To (Bot)`, `User Message`, `Bot Reply`) to match the workflow’s mapping.
- For WhatsApp, ensure Twilio’s WhatsApp sandbox or production number is properly configured.
- Test the workflow thoroughly before deploying in a production environment.
