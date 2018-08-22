# Configuración inicial



# Máquina Virtual 


## Acceso a máquina

- Máquina virtual Ubuntu mediante Vmware
  - Usuario: curso
  - pwd: P@ssw0rd


## Lista de aplicaciones instaladas

- Chrome
- Postman
- Guake
- Visual Code Editor


## Lista de paquetes instalados
- zsh & Oh-my-zsh
- git
- Docker CE y Docker Compose
- curl y wget
- nvm, node, avn
- chrome-gnome-shell



# Instalación de aplicaciones


## Instalación de Chrome

- Desde Ubuntu 18 no es necesario ejecutar apt update después de añadir el repo :-)

```bash
wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
sudo add-apt-repository "deb http://dl.google.com/linux/chrome/deb/ stable main"
sudo apt-get install google-chrome-stable
```


## Instalación de postman

```bash
snap install postman
```


## Instalación de guake

```bash
sudo apt install guake
```

- La versión 3.0.5 tiene un bug, por lo que al hacer exit se queda colgado. [Lo compilamos mejor](http://guake.readthedocs.io/en/latest/user/installing.html#install-from-source)


## Configuración de guake

- Cambiamos preferencias F1 para que se vea
- Cambiamos nivel de transparencia


## Configuración modo nocturno

- Ajustes -> Pantalla -> Se pueden ajustar los azules :-)


## Instalación de Visual Code Editor

- [Instrucciones en la web](https://code.visualstudio.com/docs/setup/linux):

  ```bash
  curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
  sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
  sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'
  sudo apt-get update
  sudo apt-get install code # or code-insiders
  ```

- Peviamente instalamos curl (y wget ya que estamos):

  ```bash
  sudo apt install curl wget
  ```

## Configuración gnome-shell

- Mediante web https://extensions.gnome.org/
- Instalamos extensión *Integración con GNOME Shell*
- Instalamos conector:
  ```
  sudo apt install chrome-gnome-shell
  ```
- Instalo aplicación [Alternate Tab](https://extensions.gnome.org/extension/15/alternatetab/)
  - Cambio de aplicación ALT+TAB sin agrupar por aplicación


# Instalación de paquetes


## Instalación de Docker CE

- [Ver documentación](https://docs.docker.com/v17.09/engine/installation/linux/docker-ce/ubuntu/#set-up-the-repository)

```bash
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-update
sudo apt-get install docker-ce
```

- Comprobamos que esté arrancado:
```bash
sudo systemctl status docker
```


## Configuración Docker CE

- Ejecutar el comando docker sin sudo:
```bash
sudo usermod -aG docker ${USER}
```


## Instalación Docker Compose

- [Ver documentación](https://docs.docker.com/compose/install/)

```bash
sudo curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

- Testear instalación:
```bash
$ docker-compose --version
docker-compose version 1.22.0, build 1719ceb
```


## Instalación de zsh

- Algunos prefieren fish
- Otros son fieles a bash
- Yo prefiero zsh:
  ```bash
  sudo apt install zsh
  chsh -s $(which zsh)
  ```


## Instalación de Oh-my-zsh

- [Ver instrucciones y configuración](https://ohmyz.sh/)

```bash
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

- Plugins añadidos: yarn npm node sudo web-search



# Instalación de node.js


## Tipos de instalación

- Desde el [sitio web de Node.js](https://nodejs.org/en/)
- Mediante un gestor de versiones:
  - Varias versiones de Node.js en la misma máquina
  - Evitar tener que hacer sudo cuando instalemos paquetes de forma global
    - Los paquetes globales se instalan para un único usuario y version de node
    - Los paquetes globales sirven para cualquier proyecto


## Gestores de versiones de node

- Los gestores de versiones más habituales son:

- [nvm](https://github.com/creationix/nvm) para Linux/Mac
- [nvm-windows](https://github.com/coreybutler/nvm-windows) para Windows (no tiene nada que ver con nvm)
- [n](https://github.com/tj/n) para Linux/Mac


## Instalación de nvm
- [Ver instrucciones](https://github.com/creationix/nvm)

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
```


## Configuración de nvm

- Añado en el .zshrc para que se ejecute:

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```


## Instalación node mediante nvm

- Ver versiones instaladas:
  ```
  nvm ls
  ```
- Instalar una versión de node (hasta ultimo patch):
  ```
  nvm install 8.11
  ```
- Instala la última versión lts:
  ```
  nvm install node --lts
```
- Instala la última versión disponible:
  ```
  nvm install node
  ```


## Seleccionar versión de node

- Usar una versión en particular:
  ```
  nvm use 8.11.3
  ```

- Usar una versión en particular al abrir shell:
  ```
  nvm alias default 8.11.3
  ```


## Seleccionar versión de node por proyecto

- Para un proyecto en particular, mediante un fichero .nvmrc

- Ojo, hay que ejecutar ```nvm use``` "a mano" para que lea la versión
  - Otra opción es modificar el .zshrc
  - Otra opción es instalar un paquete adicional: **avn**


## Instalación de avn
- [avn](https://github.com/wbyoung/avn) sirve para *detectar la versión de node de cada proyecto* y cambiar el ejecutable que se utiliza

- avn se instala mediante el gestor de paquetes
  ```bash
  $ npm install -g avn avn-nvm
  $ avn setup
  ```
- Es un poco lento, no del gusto de todos


## Configuración avn
- Creamos fichero *.node-versión* dentro de nuestro proyecto
  ```
  cd proyecto
  echo v10 >.node-version
  ```
- Cada vez que entremos en el directorio se activará la versión de node correspondiente
  - ¡Debe estar instalada!


## Linter para Node.js

- Eslint analiza el código y nos muestra errores.
- Eslint avisa de muchos errores, pero [solo arregla algunos](https://eslint.org/docs/rules/)
  - https://medium.com/@netczuk/your-last-eslint-config-9e35bace2f99
- Utilizaremos prettier 
  - Formateador de código
  - Se combina con eslint y arregla más errores


## Instalación y configuración de git

```bash
sudo apt install git
git config --global user.name "Your Name"
git config --global user.email "youremail@domain.com"
git config --global core.editor "vim"
```
- Comprobamos:
```
$ git config --list
```


## Configuración GitHub
- [Crear cuenta en GitHub](https://github.com/join?source=header-home) si no tienes
- Generar clave ssh y exportar pública a GitHub si no usas ssh en vez de https



# Editor de código


## Visual Code Editor

- Utilizaremos [Visual Code Editor](https://code.visualstudio.com/)
- Es un producto open source de Microsoft realizado mediante node.js ([electron](https://electronjs.org/))
- Tiene un buen debugger para node.js


## Vistazo general

- Emmet ya viene por defecto
- Visor markdown por defecto
- Integración con GitHub
- Debug integrado
- Gestor de extensiones integrado
  - [Extensiones online](https://marketplace.visualstudio.com/)


## Edición

- Archivo, Preferencias, Configuración:

  ```
  "editor.tabSize": 2
  ```


## Jest

[Jest](https://marketplace.visualstudio.com/items?itemName=Orta.vscode-jest): autocompletado para tests mediante Jest


## Chrome Debugger

- [Debug mediante Visual Studio de código web](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome)


## Sincronizar configuración entre equipos

- [Instalamos settings sync](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) para sincronizar configuración entre equipos


## Autocompletado

- Autocompletado de path:
  - Utilizamos plugin [Path Autocomplete](https://marketplace.visualstudio.com/items?itemName=ionutvmi.path-autocomplete)
  - Para que funcione en markdown, hay que cambiar la configuración (Archivo, Preferencias, Configuración)

```json
"path-autocomplete.triggerOutsideStrings": true
```


## Linter

- Utilizaremos [eslint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
- Tendremos que instalar posteriormente el propio linter y establecer su configuración 
  - En cada proyecto en particular


## Formateador de código 
- [prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)


## Markdown linter

- [markdownlint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint)
- Cambio la configuración del linter en mi proyecto mediante *.markdownlist.json*

```json
{
  "MD012": false
}
```


## Markdown
 
- No es necesario, Visual Studio tiene un visor de Markdown integrado
- [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one)


## Servidor Web
- Se instala y se pulsa luego en el botón Go Live (parte inferior).
- [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)


## [ExpressSnippet](https://marketplace.visualstudio.com/items?itemName=vladmrnv.expresssnippet)

- Para autocompletado con Express