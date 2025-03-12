---
Atualizado: 2025-03-12  10.33
Criado: 2025-03-12  10.29
---
```run-python
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

# Dados de exemplo para a, b, c
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

# Exibindo o gráfico
plt.show()
```

