# Limiarização de imagens em escala de cinza
### Autor: [José Gabriel Pereira Tavares](https://github.com/booleangabs)
#### Chefe de Equipe - Inteligência Artifical @ Asa Branca Aerospace

## Introdução
<div align="center">
	<img src="https://user-images.githubusercontent.com/62074916/145666647-7fe90979-206f-4eeb-8ec2-c077ff34c725.png" title="threshold" width="60%"/>
</div>


A limiarização aplicada a imagens (ou *thresholding*, em inglês) consiste em um transformação dos valores de intensidade dos pixels de uma imagem, a fim de que o resultado seja uma imagem binarizada (uma imagem onde todos os pixels assumem apenas dois valores distintos). Há diversas formas de realizar a limiarização e iremos discutir algumas delas. De forma geral, se ![x](https://latex.codecogs.com/svg.latex?\inline&space;x) é um valor de intensidade entre a intensidade máxima (![i_max](https://latex.codecogs.com/svg.latex?\inline&space;i_{max})) e a intensidade mínima (![i_min](https://latex.codecogs.com/svg.latex?\inline&space;i_{min})) do tipo de dado da imagem (por exemplo, no caso de representação por inteiro sem sinal de 8 bits, ![i_{min}=0](https://latex.codecogs.com/svg.latex?\inline&space;i_{min}=0) e ![i_max=255](https://latex.codecogs.com/svg.latex?\inline&space;i_{max}=255)), a transformação do valor de ![x](https://latex.codecogs.com/svg.latex?\inline&space;x) é dada por:
 
<div align="center">
	<img src="https://latex.codecogs.com/svg.latex?%5Clarge%20%7B%5Ccolor%7BBlack%7D%20t%28x%3B%5Ctheta%29%20%3D%20%5Cbegin%7Bcases%7D%20v_1%20%26%5Cquad%5Ctext%7Bse%20%7D%20x%20%3E%20%5Ctheta%20%5C%5C%20v_2%20%26%5Cquad%5Ctext%7Bse%20%7D%20x%20%5Cleq%20%5Ctheta%20%5C%5C%20%5Cend%7Bcases%7D%7D" title="threshold" />
</div>

Aqui, ![theta](https://latex.codecogs.com/svg.latex?\inline&space;\theta)  é um parâmetro chamado de limiar. Esse parâmetro controla como os valores de intensidade irão ser afetados pela transformação. Temos também que ![v1](https://latex.codecogs.com/svg.latex?\inline&space;v_1) e ![v2](https://latex.codecogs.com/svg.latex?\inline&space;v_2), são valores que indicam o perfil da saída da transformação. Eles se alteram com o tipo da limiarização. Aqui vamos abordar apenas um dos tipos de limiarização.

## Escolha do ![theta](https://latex.codecogs.com/svg.latex?\huge&space;\huge&space;_\theta)
Uma forma comum de escolher um valor para ![theta](https://latex.codecogs.com/svg.latex?\inline&space;\theta) é utilizar o histograma da imagem. Isso é partirculamente útil quando o histograma possui dois ou mais picos distintos. A partir dessa informação, seleciona um valor de ![theta](https://latex.codecogs.com/svg.latex?\inline&space;\theta) entre tais picos.

<div align="center">
 	<img src="https://user-images.githubusercontent.com/62074916/145666132-1e12dc38-cc31-4975-88c7-7edcdac2d722.png" width="40%" style="display: inline-block;">
	<img src="https://user-images.githubusercontent.com/62074916/145666355-cb23a19d-44ef-45d3-a92e-d8db69cb24cc.png" width="40%" style="display: inline-block;">
</div>
A partir da figura acima, o valor 175 parece um candidato razoável para o valor do limiar.

## Limiarização simples
A limiarização simples executa a binarização da seguinte forma:
<div align="center">
	<img src="https://latex.codecogs.com/svg.latex?%5Clarge%20%7B%5Ccolor%7BBlack%7D%20t%28x%3B%5Ctheta%29%20%3D%20%5Cbegin%7Bcases%7D%20i_%7Bmax%7D%20%26%5Cquad%5Ctext%7Bse%20%7D%20x%20%3E%20%5Ctheta%20%5C%5C%20i_%7Bmin%7D%20%26%5Cquad%5Ctext%7Bse%20%7D%20x%20%5Cleq%20%5Ctheta%20%5C%5C%20%5Cend%7Bcases%7D%7D" style="display: inline-block;">
</div>

## Implementação
Pseudocódigo:
```
Procedimento: Limiarização simples
Entrada: I, theta, i_min, i_max // Imagem em escala de cinza, limiar, valores mínimo e máximo
Saída: R                        // Imagem binarizada

INICIO

h = Altura da imagem em pixels
w = Largura da imagem em pixels
R = Matriz vazia com dimesões h x w

PARA i: 0, 1, ..., h - 1 FAÇA
	PARA j: 0, 1, ..., w - 1 FAÇA
		SE I[i][j] > theta FAÇA
			R[i][j] <- i_max
		SENÃO FAÇA
			R[i][j] <- i_min
		FIM_SE-SENÃO
	FIM_PARA
FIM_PARA

RETORNE R

FIM
```


Python:
```python
import numpy as np

def thresholding(image: np.ndarray, theta: float, i_min: float=0, i_max: float=255) -> np.ndarray:
	"""
	Bruteforce
	"""
	h, w = image.shape
	result = np.zeros((h, w))

	for i in range(h):
		for j in range(w):
			if image[i][j] > theta:
				result[i][j] = i_max
			else:
				result[i][j] = i_max
	return result
	
def thresholding(image: np.ndarray, theta: float, i_min: float=0, i_max: float=255) -> np.ndarray:
	"""
	More efficient
	"""
	result = np.zeros_like(image)
	result[image > theta] = i_max
	result[image <= theta] = i_min
	return result
```
Aplicando a limiarização como descrita acima e utilizando valor de theta da seção "Escolha do ![theta](https://latex.codecogs.com/svg.latex?\inline&space;\theta)", temos como resultado:
<div align="center">
	  <img src="https://user-images.githubusercontent.com/62074916/145666872-9bf9b577-3d9c-48fc-8b8c-6d2c64686474.png" style="display: inline-block;">
</div>

## Conclusão
Neste projeto, pudemos demonstrar o funcionamento e evidenciar a utilidade da limiarização no contexto do processamento digital de imagens. Os resultados obtidos foram condizentes com a teoria apresentada. Com a implementação do que foi apresentado na linguagem Python, foi possível praticar e fixar o conhecimento exposto. 

### Referências
GONZALEZ, Rafel C., WOODS, Richard E., **Digital Image Processing**, 4ª Edição, Pearson, 2018
