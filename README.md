# -*- coding: utf-8 -*-
"""
Created on Wed Oct  6 11:48:32 2021

@author: Fernanda Guzman
"""

import numpy as np

def matrizc(m): 
    if len(m) == len (m [0]): 
        print ("La matriz es cuadrada") 
    else: 
        print ("\ nLa matriz que ingresó no es cuadrada")
        
def read_inputs(text):
    a_temp, b_temp = text.strip().split('=')
    b_temp = eval(b_temp.strip(' '))
    a_temp = a_temp[:-1]
    a_temp = [eval(i) for i in a_temp.split()]
    
    return a_temp, b_temp

def read_file(path):
    b = []
    A = []
    with open(path, 'r') as file :
        flag = 0
        for line in file:
            if line.strip() != 'X':
                pass
            else:
                flag = 1
                continue 
            if line == 'Y':
                break
            
            if flag == 1:
                aux_1, aux_2 = read_inputs(line)
                A.append(aux_1)
                b.append(aux_2)
    return A, b

ruta = 'C:/Users/Fernanda Guzman/Documents/Fersy/UAQ/Tercer semestre/Métodos numéricos/Solución sistema de ecuaciones/Sistemas.txt'
A, b = read_file(ruta)

matrizc(A)

#Pivoteo por filas y columnas
A_np = np.array(A)
A_a1 = np.transpose(A)

b_np = np.array(b)
b_b1 = len(b)
b_b2 = []

for i in range(b_b1):
    b_b2.append([b[i]])

Ab  = np.concatenate((A, b_b2), axis = 1)
Ab_1 = np.copy(Ab)

tamano = np.shape(Ab)
n = tamano[0]
m = tamano[1]

for i in range(0,n-1,1):
    columna = abs(Ab[i:,i])
    vmax = np.argmax(columna)

    if (vmax !=0):
        temporal = np.copy(Ab[i,:])
        Ab[i,:]  = Ab[vmax+i,:]
        Ab[vmax+i,:] = temporal

for i in range(0,n-1,1):
    fila = abs(A_a1[i:,i])
    vmax = np.argmax(fila)

    if (vmax !=0):
        temporal = np.copy(A_a1[i,:])
        A_a1[i,:]  = A_a1[vmax+i,:]
        A_a1[vmax+i,:] = temporal
        A_a2 = np.transpose(A_a1)
        
Ab_2 = np.concatenate((A_a2, b_b2), axis = 1)

#Procedimiento
umbral = 0.0001 

x_np = np.zeros(len(A_a2)) 

aux_np = np.ones(len(A_a2))

for ite in range(100000):
    for i in range(len(A_a2)):
        aux_np[i] = 0.0
        x_np[i] = (b_np[i] - np.sum(x_np*aux_np*A_a2[i,:]))/A_a2[i][i]
        aux_np[i] = 1.0
    
    current_b = np.dot(A_a2,x_np)
    error = np.sum(np.abs(current_b-b_np))
    
    if error < umbral:
        break
        
print('Matriz aumentada:')
print(Ab_1)
print('Pivoteo parcial por filas')
print(Ab)
print('Pivoteo parcial por columnas')
print(Ab_2)
print('Solución')
print(x_np)
