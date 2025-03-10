---
tags:
  - python
Completo: false
Atualizado: 2025-02-28  16.16
Criado: 2025-02-28  13.55
---
```ad-summary
title: nota
Todos os pacotes disponíveis estão listados [aqui](https://github.com/mokeyish/pyodide-dist/find/master) (procure `whl`)

```


## Usando numpy`

```python file:numpy
import micropip
await micropip.install('numpy')  
import numpy as np
a = np.random.rand(3,2)
b = np.random.rand(2,5)

print(a@b)
```

## Usando matplotlib
```python file:matplotlib
import micropip
await micropip.install('matplotlib')

import matplotlib.pyplot as plt

fig, ax = plt.subplots()             # Create a figure containing a single Axes.
ax.plot([1, 2, 3, 4], [1, 4, 2, 3])  # Plot some data on the Axes.
plt.show()                           # Show the figure.
```



```python
await micropip.install('ipywidgets')
import ipywidgets as widgets


```
```python
widgets.IntSlider()

```
 IPython.display import display
w = widgets.IntSlider()
display(w)

```python
from IPython.display import display
w = widgets.IntSlider()
display(w)
```
