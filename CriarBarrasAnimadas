# Importa as bibliotecas necessárias
import csv
import bpy
import math
import os

# FUNÇÃO PEGAR OS MATERIAIS DOS OBJETOS E APLICAR A FUNÇÃO DE ALTERAR O VALOR    
def PegarMateriaisEAlterarValor(val, obj, a):
    for mat_slot in obj.material_slots:
        AlterarValor(mat_slot.material, val, a)
    
# FUNÇÃO PARA ALTERAR O VALOR
# mat = material, val = valor
def AlterarValor(mat, val, a):
    if mat is not None and mat.use_nodes and mat.node_tree is not None:
        for node in mat.node_tree.nodes:
            if node.label == "val" and node.type == "VALUE":
                print('Val')
                node.outputs["Value"].default_value = val  
            if node.label == "color" and node.type == "RGB":
                node.outputs["Color"].default_value = redColor
                node.outputs["Color"].keyframe_insert(data_path='default_value', frame = float(a[3]))
                node.outputs["Color"].default_value = greenColor 
                node.outputs["Color"].keyframe_insert(data_path='default_value', frame = float(a[4]))  
#            if node.label == "fade" and node.type == "VALUE":
#                print('Teste')
#                node.outputs["Value"].default_value = 0
#                node.outputs["Value"].keyframe_insert(data_path='default_value', frame = 100)
#                node.outputs["Value"].default_value = 1
#                node.outputs["Value"].keyframe_insert(data_path='default_value', frame = 120)

# Varíaveis
barSpacing = 1.5
barWidth = 1
barHeigth = 15
redColor = (0.913,0.220,0.014,1)
greenColor = (0,1,0,1)
val = 1

# Pega o diretório atual        
filepath = bpy.data.filepath
directory = os.path.dirname(filepath)
#print(directory)

# Abre o arquivo CSV
with open(directory + '/BlenderCronograma.csv', encoding = 'ANSI') as f:
    readout = list(csv.reader(f))
    

# Para cada linha do arquivo CSV
for a in readout:
    if len(a) != 0:
        
            
        # Duplica os materiais
        #if bpy.data.materials['mat' + a[0]] == :     
        #    mat = bpy.data.materials['FlatLaranja'].copy()
        #    mat.name = 'mat' + a[0]
        
        # CRIA A BARRA
        placement = readout.index(a)
        bpy.ops.mesh.primitive_plane_add(size = 1)
        new_bar = bpy.context.object
        new_bar.name = 'bar' + a[0]
        new_bar.active_material = bpy.data.materials['Efeito']
        new_bar.color = redColor
        new_bar.keyframe_insert("color", frame = float(a[3]))        
        new_bar.keyframe_insert("color", frame = float(a[4]))
        new_bar.color = greenColor
        new_bar.keyframe_insert("color", frame = float(a[4])+1)
        #PegarMateriaisEAlterarValor(val,new_bar,a)
        
        
        
        #Altera a cor
        
        # Anima os vértices
        for vert in new_bar.data.vertices:   
            vert.co[0] += placement * barSpacing
            if vert.co[1] < 0:
                vert.co[1] += 0.5
            if vert.co[1] > 0:
                vert.co[1] -= 0.5         
                vert.keyframe_insert('co', index = 1, frame = float(a[3]))
                vert.co[1] = barHeigth
                vert.keyframe_insert('co', index = 1, frame = float(a[4]))

        # CRIA O CONTORNO
        bpy.ops.mesh.primitive_plane_add(size = 1.2)
        new_contourn = bpy.context.object
        new_contourn.name = 'con' + a[0]
        new_contourn.active_material = bpy.data.materials['FlatBranco']    

        # Anima os vértices            
        for vertB in new_contourn.data.vertices:
            vertB.co[0] += placement * barSpacing
            print (vertB.co[1])
            if vertB.co[1] < 0:
                vertB.co[1] += 0.5
            if vertB.co[1] > 0:
                vertB.co[1] += barHeigth - 0.5            
              

        # ADICIONA O TEXTO
        
        bpy.ops.object.text_add()
        newText = bpy.context.object
        newText.name = 'text' + a[0]
        bpy.context.object.data.align_x = 'RIGHT'
        bpy.context.object.data.align_y = 'CENTER'
        bpy.ops.transform.rotate(value=-1.5708)
        bpy.ops.transform.translate(value=(placement * barSpacing,-3,0))
        bpy.context.object.data.body = a[0]
        bpy.context.object.active_material = bpy.data.materials['FlatBranco']
        
        # ADICIONA O CONTADOR
        bpy.ops.object.text_add()
        newText = bpy.context.object
        newText.name = 'text' + a[0]
        bpy.context.object.data.align_x = 'RIGHT'
        bpy.context.object.data.align_y = 'CENTER'
        bpy.ops.transform.rotate(value=-1.5708)
        bpy.ops.transform.translate(value=(placement * barSpacing,-0.3,0))
        bpy.context.object.active_material = bpy.data.materials['FlatBranco']
        
        newText.data.text_counter_props.ifAnimated = True
        newText.data.text_counter_props.sufix = '%'
        newText.data.text_counter_props.counter = 0
        newText.data.keyframe_insert(data_path="text_counter_props.counter", frame = float(a[3]))
        newText.data.text_counter_props.counter = 100
        newText.data.keyframe_insert(data_path="text_counter_props.counter", frame = float(a[4]))
                
        
