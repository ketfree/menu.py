# menu.py
import os
import subprocess
import platform

def menu():
    os.system('clear')
    print("\033[1m" + "Sistema de administración de usuarios SSH".center(50, " ") + "\033[0m")
    print("\033[1m" + "Información del sistema".center(50, " ") + "\033[0m")
    print("Sistema operativo: " + platform.system())
    print("Versión del sistema operativo: " + platform.release())
    print("\nUsuarios: ")
    print("\033[1m" + "Activos:".ljust(20, " ") + "\033[0m" + subprocess.getoutput("awk -F: '$3>=1000 {print $1}' /etc/passwd"))
    print("\033[1m" + "Expirados:".ljust(20, " ") + "\033[0m" + subprocess.getoutput("awk -F: '$3<1000 {print $1}' /etc/passwd"))
    print("\033[1m" + "Bloqueados:".ljust(20, " ") + "\033[0m" + subprocess.getoutput("cat /etc/shadow | grep '!' | awk -F: '{print $1}'"))
    print("\n\033[1m" + "Menú de opciones".center(50, " ") + "\033[0m")
    print("1. Administrar cuentas")
    print("2. Salir")
    opcion = input("\nSelecciona una opción: ")
    return opcion

def crear_usuario():
    os.system('clear')
    print("\033[1m" + "Menú de administración de usuarios SSH".center(50, " ") + "\033[0m")
    print("\nCrear nuevo usuario")
    nombre_usuario = input("Nombre de usuario: ")
    password = input("Contraseña: ")
    dias_validez = input("Validez en días: ")
    subprocess.call(["useradd", "-e", f'+{dias_validez}', "-m", "-s", "/bin/bash", nombre_usuario])
    subprocess.call(["echo", f'{nombre_usuario}:{password}', "|", "chpasswd"])
    print("\nInformación del usuario creado:")
    print("Nombre de usuario: " + nombre_usuario)
    print("Contraseña: " + password)
    print("Fecha de expiración: " + subprocess.getoutput(f'chage -l {nombre_usuario} | grep "Account expires" | awk {{\'print $4,$5,$6,$7,$8\'
