---
Atualizado: 2025-03-12  11.26
Criado: 2025-03-12  10.05
---
```python


```

```run-python
a = 145
print(a)
```




```run-python
b = a
print(b)

c = input("digite algo")
print(c)
```

```run-python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

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
plt.savefig('plot3d.png')
print("Gráfico salvo como 'plot3d.png'")

```

```run-python
import os
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

# Imprime o diretório de trabalho atual
print("Diretório de trabalho atual:", os.getcwd())

# Dados de exemplo
a = [1, 2, 3, 4, 5]
b = [2, 3, 4, 5, 6]
c = [3, 4, 5, 6, 7]

# Criando o subplot 3D
subplot3d = plt.subplot(111, projection='3d')

# Coordenadas x, y, z
x_coords, y_coords, z_coords = zip(a, b, c)

# Plotando os pontos
subplot3d.scatter(x_coords, y_coords, z_coords)

# Definindo os limites do eixo z
subplot3d.set_zlim3d([0, 9])

# Salvando o gráfico como imagem
plt.savefig('/home/paulo/Obsidian Vault/Obsidian Vault/plot/plot3d.png')
print("Gráfico salvo como 'plot3d.png'")
```
