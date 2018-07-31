# Visual Code Editor


## Configuración de extensiones


## Autocompletado

- Autocompletado de path:
  - Utilizamos plugin [Path Autocomplete](https://marketplace.visualstudio.com/items?itemName=ionutvmi.path-autocomplete)
  - Para que funcione en markdown, hay que cambiar la configuración (Code->Preferences->Settings)

```json
"path-autocomplete.triggerOutsideStrings": true
```

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