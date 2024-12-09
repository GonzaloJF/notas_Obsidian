Aquí tienes una lista de los comandos básicos de Git que te ayudarán a manejar archivos, hacer commits, subir cambios a un repositorio remoto, y más.
1. Inicializar un Repositorio

bash

``git init``

    Crea un nuevo repositorio de Git en el directorio actual.

2. Clonar un Repositorio

bash

``git clone https://github.com/usuario/repositorio.git``

    Clona un repositorio remoto a tu máquina local.

3. Agregar Archivos al Índice (Staging Area)

bash

``git add archivo.txt``

    Añade un archivo específico al área de staging.

bash

``git add .``

    Añade todos los archivos y cambios en el directorio actual al área de staging.

4. Hacer un Commit

bash

``git commit -m "Mensaje del commit"``

    Crea un commit con los archivos que han sido añadidos al área de staging. El mensaje debería describir los cambios realizados.

5. Eliminar Archivos

bash

``git rm archivo.txt``

    Elimina un archivo del repositorio y del sistema de archivos.

bash

``git rm --cached archivo.txt``

    Elimina un archivo del repositorio, pero lo deja en el sistema de archivos. Esto es útil si quieres dejar de seguir un archivo con Git.

6. Renombrar o Mover Archivos

bash

``git mv archivo_antiguo.txt archivo_nuevo.txt``

    Renombra o mueve un archivo.

7. Ver el Estado del Repositorio

bash

``git status``

    Muestra el estado de los archivos en tu repositorio: cambios que están en el área de staging, archivos modificados, archivos no seguidos, etc.

8. Ver el Historial de Commits

bash

``git log``

    Muestra el historial de commits. Puedes usar git log --oneline para una vista más compacta.

9. Revertir Cambios

    Antes del Staging (Cambios locales):

    bash

``git checkout -- archivo.txt``

    Deshace los cambios en un archivo antes de haberlo agregado al área de staging.

Después del Staging:

bash

``git reset HEAD archivo.txt``

    Elimina un archivo del área de staging pero deja los cambios en el archivo en tu directorio de trabajo.

Revertir un Commit:

bash

    git revert <hash_del_commit>

        Crea un nuevo commit que deshace los cambios de un commit anterior. El <hash_del_commit> lo obtienes con git log.

10. Actualizar el Repositorio Local

bash

``git pull``

    Actualiza tu repositorio local con los últimos cambios del repositorio remoto.

11. Subir Cambios a un Repositorio Remoto

bash

``git push origin main``

    Sube tus commits al repositorio remoto. Reemplaza main con la rama en la que estás trabajando si es diferente.

12. Crear y Cambiar de Ramas

    Crear una Nueva Rama:

    bash

``git branch nombre-de-la-rama

    Crea una nueva rama con el nombre que especifiques.

Cambiar de Rama:

bash

``git checkout nombre-de-la-rama

    Cambia a una rama existente.

Crear y Cambiar a una Nueva Rama en un Solo Comando:

bash

    git checkout -b nombre-de-la-rama

13. Fusionar Ramas (Merge)

bash

``git merge nombre-de-la-rama

    Fusiona la rama especificada con la rama en la que te encuentras actualmente.

14. Eliminar una Rama

    En Local:

    bash

``git branch -d nombre-de-la-rama

    Elimina una rama local.

En Remoto:

bash

    git push origin --delete nombre-de-la-rama

        Elimina una rama en el repositorio remoto.

15. Configurar un Repositorio Remoto

bash

``git remote add origin https://github.com/usuario/repositorio.git

    Asocia tu repositorio local con un repositorio remoto.
para generar la ssh-keygen

ssh-keygen -t ed25519 -C "tuemail@example.com"