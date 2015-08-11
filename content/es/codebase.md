## I. Base de código
### Una base de código bajo control de versiones, múltiples despliegues

Una aplicación twelve-factor está siempre mantenida bajo un sistema de control de versiones, como [Git](http://git-scm.com/), [Mercurial](http://mercurial.selenic.com/), o [Subversion](http://subversion.apache.org/).  Una copia de la base de datos de control de versiones es conocido como *repositorio de código*, y se abrevia a menudo como *repositorio* o simplemente *repo*.

Una *base de código* es cualquier repositorio único (en un sistema de control de versiones centralizado como Subversion), o cualquier conjunto de repositorios que comparten un commit raíz (en un sistema de control de versiones decentralizado como Git).

![Una base de código para múltiples despliegues](/images/codebase-deploys.png)

Siempre hay una correlación uno a uno entre la base de código y la aplicación:

* Si hay múltiples bases de código, no se trata de una aplicación -- sino de un sistema distruido.  Cada componente en un sistema distribuido es una aplicación, y cada uno puede de forma individual cumplir con twelve-factor.
* Si hay múltiples aplicaciones que comparten el mismo código se trata de una violación de twelve-factor.  La solución aquí es factorizar el código compartido en librerías que puedan ser incluídas a mediante el [gestor de dependencias](./dependencies).

Solo hay una base de código por aplicación, pero habrán muchos despliegues de la aplicación.  Un *despliegue* es una instancia de la aplicación ejecutándose.  Normalmente esto es en un sitio de producción, y uno o más sitios de staging.  De forma adicional, cada desarrollador tiene una copia de la aplicación corriendo en su entorno de desarrollo local, cada uno de los cuales puede ser calificado también como despliegue.

La base de código es la mismo a lo largo de todos los despliegues, aunque distintas versiones pueden estar activas en cada despliegue.  Por ejemplo, un desarrollador tiene algunos commits que no ha desplegado todavía al entorno de staging; el entorno de staging tiene algunos commits que todavía no han sido desplegadas a producción.  Pero todos ellos comparten la misma base de código, haciéndolos de esta forma identificables como despliegues distintos de la misma aplicación.

