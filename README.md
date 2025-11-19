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


