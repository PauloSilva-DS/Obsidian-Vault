---
Atualizado: 2025-03-13  10.57
Criado: 2025-03-12  10.05
---



```run-python
import matplotlib.pyplot as plt
import numpy as np


# Definindo os arrays para as coordenadas
a = np.array([1, 2, 8])  # Coordenadas x
b = np.array([5, 6, 3])  # Coordenadas y
c = np.array([2, 3, 4])  # Coordenadas z (adicionando um novo array para z)

# Criando o subplot 3D
subplot3d = plt.subplot(111, projection='3d')

# Usando scatter para plotar os pontos
subplot3d.scatter(a, b, c)

# Definindo os limites do eixo z
subplot3d.set_zlim3d([0, 9])

# Mostrando o gráfico
plt.savefig('/home/paulo/Obsidian Vault/Obsidian Vault/plot/plot3d.png')
print("Gráfico salvo como 'plot3d.png'")



```






