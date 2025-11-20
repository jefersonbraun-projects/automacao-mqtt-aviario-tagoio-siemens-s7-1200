# configuracoes-twilio.md

### Informações da Conta (Account Dashboard)

Após a criação da conta no Twilio, foram acessadas as informações essenciais de autenticação disponíveis no painel principal. Nesta etapa, foram obtidos:

- **Account SID** – Identificador único da conta, utilizado para autenticação das requisições feitas à API.
- **Auth Token** – Token secreto necessário para validar chamadas à API do Twilio.
- **Twilio Phone Number** – Número virtual fornecido para envio de mensagens via WhatsApp no ambiente Sandbox.

Esses dados são utilizados pelo módulo de comunicação configurado no TagoIO para permitir o envio de notificações autenticadas via Twilio.

<img width="1063" height="971" alt="image" src="https://github.com/user-attachments/assets/9d6aedea-132c-4920-8dee-dd9232024b54" />

Em seguida, foi configurado o **Twilio Sandbox for WhatsApp**, que permite realizar testes de envio de mensagens sem aprovação prévia da Meta. Nesta interface foram definidas ou consultadas:

- **Webhook de entrada (When a message comes in)** – URL utilizada pelo Sandbox para encaminhar mensagens recebidas.
- **Número do Sandbox WhatsApp** – Telefone fornecido pelo Twilio para testes.
- **Código de participação ("join XXXXX")** – Utilizado para registrar o número pessoal como participante do Sandbox.
- **Lista de participantes autorizados** – Confirmação dos números vinculados ao ambiente de testes.

As informações desse Sandbox foram utilizadas pela função do TagoIO responsável pelo envio de mensagens via WhatsApp, por meio da API do Twilio.

<img width="1211" height="896" alt="image" src="https://github.com/user-attachments/assets/ccda400d-2871-41a7-8695-7fbb0ed3e8e7" />
