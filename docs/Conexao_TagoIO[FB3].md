#Conexao_TagoIO[FB3].md

Este documento apresenta uma visão geral da estrutura e do funcionamento do bloco funcional responsável pela comunicação MQTT entre o CLP e a plataforma TagoIO.
O bloco Conexão_TagoIO é organizado em diversas networks que realizam, de forma sequencial, a inicialização da conexão MQTT, a montagem do payload JSON, a publicação dos dados e a lógica de envio periódico das variáveis para a nuvem.

<img width="1183" height="488" alt="image" src="https://github.com/user-attachments/assets/605e4cb3-d16c-4821-97c9-ce202f280800" />

##Network 1

<img width="688" height="237" alt="image" src="https://github.com/user-attachments/assets/f0359e17-f9be-41b7-9a02-4b296aad9d65" />

##Network 2

<img width="682" height="677" alt="image" src="https://github.com/user-attachments/assets/60bbe0c0-d287-4f0b-8ac5-24d41f33cf38" />

##Network 3

O código converte a variável de processo para string, formata a data/hora atual, seleciona o tipo de mensagem (normal ou alarme) e monta a estrutura JSON que será publicada via MQTT para o TagoIO.

```scl
// ------------ Converter Variável para String -------------
#temp_int := ROUND("GLOBAL_VARS".Var_Value["GLOBAL_VARS".indice_var] * 10.0);
#temp_real := INT_TO_REAL(#temp_int) / 10.0;
"MQTT".STR_Var_teste := REAL_TO_STRING(#temp_real);

IF LEFT(IN := "MQTT".STR_Var_teste, L := 1) = '+' THEN
    "MQTT".STR_Var_teste := RIGHT(IN := "MQTT".STR_Var_teste, L := LEN("MQTT".STR_Var_teste) - 1);
END_IF;

// ---------- Estruturando a Data ----------
#Aux_Ano := CONCAT(
    IN1 := RIGHT(IN := UINT_TO_STRING("GLOBAL_VARS".Date_time.YEAR),
                 L := LEN(UINT_TO_STRING("GLOBAL_VARS".Date_time.YEAR)) - 1),
    IN2 := '-'
);

#Aux_Mes := CONCAT(
    IN1 := RIGHT(IN := UINT_TO_STRING("GLOBAL_VARS".Date_time.MONTH),
                 L := LEN(UINT_TO_STRING("GLOBAL_VARS".Date_time.MONTH)) - 1),
    IN2 := '-'
);

#Aux_Dia := CONCAT(
    IN1 := RIGHT(IN := UINT_TO_STRING("GLOBAL_VARS".Date_time.DAY),
                 L := LEN(UINT_TO_STRING("GLOBAL_VARS".Date_time.DAY)) - 1),
    IN2 := ' '
);

#Aux_Hora := CONCAT(
    IN1 := RIGHT(IN := UINT_TO_STRING("GLOBAL_VARS".Date_time.HOUR),
                 L := LEN(UINT_TO_STRING("GLOBAL_VARS".Date_time.HOUR)) - 1),
    IN2 := ':'
);

#Aux_Minuto := CONCAT(
    IN1 := RIGHT(IN := UINT_TO_STRING("GLOBAL_VARS".Date_time.MINUTE),
                 L := LEN(UINT_TO_STRING("GLOBAL_VARS".Date_time.MINUTE)) - 1),
    IN2 := ':'
);

#date_time := CONCAT(
    IN1 := #Aux_Ano,
    IN2 := CONCAT(
        IN1 := #Aux_Mes,
        IN2 := CONCAT(
            IN1 := #Aux_Dia,
            IN2 := CONCAT(
                IN1 := #Aux_Hora,
                IN2 := CONCAT(IN1 := #Aux_Minuto, IN2 := '00')
            )
        )
    )
);

// ------------- Verificar se é Alarme para ajustar mensagem -------------
IF "Rotina_Alarmes_DB".Pub_Alarme THEN
    #Var_name := 'evento_registrado';
    #Var_Value := "Rotina_Alarmes_DB".mensagem_alarme;
    #Var_Type := 2;
    #Var_Unit := '';
ELSE
    #Var_name := "GLOBAL_VARS".Var_Name["GLOBAL_VARS".indice_var];
    #Var_Value := "MQTT".STR_Var_teste;
    #Var_Type := 3;
    #Var_Unit := '%';
END_IF;

// ---------- Estrutura do JSON ----------

// Posição 0 – Início do array JSON
"MQTT".Json_TAGOIO[0].type := 1;
"MQTT".Json_TAGOIO[0].key := '';
"MQTT".Json_TAGOIO[0].value := '';
"MQTT".Json_TAGOIO[0].depth := 0;
"MQTT".Json_TAGOIO[0].closingElement := FALSE;

// Posição 1 – Nome da variável
"MQTT".Json_TAGOIO[1].type := 2;
"MQTT".Json_TAGOIO[1].key := 'variable';
"MQTT".Json_TAGOIO[1].value := #Var_name;
"MQTT".Json_TAGOIO[1].depth := 2;
"MQTT".Json_TAGOIO[1].closingElement := FALSE;

// Posição 2 – Valor
"MQTT".Json_TAGOIO[2].type := #Var_Type;
"MQTT".Json_TAGOIO[2].key := 'value';
"MQTT".Json_TAGOIO[2].value := #Var_Value;
"MQTT".Json_TAGOIO[2].depth := 2;
"MQTT".Json_TAGOIO[2].closingElement := FALSE;

// Posição 3 – Unidade
"MQTT".Json_TAGOIO[3].type := 2;
"MQTT".Json_TAGOIO[3].key := 'unit';
"MQTT".Json_TAGOIO[3].value := #Var_Unit;
"MQTT".Json_TAGOIO[3].depth := 2;
"MQTT".Json_TAGOIO[3].closingElement := FALSE;

// Posição 4 – Data/hora
"MQTT".Json_TAGOIO[4].type := 2;
"MQTT".Json_TAGOIO[4].key := 'time';
"MQTT".Json_TAGOIO[4].value := #date_time;
"MQTT".Json_TAGOIO[4].depth := 2;
"MQTT".Json_TAGOIO[4].closingElement := TRUE;

// Posição 5 – Encerramento
"MQTT".Json_TAGOIO[5].type := -1;
"MQTT".Json_TAGOIO[5].key := '';
"MQTT".Json_TAGOIO[5].value := 'NULL';
"MQTT".Json_TAGOIO[5].depth := -1;
"MQTT".Json_TAGOIO[5].closingElement := FALSE;
```
