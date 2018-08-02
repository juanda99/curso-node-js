# Configuración inicial



# Máquina Virtual 


## Acceso a máquina

- Máquina virtual Ubuntu mediante Vmware
  - Usuario: curso
  - pwd: P@ssw0rd
- Instalación Visual Studio Code


## Lista de software instalado

- Chrome
- Postman
- Guake
- Visual Code Editor


## Lista de paquetes instalados
- zsh & Oh-my-zsh
- git
- Docker CE
- curl y wget
- nvm, node, avn



# Instalación de software


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

- Peviamente instalamos curl:

  ```bash
  sudo apt install curl
  ```


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
```
sudo apt install zsh
chsh -s $(which zsh)
```


## Instalación de Oh-my-zsh

- [Ver instrucciones y configuración](https://ohmyz.sh/)

```bash
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

- Plugins añadidos: yarn npm node sudo web-search


## Instalación de git

```bash
sudo apt install git
git config --global user.name "Your Name"
git config --global user.email "youremail@domain.com"
```


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

## Instalación de node

```bash
➜  ~ nvm install node
Downloading and installing node v10.7.0...
Downloading https://nodejs.org/dist/v10.7.0/node-v10.7.0-linux-x64.tar.xz...
########################## 100,0%
Computing checksum with sha256sum
Checksums matched!
Now using node v10.7.0 (npm v6.1.0)
Creating default alias: default -> node (-> v10.7.0)

➜  ~ nvm install --lts
Installing latest LTS version.
Downloading and installing node v8.11.3...
Downloading https://nodejs.org/dist/v8.11.3/node-v8.11.3-linux-x64.tar.xz...
######################## 100,0%
Computing checksum with sha256sum
Checksums matched!
Now using node v8.11.3 (npm v5.6.0)
```


## Instalación de avn
- [avn sirve para detectar la versión de node de cada proyecto y cambiar el ejecutabla que se utiliza](https://github.com/wbyoung/avn)

- avn se instala mediante el gestor de paquetes
    - Tendremos que tener previamente alguna versión de node instalada.

```bash
$ npm install -g avn avn-nvm
$ avn setup
Now when you cd into a directory with a .node-version file, avn will automatically detect the change and use your installed version manager to switch to that version of node. What goes in your .node-version file? A semver version number corresponding to the version of Node.js that your project uses.
```

- Es un poco lento, no del gusto de todos


# Configuración de Visual Code Editor

## 

- Emmet ya viene por defecto
- Instalamos settings sync para sincronizar configuración entre equipos
- Ver extensiones: https://marketplace.visualstudio.com/



Jest: para tests https://marketplace.visualstudio.com/items?itemName=Orta.vscode-jest
https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome


## Edición

- Archivo, Preferencias, Configuración:

  ```
  "editor.tabSize": 2
  ```


## Autocompletado

- Autocompletado de path:
  - Utilizamos plugin [Path Autocomplete](https://marketplace.visualstudio.com/items?itemName=ionutvmi.path-autocomplete)
  - Para que funcione en markdown, hay que cambiar la configuración (Archivo, Preferencias, Configuración)

```json
"path-autocomplete.triggerOutsideStrings": true
```


## Formateador de código 
- [prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)


## Linter

- Utilizaremos [eslint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
- Tendremos que instalar posteriormente el propio linter y establecer su configuración 
    - En cada proyecto en particular


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