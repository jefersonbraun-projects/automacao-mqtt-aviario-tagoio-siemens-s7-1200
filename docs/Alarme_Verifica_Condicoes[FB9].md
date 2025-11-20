# Alarme_Verifica_Condicoes[FB9]

O bloco Alarme_Verifica_Condicoes é estruturado em duas networks: a primeira executa a verificação dos limites superior e inferior da variável monitorada e define o estado de alarme, enquanto a segunda monta o texto final da mensagem a partir do valor da variável e do limite ultrapassado.

## Network 1
A Network 1 executa a lógica de verificação dos limites superior e inferior da variável monitorada, aplica o tempo de confirmação do alarme e define o estado final de disparo.
<img width="1081" height="782" alt="image" src="https://github.com/user-attachments/assets/cdb1e147-14a2-4b0e-95d8-a33709c5f93f" />

## Network 2
A Network 2 converte o valor monitorado para string, identifica qual limite foi ultrapassado e monta a mensagem final de alarme com texto e unidade

```scl
#temp_int := INT_TO_REAL(#Variavel_Monitorada);
#Var_Value_STR := INT_TO_STRING(#temp_int);

IF LEFT(IN := #Var_Value_STR, L := 1) = '+' THEN
    #Var_Value_STR := RIGHT(IN := #Var_Value_STR, L := LEN(#Var_Value_STR) - 1);
END_IF;

IF #Limite_Sup_Atingido THEN
    #Texto_Inicial := #Texto_Inicial_Lim_Sup;
END_IF;

IF #Limite_Inf_Antigido THEN
    #Texto_Inicial := #Texto_Inicial_Lim_Inf;
END_IF;

IF (#Limite_Sup_Atingido OR #Limite_Inf_Antigido) AND #Publica_Alarme THEN
    #Mensagem_Alarme := CONCAT(
        IN1 := #Texto_Inicial,
        IN2 := CONCAT(
            IN1 := #Var_Value_STR,
            IN2 := #Unidade_Medida
        )
    );
END_IF;
```
