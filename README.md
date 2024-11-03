# tarea_grafos
import numpy as gr # Usamos gr como alias para numpy
import sys

class Grafo:

    def __init__(self):
        #constriumos el objeto grafo, inicializamos en una matriz de 8 x 8 con ceros.
        #definimos los nodos desde la A hasta la H
        fila = 8
        columna = 8
        self.Matriz = gr.zeros((fila,columna), dtype=int)
        self.nodos = ['A','B','C','D','E','F','G','H']

    def agregar_peso (self, nodo1, nodo2, peso):
        #Convierto las letras a indice
        index1 = self.nodos.index(nodo1)
        index2 = self.nodos.index(nodo2)
        #Agrego el peso a la matriz
        self.Matriz[index1][index2]= peso

    def mostrar_matriz___(self):
        print("Matriz de adyacencia:")
        print(" " + " ".join(self.nodos)) # con .join uno los elementos en una sola cadena
        for i, fila in enumerate(self.Matriz): #enumerate se utiliza para obtener tanto el índice (i) como el contenido de cada fila (fila)
            print(self.nodos[i], " ".join(map(str, fila)))
    
    def agregar_nodo(self, nuevo_nodo):
        if nuevo_nodo not in self.nodos: # verifico que el nodo no este
            self.nodos.append(nuevo_nodo)
            # Ampliar la matriz para incluir el nuevo nodo
            nueva_matriz = gr.zeros((len(self.nodos), len(self.nodos)), dtype=int) # genero una nueva matriz con los tamaños que tengo
            nueva_matriz[:self.Matriz.shape[0], :self.Matriz.shape[1]] = self.Matriz # copio los valores de filas y columna de la matriz
            self.Matriz = nueva_matriz
            print(f"Nodo {nuevo_nodo} agregado.")
        else:
            print("El nodo ya existe.")

    def agregar_arista(self, nodo1, nodo2, peso):
        if nodo1 in self.nodos and nodo2 in self.nodos: # busco los nodos y condiciono para que existan
            self.agregar_peso(nodo1, nodo2, peso)
            print(f"Arista agregada de {nodo1} a {nodo2} con peso {peso}.")
        else:
            print("Uno o ambos nodos no existen.")

    def ingresar_valores(self):
        print("Por favor, ingresa 8 valores para la matriz:")
        #solicito los valores de los tramos
        while True:
                #capturo errores de ingreso
                try:
                    peso_AB = int(input ("Ingrese el recorrido A-B: "))
                    peso_AC = int(input ("Ingrese el recorrido A-C: "))
                    peso_BD = int(input ("Ingrese el recorrido B-D: "))
                    peso_BG = int(input ("Ingrese el recorrido B-G: "))
                    peso_CD = int(input ("Ingrese el recorrido C-D: "))
                    peso_CF = int(input ("Ingrese el recorrido C-F: "))
                    peso_DE = int(input ("Ingrese el recorrido D-E: "))
                    peso_DF = int(input ("Ingrese el recorrido D-F: "))
                    peso_EG = int(input ("Ingrese el recorrido E-G: "))
                    peso_EH = int(input ("Ingrese el recorrido E-H: "))
                    peso_FH = int(input ("Ingrese el recorrido F-H: "))
                    # condiciono para que los valores ingresados sean positivos
                    if peso_AB <= 0 or peso_AC <= 0 or peso_BD <= 0 or peso_BG <= 0 or peso_CD <= 0 or peso_CF <= 0 or peso_DE <= 0 or peso_DF <= 0 or peso_EG <= 0 or peso_FH <= 0 or peso_EH <= 0:
                        print("Los núemros de los recorridos deben ser positivos , ((((()))))")
                        print("--------- Intente otra vez ------" )
                        break
                    else:
                        self.agregar_peso('A', 'B', peso_AB)
                        self.agregar_peso('A', 'C', peso_AC)
                        self.agregar_peso('B', 'D', peso_BD)
                        self.agregar_peso('B', 'G', peso_BG)
                        self.agregar_peso('C', 'D', peso_CD)
                        self.agregar_peso('C', 'F', peso_CF)
                        self.agregar_peso('D', 'F', peso_DF)
                        self.agregar_peso('D', 'E', peso_DE)
                        self.agregar_peso('E', 'G', peso_EG)
                        self.agregar_peso('E', 'H', peso_EH)
                        self.agregar_peso('F', 'H', peso_FH)
                        break
                except ValueError:
                    print("Por favor, ingresa un número entero válido.")

    def dijkstra (self, inicio, fin):
            num_nodos = len(self.nodos)
            distancias = [float('inf')] * num_nodos
            distancias[self.nodos.index(inicio)] = 0
            visitados = [False] * num_nodos
            predecesores = [None] * num_nodos

            for _ in range(num_nodos): # el _ significa que no es necesario un valor sino que se usa para recorrer el bucle
                nodo_actual = min((i for i in range(num_nodos) if not visitados[i]), #Este es un generador que itera sobre todos los índices de los nodos (de 0 a num_nodos - 1).
                                   key=lambda i: distancias[i], default=None)       #if not visitados[i] filtra solo los nodos que no han sido visitados. 
                                                                                 #Así, solo se consideran aquellos nodos que aún no han sido procesados por el algoritmo.
                if nodo_actual is None or distancias[nodo_actual] == float('inf'):
                    break
                visitados[nodo_actual] = True
                for vecino in range(num_nodos):
                    peso_arista = self.Matriz[nodo_actual][vecino]
                    if peso_arista > 0:
                        nueva_distancia = distancias[nodo_actual] + peso_arista
                        if nueva_distancia < distancias[vecino]:
                            distancias[vecino] = nueva_distancia
                            predecesores[vecino] = nodo_actual

            camino = []
            nodo = self.nodos.index(fin)
            while nodo is not None:
                camino.append(self.nodos[nodo])
                nodo = predecesores[nodo]
            camino.reverse()
            return camino, distancias[self.nodos.index(fin)]
       


grafo = Grafo() # creo una instancia del objeto
print(grafo.mostrar_matriz___())
grafo.ingresar_valores()
print(grafo.mostrar_matriz___())

# Agregar un nuevo nodo
#grafo.agregar_nodo('I')

# Agregar una nueva arista
#grafo.agregar_arista('A', 'I', 5)

# Mostrar la matriz actualizada
#print(grafo.mostrar_matriz___())

 # Solicitar al usuario el nodo de inicio
inicio = input("Ingresa el nodo de inicio (A-H): ").strip().upper()

if inicio in grafo.nodos: 
        camino, distancia = grafo.dijkstra(inicio, 'H')
        print(f"El camino más corto de {inicio} a H es: {' -> '.join(camino)} con una distancia de {distancia}.")
else:
    print("Nodo de inicio inválido. Debe ser entre A y H.")
