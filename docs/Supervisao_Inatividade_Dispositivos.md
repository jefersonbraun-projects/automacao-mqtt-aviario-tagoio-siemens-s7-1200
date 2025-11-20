# Supervisao_Inatividade_Dispositivo

O seguinte script é executado pelo módulo de Análises do TagoIO, configurado para rodar automaticamente em um intervalo de cinco minutos. Desenvolvido em Python, ele verifica a inatividade dos dispositivos filtrados por uma TAG definida e calcula o tempo decorrido desde o último envio de dados. Caso algum dispositivo ultrapasse o limite estabelecido, o script registra no bucket de um dispositivo específico uma variável indicando a condição de inatividade.


```python
# /// script
# dependencies = [
#   "tagoio-sdk"
# ]
# ///

from datetime import datetime
from tagoio_sdk import Account, Analysis, Device
from tagoio_sdk.modules.Utils.envToJson import envToJson


def my_analysis(context, scope=None):

    context.log("=== Iniciando análise de inatividade ===")

    # Variáveis de ambiente
    env = envToJson(context.environment)

    account_token = env.get("account_token")
    tag_key = env.get("tag_key")
    tag_value = env.get("tag_value")
    device_token = env.get("device_token")

    # Verificações
    if not account_token:
        return context.log("[ERRO] account_token não definido.")

    if not tag_key or not tag_value:
        return context.log("[ERRO] tag_key e tag_value são obrigatórios.")

    if not device_token:
        return context.log("[ERRO] device_token não definido.")

    context.log("[INFO] Account Token carregado.")
    context.log(f"[INFO] Device Token carregado: {device_token[:8]}... (ocultado)")
    context.log(f"[INFO] Buscando devices com TAG: {tag_key} = {tag_value}")

    # Conexão com a conta
    account = Account({"token": account_token})

    # Listagem dos devices
    devices = account.devices.listDevice({
        "page": 1,
        "amount": 1000,
        "fields": ["id", "name", "last_input"],
    })

    if not devices:
        return context.log("[AVISO] Nenhum device encontrado.")

    context.log(f"[INFO] {len(devices)} dispositivos encontrados.")

    now = datetime.utcnow()

    # Instancia o device único para escrita
    device_writer = Device({"token": device_token})

    # Loop dos devices encontrados
    for dev in devices:

        context.log(f"--- Analisando dispositivo: {dev['name']} ---")

        last_input = dev.get("last_input")
        if not last_input:
            context.log("[AVISO] Este dispositivo nunca enviou dados.")
            continue

        diff = (now - last_input).total_seconds() // 60
        context.log(f"[INFO] Inatividade: {diff} minutos")

        # Regra dos 40 minutos
        if diff > 5:
            context.log("[ALERTA] Mais de 40 minutos de inatividade detectados.")
            context.log("[INFO] Gravando variável 'evento_registrado' no device destino...")

            try:
                device_writer.sendData({
                    "variable": "evento_registrado",
                    "value": "O dispositivo está inativo por mais de 40 minutos.",
                    "metadata": {"minutos_inativo": diff}
                })
                context.log("[SUCESSO] Evento registrado com sucesso!")

            except Exception as e:
                context.log(f"[ERRO] Falha ao escrever no device: {str(e)}")

        else:
            context.log("[OK] Dentro do limite. Nenhum evento registrado.")

    context.log("=== Análise concluída ===")
```



