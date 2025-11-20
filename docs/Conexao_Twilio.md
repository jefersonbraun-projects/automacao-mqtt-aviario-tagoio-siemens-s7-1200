# Conexao_Twilio

A API do Twilio é executada diretamente pelo módulo de Actions do TagoIO, que fornece uma interface pronta para configuração dos parâmetros necessários ao envio de mensagens via WhatsApp. A imagem abaixo ilustra a tela de configuração da Action responsável pelo disparo das notificações:

O TagoIO abstrai as chamadas diretas à API do Twilio e permite que todos os campos necessários sejam configurados pela interface. Os parâmetros utilizados são:

- **Phone number(s):** Número(s) de destino no formato internacional. O TagoIO envia a mensagem para cada número listado, utilizando o endpoint oficial do Twilio.

- **From (Twilio Sandbox Number):** Número de origem fornecido pelo Twilio, normalmente o número padrão do Sandbox para WhatsApp (+14155238886). É obrigatório para permitir a autenticação da mensagem.

- **Message:** Texto bruto que será enviado ao WhatsApp. Aceita templates, variáveis e substituições dinâmicas configuradas pelo usuário.

- **Template Variables:** Estrutura opcional de chave–valor usada quando o usuário emprega modelos de mensagem do Twilio. Essas variáveis são substituídas automaticamente no momento do envio.

- **Template SID:** Identificador único de Template do Twilio, utilizado apenas quando mensagens estruturadas (templates aprovados) são usadas. O valor é armazenado como Secret no TagoIO.

- **Twilio SID (TWILIO_ACCOUNT_SID):** Código único da conta Twilio. Permite que o TagoIO autentique a requisição antes de acessar o endpoint `/Messages.json`.

- **Twilio Auth Token (TWILIO_AUTH_TOKEN):** Token de autenticação da API do Twilio. O TagoIO utiliza esse token para assinar todas as requisições enviadas ao Twilio. Também é armazenado como Secret.


<img width="1639" height="896" alt="image" src="https://github.com/user-attachments/assets/995cffa8-8acf-46ba-be31-b49a471f7c18" />

A seguir exemplo de código para acesso a API do Twilio

```javascript
/**
 
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
```
