# Introducción a la Programación en Shell

## Descripción del Curso
**Facultad de Ciencias de la Salud - Programa de Biología**  
**Curso:** Principios de programación - Bioinformática  
**Semestre:** 2025-2  

**Docentes:**
- Frank Guzman Escudero
- Manuel Ramírez Sáenz

## Logro de Aprendizaje
Al finalizar esta sesión, el estudiante conoce la utilidad y beneficios de la programación en shell a través de scripts.

## 🛠️ ¿Qué es Shell?

El **shell** es un intérprete de comandos que provee una interfaz entre el usuario y el kernel del sistema operativo. Interpreta los comandos que el usuario escribe para que puedan ser ejecutados.

![](https://i.sstatic.net/LBRdx.png)

## 📝 ¿Qué es un Shell Script?

Un **shell script** es un archivo que contiene una secuencia de comandos escritos en un lenguaje de shell específico (como Bash). Estos scripts se utilizan para:

- Automatizar tareas.
- Realizar una serie de acciones en el sistema operativo.
- Ejecutar procesos repetitivos de manera eficiente.

## 🚀 Ventajas del Uso de Scripts

### ⚡ AUTOMATIZACIÓN
- Permite automatizar tareas repetitivas y ahorrar tiempo.
- **Filosofía:** "Construye una vez y ejecuta muchas veces".

### 🌐 PORTABILIDAD
- Los scripts pueden ejecutarse en varias plataformas y sistemas operativos.
- Compatible a través de máquinas virtuales.

### 🔧 FLEXIBILIDAD
- Altamente personalizables.
- Pueden combinarse con otros lenguajes de programación.

### 📖 ACCESIBILIDAD
- Fáciles de escribir.
- No requieren herramientas especiales.
- Editables con cualquier editor de texto.
- La mayoría de sistemas operativos incluyen intérprete de shell.

### 🔗 INTEGRACIÓN
- Compatible con bases de datos.
- Integración con servidores web.
- Conexión con servicios en la nube.

### 🐛 DEPURACIÓN
- Herramientas incluidas para identificar problemas rápidamente.
- Facilita la corrección de errores.

## 📝 Editores de Texto

### VIM - Vi IMproved

**Características:**
- Editor avanzado multiplataforma.
- Gratuito y código abierto.
- Creado por Bram Moolenaar en 1991.
- Basado en el editor Vi original (Bill Joy, 1976).

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
- Editor básico y fácil de usar.
- Ideal para usuarios principiantes.
- Funciones de edición esenciales.

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
- Editor moderno con interfaz gráfica.
- Extensiones para programación.
- Soporte completo para shell scripting.

## 🐚 Tipos de Shell

### Shell Bourne (sh)

- **Creador:** Steven Bourne
- Primera shell tradicional de sistemas UNIX
- Amplia compatibilidad con scripts
- Símbolo del sistema: `$`

### Bash (Bourne Again Shell)

- **Más moderno y utilizado**
- Desarrollado para el Proyecto GNU (Proyecto colaborativo que tiene como objetivo desarrollar un sistema operativo libre).
- Predeterminado en la mayoría de distribuciones Linux.
- Características: historial ilimitado, control de trabajos, funciones, alias.
- Símbolo del sistema: `$`.

### Korn Shell (ksh)

- **Creador:** David Korn.
- Historial de órdenes y edición en línea.
- Características de programación ampliadas.
- Símbolo del sistema: `$`

### Z Shell (zsh)

- Sistema de completado avanzado.
- Temas y plugins.
- Poderosa expansión de comandos.
- Símbolo del sistema: `$`.

### C Shell (csh)

- **Creador:** Bill Joy (Berkeley University).
- Más utilizado en sistemas BSD.
- Características para programadores en C.
- Símbolo del sistema: `%`.

### Tenex C Shell (tcsh)

- Superconjunto de csh.
- Más rápido que csh.
- Símbolo del sistema: `%`.

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
chsh -s /bin/ # tabuleamos
```

### Instalar documentación de Bash

```bash
sudo apt-get install bash-doc
man bash
```

## El Shebang `#!`

Los scripts deben empezar con `#!` seguido del programa intérprete:

```bash
#!/bin/bash
```
**¿Qué significa?**
- `#!` = "intérprete de comandos" (shebang)
- `/bin/bash` = ubicación del intérprete bash que queremos usar

**¿Por qué es importante?**
- Le dice al sistema operativo **cómo ejecutar el archivo**
- Sin él, el sistema no sabría que es un script bash
- Debe ser **la primera línea** del archivo

**Analógía:** Como poner una etiqueta que dice "Abre esto con Bash"

## Comentarios

Los comentarios comienzan con `#` y el sistema los ignora:

```bash
#!/bin/bash
# Este es un comentario
# Fecha: 15/05/2024
# Autor: Tu nombre

echo "Esta línea SÍ se ejecuta"
# echo "Esta línea NO se ejecuta (es comentario)"
```

**Buena práctica:** Documenta qué hace tu script con comentarios claros.

## Variables

Una **variable** es un contenedor que almacena un valor (texto, números, rutas de archivos, etc.).

#### Crear una variable

```bash
NOMBRE_PROYECTO="Analisis_RNA"
NUMERO_MUESTRAS=50
RUTA_DATOS="/home/usuario/datos"
```

**Reglas para nombres de variables:**
- Usa MAYÚSCULAS por convención
- Puedes usar números y guiones bajos
- No pueden empezar con número
- No uses espacios alrededor del `=`



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

### **Creación de Scripts de Shell para Bioinformática**

#### **4.1. Estructura de un Script de Shell**

- **Shebang:** `#!/bin/bash`.
- **Comentarios:** `# Esto es un comentario`.
- **Variables:** `ruta="/ruta/al/directorio"`.
- **Ejecución de comandos:** `fastq-dump SRR123456`.

#### **Ejemplo de Script para Descargar Datos de SRA**

```bash
#!/bin/bash
# Descargar un archivo FASTQ desde SRA
ID="SRR1553607"
fastq-dump --split-files -X 10000 $ID
echo "Descarga completada: $ID.fastq"
```

#### **Ejemplo de Script para Buscar y Descargar Datos de NCBI**

```bash
#!/bin/bash
# Buscar y descargar datos de NCBI
QUERY="Homo sapiens[Organism] AND RNA-Seq[All Fields]"
esearch -db nucleotide -query "$QUERY" | efetch -format fasta > datos.fasta
echo "Datos descargados: datos.fasta"
```

#### **Automatización de Tareas con Variables y Bucles**

  ```bash
  #!/bin/bash
  ID="SRR1553607"
  OUTPUT="raw_data/$ID.fastq"
  fastq-dump --split-files -X 10000 $ID -O $OUTPUT
  echo "Archivo guardado en: $OUTPUT"
  ```

#### **Uso de Bucles para Procesar Múltiples Archivos**

  ```bash
  #!/bin/bash
  # Descargar múltiples archivos FASTQ
  for ID in SRR1972948 SRR1972956 SRR1972955
  do
    fastq-dump --split-files -X 10000 $ID
    echo "Descargado: $ID.fastq"
  done
  ```

#### **Flujo de Trabajo Automatizado**
```bash
#!/bin/bash
# Flujo de trabajo automatizado
IDS=("SRR1972948" "SRR1972956" "SRR1972955")
for ID in ${IDS[@]}
do
  # Descargar archivo FASTQ
  fastq-dump --split-files -X 10000 $ID
  
  # Procesar archivo FASTQ
  bio analyze $ID.fastq > analisis_$ID.txt
  
  # Generar informe
  echo "Análisis completado para $ID" >> informe.txt
done
echo "Flujo de trabajo completado."
```
## 📚 Características de BASH

- Ejecución secuencial o paralela de órdenes
- Control de trabajo
- Variables, operadores, estructuras de control

## 🎓 Palabras Clave
 
- **Programación en Shell**
- **Scripts**
- **Automatización**
- **Eficiencia**
- **Tipos de shell**

## 📖 Referencias Bibliográficas

1. **Hausenblas, M.** (2022). *Learning Modern Linux*. O'Reilly Media, Inc. Chapter 1 and 2.
2. **Blum, R., & Bresnahan, C.** (2021). *Linux command line and Shell scripting bible* (Fourth edition). Wiley. Capítulo 11.

---
