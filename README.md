# Práctica 3: gh-cli
**Laura Manzini**

alu0101531700@ull.edu.es

1. [Introducción](intro)
2. [gh alias](#alias)
3. [alias gh create-repo](#create)
4. [alias gh delete-repo](#delete)
5. [Seleccionar las organizaciones a las que se apartenece](#selectorgs)
6. [gh org-list](#orglist)
7. [gh extension](#extension)




<a name = "intro"><a>
## 1. Introducción

El comando `gh` es el comando de GitHub que se emplega en el terminal.


<a name = "alias"><a>
## 2. gh alias

El comando `gh alias` se utiliza para semplificar y crear _shortcut_ para todos los comandos de gh que se utilizan más frecuentemente. La [documentación](https://cli.github.com/manual/gh_alias) explica como utilizar el comando.

Es importante explicitar todos los _arguments_ y también los _flags_ del comando de lo que se quiere hacer el alias.

El comando base que se utiliza para hacer un alias es el siguiente:

 `gh alias <comando> [flags]`

 Los comandos que son disponibles para el utilizo de alias son los siguientes:

 * set: para crear un nuevo alias
 * delete: para eliminar un alias ya existente
 * list: para enumerar los alias 

<a name = "create"><a>
## 3. alias gh create-repo

En primero lugar para crear un repositorio sobre gitpod es necesario autenticarse ejecutendo el código:
 
`gh auth login`

Será necesario generar un token sobre el perfil de GitHub y luego pegarlo sobre el terminal.

Un vez que nos encontramos en nuestro _workspace_ es posible crear un repositorio ejecutando el código:

`gh repo create org/repo`

Si quieremos por ejemplo crear un repositorio sobre la organizacion ULL-ESIT-DMSI-1920 ejecutamos el siguiente código:

 `gh repo create ULL-ESIT-DMSI-1920/prueba-lauramanzini`

![Create repo](Img2_create1.jpg)

Para visualizar una lista de los repositorios que estan entre la organización se ejecuta `gh repo list ULL-ESIT-DMSI-1920`.

![Repo list](Img2_view.jpg)

Para crear un comando alias que sea capaz de crear un repositorio sobre una organización que ya existe ejecutamo el comando `gh alias set` como sigue:

```
gh alias set repo-create 'repo create ULL-ESIT-DMSI-1920/$1'
gh repo-create prueba1
```

<a name = "delete"><a>
## 4. alias gh delete-repo

Para eliminar un repositorio se utiliza el comando `gh api` para tener una lista de los _flags_ que son disponibles para el comando.

![api help](/Img1_gh_api_help.jpg)

El flag `-X` nos dice que va a ser posible utilizar uno de los metodos (get,delete ...) para cambiar los repositorios.

Se puede consultar la documentación para eliminar un repositorio se encuentra en [las referencias de los repositorios API](https://docs.github.com/en/rest/reference/repos). 

Desde la documentación podemos ver como a través del comando `curl` es posible eliminar el repositorio.

Imaginamos de tener un repositorio llamado `prueba-lauramanzini` en la organización `ULL-ESIT-DMSI-1920`. La documentación indica que utilizando el siguiente código será posible eliminar el dicho repositorio:

```
curl \
  -X DELETE \
  -H "Accept: application/vnd.github.v3+json" \
  https://api.github.com/repos/ULL-ESIT-DMSI-1920/prueba-lauramanzini
```

El comando `curl` no va a funcionar por que no se encuentra la documentación necesaria para hacer el cambio. De otra manera se utiliza el comando `gh api`:

```
gh api \
  -X DELETE \
  -H "Accept: application/vnd.github.v3+json" \
  https://api.github.com/repos/ULL-ESIT-DMSI-1920/prueba-lauramanzini
```

![delete repo](/Img4_delete_repo.jpg)

Para crear un comando alias que pueda eliminar un repositorio seleccionado en una organización utilizamos el comando `gh alias set`.

`gh alias set repo-delete 'api \-X DELETE "/repos/org/$1"`

*Nota*:  Cuando ejecutamos el comando repo-delete será necesario explicitar la organización y también el nombre del repositorio que queremos eliminare 

<a name = "selectorgs"><a>
## 5. Seleccionar las organizaciones a las que se apartenece

Para ver todas las organizaciones a las que pertenezco utilizo el comando  se consulta la [documentación de GitHub](https://docs.github.com/en/rest/reference/orgs#list-organization-memberships-for-the-authenticated-user) y se encuentra el comando `gh api  /user/memberships/orgs` que nos permite de obtener una lista de todas las afiliaciones a organizaciones.

Añadendo el comando `| jq` al final se puede acceder al file .json con toda la información sobre las organizaciones a las que el mi perfil de GitHub está asociado.

El file json que obtenimos es un file que es muy largo y a veces bastante dificil de entender. Para solucionar este problema se utiliza el comando `--paginate` realice solicitudes HTTP adicionales para obtener todas las páginas de resultados. Ejecutamos:

 `gh api --paginate /user/memberships/orgs | jq`.

![gh api paginate](/Img6_gh_memberships_jq.jpg)

Para obtener las informacciones que están contenida en el file json que hemos obtenido es necesario instalar el [jq json queries](https://stedolan.github.io/jq/). Esta herramienta nos permite de acceder a la información contenida en qualquier file json.

Los comandos más utilizados se pueden consultar al siguiente [enlace](https://stedolan.github.io/jq/manual/#Basicfilters).

Después de instalar la herramienta se ejecuta el código `brew install jq` sobre GitPod:

![jq install](/Img7_jq_install.jpg)

El file json es un array de elementos y para acceder a esos se utiliza la siguiente reglas:

* Se utilizan las comillas simples '' para indicar los elementos del file json que queremos ver
* En los corchetes ponemos la posicción del elemento del array que queremos seleccionar
* Despues de los corchetes ponemos un punto `.` cada vez que queremos acceder a uno de los elementos del elemento del array seleccionado el los corchetes.

Por ejemplo ejecutando el código `gh api /user/memberships/orgs | jq '.[0]'` obtenimos toda la información relativa el primero elemento del array, es decir el elemento que es en la posicción 0 del array.

Una otra manera de obtener las misma informaciones pero en formato diferente es ejecutando el código:
 `gh api /user/memberships/orgs --jq '.[0]'`

Para acceder a las organizaciones a las que apartenezco es necesario acceder en primero al campo *organizations* y luego al campo *login* de lo mismo. Se ejecuta:

 `gh api /user/memberships/orgs jq '.[].organization.login'`.

![Organizations list](/Img8_organizations_login.jpg)

<a name = "orgslist"><a>
## 6. orgs-list

El objectivo ahora es crear un comando alias que nos permite de acceder a las organizaciones. El comando que se ejecuta para obtener esta información es:

 `gh api /user/memberships/orgs | jq '.[].organization | .login, .url'`

![organization login y url](/Img9_login_url.jpg)

Una vez que hemos encontrado el comando para obtener la información solicitada podemos creare el comando **orgs-list** ejecutando el código:

 `gh alias set orgs-list "api /user/memberships/orgs | jq '.[].organization | .login, .url'"`

![orgs-list alias](/Img9_alias_orgs-list.jpg)

<a name = "extension"><a>
## 7. gh extension

Una gh extension es una extensión de un comando de GitHub CLI que una vez instalada o creada se puede utilizar.

El comando `gh extension` tiene 5 comandos:

*  create: para crear una nueva gh extension
*  install: para instalar una gh extension desde un repositorio que ya existe
*  list: muestra la lista de todos los gh extension que se han instalado
*  remove: quita una gh extension ya instalada
*  upgrade: hace un upgrade de una gh extension ya instalada

Para major información se consulte la [documentación de GitHub](https://docs.github.com/en/github-cli/github-cli/creating-github-cli-extensions).

La extension que creo es `gh-repo-view`. 

Se utiliza el metodo `gh extension create` para crear una extension del comando que será llamado `gh-repo-view` ejecutando el codigo:

```
gh extension create gh-repo-view
cd gh-repo-view
gh repo create --public ULL-ESIT-DMSI-1920/gh-repo-view
```
Ahora tenemos un repositorio creado en la organización ULL-ESIT-DMSI-1920 que es llamado gh-repo-view.

Se puede también hacer un commit sobre este repositorio de la siguiente manera:

```
git commit -am 'Primer prueba de laura'
git push --set-upstream origin master
```

Una vez que hemos creado el repositorio en la organización es necesario crear un submodulo entre el repositorio que tenemos con el comando `git submodule add <URL>`. 

Ejecutamos entoncés en la carpeta gh-cli-lauramanzini el siguiente codigo:

`git submodule add https://github.com/ULL-ESIT-DMSI-1920/gh-repo-view.git`

Ahora a través del comando `ls -la` es posible comprobar la presencia del fichero `.gitsubmodules`. Es todavía necesario hacer un commit de los cambio que se han aportado al repositorio. Ejecutamos:

```
git commit -m Commit16
git push
```


[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-f059dc9a6f8d3a56e377f745f24479a46679e63a5d9fe6f495e02850cd0d8118.svg)](https://classroom.github.com/online_ide?assignment_repo_id=6022596&assignment_repo_type=AssignmentRepo)
