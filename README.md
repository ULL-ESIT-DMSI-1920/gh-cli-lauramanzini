# Práctica 3: gh-cli
**Laura Manzini**

alu0101531700@ull.edu.es

 1. [Introducción](intro)
 2. [gh create repo](create)
 3. [gh delete repo](delete)
 4. [gh alias](#alias)
  4.1 [gh alias set](#aliasset)
  4.2 [gh alias list](#aliaslist)
  4.3 [gh alias delete](#aliasdelete)



<a name = "intro"><a>
## 1. Introducción

El comando `gh` es el comando de GitHub que se emplega en el terminal.

<a name = "create"><a>
## 2. gh create repo

En primero lugar para crear un repositorio sobre gitpod es necesario autenticarse ejecutendo el codigo 
`gh auth login`.

Será necesario generar un token  sobre en su perfil de GitHub y luege pegarlo sobre el terminal.

Un vez que nos encontramos en nuestro _workspace_ es posible crear un repositorio ejecutando el codigo `gh repo create ULL-ESIT-DMSI-1920/prueba-lauramanzini` se puede crear el repositorio.

![Create repo]()

En nuestro caso el repositorio será parte de la organización ULL-ESIT-DMSI-1920.

Para visualizar una lista de los repositorios que estan entre la organización se ejecuta `gh repo list ULL-ESIT-DMSI-1920`.

![Repo list]()

<a name = "delete"><a>
## 3. gh delete repo

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
## 4. gh alias

El `gh alias set` se utiliza para semplificar y crear _shortcut_ para todos los comandos que se utilizan más frecuentemente de gh. La [documentación](https://cli.github.com/manual/gh_alias) explica como utilizar el comando.

Es importante explicitar todos los _arguments_ y también los _flags_ del comando de lo que se quiere hacer el alias.

El comando base que se utiliza para hacer un alias es el siguiente:

 `gh alias <comando> [flags]`

 Los comandos que son disponibles para el utilizo de alias son los siguientes:

 * delete
 * list
 * set

 Se puede requisir la lista compleda de todos los argomentos ejecutando `gh alias <comando> --help`

<a name = "aliasset"><a>
 ## 4.1 alias set

El comando de alias `set` permite que crear un nuevo alias. Creamos por ejemplo para el comando `gh repo list`. En este caso el alias que utilizaremos serà `list`.

![alias create](/Imagenes/Img7_alias_set)

Se nota cómo **no** se pone el comando *gh* entre los ápices del comando que nos quieremos hacer el alias. 

Si no

<a name = "aliaslist"><a>
 ## 4.2 alias list

Para obtener una lista de los alias que se han credo se utiliza el comando `gh alias list`.

![alias list](/Imagenes/Img_alias_list)

<a name = "aliasdelete"><a>
 ## 4.2 alias delete

 Para eliminar un alias se utiliza el comando `gh alias delete <alias>`

![alias delete](/Imagenes/Img_alias_delete)

[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-f059dc9a6f8d3a56e377f745f24479a46679e63a5d9fe6f495e02850cd0d8118.svg)](https://classroom.github.com/online_ide?assignment_repo_id=6022596&assignment_repo_type=AssignmentRepo)
