# O que entregar / discutir:
# 19.	Codigo completo (com a funcao calcular_gap implementada e o loop funcionando).
import numpy as np
import itertools

def mochila_otima(pesos, valores, capacidade):
    n = len(pesos)
    melhor = 0

    for comb in itertools.product([0, 1], repeat=n):
        peso = sum(pesos[i] for i in range(n) if comb[i] == 1)

        if peso <= capacidade:
            valor = sum(valores[i] for i in range(n) if comb[i] == 1)

            if valor > melhor:
                melhor = valor

    return melhor


def mochila_gulosa(pesos, valores, capacidade):
    n = len(pesos)
    densidade = [(valores[i] / pesos[i], i) for i in range(n)]
    densidade.sort(reverse=True)

    valor_total = 0
    peso_atual = 0

    for _, i in densidade:
        if peso_atual + pesos[i] <= capacidade:
            peso_atual += pesos[i]
            valor_total += valores[i]

    return valor_total


def calcular_gap(valor_heuristica, valor_otimo):
    if valor_otimo == 0:
        return 0

    return ((valor_otimo - valor_heuristica) / valor_otimo) * 100


np.random.seed(42)

n_itens = 12
capacidade = 30
n_instancias = 20

gaps = []

print("Rodando", n_instancias, "instancias...\n")

for k in range(n_instancias):
    pesos = np.random.randint(1, 15, size=n_itens)
    valores = np.random.randint(10, 50, size=n_itens)

    otimo = mochila_otima(pesos, valores, capacidade)
    heur = mochila_gulosa(pesos, valores, capacidade)

    gap = calcular_gap(heur, otimo)
    gaps.append(gap)

    print(
        f"Instancia {k + 1:2d} | "
        f"Otimo: {otimo:4d} | "
        f"Gulosa: {heur:4d} | "
        f"Gap: {gap:5.1f}%"
    )

print("\n===== RESUMO =====")
print(f"Gap medio     : {np.mean(gaps):.2f}%")
print(f"Gap minimo    : {np.min(gaps):.2f}%")
print(f"Gap maximo    : {np.max(gaps):.2f}%")
print(f"Desvio padrao : {np.std(gaps):.2f}%")

# 20.	Valor do gap medio obtido.
RESUMO =====
Gap medio     : 0.39%
Gap minimo    : 0.00%
Gap maximo    : 4.19%
Desvio padrao : 1.03%

# 21.	Resposta: “A heuristica gulosa e boa o suficiente para este problema? Em quais situacoes voce usaria ela e em quais preferiria gastar mais tempo para achar o otimo?”
A heurística gulosa é útil por ser rápida e eficiente, mas não garante a melhor solução. Para problemas pequenos ou que exigem precisão, é melhor usar um método exato.
