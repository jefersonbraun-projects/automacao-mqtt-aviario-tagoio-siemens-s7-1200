# Monitoramento e Notificação Ambiental com CLP Siemens S7-1200, MQTT, TagoIO e WhatsApp

Este projeto foi desenvolvido para demonstrar uma arquitetura industrial aplicada ao monitoramento ambiental em aviários, utilizando um CLP Siemens S7-1200 como unidade central de processamento. O objetivo é executar de forma confiável atividades críticas como aquisição de variáveis, tratamento de limites, geração de alarmes e envio periódico de dados para um broker MQTT. A partir desses dados, a plataforma TagoIO realiza o armazenamento histórico, visualização em dashboards e disparo de notificações automáticas via WhatsApp.

A proposta central é garantir robustez tanto no nível de software quanto no nível de hardware.
Ao empregar um CLP industrial — projetado para operar 24/7 em ambientes adversos — o sistema mantém alta disponibilidade, confiabilidade e possibilidade de expansão, diferentemente de soluções domésticas baseadas em microcontroladores ou computadores pessoais. Além disso, o uso de MQTT permite integrar o CLP com aplicações externas, serviços em nuvem e ferramentas de IoT de maneira padronizada e escalável.

O sistema foi projetado para ser modular, reproduzível e de fácil manutenção, permitindo sua aplicação em estruturas avícolas reais e em ambientes acadêmicos para estudo de automação industrial, protocolos IoT e integração com serviços em nuvem.

## Ambientes Suportados

O projeto foi desenvolvido com base no padrão IEC 61131-3, utilizado amplamente em controladores lógicos programáveis. Embora diferentes fabricantes possuam seus próprios ambientes de desenvolvimento, todos seguem esta norma, o que permite modularidade, padronização e facilidade de manutenção.

Este repositório foi construído especificamente para:

- **TIA Portal V20 – Siemens S7-1200 (G2)**

Além disso, vários ambientes compatíveis com IEC 61131-3 podem utilizar conceitos similares aos deste projeto, incluindo:

- **CODESYS V3 – 3S-Smart Software Solutions**
- **e!COCKPIT – WAGO**
- **TwinCAT 3 – Beckhoff**
- **Outras plataformas baseadas em LAD, FBD ou ST**

Observação: As bibliotecas **LMQTT** e **LStream** utilizadas aqui são exclusivas da linha Siemens S7-1200/S7-1500.

## Arquitetura

<img width="945" height="559" alt="image" src="https://github.com/user-attachments/assets/3dbd424b-c79f-457a-ad6e-0ca41e69c70b" />
