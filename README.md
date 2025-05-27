# Introducción a la Programación en Shell

## Descripción del Curso
**Facultad de Ciencias de la Salud - Programa de Biología**  
**Curso:** Principios de programación - Bioinformática  
**Semestre:** 2024-2  

**Docentes:**
- Frank Guzman Escudero
- Manuel Ramírez Sáenz

## Logro de Aprendizaje
Al finalizar esta sesión, el estudiante conoce la utilidad y beneficios de la programación en shell a través de scripts.

## Contenido del Repositorio

### 📁 Estructura de Carpetas
```
shell-programming-intro/
├── README.md
├── scripts/
│   ├── ejemplos/
│   │   └── script_ejemplo.sh
│   └── practicas/
│       ├── actividad1.sh
│       └── actividad2.sh
├── editores/
│   ├── vim-guia.md
│   ├── nano-guia.md
│   └── vscode-setup.md
├── datos/
│   ├── data.txt
│   ├── data1.txt
│   └── data1ordenada.txt
└── recursos/
    ├── tipos-shell.md
    └── comandos-basicos.md
```

## 🛠️ ¿Qué es Shell?

El **shell** es un intérprete de comandos que provee una interfaz entre el usuario y el kernel del sistema operativo. Interpreta los comandos que el usuario escribe para que puedan ser ejecutados.

### Arquitectura de Linux
- **Hardware**
- **El kernel (Núcleo)**
- **Espacio del Usuario**
  - **Shell** (interfaz de usuario)

## 📝 ¿Qué es un Shell Script?

Un **shell script** es un archivo que contiene una secuencia de comandos escritos en un lenguaje de shell específico (como Bash). Estos scripts se utilizan para:
- Automatizar tareas
- Realizar una serie de acciones en el sistema operativo
- Ejecutar procesos repetitivos de manera eficiente

## 🚀 Ventajas del Uso de Scripts

### ⚡ AUTOMATIZACIÓN
- Permite automatizar tareas repetitivas y ahorrar tiempo
- **Filosofía:** "Construye una vez y ejecuta muchas veces"

### 🌐 PORTABILIDAD
- Los scripts pueden ejecutarse en varias plataformas y sistemas operativos
- Compatible a través de máquinas virtuales

### 🔧 FLEXIBILIDAD
- Altamente personalizables
- Pueden combinarse con otros lenguajes de programación

### 📖 ACCESIBILIDAD
- Fáciles de escribir
- No requieren herramientas especiales
- Editables con cualquier editor de texto
- La mayoría de sistemas operativos incluyen intérprete de shell

### 🔗 INTEGRACIÓN
- Compatible con bases de datos
- Integración con servidores web
- Conexión con servicios en la nube

### 🐛 DEPURACIÓN
- Herramientas incluidas para identificar problemas rápidamente
- Facilita la corrección de errores

## 📝 Editores de Texto

### VIM - Vi IMproved
**Características:**
- Editor avanzado multiplataforma
- Gratuito y código abierto
- Creado por Bram Moolenaar en 1991
- Basado en el editor Vi original (Bill Joy, 1976)

**Instalación:**
```bash
sudo apt install vim
```

**Modos de Vim:**
- **Modo Normal** (predeterminado)
- **Modo Insertar** (tecla `i`)
- **Modo Visual** (tecla `v`)
- **Modo Línea de Comandos** (tecla `:`)

**Comandos básicos:**
- `vim archivo.txt` - Abrir archivo
- `i` - Entrar en modo insertar
- `ESC` - Volver al modo normal
- `:w` - Guardar
- `:q` - Salir
- `:wq` - Guardar y salir
- `:q!` - Salir sin guardar

### NANO
**Características:**
- Editor básico y fácil de usar
- Ideal para usuarios principiantes
- Funciones de edición esenciales

**Instalación:**
```bash
sudo apt-get install nano
```

**Comandos básicos:**
- `Ctrl+X` - Salir
- `Ctrl+O` - Guardar
- `Ctrl+K` - Cortar línea
- `Ctrl+U` - Pegar
- `Ctrl+J` - Justificar párrafo
- `Alt+U` - Deshacer
- `Ctrl+A` - Ir al inicio de línea
- `Ctrl+E` - Ir al final de línea

### Visual Studio Code
- Editor moderno con interfaz gráfica
- Extensiones para programación
- Soporte completo para shell scripting

## 🐚 Tipos de Shell

### Shell Bourne (sh)
- **Creador:** Steven Bourne
- Primera shell tradicional de sistemas UNIX
- Amplia compatibilidad con scripts
- Símbolo del sistema: `$`

### Bash (Bourne Again Shell)
- **Más moderno y utilizado**
- Desarrollado para el Proyecto GNU
- Predeterminado en la mayoría de distribuciones Linux
- Características: historial ilimitado, control de trabajos, funciones, alias
- Símbolo del sistema: `$`

### Korn Shell (ksh)
- **Creador:** David Korn
- Historial de órdenes y edición en línea
- Características de programación ampliadas
- Símbolo del sistema: `$`

### Z Shell (zsh)
- Sistema de completado avanzado
- Temas y plugins
- Poderosa expansión de comandos
- Símbolo del sistema: `$`

### C Shell (csh)
- **Creador:** Bill Joy (Berkeley University)
- Más utilizado en sistemas BSD
- Características para programadores en C
- Símbolo del sistema: `%`

### Tenex C Shell (tcsh)
- Superconjunto de csh
- Más rápido que csh
- Símbolo del sistema: `%`

## 🔧 Comandos del Sistema

### Identificar tu shell actual
```bash
echo $SHELL
```

### Verificar versión de Bash
```bash
echo $BASH_VERSION
```

### Cambiar shell (donde esté permitido)
```bash
chsh -s /bin/bash
```

### Instalar documentación de Bash
```bash
sudo apt-get install bash-doc
man bash
```

## #️⃣ Shebang

Los scripts deben empezar con `#!` seguido del programa intérprete:

```bash
#!/bin/bash
```

**Ejemplo de script:**
```bash
#!/bin/bash
# Fecha de escritura: 15/05/2024
# Este comando informa el directorio actual de trabajo

lugar=$(pwd)
echo "En este momento te encuentras trabajando en $lugar, no vayas a perderte"
```

**Ejecución:**
```bash
nano script.sh
bash script.sh
```

## 🎯 Actividades Prácticas

### Actividad 1: Arrays y Archivos
```bash
# A. Crear array en terminal
mutacion=("ATCGATCGATCG" "ATCGATCGATGG")
echo "${mutacion[@]}"

# B. Crear archivo con nano
nano data.txt
# Contenido:
# ATCGATCGATCG
# ATCGATCGATGG

cat data.txt
```

### Actividad 2: Manipulación de Datos Genómicos
Crear archivo `data1.txt` con:
```
Gen Log2 (FC) Perfil de Expresion
MAP3K20 -2.636 Subexpresado
HDAC5 -1.560 Subexpresado
BUB1 3.187 Sobreexpresado
EZH2 2.308 Sobreexpresado
AURKB 4.174 Sobreexpresado
```

**Comandos de procesamiento:**
```bash
sort -n data1.txt
sort -k2,2 -o data1ordenada.txt data1.txt
cat data1ordenada.txt
```

## 📚 Características de BASH

- Ejecución secuencial o paralela de órdenes
- Control de trabajo
- Variables, operadores, estructuras de control
- Funciones
- Redirecciones de entrada y salida
- Ejecución interactiva de comandos
- Creación de alias

## 🎓 Palabras Clave
- **Programación en Shell**
- **Scripts**
- **Automatización**
- **Eficiencia**
- **Tipos de shell**

## 📖 Referencias Bibliográficas

1. **Hausenblas, M.** (2022). *Learning Modern Linux*. O'Reilly Media, Inc. Chapter 1 and 2.
2. **Blum, R., & Bresnahan, C.** (2021). *Linux command line and Shell scripting bible* (Fourth edition). Wiley. Capítulo 11.

## 🔗 Enlaces Útiles

- [Vim Official](https://www.vim.org/about.php)
- [Vim Adventures - Juego para aprender Vim](https://vim-adventures.com/)
- [Nano Editor](https://www.nano-editor.org/)
- [Visual Studio Code](https://code.visualstudio.com/)

---

**Universidad Peruana de Ciencias Aplicadas**  
*Facultad de Ciencias de la Salud - Programa de Biología*
