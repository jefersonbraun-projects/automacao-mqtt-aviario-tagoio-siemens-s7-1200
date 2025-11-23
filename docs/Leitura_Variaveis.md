# Leitura_Variaveis

O bloco *Leitura_Variaveis* é responsável por interpretar os dados recebidos via MQTT pelo módulo `MQTT_SUB`, extrair o identificador da variável publicada e atualizar os valores internos correspondentes. A lógica é estruturada em três Networks, sendo a Network 3 a responsável pelo tratamento do payload recebido.

## Network 1 — Habilita conexão com a nuvem
Executa temporização inicial e habilita o bloco de assinatura MQTT após estabilização da comunicação.

<img width="1354" height="630" alt="image" src="https://github.com/user-attachments/assets/32fc8bbf-a090-4908-98cc-affa980b6488" />

## Network 2 — Chamada do bloco MQTT
Responsável pela instanciação e execução do bloco funcional `LMQTT_Client` para receber mensagens via protocolo MQTT.

<img width="706" height="758" alt="image" src="https://github.com/user-attachments/assets/f434d7d1-9f85-48ce-a633-403a293661a6" />

## Network 3 — Tratamento de dados recebidos

A Network 3 realiza a leitura dos bytes recebidos no vetor `receivedMsgPayload`, converte o identificador da variável (`s01` a `s11`) e o valor correspondente, aplicando o processamento necessário para cada variável da aplicação.

### Código SCL

```scl
"MQTT_SUB".STR_VAR_REC := CONCAT(IN1 := BYTE_TO_CHAR("MQTT_SUB".receivedMsgPayload[0]), IN2 :=
                                   CONCAT(IN1 := BYTE_TO_CHAR("MQTT_SUB".receivedMsgPayload[1]), IN2 := BYTE_TO_CHAR("MQTT_SUB".receivedMsgPayload[2])));


"MQTT_SUB".STR_VALOR := CONCAT(IN1 := BYTE_TO_CHAR("MQTT_SUB".receivedMsgPayload[4]), IN2 :=
                                 CONCAT(IN1 := BYTE_TO_CHAR("MQTT_SUB".receivedMsgPayload[5]), IN2 :=
                                        CONCAT(IN1 := BYTE_TO_CHAR("MQTT_SUB".receivedMsgPayload[6]), IN2 :=
                                               CONCAT(IN1 := BYTE_TO_CHAR("MQTT_SUB".receivedMsgPayload[7]), IN2 := BYTE_TO_CHAR("MQTT_SUB".receivedMsgPayload[8])))));

"MQTT_SUB".Valor_Var := STRING_TO_REAL("MQTT_SUB".STR_VALOR);

IF "MQTT_SUB".STR_VAR_REC = 's11' THEN
    "GLOBAL_VARS".Envia_todas_vars := TRUE;

    FOR #i := 0 TO 10 DO
        "MQTT_SUB".receivedMsgPayload[#i] := 0;
    END_FOR;
END_IF;

IF "MQTT_SUB".STR_VAR_REC = 's01' THEN
    "MQTT_SUB".s01 := "MQTT_SUB".Valor_Var;
END_IF;

IF "MQTT_SUB".STR_VAR_REC = 's02' THEN
    "MQTT_SUB".s02 := "MQTT_SUB".Valor_Var;
END_IF;

IF "MQTT_SUB".STR_VAR_REC = 's03' THEN
    "MQTT_SUB".s03 := "MQTT_SUB".Valor_Var;
END_IF;

IF "MQTT_SUB".STR_VAR_REC = 's04' THEN
    "MQTT_SUB".s04 := "MQTT_SUB".Valor_Var;
END_IF;

IF "MQTT_SUB".STR_VAR_REC = 's05' THEN
    "MQTT_SUB".s05 := "MQTT_SUB".Valor_Var;
END_IF;

IF "MQTT_SUB".STR_VAR_REC = 's06' THEN
    "MQTT_SUB".s06 := "MQTT_SUB".Valor_Var;
END_IF;

IF "MQTT_SUB".STR_VAR_REC = 's07' THEN
    "MQTT_SUB".s07 := "MQTT_SUB".Valor_Var;
END_IF;

IF "MQTT_SUB".STR_VAR_REC = 's08' THEN
    "MQTT_SUB".s08 := "MQTT_SUB".Valor_Var;
END_IF;

IF "MQTT_SUB".STR_VAR_REC = 's09' THEN
    "MQTT_SUB".s09 := "MQTT_SUB".Valor_Var;
END_IF;

IF "MQTT_SUB".STR_VAR_REC = 's10' THEN
    "MQTT_SUB".s10 := "MQTT_SUB".Valor_Var;
END_IF;
```
