# Elementos de la Sintaxis de Shell Scripting I
## Variables, Shell y Operaciones

## Información del Curso
- **Programa:** Biología - Bioinformática
- **Curso:** Principios de programación
- **Docentes:** Frank Guzman Escudero, Manuel Ramírez Sáenz
- **Semestre:** 2025-02

## Logro de la Sesión
Al finalizar la sesión, el estudiante será capaz de:
- Definir y utilizar variables en shell scripting
- Diferenciar entre variables locales y globales
- Comprender el concepto de shell vs subshell
- Manipular la variable de entorno PATH
- Aplicar variables especiales en scripts bioinformáticos
- Realizar operaciones aritméticas y expansión de comandos

**Palabras clave:** Variables, Shell, Subshell, PATH, Operaciones, Expansión de comandos

---

## 1. CONCEPTOS FUNDAMENTALES SOBRE VARIABLES

### ¿Qué es una Variable?

Una **variable** es una ubicación en memoria donde se almacena un valor que puede ser:
- **Creado** durante la ejecución del script
- **Modificado** según las necesidades
- **Guardado** para uso posterior en el mismo script
- **Eliminado** cuando ya no se necesita

**Analogía:** Una variable es como una caja etiquetada donde guardas información.

### Características Importantes de las Variables

1. **Naturaleza múltiple:** Pueden almacenar diferentes tipos de datos (texto, números, rutas)
2. **Dinámicas:** Su valor puede cambiar durante la ejecución
3. **Reutilizables:** Se pueden usar múltiples veces en un script
4. **Eficientes:** Evitan repetir valores literal en todo el código

**Ejemplo de ventaja:**

```bash
# SIN variables (repetitivo y difícil de mantener)
echo "Proyecto: Mi_Genoma"
mkdir Mi_Genoma
cp archivo.fasta Mi_Genoma/
cd Mi_Genoma

# CON variables (flexible y reutilizable)
PROYECTO="Mi_Genoma"
echo "Proyecto: $PROYECTO"
mkdir $PROYECTO
cp archivo.fasta $PROYECTO/
cd $PROYECTO
```

### Sintaxis Básica para Crear una Variable

```bash
nombre_variable=valor
```

**Reglas importantes:**
- ⚠️ **NO debe haber espacios** alrededor del signo `=`
- Las variables pueden contener letras, números y guiones bajos
- No pueden empezar con número
- Por convención, usa MAYÚSCULAS para nombres

**Ejemplos correctos:**

```bash
PROYECTO="Analisis_RNA"        # Correcto
NUMERO_MUESTRAS=50             # Correcto
RUTA_DATOS="/home/usuario/datos" # Correcto
```

**Ejemplos INCORRECTOS:**

```bash
PROYECTO = "Analisis_RNA"      # ❌ Espacios alrededor de =
numero_muestras = 50           # ❌ Espacios alrededor de =
123VARIABLE="valor"            # ❌ Comienza con número
```

### Acceso a Variables: El Símbolo `$`

Para usar el valor de una variable, debes antecederla con el símbolo `$`:

```bash
#!/bin/bash
NOMBRE="Juan"
EDAD=25

echo $NOMBRE          # Imprime: Juan
echo $EDAD            # Imprime: 25
echo "Mi nombre es $NOMBRE"  # Imprime: Mi nombre es Juan
```

**Dos formas de acceder a variables:**

```bash
# Forma 1: Simple (más común)
echo $VARIABLE

# Forma 2: Con llaves (más explícita)
echo ${VARIABLE}
```

La forma con llaves es útil cuando la variable está seguida de texto:

```bash
ARCHIVO="genoma"
# Sin llaves (confuso)
echo $ARCHIVOv1.fasta    # Imprime: (nada, porque busca $ARCHIVOv1)

# Con llaves (claro)
echo ${ARCHIVO}v1.fasta  # Imprime: genomav1.fasta
```

---

## 2. TIPOS DE VARIABLES: LOCALES Y GLOBALES

### Variables Locales

Una **variable local** es accesible **solo** en el shell o subshell donde se definió.

**Sintaxis:**
```bash
variable=valor
```

**Características:**
- Alcance limitado al script o sesión actual
- No se heredan a subshells
- Ideales para valores temporales

**Ejemplo:**

```bash
#!/bin/bash
# Script: ejemplo_local.sh

# Definir variable local
MUESTRA="bacteria_001"

echo "Dentro del script: $MUESTRA"  # ✓ Funciona
```

**Ejecución:**
```bash
bash ejemplo_local.sh
# Output: Dentro del script: bacteria_001

echo $MUESTRA
# Output: (vacío - la variable no existe fuera del script)
```

### Variables Globales (De Entorno)

Una **variable global** es accesible desde cualquier shell, subshell o script.

**Sintaxis:**
```bash
export variable=valor
```

**Características:**
- Alcance global (toda la sesión)
- Se heredan a subshells
- Persisten durante toda la sesión de usuario
- Útiles para configuraciones que deben ser compartidas

**Ejemplo:**

```bash
#!/bin/bash
# Script: ejemplo_global.sh

# Definir variable global
export LABORATORIO="Genómica Computacional"

echo "Laboratorio: $LABORATORIO"  # ✓ Funciona

# Crear un subshell
bash -c 'echo "En subshell: $LABORATORIO"'  # ✓ También funciona
```

**Ejecución:**
```bash
source ejemplo_global.sh
# Output: Laboratorio: Genómica Computacional
# Output: En subshell: Genómica Computacional

echo $LABORATORIO
# Output: Genómica Computacional (persiste en la sesión)
```

### Comparación: Local vs Global

| Aspecto | Local | Global |
|---------|-------|--------|
| Sintaxis | `variable=valor` | `export variable=valor` |
| Alcance | Solo script actual | Toda la sesión |
| Heredada a subshell | ❌ No | ✅ Sí |
| Persiste en sesión | ❌ No | ✅ Sí |
| Caso de uso | Valores temporales | Configuraciones compartidas |

---

## 3. SHELL VS SUBSHELL

### Concepto: ¿Qué es un Shell y un Subshell?

Cuando abres una terminal, estás en un **shell** (intérprete de comandos). Si ejecutas un script o comando que abre otro shell, creas un **subshell** (shell hijo).

**Visualización:**

```
TERMINAL PRINCIPAL
│
├── SHELL PADRE (tu sesión)
│   │   [VARIABLE_1 = "valor_1"]
│   │
│   └─→ SUBSHELL (script o comando)
│       [VARIABLE_1 = "valor_1" (heredada)]
│       [VARIABLE_2 = "valor_2" (nueva)]
│
└── Variables del padre NO se modifican por cambios en subshell
```

### Comportamiento de Variables en Shell vs Subshell

#### Variables Locales: NO se heredan

```bash
#!/bin/bash
# Script: padre.sh

VARIABLE_LOCAL="valor del padre"

echo "En el padre: $VARIABLE_LOCAL"

# Crear un subshell
bash -c 'echo "En el subshell: $VARIABLE_LOCAL"'
```

**Ejecución:**
```bash
bash padre.sh

# Output:
# En el padre: valor del padre
# En el subshell: (vacío)
```

#### Variables Globales: SÍ se heredan

```bash
#!/bin/bash
# Script: padre_global.sh

export VARIABLE_GLOBAL="valor del padre"

echo "En el padre: $VARIABLE_GLOBAL"

# Crear un subshell
bash -c 'echo "En el subshell: $VARIABLE_GLOBAL"'
```

**Ejecución:**
```bash
bash padre_global.sh

# Output:
# En el padre: valor del padre
# En el subshell: valor del padre
```

### Modificaciones en Subshell NO afectan al Shell Padre

```bash
#!/bin/bash
# Script: modificacion.sh

export VARIABLE="valor inicial"

echo "Antes - Padre: $VARIABLE"

# Crear subshell y modificar variable
bash -c 'export VARIABLE="valor modificado"; echo "En subshell: $VARIABLE"'

# Verificar en el padre
echo "Después - Padre: $VARIABLE"
```

**Ejecución:**
```bash
bash modificacion.sh

# Output:
# Antes - Padre: valor inicial
# En subshell: valor modificado
# Después - Padre: valor inicial   ← NO cambió
```

### Comandos Importantes para Shell y Subshell

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| `bash` | Crear un nuevo subshell | `bash` |
| `exit` | Salir del subshell actual | `exit` |
| `export` | Convertir variable local a global | `export VARIABLE="valor"` |
| `$$` | PID del shell actual | `echo $$` |
| `$SHELL` | Shell por defecto del usuario | `echo $SHELL` |

**Demostraciones prácticas:**

```bash
# Ver el PID del shell actual
echo "PID del shell padre: $$"

# Crear un subshell y ver su PID diferente
bash -c 'echo "PID del subshell: $$"'

# Verificar shell actual
echo "Shell actual: $SHELL"
```

---

## 4. VARIABLES DE ENTORNO GLOBALES DEL SISTEMA

### ¿Qué son Variables de Entorno?

Las **variables de entorno** son variables globales predefinidas por el sistema operativo que contienen información sobre la configuración del sistema.

### Variables Importantes del Sistema

| Variable | Descripción | Ejemplo |
|----------|-------------|---------|
| `$PATH` | Directorios donde el sistema busca comandos | `/usr/local/bin:/usr/bin:/bin` |
| `$HOME` | Directorio personal del usuario | `/home/estudiante` |
| `$USER` | Nombre del usuario actual | `estudiante` |
| `$SHELL` | Shell por defecto del usuario | `/bin/bash` |
| `$PWD` | Directorio de trabajo actual | `/home/estudiante/proyecto` |
| `$HOSTNAME` | Nombre de la máquina | `laptop-001` |
| `$LANG` | Idioma del sistema | `es_PE.UTF-8` |
| `$TERM` | Tipo de terminal | `xterm-256color` |

### Visualizar Variables de Entorno

#### Opción 1: Ver una variable específica

```bash
echo $HOME
# Output: /home/estudiante

echo $USER
# Output: estudiante
```

#### Opción 2: Usar `printenv`

```bash
# Ver una variable específica
printenv HOME
# Output: /home/estudiante

# Ver todas las variables
printenv

# Ver primeras líneas
printenv | head
```

#### Opción 3: Usar `env`

```bash
# Ver todas las variables con env
env

# Ver variables que contienen "PATH"
env | grep PATH
```

### Ejemplo Práctico: Inspeccionar tu Entorno

```bash
#!/bin/bash
# Script: inspeccionar_entorno.sh

echo "=== INFORMACIÓN DEL ENTORNO ==="
echo ""
echo "Usuario: $USER"
echo "Directorio home: $HOME"
echo "Directorio actual: $PWD"
echo "Shell: $SHELL"
echo "Máquina: $HOSTNAME"
echo ""
echo "=== RUTAS EN PATH ==="
echo $PATH | tr ':' '\n'
```

**Ejecución:**
```bash
bash inspeccionar_entorno.sh

# Output:
# === INFORMACIÓN DEL ENTORNO ===
# 
# Usuario: estudiante
# Directorio home: /home/estudiante
# Directorio actual: /home/estudiante/proyecto
# Shell: /bin/bash
# Máquina: laptop-001
# 
# === RUTAS EN PATH ===
# /usr/local/bin
# /usr/bin
# /bin
# ...
```

---

## 5. MANIPULACIÓN DE LA VARIABLE PATH

### ¿Qué es PATH?

`PATH` es una **variable de entorno** que contiene una lista de **directorios separados por `:`** donde el sistema busca comandos ejecutables cuando escribes un comando.

**Estructura:**
```
/usr/local/bin : /usr/bin : /bin : /usr/sbin : /sbin
```

Cuando escribes un comando como `ls`, el sistema busca el archivo ejecutable `ls` en estos directorios, en orden, hasta encontrarlo.

### Ver el PATH Actual

```bash
# Ver PATH completo
echo $PATH

# Ver cada directorio en una línea
echo $PATH | tr ':' '\n'

# Output típico:
# /usr/local/bin
# /usr/bin
# /bin
# /usr/sbin
# /sbin
```

### Agregar Directorios al PATH

#### Opción 1: Agregar al final del PATH

```bash
export PATH=$PATH:/nueva/ruta
```

**Ejemplo:**

```bash
#!/bin/bash
# Script: agregar_path.sh

# Mostrar PATH original
echo "PATH original:"
echo $PATH | tr ':' '\n'

# Agregar nueva ruta
export PATH=$PATH:/home/usuario/mis_programas

echo ""
echo "PATH actualizado:"
echo $PATH | tr ':' '\n'
```

#### Opción 2: Agregar al inicio del PATH (búsqueda prioritaria)

```bash
export PATH=/nueva/ruta:$PATH
```

**Ejemplo:**

```bash
#!/bin/bash
# Si hay versiones conflictivas de un comando, esta ruta se busca primero
export PATH=/home/usuario/herramientas_nuevas:$PATH
```

### Caso Práctico: Instalación de Herramienta Bioinformática

```bash
#!/bin/bash
# Script: instalar_fastqc.sh
# Instala FastQC y lo agrega al PATH

# Crear directorio para herramientas
mkdir -p ~/biotools
cd ~/biotools

# Descargar FastQC
echo "Descargando FastQC..."
wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.12.1.zip

# Descomprimir
echo "Descomprimiendo..."
unzip fastqc_v0.12.1.zip

# Agregar al PATH
export PATH=$PATH:~/biotools/FastQC

# Verificar
echo "Verificando PATH:"
echo $PATH | tr ':' '\n'

# Probar comando
fastqc --version
```

### Hacer el PATH Permanente

Para que los cambios persistan entre sesiones, agrega la línea al archivo `~/.bashrc`:

```bash
# Abrir archivo de configuración
nano ~/.bashrc

# Agregar al final:
export PATH=$PATH:~/biotools/FastQC

# Guardar y recargar
source ~/.bashrc
```

---

## 6. OPERACIONES CON VARIABLES

### Comillas Simples vs Comillas Dobles

#### Comillas Dobles (`" "`)

Las comillas dobles **permiten la expansión** de variables dentro de ellas:

```bash
#!/bin/bash
NOMBRE="Juan"

echo "Hola $NOMBRE"            # Resultado: Hola Juan
echo "Hola ${NOMBRE}"          # Resultado: Hola Juan
echo "El usuario es: $USER"    # Resultado: El usuario es: estudiante
```

**Cuándo usar:** Cuando necesitas que las variables se expandan

#### Comillas Simples (`' '`)

Las comillas simples **no permiten expansión** - el texto es literal:

```bash
#!/bin/bash
NOMBRE="Juan"

echo 'Hola $NOMBRE'            # Resultado: Hola $NOMBRE (literal)
echo 'El usuario es: $USER'    # Resultado: El usuario es: $USER (literal)
```

**Cuándo usar:** Cuando necesitas texto exacto sin variable

### Comparación Práctica

```bash
#!/bin/bash
# Script: comillas_ejemplo.sh

ARCHIVO="genoma.fasta"
RUTA="/home/usuario/datos"

echo "=== CON COMILLAS DOBLES ==="
echo "Archivo: $ARCHIVO"              # ✓ Expande variable
echo "Ruta: $RUTA"                    # ✓ Expande variable

echo ""
echo "=== CON COMILLAS SIMPLES ==="
echo 'Archivo: $ARCHIVO'              # ✗ No expande
echo 'Ruta: $RUTA'                    # ✗ No expande

echo ""
echo "=== SIN COMILLAS ==="
echo Contenido de $ARCHIVO            # Funciona pero no recomendado si hay espacios
```

**Output:**
```
=== CON COMILLAS DOBLES ===
Archivo: genoma.fasta
Ruta: /home/usuario/datos

=== CON COMILLAS SIMPLES ===
Archivo: $ARCHIVO
Ruta: $RUTA

=== SIN COMILLAS ===
Contenido de genoma.fasta
```

### Operaciones Aritméticas

#### Método 1: `$(( ))`

```bash
#!/bin/bash
suma=$((13 + 8))
echo "Suma: $suma"              # Resultado: 21

resta=$((20 - 5))
echo "Resta: $resta"            # Resultado: 15

multiplicacion=$((4 * 5))
echo "Multiplicación: $multiplicacion"  # Resultado: 20

division=$((100 / 5))
echo "División: $division"      # Resultado: 20

modulo=$((17 % 5))
echo "Módulo: $modulo"          # Resultado: 2
```

#### Método 2: `$[ ]` (menos común)

```bash
suma=$[13 + 8]
echo $suma                      # Resultado: 21
```

#### Método 3: `bc` (para decimales)

```bash
#!/bin/bash
# bc permite decimales
resultado=$(echo "10.5 + 3.2" | bc)
echo "Resultado: $resultado"    # Resultado: 13.7

promedio=$(echo "scale=2; (85 + 90 + 78) / 3" | bc)
echo "Promedio: $promedio"      # Resultado: 84.33
```

### Ejemplo Práctico: Cálculos Bioinformáticos

```bash
#!/bin/bash
# Script: calculos_genomica.sh

# Conteos de secuencias
SECUENCIAS_TOTAL=1000000
SECUENCIAS_PROCESADAS=$((SECUENCIAS_TOTAL / 10))

echo "Total de secuencias: $SECUENCIAS_TOTAL"
echo "Procesadas: $SECUENCIAS_PROCESADAS"
echo "Porcentaje: $((SECUENCIAS_PROCESADAS * 100 / SECUENCIAS_TOTAL))%"

# Cálculo de tamaño de archivo
TAMAÑO_BYTES=1048576
TAMAÑO_MB=$(echo "scale=2; $TAMAÑO_BYTES / 1048576" | bc)
echo "Tamaño en MB: $TAMAÑO_MB"
```

### Expansión de Comandos

La **expansión de comandos** permite guardar el resultado de un comando en una variable.

#### Sintaxis Moderna: `$(comando)`

```bash
#!/bin/bash
# Obtener fecha actual
FECHA=$(date +"%Y-%m-%d")
echo "Hoy es: $FECHA"

# Obtener usuario actual
USUARIO=$(whoami)
echo "Usuario: $USUARIO"

# Obtener directorio actual
DIRECTORIO=$(pwd)
echo "Directorio: $DIRECTORIO"

# Contar archivos
NUM_ARCHIVOS=$(ls -1 | wc -l)
echo "Número de archivos: $NUM_ARCHIVOS"
```

#### Sintaxis Clásica: `` `comando` `` (backticks)

```bash
# Menos recomendada, pero aún funciona
FECHA=`date +"%Y-%m-%d"`
echo "Hoy es: $FECHA"
```

### Ejemplo Práctico: Información del Sistema

```bash
#!/bin/bash
# Script: info_sistema.sh

echo "=== INFORMACIÓN DEL SISTEMA ==="
echo ""
echo "Fecha y hora: $(date)"
echo "Usuario: $(whoami)"
echo "Directorio actual: $(pwd)"
echo "Hostname: $(hostname)"
echo "Kernel: $(uname -r)"
echo ""
echo "=== INFORMACIÓN DEL PROYECTO ==="

PROYECTO="Analisis_RNA"
FECHA_INICIO=$(date +"%Y-%m-%d")
ARCHIVOS_CREADOS=$(ls -1 | wc -l)

echo "Proyecto: $PROYECTO"
echo "Fecha inicio: $FECHA_INICIO"
echo "Archivos en directorio: $ARCHIVOS_CREADOS"
```

---

## 7. VARIABLES ESPECIALES DE SCRIPT

Las **variables especiales** son variables predefinidas por bash que contienen información sobre el script y sus argumentos.

### Variables Especiales Principales

| Variable | Descripción | Ejemplo |
|----------|-------------|---------|
| `$0` | Nombre del script | `script.sh` |
| `$1, $2, $3...` | Argumentos posicionales (1º, 2º, 3º, etc.) | `$1` = primer arg |
| `$#` | Número total de argumentos | `3` |
| `$$` | PID (Process ID) del shell actual | `1234` |
| `$?` | Código de salida del último comando | `0` (éxito), `1` (error) |

### `$0`: Nombre del Script

```bash
#!/bin/bash
# Script: mostrar_nombre.sh

echo "Nombre del script: $0"
echo "Nombre sin ruta: $(basename $0)"
```

**Ejecución:**
```bash
bash mostrar_nombre.sh
# Output:
# Nombre del script: mostrar_nombre.sh
# Nombre sin ruta: mostrar_nombre.sh

bash ~/proyectos/mostrar_nombre.sh
# Output:
# Nombre del script: /home/usuario/proyectos/mostrar_nombre.sh
# Nombre sin ruta: mostrar_nombre.sh
```

### `$1, $2, $3...`: Argumentos Posicionales

```bash
#!/bin/bash
# Script: procesar_argumentos.sh

echo "Primer argumento: $1"
echo "Segundo argumento: $2"
echo "Tercer argumento: $3"
```

**Ejecución:**
```bash
bash procesar_argumentos.sh bacteria genoma secuencias
# Output:
# Primer argumento: bacteria
# Segundo argumento: genoma
# Tercer argumento: secuencias
```

### `$#`: Número Total de Argumentos

```bash
#!/bin/bash
# Script: contar_argumentos.sh

echo "Número total de argumentos: $#"

# Ejemplo de validación
if [ $# -eq 0 ]; then
    echo "ERROR: No se proporcionaron argumentos"
    exit 1
fi

echo "Argumentos recibidos: $@"
```

**Ejecución:**
```bash
bash contar_argumentos.sh
# Output: Número total de argumentos: 0
# Output: ERROR: No se proporcionaron argumentos

bash contar_argumentos.sh genoma.fasta analisis.txt
# Output: Número total de argumentos: 2
# Output: Argumentos recibidos: genoma.fasta analisis.txt
```

### `$$`: PID del Proceso

```bash
#!/bin/bash
# Script: mostrar_pid.sh

echo "PID de este script: $$"
echo "PID del shell padre: $PPID"

# Crear subshell y mostrar su PID
bash -c 'echo "PID del subshell: $$"'
```

**Ejecución:**
```bash
bash mostrar_pid.sh
# Output:
# PID de este script: 12345
# PID del shell padre: 12340
# PID del subshell: 12346
```

### `$?`: Código de Salida del Último Comando

```bash
#!/bin/bash
# Script: verificar_exito.sh

# Comando exitoso
ls /home
echo "Código de salida ls: $?"  # Output: 0

# Comando fallido
ls /directorio_inexistente 2>/dev/null
echo "Código de salida: $?"     # Output: 2 (error)
```

**Códigos comunes:**
- `0` = Éxito
- `1` = Error general
- `2` = Uso incorrecto
- `127` = Comando no encontrado

---

## 8. EJEMPLO PRÁCTICO COMPLETO: SCRIPT DE INFORMACIÓN

Este script integra todos los conceptos aprendidos:

```bash
#!/bin/bash
# Script: analizar_archivo.sh
# Descripción: Analiza un archivo FASTA
# Autor: [Tu nombre]
# Fecha: [Fecha]
# Uso: bash analizar_archivo.sh archivo.fasta [directorio_salida]

# ========== VALIDACIÓN DE ARGUMENTOS ==========
# Verificar número de argumentos
if [ $# -lt 1 ]; then
    echo "Uso: $0 archivo.fasta [directorio_salida]"
    echo "Ejemplo: $0 bacteria.fasta resultados/"
    exit 1
fi

# ========== VARIABLES DE ENTRADA ==========
ARCHIVO_ENTRADA="$1"
DIRECTORIO_SALIDA="${2:-.}"    # Si no proporciona, usa directorio actual

# ========== VARIABLES DE SISTEMA ==========
USUARIO=$(whoami)
FECHA=$(date +"%Y-%m-%d")
HORA=$(date +"%H:%M:%S")
SCRIPT_NAME=$(basename $0)
SCRIPT_PID=$$

# ========== VARIABLES DE CÁLCULO ==========
# Nombre del archivo sin extensión
NOMBRE_BASE=$(basename "$ARCHIVO_ENTRADA" .fasta)

# Contar secuencias
NUM_SECUENCIAS=$(grep -c "^>" "$ARCHIVO_ENTRADA")

# Tamaño del archivo
TAMAÑO_BYTES=$(wc -c < "$ARCHIVO_ENTRADA")
TAMAÑO_KB=$((TAMAÑO_BYTES / 1024))

# ========== CREAR DIRECTORIO DE SALIDA ==========
mkdir -p "$DIRECTORIO_SALIDA"

# ========== ARCHIVO DE REPORTE ==========
REPORTE="$DIRECTORIO_SALIDA/${NOMBRE_BASE}_reporte.txt"

# ========== GENERAR REPORTE ==========
cat > "$REPORTE" << EOF
==============================================
REPORTE DE ANÁLISIS DE ARCHIVO FASTA
==============================================

INFORMACIÓN DE EJECUCIÓN:
------------------------
Fecha: $FECHA a las $HORA
Usuario: $USUARIO
Script: $SCRIPT_NAME (PID: $SCRIPT_PID)

INFORMACIÓN DEL ARCHIVO:
------------------------
Nombre: $ARCHIVO_ENTRADA
Nombre base: $NOMBRE_BASE
Tamaño: $TAMAÑO_BYTES bytes ($TAMAÑO_KB KB)

ANÁLISIS:
------------------------
Número de secuencias: $NUM_SECUENCIAS
Secuencias promedio por KB: $((NUM_SECUENCIAS * 1024 / TAMAÑO_KB))

DIRECTORIO DE SALIDA:
------------------------
$DIRECTORIO_SALIDA

==============================================
FIN DEL REPORTE
==============================================
EOF

# ========== MOSTRAR INFORMACIÓN EN PANTALLA ==========
echo ""
echo "╔════════════════════════════════════════╗"
echo "║ ANÁLISIS COMPLETADO EXITOSAMENTE      ║"
echo "╚════════════════════════════════════════╝"
echo ""
echo "📄 Archivo: $ARCHIVO_ENTRADA"
echo "📊 Secuencias: $NUM_SECUENCIAS"
echo "💾 Tamaño: $TAMAÑO_KB KB"
echo "📝 Reporte guardado en: $REPORTE"
echo ""
```

**Ejecución:**
```bash
bash analizar_archivo.sh bacteria.fasta

# Output:
# ╔════════════════════════════════════════╗
# ║ ANÁLISIS COMPLETADO EXITOSAMENTE      ║
# ╚════════════════════════════════════════╝
#
# 📄 Archivo: bacteria.fasta
# 📊 Secuencias: 1250
# 💾 Tamaño: 256 KB
# 📝 Reporte guardado en: ./bacteria_reporte.txt
```

---

## 9. BUENAS PRÁCTICAS

### Nomenclatura de Variables

```bash
# ✓ BIEN: Nombres descriptivos en MAYÚSCULAS
ARCHIVO_ENTRADA="bacteria.fasta"
DIRECTORIO_SALIDA="/resultados"
NUM_SECUENCIAS=1000

# ✗ MAL: Nombres vagos
a="bacteria.fasta"
d="/resultados"
n=1000

# ✗ MAL: Espacios en nombres
ARCHIVO ENTRADA="bacteria.fasta"
```

### Uso de Comillas

```bash
# ✓ BIEN: Siempre usar comillas en variables
mkdir "$DIRECTORIO"
echo "$USUARIO"

# ✗ MAL: Sin comillas (problemas con espacios)
mkdir $DIRECTORIO
echo $USUARIO
```

### Comentarios y Documentación

```bash
#!/bin/bash
# Script: analizar_genoma.sh
# Descripción: Análisis completo de archivo genómico
# Autor: [Tu nombre]
# Fecha: 2025-02-15
# Uso: bash analizar_genoma.sh archivo.fasta

# Definir variables importantes
ARCHIVO="$1"           # Primer argumento del script
FECHA=$(date +%Y-%m-%d)  # Fecha actual en formato YYYY-MM-DD
```

---

## 10. ACTIVIDADES PRÁCTICAS

### Actividad 1: Crear y Manipular Variables (15 minutos)

1. Crear script `mi_laboratorio.sh`:
```bash
#!/bin/bash
LABORATORIO="Genómica Computacional"
ANALISIS="RNA-seq"
AÑO=2025

echo "Laboratorio: $LABORATORIO"
echo "Análisis: $ANALISIS"
echo "Año: $AÑO"
echo "Proyecto: ${LABORATORIO}_${ANALISIS}_${AÑO}"
```

2. Ejecutar: `bash mi_laboratorio.sh`

### Actividad 2: Variables Locales vs Globales (20 minutos)

1. Crear `test_local.sh`:
```bash
#!/bin/bash
VARIABLE_LOCAL="solo aquí"
export VARIABLE_GLOBAL="en todas partes"

echo "En script: $VARIABLE_LOCAL y $VARIABLE_GLOBAL"
bash -c 'echo "En subshell: LOCAL=$VARIABLE_LOCAL GLOBAL=$VARIABLE_GLOBAL"'
```

2. Ejecutar y observar diferencias

### Actividad 3: Manipulación de PATH (15 minutos)

1. Ver PATH actual: `echo $PATH | tr ':' '\n'`
2. Crear directorio: `mkdir ~/mi_programa`
3. Agregar al PATH: `export PATH=$PATH:~/mi_programa`
4. Verificar: `echo $PATH | grep mi_programa`

### Actividad 4: Variables Especiales (20 minutos)

1. Crear `info_script.sh`:
```bash
#!/bin/bash
echo "Script: $0"
echo "Argumentos: $#"
echo "Primer arg: $1"
echo "Segundo arg: $2"
echo "PID: $$"
```

2. Ejecutar: `bash info_script.sh datos.fasta resultados`

---

## 11. AUTOEVALUACIÓN

### Preguntas de Comprensión

1. **¿Cuál es la diferencia entre `variable=valor` y `export variable=valor`?**
   - R: La primera crea una variable local, la segunda la hace global

2. **¿Por qué una variable en subshell no afecta al shell padre?**
   - R: Subshells son procesos independientes con memoria separada

3. **¿Qué hace `echo $PATH | tr ':' '\n'`?**
   - R: Muestra cada directorio del PATH en línea separada

4. **¿Cuándo usarías comillas simples vs dobles?**
   - R: Dobles para expansión, simples para texto literal

5. **¿Qué variable contiene el número de argumentos de un script?**
   - R: `$#`

---

## 12. PALABRAS CLAVE

- Variable
- Shell / Subshell
- Variable local
- Variable global
- Entorno
- PATH
- Expansión de comandos
- Variables especiales
- Argumentos posicionales

---

## 13. REFERENCIAS BIBLIOGRÁFICAS

1. Hausenblas, M. (2022). *Learning Modern Linux*. O'Reilly Media, Inc.
2. Blum, R., & Bresnahan, C. (2021). *Linux command line and Shell scripting bible*. Wiley.
