# Publicacao_limites

A Action *publicacao_limites* é responsável por enviar ao PLC os novos valores de setpoints configurados no dashboard do TagoIO. A automação monitora continuamente as variáveis `s01` a `s10`, e sempre que qualquer uma delas é alterada, a Action publica automaticamente o novo valor no tópico MQTT **setpoints**, que posteriormente é tratado pelo bloco *Leitura_Variaveis* no CLP.

A imagem abaixo ilustra a tela de configuração dessa ação:

*(inserir imagem aqui)*

Os parâmetros utilizados são:

- **Trigger (s01 a s10):** Conjunto de variáveis monitoradas pelo TagoIO. Qualquer alteração em qualquer uma delas aciona a publicação automática dos novos setpoints.

- **Type of action – Publish to MQTT:** Define que a saída da Action será uma publicação MQTT destinada ao dispositivo configurado.

- **Publish to the device:** Dispositivo alvo que receberá os setpoints; no caso, o CLP conectado ao broker do TagoIO.

- **Topic (setpoints):** Tópico MQTT utilizado para envio dos dados de setpoints ao PLC. O bloco *Leitura_Variaveis* é responsável por interpretar este tópico.

- **Payload ($VARIABLE$-$VALUE$):** Estrutura dinâmica enviada ao PLC contendo o identificador da variável alterada e o valor correspondente, no formato esperado pelo bloco de leitura.

Essa Action implementa o fluxo de envio de informações **do TagoIO para o PLC**, garantindo que qualquer ajuste realizado na interface do usuário seja refletido imediatamente no controlador.

<img width="1647" height="901" alt="image" src="https://github.com/user-attachments/assets/50cce434-3d18-44f9-9518-62fb09bc5484" />
