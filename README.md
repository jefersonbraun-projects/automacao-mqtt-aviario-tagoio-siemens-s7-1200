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

### Principais configurações do dispositivo
Arquivo contendo as principais configurações do CLP definidas no TIA Portal.

- [principais_configuracoes_dispositivo](./docs/principais_configuracoes_dispositivo.md)

### Main [OB1]

 O bloco Main [OB1] é o bloco de organização cíclico principal do TIA Portal, responsável por executar continuamente a lógica do CLP em cada ciclo de varredura; neste projeto, é a partir dele que são chamados os blocos de função principais, cuja chamada foi implementada em linguagem Ladder.
 
- [Main(OB1)](./docs/Main[OB1].md)
 
### Bloco de função Conexao_TagoIO[FB3]

O bloco Conexão_TagoIO gerencia toda a comunicação MQTT do CLP com a plataforma TagoIO, incluindo conexão, serialização do JSON e publicação das variáveis.

- [Conexao_TagoIO](./docs/Conexao_TagoIO[FB3].md)

### Bloco de função Simulacao_variaveis_DB[FB7]

O bloco Simulação_variáveis executa a geração dos valores simulados usados nos testes, aplicando o mesmo algoritmo de simulação para cada variável do processo.

- [Simulacao_variaveis](./docs/Simulacao_variaveis_DB[FB7].md)

### Bloco de função Rotina_Alarmes[FB8]

O bloco Rotina_Alarmes executa a verificação dos limites de cada variável monitorada e determina o estado e a mensagem de alarme correspondente.

- [Rotina_Alarmes](./docs/Rotina_Alarmes[FB8].md)

### Bloco de função Leitura_Variaveis[FB1]

O bloco Leitura_Variaveis interpreta o payload recebido via MQTT, identifica qual variável foi enviada e atualiza o valor correspondente no CLP.

- [Leitura_Variaveis.md](./docs/Leitura_Variaveis.md)

### Demais blocos funcionais desenvolvidos

- [Alarme_Verifica_Condicoes](./docs/Alarme_Verifica_Condicoes[FB9].md)
- [Variavel_simu](./docs/Variavel_simu[FB4].md)
  
### Documentos relevantes
- [Libraries_Comm_Controller](./docs/manuais-e-datasheets/109780503_Libraries_Comm_Controller_DOC_V2_3_0_en.pdf)
- [Librarie LStream](./docs/manuais-e-datasheets/109781165_LStream_DOC_v1_6_en.pdf)
- [S71200_G2_system_manual](./docs/manuais-e-datasheets/S71200_G2_system_manual_en-US_en-US.pdf)

## Configurações e Scripts do TagoIO

A seguir estão documentados os scripts configurados na plataforma TagoIO para compor a lógica de monitoramento, processamento e integração utilizados neste projeto. Cada módulo foi desenvolvido em Python e executa de forma automática na plataforma, desempenhando funções específicas como supervisão de inatividade, cálculos estatísticos e comunicação externa.

- [Supervisao_Inatividade_Dispositivos](./docs/Supervisao_Inatividade_Dispositivos.md)
- [Calculo_min_Max](./docs/Calculo_min_Max.md)
- [Publicacao_limites](./docs/Publicacao_limites.md)
- [Conexao_Twilio](./docs/Conexao_Twilio.md)

## Configurações no Twilio

A seguir documento com as configurações realizadas no Twilio para habilitar o envio de mensagens WhatsApp utilizadas neste projeto.

- [configuracoes-twilio](./docs/configuracoes-twilio.md)




