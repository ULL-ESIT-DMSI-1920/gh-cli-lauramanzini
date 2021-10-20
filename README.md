# Práctica 3: gh-cli
**Laura Manzini**

**alu0101531700@ull.edu.es **

### 1. [Introducción](intro)
### 2. [gh create repo](create)
### 3. [gh delete repo](delete)
### 4. [gh alias](#alias)
### 5. 
### 6. 
### 7. 


<a name = "intro"><a>
### 1. Introducción

El comando `gh` es el comando de GitHub que se emplega en el terminal.

<a name = "create"><a>
### 2. gh create repo

En primero lugar para crear un repositorio sobre gitpod es necesario autenticarse ejecutendo el codigo 
`gh auth login`.

Será necesario generar un token  sobre en su perfil de GitHub y luege pegarlo sobre el terminal.

Un vez que nos encontramos en nuestro _workspace_ es posible crear un repositorio ejecutando el codigo `gh repo create ULL-ESIT-DMSI-1920/prueba-lauramanzini` se puede crear el repositorio.

![Create repo]()

En nuestro caso el repositorio será parte de la organización ULL-ESIT-DMSI-1920.

Para visualizar una lista de los repositorios que estan entre la organización se ejecuta `gh repo list ULL-ESIT-DMSI-1920`.

![Repo list]()

<a name = "delete"><a>
### 3. gh delete repo

Para eliminar un repositorio se puede utilizar el comando `gh api --help` para tener una lista de los _flags_.

![api help](/Imagenes/)

El flag `-X` nos dice que va a ser posible utilizar uno de los metodos (get/delete ...) para hacer cambios a los repositorios.

Se puede cons documentación para eliminar un repositorio se encuentra en [las referencias de los repositorios API](https://docs.github.com/en/rest/reference/repos) 

![documentación](/Imagenes)

Desde la documentación podemos ver como a través del comand

Ejecutando el codigo

```
curl \
  -X DELETE \
  -H "Accept: application/vnd.github.v3+json" \
  https://api.github.com/repos/ULL-ESIT-DMSI-1920/prueba-lauramanzini
```

El comando `curl` no va a funcionar por que no se encuentra la documentación necesaria para hacer el cambio.

Entoncés se utiliza el comando `gh api`:

```
gh api \
  -X DELETE \
  -H "Accept: application/vnd.github.v3+json" \
  https://api.github.com/repos/ULL-ESIT-DMSI-1920/prueba-lauramanzini
```

<a name = "alias"><a>
### 4. gh alias

El `gh alias set` se utiliza para semplificar y crear _shortcut_ para todos los comandos que se utilizan más frecuentemente.

Es importante explicitar todos los _arguments_ y también los _flags_ del comando de lo que se quiere hacer el alias.

El comando base que se utiliza para hacer un alias es el siguiente:

 `gh alias set <alias> <expansion> [flags]`

 


[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-f059dc9a6f8d3a56e377f745f24479a46679e63a5d9fe6f495e02850cd0d8118.svg)](https://classroom.github.com/online_ide?assignment_repo_id=6022596&assignment_repo_type=AssignmentRepo)
