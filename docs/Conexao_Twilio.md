# Conexao_Twilio



/**
 * Generic WhatsApp sending logic using Twilio API.
 * This example represents how an automation platform can execute
 * a request to Twilio using the configuration fields defined by the user.
 */

const axios = require("axios");

/**
 * Sends a WhatsApp message using Twilio.
 * @param {Object} params - Generic parameter object.
 * @param {string} params.toPhoneNumber - Destination phone number.
 * @param {string} params.fromPhoneNumber - Twilio Sandbox or WhatsApp-enabled number.
 * @param {string} params.messageText - Raw message body.
 * @param {Object} [params.templateVariables] - Optional variables used for template substitution.
 * @param {string} params.twilioAccountSid - Twilio Account SID.
 * @param {string} params.twilioAuthToken - Twilio Auth Token.
 * @param {string} [params.templateSid] - Optional Messaging Service SID.
 */
async function sendWhatsAppMessage(params) {
  const {
    toPhoneNumber,
    fromPhoneNumber,
    messageText,
    templateVariables,
    twilioAccountSid,
    twilioAuthToken,
    templateSid
  } = params;

  // -----------------------------
  // Template variable substitution
  // -----------------------------
  let finalMessage = messageText;

  if (templateVariables && typeof templateVariables === "object") {
    for (const key of Object.keys(templateVariables)) {
      const placeholder = `{{${key}}}`;
      const value = templateVariables[key];
      finalMessage = finalMessage.replace(placeholder, value);
    }
  }

  // -----------------------------
  // Twilio API endpoint
  // -----------------------------
  const url = `https://api.twilio.com/2010-04-01/Accounts/${twilioAccountSid}/Messages.json`;

  // -----------------------------
  // Request payload
  // -----------------------------
  const payload = new URLSearchParams();
  payload.append("From", `whatsapp:${fromPhoneNumber}`);
  payload.append("To", `whatsapp:${toPhoneNumber}`);
  payload.append("Body", finalMessage);

  if (templateSid) {
    payload.append("MessagingServiceSid", templateSid);
  }

  // -----------------------------
  // API call
  // -----------------------------
  try {
    const result = await axios({
      method: "POST",
      url,
      auth: {
        username: twilioAccountSid,
        password: twilioAuthToken
      },
      headers: {
        "Content-Type": "application/x-www-form-urlencoded"
      },
      data: payload.toString()
    });

    console.log("Message sent. SID:", result.data.sid);
    return result.data;

  } catch (error) {
    console.error("WhatsApp sending error:", error.response?.data || error);
    throw error;
  }
}

// ---------------------------------
// Generic usage example
// ---------------------------------
sendWhatsAppMessage({
  toPhoneNumber: "+550000000000",
  fromPhoneNumber: "+14155238886",
  messageText: "New event: {{event_name}}",
  templateVariables: {
    event_name: "Example"
  },
  twilioAccountSid: "ACXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
  twilioAuthToken: "your_twilio_auth_token"
});
