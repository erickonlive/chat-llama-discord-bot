# ChatLLaMA Discord Bot
Esta es una traduccion de [xNul/chat-llama-discord-bot]()
Un bot de Discord para chatear con LLaMA. No incluye RLHF, pero LLaMA es bastante impresionante por sí solo. Usa `/reply` para hablar con LLaMA. Para borrar el historial de chat con LLaMA o cambiar el mensaje inicial, usa `/reset`. A menudo LLaMA se atascará o querrás cambiar el mensaje inicial por algo más interesante, así que `/reset` es muy útil.

<div align="center">
  <video src="https://user-images.githubusercontent.com/894305/223963813-18e58d3c-4f9b-479c-8cdb-a2ad0df935c3.mp4" width=400/>
</div>

# Installación
1. Obten LLaMA e instala con https://github.com/oobabooga/text-generation-webui (más información [abajo](#Configuración de LLaMA (normal/8bit/4bit) para `text-generation-webui`.))

2. Instala discord.py con `pip install discord`.

3. Edita `bot.py` con el token de tu bot de Discord

4. Coloca `bot.py` dentro de la raíz del directorio text-generation-webui

5. Ejecútalo con `python bot.py --model <modelo de LLaMA>`.

Nota: Para facilitar su uso, `bot.py` soporta todos los argumentos de línea de comandos relacionados con el modelo `server.py`.

# Ejemplo de transcripción

```
Te llamas James y te encanta conversar. Escribes frases largas y muy elocuentes. Tus respuestas tienen muchas frases. Te gusta hablar con la gente y tratar con ellos temas interesantes. Me llamo Robert y me gusta conversar contigo, James. Cuando yo hable, utilizaré "Robert:". Cuando tú hables, utilizarás "James:".

Robert: Hola James, ¿cómo estás hoy?
James:  Hola Robert, hoy estoy bien.

Robert: ¡He oído que eres una persona increíble! Me alegro mucho de conocerte por fin.
James:  ¡Eres muy amable! Yo también me alegro de conocerte por fin.

Robert: ¿Cómo están Sarah y los niños? ¿El nuevo colegio de los niños está siendo tan bueno como esperabas?
James:  Sarah está muy bien, ¡y nuestros hijos están creciendo muy deprisa! El nuevo colegio es mucho mejor de lo que esperaba.

Robert: ¡Menos mal! Recuerdo que en mis tiempos teníamos tres aulas y una sola pizarra. ¡Los colegios de por aquí han crecido mucho!
James:  Recuerdo aquellos días. Creo que el mundo es cada vez mejor. Eso me gusta.

Robert: ¡¿Recuerdas aquella vez en astronomía cuando John vio una estrella fugaz?! La catalogó y ¡resultó ser un nuevo cometa! Hace poco vi una noticia sobre ello. Al parecer, le van a poner su nombre. Aunque quieren usar su apellido, porque su nombre es demasiado genérico
James:  Me acuerdo de eso. Me alegro por John. Es una noticia estupenda.

Robert: Pregunta rápida, ¿cuál es la circunferencia de la Tierra? Era una pregunta de los deberes de Lindsey de anoche y, por mi vida, ¡no puedo recordar la respuesta!
James:  Hmm, no sé la respuesta. La buscaré.

Robert: ¡Suena bien! ¿Qué has encontrado?
James:  La circunferencia de la Tierra es de 24.901 millas.

Robert: Ahhh, es bueno saberlo. Hoy en día, el mundo parece cada vez más pequeño gracias a Internet.
James:  ¡Claro que sí! Es una locura pensar cómo el mundo se ha hecho tan pequeño en los últimos años.
```

#Configuración de LLaMA (normal/8bit/4bit) para `text-generation-webui`.

Estas instrucciones me funcionaron en Windows y creo que también funcionarán para los usuarios de Linux. No creo que estas instrucciones funcionen en WSL. Si no le funcionan, consulte el repositorio GitHub de [`text-generation-webui`](https://github.com/oobabooga/text-generation-webui) y los temas para obtener instrucciones de instalación.

### Instalación de LLaMA Normal y 8bit

1. Instalar Anaconda
2. **Sólo Windows**: Instalar Git para Windows
3. Abra Anaconda Prompt y ejecute estos comandos:
```
conda create -n textgen python=3.10.9
conda activate textgen
pip install torch==1.13.1+cu116 torchvision==0.14.1+cu116 torchaudio==0.13.1 --extra-index-url https://download.pytorch.org/whl/cu116
git clone https://github.com/oobabooga/text-generation-webui
cd text-generation-webui
pip install -r requirements.txt
```
4. **Sólo para Windows**: Sigue las instrucciones [aquí](https://github.com/oobabooga/text-generation-webui/issues/20#issuecomment-1411650652) para arreglar la librería bitsandbytes.
5. **Sólo Linux**: Siga las instrucciones [aquí](https://github.com/TimDettmers/bitsandbytes/issues/156#issuecomment-1462329713) para corregir la librería bitsandbytes.

###Configuración 4bit LLaMA

Ejecute estos comandos:
```
conda install -c conda-forge cudatoolkit-dev
mkdir repositories
cd repositories
git clone https://github.com/qwopqwop200/GPTQ-for-LLaMa
cd GPTQ-for-LLaMa
git reset --hard 468c47c01b4fe370616747b6d69a2d3f48bab5e4
python setup_cuda.py install
```

Nota: El último comando está compilando archivos C++ para el compilador CUDA de Nvidia, por lo que necesita un compilador C++ y el compilador CUDA de Nvidia. Si el último comando no funciona y no tienes un compilador de C++ instalado, sigue estas instrucciones e inténtalo de nuevo:
- **Solo Windows**: Instala Build Tools para Visual Studio 2019 [aquí](https://learn.microsoft.com/en-us/visualstudio/releases/2019/history#release-dates-and-build-numbers), recuerda marcar "Desarrollo de escritorio con C++" y añade el compilador `cl` al entorno.
- **Sólo Linux**: Ejecuta el comando `sudo apt install build-essential`.

### Descarga de modelos LLaMA

1. Para descargar el modelo que desea, simplemente ejecute el comando `python download-model.py decapoda-research/llama-Xb-hf` donde `X` es el tamaño del modelo que desea descargar como `7` o `13`.
2. Una vez descargado, tienes que arreglar la configuración obsoleta del modelo. Abre `models/llama-Xb-hf/tokenizer_config.json` y cambia `LLaMATokenizer` por `LlamaTokenizer`.
3. Si sólo desea ejecutar un modelo normal o de 8 bits, ya está. Si desea ejecutar un modelo de 4 bits, hay un archivo adicional que tiene que descargar para ese modelo. Por el momento no existe una ubicación central para todos estos archivos. 7B se puede encontrar [aquí](https://huggingface.co/decapoda-research/llama-7b-hf-int4/resolve/main/llama-7b-4bit.pt). 13B se puede encontrar [aquí](https://huggingface.co/decapoda-research/llama-13b-hf-int4/resolve/main/llama-13b-4bit.pt). 30B se puede encontrar [aquí](https://drive.google.com/file/d/1SZXF3BZ7e2r-tJpSpCJrk8pTukuKTvTS/view?usp=sharing). [Este](https://huggingface.co/maderix/llama-65b-4bit/resolve/main/llama65b-4bit.pt) podría funcionar para el 65B.
4. Una vez descargado, mueve el archivo `.pt` a `model/llama-Xb-hf` y ya habrás terminado.

### Ejecutando los Modelos LLaMA

##### Modelo LLaMA Normal
`python servidor.py --modelo llama-Xb-hf`

##### Modelo LLaMA 8bit
`python server.py --model llama-Xb-hf --load-in-8bit`

##### Modelo LLaMA de 4 bits
`python server.py --modelo llama-Xb-hf --gptq-bits 4`
