# Calculadora-Integrales
Este el código para una calculadora que resuelve operaciones como las integrales, aun lo estoy probando 

import sympy as sp
import cv2
import pytesseract
import tkinter as tk
from tkinter import ttk, messagebox

# Función para mostrar la solución de la integral
def show_integral():
    integral = sp.integrate(sp.sin(x), x)
    result_text.set(f"Integral de sin(x): {integral}")

# Función para mostrar las soluciones de la ecuación trigonométrica
def show_solutions():
    solutions = sp.solve(sp.Eq(sp.sin(x), 0), x)
    result_text.set(f"Soluciones de sin(x) = 0: {solutions}")

# Función para iniciar la cámara y hacer OCR
def start_camera():
    cap = cv2.VideoCapture(0)
    while True:
        ret, frame = cap.read()
        if not ret:
            break
        
        # Convertir la imagen a escala de grises
        gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
        
        # Usar pytesseract para hacer OCR en la imagen
        text = pytesseract.image_to_string(gray)
        print("Texto detectado:", text)
        
        # Mostrar la imagen de la cámara
        cv2.imshow('frame', frame)
        
        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    cap.release()
    cv2.destroyAllWindows()

# Crear la ventana principal
root = tk.Tk()
root.title("Aplicación de Procesamiento y Cálculo")
root.geometry("400x300")

# Definir la variable de SymPy
x = sp.symbols('x')

# Variable para mostrar resultados en la interfaz
result_text = tk.StringVar()

# Crear botones y etiquetas
ttk.Label(root, text="Seleccione una operación:").pack(pady=10)

ttk.Button(root, text="Iniciar Cámara y OCR", command=start_camera).pack(pady=5)
ttk.Label(root, textvariable=result_text).pack(pady=20)

# Iniciar la aplicación
root.mainloop()
