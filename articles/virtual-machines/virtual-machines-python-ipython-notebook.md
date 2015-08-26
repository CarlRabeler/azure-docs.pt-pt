<properties
	pageTitle="Bloc de notas de IPython - Tutorial de Azure"
	description="Un tutorial que muestra cómo implementar el bloc de notas de IPython en Azure, utilizando máquinas virtuales de Linux o Windows."
	services="virtual-machines"
	documentationCenter="python"
	authors="huguesv"
	manager="wpickett"
	editor=""/>

<tags
	ms.service="virtual-machines"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-multiple"
	ms.devlang="python"
	ms.topic="article"
	ms.date="05/20/2015"
	ms.author="huvalo"/>


# Bloc de notas de IPython en Azure

El [proyecto IPython](http://ipython.org) proporciona una colección de herramientas para la informática científica que incluye potentes shells interactivos, bibliotecas paralelas de elevado rendimiento y facilidad de uso, así como un entorno basado en web conocido como bloc de notas de IPython. El bloc de notas ofrece un entorno de trabajo para la informática interactiva que combina la ejecución del código con la creación de documentos informáticos activos. Estos archivos del bloc de notas pueden contener texto arbitrario, fórmulas matemáticas, código de entrada, resultados, gráficos, vídeos y cualquier otra clase de contenido multimedia que pueda mostrar cualquier explorador web moderno.

Ya sea la primera toma de contacto con Python y desee aprenderlo en un entorno divertido e interactivo o bien esté poniendo en práctica la informática técnica o en paralelo, el bloc de notas de IPython constituirá una excelente elección. Para ilustrar sus capacidades, la captura de pantalla siguiente muestra IPython Notebook en uso, junto con los paquetes SciPy y Matplotlib, para analizar la estructura de una grabación de sonido.

![Instantánea](./media/virtual-machines-python-ipython-notebook/ipy-notebook-spectral.png)

En este artículo se explicará cómo implementar IPython Notebook en Microsoft Azure mediante máquinas virtuales (VM) con Linux o Windows. Al utilizar el bloc de notas de IPython en Azure, puede proporcionar fácilmente una interfaz accesible por web a los recursos informáticos escalables con toda la potencia de Python y sus múltiples bibliotecas. Como la instalación se realiza en la nube, los usuarios podrán obtener acceso a estos recursos sin necesidad de configuración local alguna, salvo un explorador web moderno.

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## Creación y configuración de una máquina virtual en Azure

El primer paso es crear una máquina virtual (VM) que se ejecute en Azure. Esta máquina virtual consiste en un sistema operativo completo en la nube y se utilizará para ejecutar el bloc de notas de IPython. Azure es capaz de ejecutar máquinas virtuales de Linux y Windows, y trataremos la instalación de IPython en ambos tipos de máquinas virtuales.

### Máquina virtual de Linux

Siga las instrucciones indicadas [aquí][portal-vm-linux] para crear una máquina virtual de la distribución *OpenSUSE* o *Ubuntu*. En este tutorial se utiliza OpenSUSE 13.2 y Ubuntu Server 14.04 LTS. Asumiremos el nombre de usuario predeterminado *azureuser*.

### Máquina virtual de Windows

Siga las instrucciones indicadas [aquí][portal-vm-windows] para crear una máquina virtual de la distribución *Windows Server 2012 R2 Datacenter*. En este tutorial, asumiremos que el nombre de usuario es *azureuser*.

## Creación de un extremo para IPython Notebook

Este paso se aplica tanto a las máquinas virtuales de Linux como a las de Windows. Más adelante configuraremos IPython para que ejecute su servidor de bloc de notas en el puerto 9999. Para que este puerto esté disponible públicamente, deberemos crear un extremo en el Portal de administración de Azure. Este extremo abre un puerto en el firewall de Azure y asigna el puerto público (HTTPS, 443) al puerto privado en la máquina virtual (9999).

Para crear un extremo, vaya al panel de la máquina virtual, haga clic en **Extremos**, a continuación en **Agregar extremo** y cree un nuevo extremo (denominado `ipython_nb` en este ejemplo). Seleccione **TCP** como protocolo, **443** como puerto público y **9999** como puerto privado.

![Instantánea](./media/virtual-machines-python-ipython-notebook/ipy-azure-linux-005.png)

Después de este paso, la pestaña Panel de **Extremos** será similar al que aparece en la siguiente captura de pantalla.

![Instantánea](./media/virtual-machines-python-ipython-notebook/ipy-azure-linux-006.png)

## Instalación del software necesario en la máquina virtual

Para ejecutar el bloc de notas de IPython en nuestra máquina virtual, primero tenemos que instalar IPython y sus dependencias.

### Linux (OpenSUSE)

Para instalar IPython y sus dependencias, conéctese con SSH en la máquina virtual Linux y complete los pasos siguientes.

Instale [NumPy][NumPy], [Matplotlib][Matplotlib], [Tornado][Tornado] y otras dependencias de IPython con los siguientes comandos.

    sudo zypper install python-matplotlib
    sudo zypper install python-tornado
    sudo zypper install python-jinja2
    sudo zypper install ipython

### Linux (Ubuntu)

Para instalar IPython y sus dependencias, conéctese con SSH en la máquina virtual Linux yrealice los pasos siguientes.

En primer lugar, recupere nuevas listas de paquetes con el comando siguiente.

    sudo apt-get update

Instale [NumPy][NumPy], [Matplotlib][Matplotlib], [Tornado][Tornado] y otras dependencias de IPython con los siguientes comandos.

    sudo apt-get install python-matplotlib
    sudo apt-get install python-tornado
    sudo apt-get install ipython
    sudo apt-get install ipython-notebook

### Windows

Para instalar IPython y sus dependencias en la máquina virtual de Windows, use el Escritorio remoto para conectarse a la máquina virtual. A continuación, realice los pasos siguientes, utilizando Windows PowerShell para ejecutar todas las acciones de la línea de comandos.

**Nota:** para realizar descargas con Internet Explorer, tendrá que cambiar algunas opciones de seguridad. En **Administrador de servidores**, haga clic en **Servidor local**, a continuación en **Configuración de seguridad mejorada de IE** y desactívelo para administradores. Puede volver a habilitarlo después de instalar IPython.

1.  Descargue e instale la última versión de 32 bits de [Python 2.7][]. Necesitará agregar `C:\Python27` y `C:\Python27\Scripts` a la variable de entorno `PATH`.

1.  Instale [Tornado][Tornado] y [PyZMQ][PyZMQ], y otras dependencias de IPython con los siguientes comandos.

        easy_install tornado
        easy_install pyzmq
        easy_install jinja2
        easy_install six
        easy_install python-dateutil
        easy_install pyparsing

1.  Descargue e instale [NumPy][NumPy] mediante el instalador binario `.exe` disponible en su sitio web. En el momento en que se redacta esto, la versión más reciente es numpy-1.9.1-win32-superpack-python2.7.exe.

1.  Instale [Matplotlib][Matplotlib] con el comando siguiente.

        pip install matplotlib==1.4.2

1.  Descargue e instale [OpenSSL][].

	* Encontrará Visual C++ 2008 Redistributable (necesario) en la misma página de descarga.

	* Necesitará agregar `C:\OpenSSL-Win32\bin` a su variable de entorno `PATH`.

> [AZURE.NOTE]Al instalar OpenSSL, use la versión 1.0.1g, o posterior, ya que incluyen una corrección para la vulnerabilidad de seguridad Heartbleed.

1.  Instale IPython con el comando siguiente.

        pip install ipython==2.4

1.  Abra un puerto en el Firewall de Windows. En Windows Server 2012, el firewall bloqueará de manera predeterminada las conexiones entrantes. Para abrir el puerto 9999, siga estos pasos:

    - Inicie **Firewall de Windows con seguridad avanzada** en la pantalla de inicio.

    - Haga clic en **Reglas de entrada** en el panel izquierdo.

	- Haga clic en **Nueva regla** en el panel Acciones.

	- En el Asistente para nueva regla de entrada, seleccione **Puerto**.

	- En la pantalla siguiente, seleccione **TCP** e introduzca **9999** en **Puertos locales específicos**.

	- Acepte los valores predeterminados, asigne un nombre a la regla y haga clic en **Finalizar**.

### Configuración del bloc de notas de IPython

A continuación, configuraremos el bloc de notas de IPython. El primer paso consiste en crear un perfil de configuración de IPython personalizado para encapsular la información de configuración con el siguiente comando.

    ipython profile create nbserver

A continuación, mediante el comando `cd`, cámbiese al directorio del perfil para crear nuestro certificado SSL y modifique el archivo de configuración de perfiles.

En Linux, use el comando siguiente.

    cd ~/.ipython/profile_nbserver/

En Windows, use el comando siguiente.

    cd \users\azureuser\.ipython\profile_nbserver

Utilice el comando siguiente para crear el certificado SSL (Linux y Windows).

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Tenga en cuenta que como estamos creando un certificado SSL autofirmado, al conectarse al bloc de notas el explorador le mostrará una advertencia de seguridad. En un uso de producción a largo plazo, querrá utilizar un certificado correctamente firmado asociado a la organización. Como la administración de certificados va más allá del ámbito de esta demostración, de momento nos ceñiremos a un certificado autofirmado.

Además de utilizar un certificado, también debe proporcionar una contraseña para proteger el bloc de notas de un uso no autorizado. Por motivos de seguridad, IPython utiliza contraseñas cifradas en su archivo de configuración; así pues, tendrá que cifrar primero la contraseña. IPython proporciona una utilidad para hacerlo; en el símbolo del sistema ejecute el comando siguiente.

    python -c "import IPython;print IPython.lib.passwd()"

Le solicitará una contraseña y su confirmación y, a continuación, imprimirá la contraseña de la forma siguiente.

    Enter password: 
    Verify password: 
    sha1:b86e933199ad:a02e9592e59723da722.. (elided the rest for security)
    
A continuación, modificaremos el archivo de configuración del perfil, que es el archivo `ipython_notebook_config.py` en el directorio de perfil en el que se encuentra. Tenga en cuenta que es posible que este archivo no exista; si es así, créelo. Estearchivo tiene varios campos que, de manera predeterminada, se convierten en comentario. Puede abrir este archivo con el editor de texto que prefiera y debe asegurase de que al menos tiene el contenido siguiente.

    c = get_config()
    
    # This starts plotting support always with matplotlib
    c.IPKernelApp.pylab = 'inline'
    
    # You must give the path to the certificate file.
    
    # If using a Linux VM:
    c.NotebookApp.certfile = u'/home/azureuser/.ipython/profile_nbserver/mycert.pem'
    
    # And if using a Windows VM:
    c.NotebookApp.certfile = r'C:\Users\azureuser\.ipython\profile_nbserver\mycert.pem'
    
    # Create your own password as indicated above
    c.NotebookApp.password = u'sha1:b86e933199ad:a02e9592e5 etc... '
    
    # Network and browser details. We use a fixed port (9999) so it matches
    # our Azure setup, where we've allowed traffic on that port
    
    c.NotebookApp.ip = '*'
    c.NotebookApp.port = 9999
    c.NotebookApp.open_browser = False

### Ejecución del bloc de notas de IPython

En este momento, estamos preparados para iniciar el bloc de notas de IPython. Para ello, vaya al directorio en el que desea almacenar los cuadernos e inicie el servidor de IPython Notebook con el siguiente comando.

    ipython notebook --profile=nbserver

Ahora debería poder obtener acceso al bloc de notas de IPython en la dirección `https://[Your Chosen Name Here].cloudapp.net`.

La primera vez que se accede a un cuaderno, la página de inicio de sesión pide una contraseña.

![Instantánea](./media/virtual-machines-python-ipython-notebook/ipy-notebook-001.png)

Y una vez haya iniciado sesión, verá el panel de control del bloc de notas de IPython, que es el centro de control para todas las operaciones del bloc de notas. Desde esta página se pueden crear cuadernos nuevos y abrir los existentes.

![Instantánea](./media/virtual-machines-python-ipython-notebook/ipy-notebook-002.png)

Si hace clic en el botón **New Notebook** (Nuevo cuaderno), verá la siguiente página de inicio.

![Instantánea](./media/virtual-machines-python-ipython-notebook/ipy-notebook-003.png)

El área marcada con el mensaje `In []:` es el área de entrada y ahí puede escribir cualquier código Python válido, que se ejecutará cuando presione `Shift-Enter` o haga clic en el icono "Play" (el triángulo que apunta a la derecha en la barra de herramientas).

Al haber configurado el cuaderno para que se inicie automáticamente con compatibilidad para NumPy y Matplotlib, incluso se pueden generar cifras, como se muestra en la siguiente captura de pantalla.

![Instantánea](./media/virtual-machines-python-ipython-notebook/ipy-notebook-004.png)

## Los documentos informáticos activos con medios enriquecidos son un potente paradigma.

El propio cuaderno le resultará muy familiar a cualquiera que haya utilizado Python y un procesador de textos, ya que, en cierto modo, es una mezcla de ambas cosas: permite ejecutar bloques de código de Python, pero también permite conservar notas y cualquier otro texto cambiando el estilo de una celda de "Code" a "Markdown" con el menú desplegable de la barra de herramientas.

![Instantánea](./media/virtual-machines-python-ipython-notebook/ipy-notebook-005.png)


Sin embargo, es mucho más que un procesador de texto, ya que el bloc de notas de IPython permite la combinación de informática y abundante contenido multimedia (texto, gráficos, vídeo y prácticamente todo lo que se pueda mostrar en cualquier explorador web moderno). Por ejemplo, permite combinar vídeos explicativos con cálculos con fines educativos.

![Instantánea](./media/virtual-machines-python-ipython-notebook/ipy-notebook-006.png)

O bien, como se muestra en la siguiente captura de pantalla, insertar sitios web externos que permanecen dinámicos y usables, en un archivo de cuaderno.

![Instantánea](./media/virtual-machines-python-ipython-notebook/ipy-notebook-007.png)

Y con la potencia de las gran cantidad de excelentes bibliotecas de Python para la informática técnica y científica, en la siguiente captura de pantalla, se puede realizar un cálculo simple con la misma facilidad que un análisis de red complejo, todo ello en un único entorno.

![Instantánea](./media/virtual-machines-python-ipython-notebook/ipy-notebook-008.png)

Este paradigma de combinar la potencia de la web moderna con la informática activa ofrece muchas posibilidades, y es perfectamente adecuado para la nube; el bloc de notas se puede usar:

* Como bloc de dictado de cálculo para registrar el trabajo de exploración sobre un problema.

* Para compartir resultados con compañeros, ya sea en forma de cálculo "activo" o en formatos para impresión (HTML, PDF),

* Para distribuir y presentar materiales de formación dinámicos que impliquen cálculos, con el fin de que los estudiantes puedan experimentar inmediatamente con el código real, modificarlo y volver a ejecutarlo de manera interactiva.

* Para proporcionar "documentos ejecutables" que presentan los resultados de la investigación de manera que otras puedan reproducirlos, validarlos y ampliarlos inmediatamente.

* Como plataforma de informática de colaboración: varios usuarios pueden iniciar sesión en el mismo servidor de IPython Notebook para compartir una sesión de cálculo dinámica.



Si se dirige al [repositorio][] del código fuente de IPython, encontrará un directorio completo con ejemplos de blocs de notas que se pueden descargar para experimentar en su propia máquina virtual de IPython de Azure. Solo tiene que descargar los archivos `.ipynb` desde el sitio y cargarlos en el panel de la máquina virtual del bloc de notas de Azure (o descargarlos directamente en la máquina virtual).

## Conclusión

El bloc de notas de IPython ofrece una sólida interfaz para tener acceso de manera interactiva a la potencia del ecosistema Python en Azure. Abarca una amplia variedad de casos de uso que incluye la exploración simple y el aprendizaje de Python, análisis de datos y visualización, simulación e informática en paralelo. Los documentos del bloc de notas resultantes contienen un registro completo de los cálculos que se realizan y que se pueden compartir con otros usuarios de IPython. El bloc de notas de IPython se puede usar como una aplicación local, pero es perfectamente adecuada para las implementaciones de nube en Azure.

Las características centrales de IPython también están disponibles en Visual Studio mediante [Python Tools for Visual Studio][] (PTVS). PTVS es un complemento gratuito y de código abiertode Microsoft que convierte a Visual Studio unentorno de desarrollo avanzado de Python, que incluye un editor avanzado con la integración de IntelliSense, depuración,creación de perfiles y la informática en paralelo.




[Tornado]: http://www.tornadoweb.org/ "Tornado"
[PyZMQ]: https://github.com/zeromq/pyzmq "PyZMQ"
[NumPy]: http://www.numpy.org/ "NumPy"
[Matplotlib]: http://matplotlib.sourceforge.net/ "Matplotlib"

[portal-vm-windows]: /manage/windows/tutorials/virtual-machine-from-gallery/
[portal-vm-linux]: /manage/linux/tutorials/virtual-machine-from-gallery/

[repositorio]: https://github.com/ipython/ipython
[python Tools for visual studio]: http://aka.ms/ptvs

[Python 2.7]: http://www.python.org/download
[OpenSSL]: http://slproweb.com/products/Win32OpenSSL.html

<!---HONumber=August15_HO6-->