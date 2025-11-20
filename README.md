# Monitoramento e Notificação Ambiental com CLP Siemens S7-1200, MQTT, TagoIO e WhatsApp

Este projeto foi desenvolvido para demonstrar uma arquitetura industrial aplicada ao monitoramento ambiental em aviários, utilizando um CLP Siemens S7-1200 como unidade central de processamento. O objetivo é executar de forma confiável atividades críticas como aquisição de variáveis, tratamento de limites, geração de alarmes e envio periódico de dados para um broker MQTT. A partir desses dados, a plataforma TagoIO realiza o armazenamento histórico, visualização em dashboards e disparo de notificações automáticas via WhatsApp.

A proposta central é garantir robustez tanto no nível de software quanto no nível de hardware.
Ao empregar um CLP industrial — projetado para operar 24/7 em ambientes adversos — o sistema mantém alta disponibilidade, confiabilidade e possibilidade de expansão, diferentemente de soluções domésticas baseadas em microcontroladores ou computadores pessoais. Além disso, o uso de MQTT permite integrar o CLP com aplicações externas, serviços em nuvem e ferramentas de IoT de maneira padronizada e escalável.

O sistema foi projetado para ser modular, reproduzível e de fácil manutenção, permitindo sua aplicação em estruturas avícolas reais e em ambientes acadêmicos para estudo de automação industrial, protocolos IoT e integração com serviços em nuvem.

## Ambientes de Desenvolvimento

- **TIA Portal V20 (Siemens)**  
  Utilizado para o desenvolvimento e a programação do CLP Siemens S7-1200, abrangendo STEP 7 Professional e os ambientes de configuração integrados do TIA Portal.

- **TagoIO – Plataforma Cloud**  
  Os dashboards foram desenvolvidos utilizando o recurso nativo de **Blueprint Dashboard**
  As análises automatizadas foram implementadas por meio do **Runtime Python (python-rt2025)** disponível na própria plataforma.

- **Twilio API – WhatsApp Business**  
  A funcionalidade de envio de mensagens foi implementada utilizando o modo **WhatsApp Sandbox**, recurso nativo da API do Twilio para testes e validação de fluxos de notificação.


## Arquitetura

A arquitetura do sistema é composta por um CLP Siemens S7-1200, responsável pela aquisição dos dados ambientais e acionamento de atuadores no alojamento das aves, que se conecta à nuvem por meio da internet utilizando o protocolo MQTT. As informações coletadas são publicadas no MQTT Broker do TagoIO, onde são processadas, armazenadas e disponibilizadas para visualização em dashboards. A partir desses dados, a plataforma executa ações automáticas que incluem o envio de notificações via WhatsApp, utilizando a API do Twilio integrada à infraestrutura do WhatsApp Business da Meta. Os usuários podem acessar os dashboards e receber alertas em qualquer lugar com acesso à internet, permitindo acompanhamento remoto, supervisão contínua e resposta rápida a situações críticas no ambiente avícola.

<img width="945" height="559" alt="image" src="https://github.com/user-attachments/assets/3dbd424b-c79f-457a-ad6e-0ca41e69c70b" />

## Software do CLP

### Main [OB1]

 O bloco Main [OB1] é o bloco de organização cíclico principal do TIA Portal, responsável por executar continuamente a lógica do CLP em cada ciclo de varredura; neste projeto, é a partir dele que são chamados os blocos de função principais, cuja chamada foi implementada em linguagem Ladder.
 
- [Main[OB1](./docs/Main[OB1])
