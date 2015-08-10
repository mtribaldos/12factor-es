## XII. Procesos de administración
### Ejecutar tareas de administración/gestión como procesos puntuales

La [formación de procesos](./concurrency) constituye el grupo de procesos que es utilizado para desempeñar las operaciones normales de la aplicación (tales como en el manejo de solicitudes web, por ejemplo) durante su ejecución. Separadamente, los desarrolladores, a menudo querrán hacer tareas administrativas o de mantenimiento puntuales sobre la aplicación, tales como:

* Ejecutar migraciones de base de datos (por ejemplo `manage.py migrate` en Django, `rake db:migrate` en Rails).
* Lanzar una consola (también llamado terminal [REPL](http://en.wikipedia.org/wiki/Read-eval-print_loop)) para ejecutar código arbitrario o inspeccionar los modelos de la aplicación contra la base de datos en funcionamiento. La mayoría de los lenguajes proporcionan una REPL ejecutando el intérprete sin argumentos (p. ej. `python` o `perl`) o en algunos casos tienen un comando aparte (p. ej. `irb` para Ruby, `rails console` para Rails).
* Ejecutar scripts puntuales guardados en el repositorio de la aplicación (p. ej. `php scripts/fix_bad_records.php`).

Los procesos de administración puntuales deberían ser lanzados en un entorno idéntico al habitual de los [procesos de larga vida](./processes) de la aplicación. Estos son ejecutados contra una [release](./build-release-run), usando la misma [base de código](./codebase) y [configuración](./config) que cualquier otro proceso ejecutado contra esta release.  El código de administración tiene que ser desplegado junto a código de aplicación con el fin de evitar problemas de sincronización.

La misma técnica de [desacoplamiento de dependencias](./dependencies) tiene que ser usada en todos los tipos de procesos.  Por ejemplo, si el proceso web Ruby usa el comando `bundle exec thin start`, entonces una migración de base de datos tiene que usar `bundle exec rake db:migrate`.  De la misma forma, un programa Python que use Virtualenv debería usar el comando `bin/python` para ejecutar tanto el servidor web Tornado como cada proceso de administración `manage.py`.

Twelve-factor prefiere sin lugar a dudas los lenguajes que proporcionan un shell REPL embebido, y que hacen fácil ejecutar scripts de forma puntual.  En un despliegue local, los desarrolladores llaman a procesos de administración puntuales mediante un comando directo de shell dentro del directorio de checkout de la aplicación.  En un despliegue de producción, los desarrolladores pueden usar ssh u otro mecanismo de ejecución de comandos provisto por ese entorno de ejecución de despliegue para lanzar tal proceso.
