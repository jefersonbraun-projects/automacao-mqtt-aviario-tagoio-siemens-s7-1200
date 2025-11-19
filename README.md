# Avicultura 4.0 – Monitoramento e Notificação de variáveis de ambiente com CLP Siemens S7-1200, MQTT, TagoIO e WhatsApp

Este repositório contém o código-fonte, artefatos e documentação técnica do sistema desenvolvido para monitoramento ambiental aplicado à avicultura industrial. A solução integra um Controlador Lógico Programável (CLP Siemens S7-1200 G2) à nuvem por meio do protocolo MQTT, com supervisão via TagoIO e envio de notificações automáticas pelo WhatsApp utilizando Twilio.

O projeto foi desenvolvido no contexto de um Trabalho de Conclusão de Curso (Engenharia Elétrica – UNISINOS) e demonstra a viabilidade de uma arquitetura conectada, escalável e replicável para controle ambiental em aviários.

---

# 1. Visão Geral do Sistema

<img width="945" height="559" alt="image" src="https://github.com/user-attachments/assets/00556f8c-a55d-43a1-9b75-b25b9bd4ee97" />


A arquitetura do projeto está organizada em quatro camadas principais:

## 1.1 Camada de Campo – CLP Siemens S7-1200  
- Aquisição de variáveis ambientais (temperatura, umidade, nível de água, nível de ração, pH e cloro).  
- Bibliotecas utilizadas: **LMQTT** (cliente MQTT nativo) e **LStream** (serialização JSON).  
- Estruturas de simulação e alarmes.  
- Programação em **Ladder (LAD)** e **SCL**.

## 1.2 Camada de Comunicação – MQTT  
- Conexão com broker MQTT do TagoIO.  
- Publicação periódica JSON no tópico `tago/data/post`.  
- Reconexão automática e tratamento de erros.

## 1.3 Camada de Nuvem – TagoIO  
- Banco de dados de séries temporais.  
- Payload Parser.  
- Regras automáticas (Actions) para alertas.  
- Dashboards configuráveis e exportáveis via Blueprint.

## 1.4 Camada de Notificação – WhatsApp/Twilio  
- Envio automático de alertas quando variáveis excedem limites.  
- Compatível com Sandbox e modo produção.
---

# 2. Funcionalidades Principais

- Monitoramento contínuo de variáveis ambientais.  
- Comunicação segura via MQTT com payloads JSON.  
- Dashboards e gráficos em tempo real no TagoIO.  
- Sistema de alarmes modular no CLP.  
- Envio automático de mensagens via WhatsApp.  
- Reprodutibilidade total do ambiente para fins acadêmicos.

---

# 3. Estrutura do Repositório

Avicultura4.0/
├── docs/
│ ├── architecture/
│ ├── plc/
│ ├── cloud/
│ ├── tests/
│ └── tcc/
├── src/
│ ├── plc/
│ ├── tagoio/
│ ├── twilio/
│ └── tools/
├── examples/
├── CHANGELOG.md
├── LICENSE
└── README.md


---

# 4. PLC – Programação e Componentes

O projeto do CLP (TIA Portal V20) encontra-se em `/src/plc/project_archive/`.

### Blocos Funcionais (FB)
- **FB_Conexao_MQTT** — gerenciamento MQTT  
- **FB_JSON_Serializer** — construção do payload JSON  
- **FB_Alarmes** — tratamento de limites e alarmes  
- **FB_Simulacao** — geração de dados para testes  
- **FB_Heartbeat** — monitoramento de integridade  

### Data Blocks (DB)
- DB_Config_MQTT  
- DB_Sensores  
- DB_Alarmes  

### Tipos de Programação  
- Ladder (LAD)  
- Structured Control Language (SCL)  

As lógicas estão documentadas em `/docs/plc/`.

---

# 5. Comunicação MQTT

- **Broker:** TagoIO  
- **Porta:** 8883 (TLS)  
- **Tópico:** `tago/data/post`  
- **Formato:** JSON  

Exemplo:

```json
{
  "variable": "temperatura",
  "value": 27.8,
  "unit": "°C",
  "location": {"lat": -29.0, "lng": -51.1}
}
```
### 6. Nuvem – TagoIO

Arquivos disponíveis em `/src/tagoio/`:

- Payload Parser  
- Blueprint do Dashboard  
- Actions (regras automáticas de notificação)  
- Configuração de dispositivos MQTT  

### Funções principais
- Armazenamento histórico  
- Visualização em dashboards  
- Envio de alertas conforme limites  
- Live Inspector para depuração  

---

## 7. Notificações via WhatsApp – Twilio

A integração com o WhatsApp permite:

- envio de alertas automáticos  
- envio de mensagens personalizadas  
- uso de Sandbox para desenvolvimento  

### Exemplo de mensagem
ALERTA – Temperatura acima do limite no Aviário 1.
Valor atual: 35.7°C

Documentação detalhada em `/src/twilio/`.

---

## 8. Testes e Validação

O diretório `/docs/tests/` inclui:

- plano de testes  
- logs MQTT  
- gráficos de simulação  
- tabelas de alarmes  
- validação de envio de mensagens  

---

## 9. Como Executar

### 1. No CLP (TIA Portal)
- Importar o projeto  
- Configurar bibliotecas **LMQTT** e **LStream**  
- Ajustar token MQTT no DB  
- Compilar e transferir para o CLP  

### 2. No TagoIO
- Criar dispositivo MQTT  
- Adicionar token ao CLP  
- Configurar parser e dashboard  
- Validar no **Live Inspector**  

### 3. Notificações
- Configurar Twilio  
- Ativar Sandbox  
- Testar alarmes no CLP  

---

## 10. Tecnologias Utilizadas

- Siemens S7-1200 G2  
- TIA Portal V20  
- LMQTT / LStream  
- MQTT 3.1.1  
- TagoIO  
- Twilio API  
- Python (scripts auxiliares)  

---

## 11. Licença

Este repositório é distribuído sob a **Licença MIT**.  
Consulte o arquivo `/LICENSE`.

---

## 12. Contato

**Autor:** Jeferson Luan Braun  
**Curso:** Engenharia Elétrica – UNISINOS  
**Projeto:** Avicultura 4.0 – Automação e Monitoramento Ambiental  


