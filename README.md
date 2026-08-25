[README.md](https://github.com/user-attachments/files/31442172/README.md)
# Nellie — Asistente de Voz Físico

Proyecto final de Integración de Sistemas, IoT y Arquitectura de Software.
Asistente de voz tipo Alexa/Google Home, desarrollado en Python, con
Raspberry Pi como hardware objetivo.

## ¿Qué hace?

Escucha en segundo plano hasta detectar la palabra de activación
("Hey Rhasspy"), graba tu pregunta hasta que dejás de hablar, la
transcribe, se la manda a la IA (Google Gemini), y te responde por voz.
Se le puede interrumpir mientras habla, y podés seguir preguntando sin
repetir la palabra de activación cada vez.

## Requisitos previos

- Python 3.11 o superior instalado ([python.org](https://www.python.org/downloads/))
- Un micrófono (USB o de auriculares)
- Conexión a internet (se usa para el reconocimiento de voz y la IA)
- Una API key gratuita de Google Gemini ([Google AI Studio](https://aistudio.google.com))

## Instalación (Windows)

### 1. Clonar el repositorio

```
git clone <URL-del-repositorio>
cd <carpeta-del-proyecto>
```

### 2. Crear el entorno virtual

Un entorno virtual (venv) mantiene las librerías de este proyecto
aisladas del resto de tu computadora.

```
python -m venv venv
```

### 3. Activar el entorno virtual

```
.\venv\Scripts\Activate.ps1
```

Vas a saber que está activo porque el prompt de la terminal empieza
con `(venv)`. **Hay que activarlo cada vez que abras una terminal
nueva para trabajar en el proyecto.**

Si PowerShell da un error de "ejecución de scripts deshabilitada", correr
antes (una sola vez):
```
Set-ExecutionPolicy -Scope Process -ExecutionPolicy RemoteSigned
```

### 4. Instalar las librerías necesarias

Con el `(venv)` ya activo:

```
pip install -r requirements.txt
```

### 5. Configurar la API key

1. Copiá el archivo `.env.ejemplo` y renombrá la copia a `.env`
2. Conseguí tu API key gratuita en [Google AI Studio](https://aistudio.google.com) (botón "Get API key")
3. Abrí `.env` y pegá tu key donde dice `pega_aqui_tu_api_key_real`

**Importante:** el archivo `.env` nunca se sube a GitHub (ya está
excluido en `.gitignore`). Cada persona que use el proyecto necesita
su propia key.

### 6. Ejecutar el asistente

```
python asistente.py
```

La primera vez puede tardar un poco más en arrancar (descarga el
modelo de detección de la palabra de activación). Después, decile
"Hey Rhasspy" para empezar a usarlo. Para cerrar el programa: `Ctrl+C`.

## Estructura del proyecto

```
config.py               # Configuración centralizada (todos los parámetros ajustables)
asistente.py            # Script principal, integra todos los módulos
test_microfono.py       # Captura de audio y detección de silencio
test_stt.py             # Conversión de voz a texto
test_ia.py              # Conexión con la API de Gemini
test_tts.py             # Conversión de texto a voz y reproducción
activacion.py           # Detección de la palabra de activación
requirements.txt        # Lista de librerías del proyecto
.env                    # API keys (no se sube a git)
```

Cada archivo `test_*.py` se puede ejecutar de forma individual para
probar ese módulo por separado, por ejemplo:

```
python test_microfono.py
```

## Ajustar el comportamiento

Todos los parámetros de sensibilidad (tiempo de silencio, umbrales de
detección de voz, modelo de IA usado, idioma, etc.) están centralizados
en `config.py`. No hace falta tocar el resto de los archivos para
ajustar el comportamiento del asistente.

## Problemas conocidos / troubleshooting

- **`ModuleNotFoundError`**: verificá que el `(venv)` esté activo antes
  de correr `pip install` o `python asistente.py`.
- **Error al instalar `pygame`**: si tenés una versión muy nueva de
  Python (3.13+), instalá `pygame-ce` en vez de `pygame` (ya está así
  en `requirements.txt`).
- **`PermissionError` en un archivo `.mp3`**: cerrá cualquier
  reproductor de audio que tenga el archivo abierto y volvé a intentar.

