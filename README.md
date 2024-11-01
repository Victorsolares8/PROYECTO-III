# PROYECTO-III
Lenguaje de programación Python escriba una aplicación (con interfaz gráfica) que pueda leer un archivo de texto (.txt, .cpp, .cs, .py, etc.)

Codigo fuente:

import re
import tkinter as tk
from tkinter import ttk
from tkinter import messagebox
from tkinter.scrolledtext import ScrolledText
from tkinter.filedialog import askopenfilename, asksaveasfile
from tkinter import colorchooser
from turtle import bgcolor
main_window=tk.Tk()

main_window.config(width=1100, height=700)




archivo_avierto=None
#Para desabilitar el boton guardar por si no hay algun archivo abierto por que este es un boton de guardar cambios si usted quiere 
#guardar un nuevo archivo precione guardar como y le puede poner la extencion que le aparezca y necesite
def desabilitar_boton():
    global archivo_avierto
    if archivo_avierto is None:
        boton_guardar.config(state=tk.DISABLED)
    else:
        boton_guardar.config(state=tk.NORMAL)
def deshacer_archivo():
    texto1.event_generate("<<Undo>>")
def rehacer_texto():
    texto1.event_generate("<<Redo>>")
texto1=ScrolledText(main_window, undo=True)
texto1.place(x=50, y=200,width=600, height=700)

estilo=ttk.Style()
estilo.configure('Tlavel', padding=20)

def mensaje_integrantes():
    messagebox.showinfo(message="Desarrollado por:\n"
                        "DIEGO SEBASTIAN VELASQUEZ TURCIOS\n"
                        "BRAYAN ONELIO TEODORO HERNANDEZ\n"
                        "VICTOR MANUEL CHAJON SOLARES Carne 7690-24-25034" , title="Grupo II")

palabra_buscadora=tk.Entry(main_window)
palabra_buscadora.place(x=150, y=75, width=800, height=35)
def abrir_texto():
    global archivo_avierto
    filetypes=(
        ('all file', '.'),
        ('text file','*.txt'),
        ('python file', '*.py'),
        ('C++', '*.cpp'),
        ('C#', '*.cs')
    )
    archivo=askopenfilename(filetypes=filetypes)
    if archivo:
        texto1.delete(1.0, tk.END)
        with open(archivo,"r") as file:
            texto1.insert(1.0, file.read())
    archivo_avierto=(open(archivo,"w"))
    desabilitar_boton()

def guardar():
    global archivo_avierto
    if archivo_avierto:
        archivo_avierto.write(str(texto1.get(1.0, "end")))
        archivo_avierto.close()
        archivo_avierto=None
        desabilitar_boton()

def guardar_como_texto():
    extensiones=(
        ('', '.'),
        ('tex','*.txt'),
        ('python', '*.py'),
        ('C++', '*.cpp'),
        ('C#', '*.cs')
    )
    archivo = asksaveasfile(filetypes = extensiones, defaultextension = extensiones)
    if archivo:
            archivo.write(str(texto1.get(1.0, "end")))
            archivo.close()

def buscar_texto():
    texto_de_busqueda = texto1.get("1.0", tk.END)
    lineas = texto_de_busqueda.splitlines()
    palabra_buscadora_text = palabra_buscadora.get()
    buscador = re.escape(palabra_buscadora_text)
    patas = re.compile(buscador)

    texto1.tag_remove("highlight", "1.0", tk.END)
    #codigo robado me canse de intentar y no me salia 
    #procedi a buscar codigos y me fije que estaba haciendo mal 
    #la busqueda en el for y pues procedi a usar la tecnica milenaria 
    for linea_num, linea in enumerate(lineas):
        start_idx = f"{linea_num + 1}.0"
        end_idx = f"{linea_num + 1}.end"

        for match in patas.finditer(linea):
            start = f"{linea_num + 1}.{match.start()}"
            end = f"{linea_num + 1}.{match.end()}"
            texto1.tag_add("highlight", start, end)
    
    texto1.tag_config("highlight", background="yellow", foreground="black")

def copiar_archivo():
    texto1.event_generate("<<Copy>>")
def pegar_archivo():
    texto1.event_generate("<<Paste>>")
def cortar_archivo():
    texto1.event_generate("<<Cut>>")


menu_formuario=tk.Menu()
opciones_menu=tk.Menu(menu_formuario, tearoff=False)
opciones_menu.add_command(
    label="Abrir",
    accelerator="Ctrl+A",
    command=abrir_texto
)

opciones_menu.add_command(
    label="Guardar",
    accelerator="Ctrl+S",
    command=guardar   
)
opciones_menu.add_command(
    label="Guardar como",
    accelerator="Ctrl+G",
    command=guardar_como_texto
)
opciones_menu.add_command(
    label="Buscar",
    accelerator="Ctrl+F"
)
menu_formuario.add_cascade(menu=opciones_menu, label="Archivo")
editar_menu=tk.Menu(menu_formuario, tearoff=False)
editar_menu.add_command(
    label="Deshacer",
    accelerator="Ctrl+Z",
    command=deshacer_archivo
)
editar_menu.add_command(
    label="Rehacer",
    accelerator="Ctrl+Y",
    command=rehacer_texto

)
menu_formuario.add_cascade(menu=editar_menu, label="Editar")
ayuda_menu=tk.Menu(menu_formuario, tearoff=False)
ayuda_menu.add_command(
    label="Informacion"
)
ayuda_menu.add_command(
    label="Manuel de usuario"
)
ayuda_menu.add_command(
    label="Integrantes",
    command=mensaje_integrantes
)
menu_formuario.add_cascade(menu=ayuda_menu, label="Ayuda")



boton_guardar=ttk.Button(text="Guardar", command=guardar)
boton_guardar.place(x=775, y=580, width=150,height=75)

boton_guardar_como=ttk.Button(text="Guardar como", command=guardar_como_texto)
boton_guardar_como.place(x=850, y=500, width=150,height=75)
boton_abrir=ttk.Button(main_window,text="Abrir", command=abrir_texto)
boton_abrir.place(x=550, y=120, width=150,height=40)

boton_buscar=ttk.Button(text="Buscar en archivo", command=buscar_texto).place(x=350, y=120, width=150,height=40)


boton_deshacer=ttk.Button(text="Deshacer", command=deshacer_archivo)
boton_deshacer.place(x=775, y=300,width=75,height=50)
boton_rehacer=ttk.Button(text="Rehacer", command=rehacer_texto)
boton_rehacer.place(x=855, y=300,width=75,height=50)
boton_copiar=ttk.Button(main_window, text="Copiar", command=copiar_archivo)
boton_copiar.place(x=735, y=350, width=75, height=50)
boton_pegar=ttk.Button(main_window, text="Pegar", command=pegar_archivo)
boton_pegar.place(x=815, y=350, width=75, height=50)
boton_cortar=ttk.Button(main_window, text="Cortar", command=cortar_archivo)
boton_cortar.place(x=895, y=350, width=75, height=50)


def salir_guardando_los_cambios_para_no_perder_nada():
    guardar()
    main_window.destroy()
boton_salida=ttk.Button(text="Salir", command=salir_guardando_los_cambios_para_no_perder_nada)
boton_salida.place(x=700, y=500, width=150,height=75)


main_window.config(menu=menu_formuario)
main_window.mainloop()

