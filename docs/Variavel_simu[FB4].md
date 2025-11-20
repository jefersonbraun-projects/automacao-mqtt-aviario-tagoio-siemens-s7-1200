# Variavel_simu[FB4]

O bloco Variavel_simu gera um valor de processo simulado com base em uma curva diária interpolada, aplicando amostragem periódica, avanço de tempo interno, interpolação linear entre pontos definidos, adição opcional de ruído pseudoaleatório e cálculo da saída final com ganho, offset e arredondamento.

**Entradas**
- **Enable**: habilita a execução do bloco e libera a amostragem periódica.  
- **Ts_ms**: período de amostragem em milissegundos.  
- **NoiseAmp**: amplitude do ruído pseudoaleatório aplicado ao valor simulado.  
- **Gain**: fator multiplicativo aplicado ao valor base.  
- **Offset**: deslocamento somado após o ganho.  
- **Seed**: valor inicial para geração de ruído com LCG (Linear Congruential Generator).  
- **Hrs[]**: pontos da curva diária (eixo do tempo, em horas).  
- **Vals[]**: valores correspondentes aos pontos da curva diária.  
- **N**: quantidade de pontos da curva (tamanho dos vetores).  

**Saídas**
- **Temp**: valor simulado bruto resultante da interpolação e ajustes.  
- **Temp_1dp**: valor final arredondado para uma casa decimal (saída utilizada pelo restante do processo).  
- **sample_trig** (interno): indica o momento exato em que ocorre uma nova amostragem.  


```scl
// --- Amostragem periódica ---
#tonSample(IN := #Enable,
           PT := #Ts_ms);

#sample_trig := FALSE;

IF (#Enable AND #tonSample.Q) THEN
    #sample_trig := TRUE;
    #tonSample.IN := FALSE; // rearma
END_IF;

// --- Avanço do tempo simulado (ciclo de 24 h) ---
IF #sample_trig THEN
    #t_acc_s := #t_acc_s + INT_TO_REAL((TIME_TO_INT(#Ts_ms) / 1000.0));

    // wrap diário
    IF #t_acc_s >= 86400.0 THEN
        #t_acc_s := #t_acc_s - 86400.0;
    END_IF;
END_IF;

// Hora corrente (0..24)
#hourNow := (#t_acc_s / 3600.0);

IF #hourNow < 0.0 THEN
    #hourNow := #hourNow + 24.0;
END_IF;

IF #hourNow >= 24.0 THEN
    #hourNow := #hourNow - 24.0;
END_IF;

// --- Seleção do segmento de interpolação [i,i+1] ---
#i := 0;

// Caso “antes do primeiro ponto” (entre 24:00→0:00 e o primeiro vértice)
IF #hourNow < #Hrs[0] THEN
    #t0 := #Hrs[#N - 1];
    #t1 := #Hrs[0] + 24.0;
    #v0 := #Vals[#N - 1];
    #v1 := #Vals[0];

    #alpha := (#hourNow + 24.0 - #t0) / (#t1 - #t0);

ELSE
    // Busca linear
    FOR #i := 0 TO #N - 2 DO
        IF (#hourNow >= #Hrs[#i]) AND (#hourNow <= #Hrs[#i + 1]) THEN
            #t0 := #Hrs[#i];
            #t1 := #Hrs[#i + 1];
            #v0 := #Vals[#i];
            #v1 := #Vals[#i + 1];

            IF (#t1 > #t0) THEN
                #alpha := (#hourNow - #t0) / (#t1 - #t0);
            ELSE
                #alpha := 0.0;
            END_IF;

            EXIT;
        END_IF;
    END_FOR;
END_IF;

// --- Interpolação linear ---
#base := #v0 + #alpha * (#v1 - #v0);

// --- Ruído simples (LCG) opcional ---
IF #sample_trig AND (#NoiseAmp > 0.0) THEN
    // LCG: Seed = (a*Seed + c) mod 2^32
    #Seed := DINT_TO_DWORD(1664525) * #Seed + 1013904223; // overflow intencional
    #tmp_dint := #Seed AND 16#7FFFFFFF; // 31 bits positivos

    #noise_u := (DINT_TO_REAL(#tmp_dint) / 2147483648.0) * 2.0 - 1.0; // [-1,1]

ELSE
    #noise_u := 0.0;
END_IF;

// --- Saída, com ganho/offset e arredondamento 0,1 °C ---
#Temp := (#base + #NoiseAmp * #noise_u);
#Temp := #Temp * #Gain + #Offset;

#Temp_1dp := DINT_TO_REAL(ROUND(#Temp * 10.0)) / 10.0;
```
