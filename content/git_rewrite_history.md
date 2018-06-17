Title: Reescribir la historia de Git
Date: 2018-06-17
Modified: 2018-06-17
Category: Git
Tags: Git
Authors: Santiago Blanco
Summary: Reescribir la historia de Git con amend y rebase

# Reescribir la historia de Git

Para corregir los errores que podamos cometer a la hora de hacer nuestros commits, git nos ofrece, al menos, un par de mandatos. En esta entrada vamos a hablar del comunmente conocido como amend, que nos permite modificar el último commit y del rebase interactivo, que nos permite editar varios commits al mismo tiempo.

Es necesario decir que estos mandatos no deberían usarse, por norma general, con commits que ya se hayan subido al servidor remoto, evitando reescribir la historia compartida (algo problemático para todos aquellos que colaboren).

## Git ammend

Nos permitirá modificar el último commit. Si simplemente queremos modificar el mensaje del último commit, haremos:

    git commit --amend

Con este mandato se abrirá el editor por defecto configurado en vim para hacer los commits y podremos editar el mensaje. Si, además, hacemos algún cambio y los agregamos habremos incluido dichos cambios en el commit anterior.

## Git rebase

Para reescribir varios commits existe la posibilidad de hacer un rebase interactivo:

    git rebase --interactive <hash>

Después de ejecutar este mandato se abre el editor configurado por defecto con los commits ordenados por orden descendente desde el más antiguo hasta el más nuevo con la acción que se desea realizar delante del commit. En los comentarios se explican las posibles acciones que se pueden realizar.
Entre las acciones que se pueden realizar destacan:

* El cambio de orden de los commits
* La eliminación de los mismos
* La modificación de sus mensajes
* La fusión de varios commits en uno solo.
