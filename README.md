# En esta actividad deberan crear un pipeline para hacer unos despliegues CI/CD de las apps deployada de la semana anterior en su respectivo servidor. 

1. El pipeline debe ejecutarse cada vez que haya un cambio en el repositorio, la rama main o master no debe deployar automaticamente, ni recibir ningún cambio, debe estar bloqueada por politica, igual la rama develop.
   
2. Se ejecutaran los pipelines de rama master o main por Pull Request de una rama con otro nombre.

3. Al hacer merge con master o main, se debe poder ejecutar el pipeline y hacer despliegue al servidor. Todo desde Jenkins en su local.

4. Subir la Imagen en Nexus y que esta sea deployada en el servidor donde esta la aplicación
