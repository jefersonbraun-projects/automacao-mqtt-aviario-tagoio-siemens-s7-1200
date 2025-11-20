Este documento apresenta as principais configurações aplicadas ao CLP no TIA Portal, incluindo informações do hardware utilizado, parâmetros de endereçamento IP e definição dos servidores DNS. Essas configurações são essenciais para garantir o correto funcionamento do controlador e sua comunicação com a rede local e com a plataforma TagoIO por meio do protocolo MQTT.

A imagem apresenta as informações gerais do módulo CPU 1212C DC/DC/DC utilizado no projeto, incluindo o modelo do hardware, a descrição técnica do dispositivo, o número de artigo (6ES7 212-1AG50-0XB0) e a versão de firmware instalada (V1.0). Esses dados caracterizam a capacidade de memória, recursos de comunicação e funcionalidades disponíveis na CPU, servindo como referência para compatibilidade, bibliotecas suportadas e parametrização no TIA Portal.
<img width="1520" height="921" alt="image" src="https://github.com/user-attachments/assets/39a29bdb-9ca4-44f4-96aa-ed89cc4a3ec5" />

Esta imagem exibe a configuração de rede PROFINET da CPU, destacando o endereço IP definido no projeto (192.168.0.35), a máscara de sub-rede (255.255.255.0) e o roteador padrão habilitado (192.168.0.1). Esses parâmetros estabelecem a conectividade do CLP com a rede local e permitem o acesso ao broker MQTT na nuvem através do gateway configurado.
<img width="1039" height="374" alt="image" src="https://github.com/user-attachments/assets/97f46e7a-9edd-4d14-91bf-1ec08851bdb7" />

A imagem mostra a configuração dos servidores DNS utilizados pela CPU para resolução de nomes de domínio, incluindo o servidor DNS público do Google (8.8.8.8) e o roteador local (192.168.0.1). Essa configuração é essencial para permitir que o CLP resolva o endereço do broker MQTT do TagoIO e acesse serviços externos por nome, em vez de depender de endereços IP fixos.
<img width="926" height="339" alt="image" src="https://github.com/user-attachments/assets/452cf364-fdc3-4614-a3ec-2045eabfc3e1" />
