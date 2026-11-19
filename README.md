# prova-2-sistemas_distribuidos

evidencia2
antes
MAXIMO_SIMULTANEO = 3
depois
AXIMO_SIMULTANEO = 2
o valor da variavel maximo_simultaneo foi trocado de 3 para 2 


evidencia 3
antes:
def sincronizar():
    replica["estoque"] = replica["estoque"]
    
depois:
def sincronizar():
    replica["estoque"] = principal["estoque"]
a função esta colocando as replicas duas vezes


evidencia 4


antes:
def consultar_estoque():
    if principal["online"]:
        return(
            "PRINCIPAL",
            principal["estoque"]
        )

    if principal["online"]:
        return(
            "REPLICA",
            replica["estoque"]
        )

    return "INDISPONIVEL", None

depois:
def consultar_estoque():
    if principal["online"]:
        return(
            "PRINCIPAL",
            principal["estoque"]
        )

    if replica["online"]:
        return(
            "REPLICA",
            replica["estoque"]
        )

    return "INDISPONIVEL", None
as duas condicionais estavam repetida 



CÓDIGO PYTHON CORRIGIDO:

import threading
import time

#servidores simulados
principal = {
    "online": True,
    "estoque": 10
}

replica = {
    "online":True,
    "estoque": 10
}

#controle de estoque

MAXIMO_SIMULTANEO = 2

semaforo = threading.Semaphore(MAXIMO_SIMULTANEO)
trava = threading.Lock()

Pedidos_confirmados = []

def processar_pedidos(numero):
    with semaforo:
        if principal["estoque"] > 0:
            principal["estoque"] -= 1
            Pedidos_confirmados.append(numero)


def sincronizar():
    replica["estoque"] = principal["estoque"]



def consultar_estoque():
    if principal["online"]:
        return(
            "PRINCIPAL",
            principal["estoque"]
        )

    if replica["online"]:
        return(
            "REPLICA",
            replica["estoque"]
        )

    return "INDISPONIVEL", None


print("=== CENTRAL DE PEDIDOS ====")

print(
    "Limite simultaneo",
    MAXIMO_SIMULTANEO
)

print(
    "Estoque inicial",
    principal["estoque"]
)


#código para fazer as requisição
trabalhadores = [
    threading.Thread(
        target=processar_pedidos,
        args=(numero,)
    )

    for numero in [101, 102, 103]
]

for trabalhador in trabalhadores:
    trabalhador.start()

for trabalhador in trabalhadores:
    trabalhador.join()


print(
    "pedidos confirmados",
    sorted(Pedidos_confirmados)
)

print(
    "Estoque principal:",
    principal["estoque"]
)

print(
    "Replicas antes",
    replica["estoque"]
)

sincronizar()

print( 
    "Replica depois:",
    replica["estoque"]   
)

#simulacao da falha

principal["online"] = False

nome, estoque = consultar_estoque()


print(
    "consulta apos falha",
    nome,
    estoque
)











