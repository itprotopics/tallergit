

# Taller de comandos GIT

- [Taller de comandos GIT](#taller-de-comandos-git)
  - [Fundamentos de Git](#fundamentos-de-git)
    - [Git](#git)
    - [Clonar repositorios remotos](#clonar-repositorios-remotos)
    - [Agregar elementos al repositorio](#agregar-elementos-al-repositorio)
    - [Guardando cambios en el repositorio](#guardando-cambios-en-el-repositorio)
    - [Revisión del histórico de confirmaciones](#revisión-del-histórico-de-confirmaciones)
    - [Etiquetas](#etiquetas)
      - [Listar etiquetas](#listar-etiquetas)
      - [Crear etiquetas](#crear-etiquetas)
      - [Nomenclatura propuesta](#nomenclatura-propuesta)
      - [Etiquetas anotadas](#etiquetas-anotadas)
      - [Etiquetado tardío](#etiquetado-tardío)
      - [Publicación de las etiquetas](#publicación-de-las-etiquetas)
      - [Extraer una etiqueta](#extraer-una-etiqueta)
  - [Ramas](#ramas)
    - [Definición de ramas](#definición-de-ramas)
    - [Comandos básicos para ramificar y fusionar](#comandos-básicos-para-ramificar-y-fusionar)
      - [Crear una rama](#crear-una-rama)
      - [Fusionar una rama](#fusionar-una-rama)
    - [Estrategia de ramificación git-flow](#estrategia-de-ramificación-git-flow)
      - [Modelo de ramas (branching) de git-flow](#modelo-de-ramas-branching-de-git-flow)
      - [Ramas principales](#ramas-principales)
      - [Ramas de soporte](#ramas-de-soporte)
        - [Ramas feature](#ramas-feature)
        - [Ramas release y bugfix](#ramas-release-y-bugfix)
        - [Ramas hotfix](#ramas-hotfix)
    - [Estrategia de ramificación GitHub flow](#estrategia-de-ramificación-github-flow)
      - [Crear una rama](#crear-una-rama-1)
      - [Agregar confirmaciones (commits)](#agregar-confirmaciones-commits)
      - [Solicitar un pull request](#solicitar-un-pull-request)
      - [Discutir y revisar el código](#discutir-y-revisar-el-código)
      - [Desplegar](#desplegar)
      - [Fusionar](#fusionar)
  - [Referencias](#referencias)

## Fundamentos de Git

### Git


* Es un sistema de control de versiones distribuido desarrollado por Linus Torvalds en 2005.
* Existen varias implementaciones desplegados como servicios en la nube tal como GitHub, BitBucket o GitLab
* El Kernel de Linux es uno de los proyectos más importantes que usan Git como controlador de versiones.
* Facilita el trabajo colaborativo.

### Clonar repositorios remotos

Si se desea obtener una copia de un repositorio Git existente el comando que se usa  `git clone`.
Se puede clonar un repositorio con `git clone [url]`

```zsh
# Clonar un repositorio
$ git clone https://alavezman.visualstudio.com/tutorial-gitflow/_git/mspmc-catalogos

# Validar la rama actual (sólo debe existir la master y es la default)
$ git branch
```

### Agregar elementos al repositorio

```zsh
# Agregar un archivo integrarlo a la rama develop local

# Checar el estatus de git
$ git status

# Agregar el nuevo archivo al area de staging de git
$ git add <archivo>

# Checar estatus y validar que está en el area de staging
$ git status

# Realizar commit al repo local
$ git commit -m “Se agrega una nueva clase”

# Realizar una modificación y realizar un nuevo commit al repo local

# Publicar la rama develop en el repo origin (remoto)
$ git push origi develop
```

### Guardando cambios en el repositorio

### Revisión del histórico de confirmaciones

```zsh
$ git log --oneline --all --decorate --graph
$ git log
$ git log --pretty=oneline
```

### Etiquetas

Esta sección está basada completamente en el artículo de Internet [Pro Git, el libro oficial de Git](https://uniwebsidad.com/libros/pro-git "Title").

Git tiene la habilidad de etiquetar (tag) puntos específicos en la historia como importantes. Generalmente la gente usa esta funcionalidad para marcar puntos donde se ha lanzado alguna versión (v1.0, y así sucesivamente).

#### Listar etiquetas

```zsh
$ git tag
```

#### Crear etiquetas

Git usa dos tipos principales de etiquetas: ligeras y anotadas. Una etiqueta ligera es muy parecida a una rama que no cambia — un puntero a una confirmación específica —. Sin embargo, las etiquetas anotadas son almacenadas como objetos completos en la base de datos de Git. Tienen su propio checksum; contienen el nombre del etiquetador, correo electrónico y fecha; tienen un mensaje de etiquetado; y pueden estar firmadas y verificadas con GNU Privacy Guard (GPG). Generalmente se recomienda crear etiquetas anotadas para disponer de toda esta información.

#### Nomenclatura propuesta

Se sugiere que las etiquetas tengan la siguiente estructura.

`V2.3.1_YYYYMMDD`

Con esto, podemos identificar la fecha en la que ser genera la etiqueta y la versión del componente.


#### Etiquetas anotadas

Crear una etiqueta anotada en Git es sencillo. La forma más fácil es especificar `-a` al ejecutar el comando `tag`:

```zsh
$ git tag -a V2.3.1_20210218 -m 'Liberación a prod de la version 2.3.1'
$ git tag
```

El parámetro `-m` especifica el mensaje, el cual se almacena con la etiqueta. Si no se especifica un mensaje para la etiqueta anotada, Git lanza el editor predeterminado  para poder escribir dicho mensaje.

Se puede ver los datos de la etiqueta junto con la confirmación que fue etiquetada usando el comando git show:

```zsh
$ git show V2.3.1_20210218
```

#### Etiquetado tardío

Se puede incluso etiquetar confirmaciones después de avanzar sobre ellos. Supongamos que el historico de confirmaciones se parece a esto:

```zsh
$ git log --pretty=oneline

15027957951b64cf874c3557a0f3547bd83b3ff6 Merge branch 'experiment'
a6b4c97498bd301d84096da251c98a07c7723e65 beginning write support
0d52aaab4479697da7686c15f77a3d64d9165190 one more thing
6d52a271eda8725415634dd79daabbc4d9b6008e Merge branch 'experiment'
0b7434d86859cc7b8c3d5e1dddfed66ff742fcbc added a commit function
4682c3261057305bdd616e23b64b0857d832627b added a todo file
166ae0c4d3f420721acbb115cc33848dfcc2121a started write support
9fceb02d0ae598e95dc970b74767f19372d61af8 updated rakefile
964f16d36dfccde844893cac5b347e7b3d44abbc commit the todo
8a5cbc430f1a9c3d00faaeffd07798508422908a updated readme
```

Ahora, supongamos que olvidamos etiquetar el proyecto en v1.2, que estaba en la confirmación "updated rakefile". Se puede etiquetar en cualquier momento. Para etiquetar esa confirmación se especifica la el checksum de la confirmación (o una parte de la misma) al final del comando:

```zsh
$ git tag -a V1.2_20210201 9fceb02

# Ver que se ha etiquetado la confirmación:
$ git tag

$ git show V1.2_20210201
```

#### Publicación de las etiquetas

Por defecto, el comando git push no transfiere etiquetas a servidores remotos. Se tiene que enviarlas explicitamente a un servidor compartido después de haberlas creado. Este proceso es igual a compartir ramas remotas, por lo que se puede ejecutar `git push origin [tagname]`.


```zsh
# Publicar una etiqueta específica
$ git push origin V1.2_20210201

# Publicar todas las etiquetas que no estén ya en el servidor remoto
$ git push origin --tags
```
#### Extraer una etiqueta

En Git, no se puede sacar (check out) una etiqueta, pues no es algo que se pueda mover. Si se quiere colocar en el directorio de trabajo una versión del repositorio que coincida con alguna etiqueta, se debe crear una rama nueva a partir de dicha etiqueta:

```zsh
$ git checkout -b release/version1.2 V1.2_20210201
```

## Ramas

### Definición de ramas

### Comandos básicos para ramificar y fusionar

#### Crear una rama
```zsh
# Generar la rama develop desde la rama master
$ git checkout -b develop master

# Validar la rama actual (debe ser la rama develop)
$ git branch
```

#### Fusionar una rama

```zsh
# Realizar el merge con la rama master (¿Se supone que develop 
# es un release a producción? Aguas con esta suposición. Sólo se presenta como ejemplo de merging).

$ git checkout master

# Validar que la rama por defecto es master
$ git branch

# Validar el estado de las ramas actual
$ git branch -a -v

# Realizar el merging
$ git merge –-no-ff develop

# Publicar el mergin en origin
$ git push origin master
```

### Estrategia de ramificación git-flow

* Es un conjunto de extensiones para Git que proporcionan una abstracción de alto nivel para funciones específicas de Git
* Está basado en un post de [Vincent Driessen (A successful Git branching model)](https://nvie.com/posts/a-successful-git-branching-model/ "Title") donde se describe un mdelo de desarrollo basado en Git.
* Posteriormente se desarrolla un software publicado en GitHub en que implementaba la funcionalidad descrita en dicho post.



#### Modelo de ramas (branching) de git-flow

![Alt](/images/estrategiageneral-gitflow.png "Title")

#### Ramas principales 

El modelo de desarrollo está basado en 2 ramas continuas (nunca desaparecen) y generalmente a menos que así convenga por cuestiones del trabajo colaborativo sólo existen en el repositorio origin (remoto).

- `master`
- `develop`

#### Ramas de soporte

El modelo git-flow además de las ramas permantentes master y develop, también usa una serie de ramas de soporte. Estas ramas de soporte tienen como objetivo:

- Ayudar al desarrollo paralelo entre los miembros del equipo
- Dar seguimiento a las nuevas características desarrolladas
- Preparación de los releases a producción
- Gestión y resolución de errores.

Los tipos de ramas para este fin son:
- `Feature`
- `Release`
- `Hotfix`
- `Bugfix`

Cada una tiene un propósito determinado y un conjunto de reglas de uso: desde que ramas se pueden generar (branching) y hacia que ramas se deben fusionar  (merging).

##### Ramas feature

Este tipo de ramas se usan para el desarrollo de una nueva carecterística para un release futuro

- Se crean a partir de develop
- Se fusionan únicamamente a la rama develop
- Convención de nombre `feature/<caracteristica>` por ejemplo `feature/metodo-get`

![Alt](/images/ramas_feature.png "Title")

```zsh
# Crear la rama desde develop (Siempre asegurarse que se tiene la
# última versión de develop

$ git checkout develop
$ git pull
$ git checkout -b feature/metodo-get develop

# Checar la rama default (debe ser feature/metodo-get)

# Realizar varios commits al repositorio local

# Si se desea compartir la rama para trabajo colaborativo
$ git push origin feature/metodo-get

# Si es una rama colaborativa antes de hacer merge a develop
# hay que asegurarse de que están todos los cambios de todos los
# programadores
$ git pull

```
Una vez que el desarrollo del `feature` ha finalizado habrá que hacer la fusión con la rama `develop`

```zsh
# Hacer el merge hacia la rama develop (Asegurarse que se tiene la última versión de develop)

# Cambiarse a develop
$ git checkout develop

# Hacer un pull desde origin
$ git pull

# Hacer el merge de  feature/metodo-get hacia develop
$ git merge –no-ff feature/metodo-get

# Eliminar la rama feature/metodo-getk
$ git branch -d feature/metodo-get

# Publicar develop en origin
$ git push origin develop


# Si la rama fue colaborativa hay que eliminarla de origin
$ git push origin :feature/metodo-get
```

El parámetro `"--no-ff"` en la fusión de las dos ramas fuerza la creación de un nuevo commit aunque la fusión se pudiera haber hecho mediante fast-forward (si se hubiera llevado a cabo de forma recursiva este nuevo commit se crea siempre). Este commit permite en un futuro localizar donde se ha incorporado una determinada funcionalidad (un conjunto de commits) y poder revertirla

Al final de la fusión las ramas se verían de la siguiente forma.

![Alt](/images/ramas_feature_merge.png "Title")

##### Ramas release y bugfix

- Se usan para la preparación de un `release` a producción
- Se crean a partir de la rama `develop`
- Se fusionan con la rama `develop` y `master`
- Convención de nombre `release/<nombre-release>`
- Estan pensadas para llevar a cabo pequeños ajustes antes de su publicación
- Para la corrección de pequeños bugs y para la preparación de los elementos propios de un `release`

Al llevar a cabo estas tareas en la rama de release se libera la rama develop para poder recibir las nuevas funcionalidades que se publicarán en un release futuro y que, por lo tanto, no afectarán a la release que se está preparando en esta rama.

![Alt](/images/ramas_release_start.png "Title")

Si posterior a la creación de la rama release se crea una rama de tipo feature, esa nueva característica ya no será parte del release actual. (Siempre hay excepciones, recuerden que de acuerdo con el manifiesto para el desarrollo ágil uno de los principios dice: Aceptamos que los requisitos cambien, incluso en etapas tardías del desarrollo. Los procesos Ágiles aprovechan el cambio para proporcionar ventaja competitiva al cliente).

![Alt](/images/ramas_release_after.png "Title")

Las ramas de tipo release se crean a partir de la rama develop en el momento en el que las funcionalidades que se encuentran en la rama develop coinciden con las que se desean desplegar en ese release.

Suponiendo que la versión estable en producción es la 1.0 y se quiere publicar un nuevo release 1.2
(Aquí es importante apegarse a las reglas del versionamiento semántico https://semver.org/)

```zsh
# Generar la rama release/1.2 desde develop
$ git checkout develop
$ git pull

$ git checkout -b release/1.2 develop

# Realizar algunos cambios al código y realizar uno o varios commits
```
Esta rama generalmente se publica en origin para que los desarrolladores puedan colaborar en la estabilización del código que saldrá a producción.

Aunque en estricto sentido la metodología publicada en el artículo de  Vincent Driessen establece que por cada bug en la rama `release` los programadores deben generar una rama específicamente para corregir cada bug, esto puede ser demasiado engorroso toda vez que al estabilizar una versión son muchos los cambios que pudieran presentarse, como se puede observar en la siguiente figura.

![Alt](/images/ramas_bugfix.png "Title")

Un cambio que podemos aplicar en este punto para reducir el número de ramas creadas es crear una sola rama `bugfix` por cada rama `release`, de tal manera que si se quiere implementar trabajo colaborativo sobre dicha rama de bugfix ésta se debe publicar en `origin` y los desarrolladores hacer sus correcciones sobre esta rama.

Lo recomendable es que los programadores que están involucrados en el release hagan los cambios  en la rama `bugfix/<nombre-release> `y sus cambios sean igualmente publicados en la rama `release/<nombre-release>` mediante un .pull request para que, quien tenga que aprobar los cambios los pueda revisar antes de integrarla con la rama `release/<nombre-release>`

La verdad es que con la experiencia hemos visto que incluso es suficiente realizar todos los cambios de estabilización en la rama `release/<nombre-release>` sin necesidad de crear nuevas ramas de tipo `bugfix`, incluso en trabajo colaborativo.

- Al terminar de estabilizar el código en la rama `release` se debe fusionar a la rama `master` y etiquetarse. 
- Asimismo, debe fusionarse a la rama `develop` para integrar todos los cambios para nuevos releases.


![Alt](/images/ramas_release_merge.png "Title")

Si es una rama colaborativa realizar el pull para integrar los cambios de todos los programadores que estabilizaron el código en la rama.

```zsh
$ git pull

# Cambiarse a master y verifica que se tiene la ultima versión

$ git checkout master
$ git pull
$ git merge –no-ff release/1.2

# Generar el tag en master de la nueva versión
$ git tag -a V1.2_20210218 -m “Se libera version el día bla bla...”

# Publicar el cambio en origin
$ git push origin master

# Enviar el tag a origin
$ git push origin V1.2_20210218

# Cambiarse a la rama develop y verificar que se tiene la última versión
$ git checkout develop
$ git pull
$ git merge –no-ff release/1.2

# Publicar el cambio en origin
$ git push origin develop

# Eliminar la rama release/1.2
$ git branch -d release/1.2

# Si se trató de una rama colaborativa (existe en origin) hay que
# eliminarla

$ git push origin :release/1.2

# Checar el estatus de las ramas despúes del cambio
$ git branch -v -a
```


##### Ramas hotfix

Este tipo de ramas se usan para la resolución de errores en producción que deben resolverse de forma inmediata.

- Se crean a partir de la rama master
- Se fusionan con la rama develop y master
- Si en ese momento existiera una rama de tipo release también debe fusionarse con dicha rama
- Convención de nombre `hotfix/<nombre-release-hotfix>`
  

![Alt](/images/ramas_hotfix.png "Title")

```zsh
# Cambiarse al a rama master y asegurarse de tener la última versión
$ git checkout master
$ git pull


# Genera la rama hotfix/1.3.1 desde master
$ git checkout -b hotfix/1.3.1 master

# Corregir los cambios hacer el commit local 
```

En este caso, la rama no debería publicarse en el `origin`, un `hotfix` como tal debe ser posible corregirlo con un solo programador. Si un bug requiriera la colaboración de un equipo y por tanto se tuviera que publicar en `origin` entonces tal vez no se trate de un `hotfix` sino de incluir nueva funcionalidad, lo cual es mejor tratarlo como un nuevo `release` más que como un `hotfix`.

```zsh
# Realizar el merge a master
$ git checkout master
$ git pull

# Integrar el hotfix a master
$ git merge --no-ff hotfix/1.3.1

# Etiquetar la versión en master
$ git tag -a V1.3.1_20210218 -m “Correccón del bug .. el día bla bla”


# Publicar en origin
$ git push origin master

# Publicar el tag en origin
$ git push origin V1.3.1_20210218

# Realizar el merge a develop
$ git checkout develop
$ git pull

# Integrar el hotfix a develop
$ git merge --no-ff hotfix/1.3.1


# Publicar en origin
$ git push origin develop


# Eliminar la rama hotfix/1.3.1
$ git branch -d hotfix/1.3.1
```

### Estrategia de ramificación GitHub flow

El flujo de GitHub es un flujo de trabajo ligero basado en ramas que admite equipos y proyectos donde los despliegues se realizan con regularidad. 

#### Crear una rama

![Alt](/images/GitHub_1.png "Title")

Cuando crea una rama en el proyecto, se crea un entorno en el que puede probar nuevas ideas. Los cambios que se realizan en una rama no afectan a la rama principal, por lo que se puede experimentar y confirmar los cambios, con la seguridad de saber que dicha rama no se fusionará hasta que esté lista para ser revisada por alguien mas.

La ramificación es un concepto central en Git, y todo el flujo de GitHub se basa en él. Solo hay una regla: cualquier elemento de la rama master siempre se puede desplegar.

**Debido a esto, es extremadamente importante que la nueva rama se cree fuera de master cuando se trabaja en una función o una solución. El nombre de la rama debe ser descriptivo**.

#### Agregar confirmaciones (commits)

![Alt](/images/GitHub_2.png "Title")

Una vez que se haya creado su rama se pueden hacer cambios sobre ella. Siempre que se agrega, edita o elimina un archivo, se está realizando una confirmación a la rama. Este proceso de agregar confirmaciones realiza un seguimiento del progreso mientras se trabaja en una rama de funciones. (features)

Los commits crean un historial del trabajo que otros pueden seguir para comprender lo que se ha hecho y por qué. Cada confirmación tiene un **mensaje de confirmación asociado**, que es una descripción que explica por qué se realizó un cambio en particular. Además, cada confirmación se considera una unidad de cambio separada. Esto permite revertir los cambios si se encuentra un error o si se decide ir en una dirección diferente.

**Los mensajes de confirmación son muy importantes, especialmente porque Git rastrea los sus cambios y luego los muestra como confirmaciones una vez que se envían al servidor**. 

#### Solicitar un pull request

![Alt](/images/GitHub_3.png "Title")

Las solicitudes de extracción inician la discusión sobre las confirmaciones. Debido a que están estrechamente integrados con el repositorio de Git subyacente, cualquiera puede ver exactamente qué cambios se fusionarían si aceptaran la solicitud.

Se puede abrir una solicitud de extracción en cualquier momento durante el proceso de desarrollo: cuando tiene poco o ningún código, pero se desea compartir algunas capturas de código o ideas generales, cuando se está atascado y se necesita ayuda o consejo, o cuando se está listo para que alguien mas revise el trabajo.

Las solicitudes de extracción son útiles para contribuir a proyectos de código abierto y para administrar cambios en repositorios compartidos. Si está utilizando un modelo de bifurcación y extracción, las solicitudes de extracción brindan una forma de notificar a los encargados del mantenimiento del proyecto sobre los cambios que desea que consideren. Si está utilizando un modelo de repositorio compartido, las solicitudes de extracción ayudan a iniciar la revisión del código y la conversación sobre los cambios propuestos antes de que se fusionen en la rama principal.


#### Discutir y revisar el código

![Alt](/images/GitHub_4.png "Title")

Una vez que se ha abierto una pull request, la persona o el equipo que revisa los cambios puede tener preguntas o comentarios. Quizás el estilo de codificación no coincide con las pautas del proyecto, al cambio le faltan pruebas unitarias o tal vez todo se ve bien y en orden. Los pull request están diseñadas para fomentar este tipo de conversación.


#### Desplegar

![Alt](/images/GitHub_5.png "Title")

Se puede desplegar desde una rama para la prueba final en producción antes de fusionarse con main o incluso con otra rama por ejemplo QA.

Una vez que el pull request ha sido revisado y la rama pase las pruebas, se puede desplegar los cambios para verificarlos en producción o en un stage anterior. Si la rama causa problemas, puede revertirse desplegando la rama master existente en producción.

Los equipos pueden tener diferentes estrategias de despliegue. Para algunos, puede ser mejor implementar en un entorno de prueba especialmente provisto. Para otros, la implementación directamente en producción puede ser la mejor opción en función de los otros elementos de su flujo de trabajo.

#### Fusionar

![Alt](/images/GitHub_6.png "Title")

Cuando los cambios se han verificado en producción, es hora de fusionar el código en la rama principal.

Una vez fusionadas, los pull request conservan un registro de los cambios históricos en el código. Debido a que se pueden buscar, permiten que cualquiera retroceda en el tiempo para comprender por qué y cómo se tomó una decisión.


## Referencias

- Pro Git, el libro oficial de Git. Scott Chacon. Traducido por Juan Murua, Alejandro Fernández, Javier Eguiluz
https://uniwebsidad.com/libros/pro-git

- https://guides.github.com/introduction/flow/

- https://nvie.com/posts/a-successful-git-branching-model/
