# Calculadora_operaciones_gas_liquido

![Alt text](imagen_2023-11-28_183542111.png)

NOMBRES: Heidy Tatiana Perez, David Andrei Joya Reinel

Para este proyecto nos enfocamos en la transferencia de masa por medio de equilibrio químico a través de la absorción de gases. Con esto en mente, utilizaremos la ley de Henry para el cálculo de los parámetros deseados.

* # Absorción de gases

Proceso muy común en ingeniería química en el cual se analiza un gas (soluto) y su disolución en un líquido (solvente) con el fin de obtener una disolución del gas deseado en el líquido.

  * # ¿Qué factores afectan la solubilidad de un gas en un líquido?

    Entre los factores que afectan la solubilidad de los gases en un líquido están:
      * Naturaleza de ambos componentes
      * Temperatura
      * Presión parcial
      * Concentración del gas disuelto en el líquido

* # Ley de Henry

Esta Ley puede aplicarse a disoluciones líquido-gas no ideales.

P = Hx  


* Variables utilizadas:

| Variable | Descripcion |
|---|---|
| H | Constnte ley de Henry |
| x | Concentración del gas en el líquido|
| p | Presión parcial del gas|

También se puede despejar la misma fórmula para determinar la masa de gas que se disolvió en el líquido.

x = P/H



Y acá está el programita para resolver el problema planteado (hasta le pusimos interfaz):

*disclaimer:* Es necesario tener instalado tkinter para que el programa corra con la GUI, si no lo tienen instalado también se puede correr sin la interfaz con el codiguito llamado "py.py"

```python

from customtkinter import *
from tkinter import PhotoImage, StringVar

# en esta funcion se calculan los gramos del gas
def calcular_gramos_gas(presion_parcial, constante_henry, cantidad_agua, gas_deseado, PM_agua, masa_molecular):
    temperatura = int(temperatura_deseada_var.get())
    constante_henry = constantes_henry_temperaturas[temperatura]
    #calcular la concentracion del gas
    x = presion_parcial / constante_henry[gas_deseado]
    #se hace una relacion molar, debido a que es una mezcla no ideal
    masa_gas = (x / (1 - x)) * (masa_molecular[gas_deseado] / PM_agua) * cantidad_agua
    # se retorna la funcion dada
    return masa_gas

#funcion para calcular la opcion de calcular la presion parcial
def calcular_presion_parcial(gramos_gas, gramos_agua, gas_deseado, masa_molecular, PM_agua):
    temperatura = int(temperatura_deseada_var.get())
    constante_henry = constantes_henry_temperaturas[temperatura]
    #se calculan los moles de cada componente y para hallar la concentracion son los moles del gas sobre los moles totales, la suma entre el gas y el liquido
    x = (gramos_gas / masa_molecular[gas_deseado]) / ((gramos_gas / masa_molecular[gas_deseado]) + (gramos_agua / PM_agua))
    # segun la ley de Henry para hallar la presion se usa la concentracion multiplicada por la constante dada a 20º
    P = x * constante_henry[gas_deseado]
    #se retorna la precion parcial calculada
    return P

def calcular():
    # acceder y utilizar el valor registrado por el usuario
    temperatura = int(temperatura_deseada_var.get())
    constante_henry = constantes_henry_temperaturas[temperatura]
    gas_deseado = gas_deseado_var.get().upper()  
    opcion = opcion_var.get()

# condicion si se cumple la entrada del usuario
    if opcion == 'la cantidad de gas disuelto en agua':
        presion_parcial = float(presion_parcial_entry.get()) 
        cantidad_agua = float(cantidad_agua_entry.get())
        # llama a la funcion creada
        calculo_si = calcular_gramos_gas(presion_parcial, constante_henry, cantidad_agua, gas_deseado, PM_agua, masa_molecular)
        # se imprime el resultado con dos decimales
        resultado_si_var.set("La cantidad de gas {} en gramos que se disolverá en el agua es: {:.2f} g".format(gas_deseado, calculo_si))
        resultado_no_var.set("")  # Limpiar el resultado "no"

# si no se cumple evalua la otra condicion  
    else:
        gramos_gas = float(gramos_gas_entry.get())
        gramos_agua = float(gramos_agua_entry.get())
        calculo_no = calcular_presion_parcial(gramos_gas, gramos_agua, gas_deseado, masa_molecular, PM_agua)
        resultado_no_var.set("La presión parcial del gas {} es: {:.2f} atm".format(gas_deseado, calculo_no),)
        resultado_si_var.set("")  # Limpiar el resultado "si"

#colorsitos que usamos para la interfaz
c_negro = '#010101'
c_morado = '#7f5af0'
c_verde = '#2cb67d'
c_gris = '#f0f0f0'


#diccionario de las constantes de henry a diferentes temperaturas
constantes_henry_temperaturas = {
    0: {'H2S': 268, 'CO2': 728, 'CO': 342, 'C2H6': 12600, 'CH4': 22400, 'NO': 16900, 'O2': 25500, 'N2':52900, 'AIRE': 43200, 'H2': 57900},
    10: {'H2S': 367, 'CO2': 1040, 'CO': 44200, 'C2H6': 18900, 'CH4': 29700, 'NO': 21800, 'O2': 32700, 'N2': 66800, 'AIRE': 54900, 'H2': 63600},  
    20: {'H2S': 483, 'CO2': 1420, 'CO': 53600, 'C2H6': 26300, 'CH4': 37600, 'NO': 26400, 'O2': 40100, 'N2': 80400, 'AIRE': 66400, 'H2': 68300},
    30: {'H2S': 609, 'CO2': 1860, 'CO': 62000, 'C2H6': 34200, 'CH4': 44900, 'NO': 31000, 'O2': 47500, 'N2': 92400, 'AIRE': 77100, 'H2': 72900},
    40: {'H2S': 745, 'CO2': 2330, 'CO': 69600, 'C2H6': 42300, 'CH4': 52000, 'NO': 35200, 'O2': 52500, 'N2': 104000,'AIRE': 87000, 'H2': 75100},
}
    

# Peso molecular del agua
PM_agua = 18  

# Diccionario de los pesos moleculares
masa_molecular = {  # peso molecular en g/mol
        'H2S': 34.08,
        'CO2': 44.01,
        'CO': 28.01,
        'C2H6': 30.08,
        'CH4': 16.05,
        'NO': 30.01,
        'O2': 32,
        'N2': 28.02,
        'AIRE': 28.96,
        'H2': 2.02
    }

#creacion de la interfaz con CTk, y configuramos algunos parametros
app = CTk()
app.geometry("990x600")
app.minsize(480, 500)
app.title("Calculadora absorcion de Gases en Agua")

# Variables para almacenar la selección del usuario
gas_deseado_var = StringVar()
opcion_var = StringVar()
resultado_si_var = StringVar()
resultado_no_var = StringVar()
temperatura_deseada_var = StringVar()

logo = PhotoImage(file='imagenes/logo_proyecto.png')
# elegimos el modo en el que queriamos la interfaz grafica:)
set_appearance_mode("dark")

logo= PhotoImage(file = 'imagenes/logo_proyecto.png')

# con label pusimos el texto y configuramos algunos parametros del texto
CTkLabel(master=app, 
         text="Gas deseado:", 
         font=("sans rerif", 12), 
         text_color= c_verde).place(relx= 0.5, rely = 0.05, anchor="center")
#con comcoBox cremos la lista y llamamos un diccionario con los elementos de la lista
combobox= CTkComboBox(master=app, values=list(masa_molecular.keys()), variable=gas_deseado_var)
#configuramos la posicion de la lista anterior
combobox.place(relx= 0.5, rely = 0.1, anchor="center")

# con label pusimos el texto y configuramos algunos parametros del texto
CTkLabel(master=app, 
         text="Temperatura deseada (centigrados):", 
         font=("sans rerif", 12), 
         text_color= c_verde).place(relx= 0.5, rely = 0.15, anchor="center")
#con comcoBox cremos la lista, y introducimos los elementos de la lista
combobox= CTkComboBox(master=app, values=["0", "10", "20", "30", "40"], variable=temperatura_deseada_var)
#configuramos la posicion de la lista anterior
combobox.place(relx= 0.5, rely = 0.2, anchor="center")

#Con label pusimos el texto y configuramos algunos parametros del texto
CTkLabel(master=app, 
         text="¿Que desea calcular?", 
         font=("sans rerif", 12), 
         text_color= c_verde).place(relx= 0.5, rely = 0.25, anchor="center")
#con comcoBox cremos la lista, y introducimos los elementos de la lista
combobox2 = CTkComboBox(master=app, values=["la cantidad de gas disuelto en agua", "la presion parcial del gas"], variable=opcion_var)
#configuramos la posicion de la lista anterior
combobox2.place(relx= 0.5, rely = 0.3, anchor="center")

#con Entry el usuario escribe la cantidad deseada
presion_parcial_entry = CTkEntry(master=app, 
                                 placeholder_text='presion parcial del gas', 
                                 border_color= c_verde, 
                                 fg_color= c_negro, 
                                 width= 220, 
                                 height= 40)
cantidad_agua_entry = CTkEntry(master=app, 
                               placeholder_text='gramos agua', 
                               border_color= c_verde, 
                               fg_color= c_negro, 
                               width= 220, 
                               height= 40)
gramos_gas_entry = CTkEntry(master=app, 
                            placeholder_text=' gramos gas', 
                            border_color= c_verde, 
                            fg_color= c_negro, 
                            width= 220, 
                            height= 40)
gramos_agua_entry = CTkEntry(master=app, 
                             placeholder_text='gramos agua', 
                             border_color= c_verde, 
                             fg_color= c_negro, 
                             width= 220, 
                             height= 40)

#en texto muestra lo que debe escribir el usuario en los bloques correspondientes
CTkLabel(master=app, 
         text="Presión parcial del gas (atm):").place(relx= 0.3, rely = 0.4, anchor="center")
#se configura la posicion y los parametros del texto
presion_parcial_entry.place(relx= 0.5, rely = 0.4, anchor="center")

#en texto muestra lo que debe escribir el usuario en los bloques correspondientes
CTkLabel(master=app, 
         text="Cantidad de agua (gramos):").place(relx= 0.3, rely = 0.5, anchor="center")
#se configura la posicion y los parametros del texto
cantidad_agua_entry.place(relx= 0.5, rely = 0.5, anchor="center")

#en texto muestra lo que debe escribir el usuario en los bloques correspondientes
CTkLabel(master=app, 
         text="Gramos de gas disueltos (gramos):").place(relx= 0.28, rely = 0.6, anchor="center")
#se configura la posicion y los parametros del texto
gramos_gas_entry.place(relx= 0.5, rely = 0.6, anchor="center")

#en texto muestra lo que debe escribir el usuario en los bloques correspondientes
CTkLabel(master=app, 
         text="Cantidad de agua (gramos):").place(relx= 0.3, rely = 0.7, anchor="center")
#se configura la posicion y los parametros del texto
gramos_agua_entry.place(relx= 0.5, rely = 0.7, anchor="center")

#creamos un botoncito y con la funcion de calcular al hacer click en el boton se realizan los calculos correspondientes depediendo de los parametros del usuario
calcular_button = CTkButton(master=app, 
                            text= "calcular", 
                            corner_radius=32, 
                            fg_color= c_verde, 
                            hover_color=c_morado,
                            border_color= c_negro, 
                            border_width=2, 
                            command=calcular)
#posicion del boton
calcular_button.place(relx= 0.5, rely = 0.8, anchor="center")

#se añade el texto de la respuesta si se cumple una condicion
CTkLabel(master=app, 
         textvariable=resultado_si_var).place(relx= 0.5, rely = 0.9, anchor="center")

#se añade el texto de la respuesta si se cumple una condicion
CTkLabel(master=app, 
         textvariable=resultado_no_var).place(relx= 0.5, rely = 0.9, anchor="center")

#para iniciar la interfaz
app.mainloop()

```

Al correr el código nos aparece una interfaz la cual nos permite elegir un gas, la temperatura a la que se está llevando a cabo el proceso, la masa del líquido (en este caso, agua) y la presión parcial (si queremos hayar la masa de gas se disolvió en el líquido) o la masa de gas (si queremos hayar la presión parcial).
