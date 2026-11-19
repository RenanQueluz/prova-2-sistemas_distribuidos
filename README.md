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






