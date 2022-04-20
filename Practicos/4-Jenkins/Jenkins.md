1. Crear `docker-compose.yml`
2. `docker-compose up -d`
3. Ingresar a `localhost:8080` 
4. Averiguar contraseña ya sea desde los logs o desde el path indicado. 
```
docker-compose exec jenkins bash
cat /var/jenkins_home/secrets/initialAdminPassword 
```
5. Contraseña en `info.config`
6. Instalar plugins
7. Admin User nuevo
- lucas
- contraseña en `info.config`
- nombre completo lucas viera
- email
8. Crear proyecto de estilo libre
9. Construir ahora
10. Historico: Console Output OK
11. Agregar echo
12. Historico: Console Output OK
13. `docker exec -it -u root jenkins /bin/bash`
14. `apt-get install vim nano`
15. Agregar `sh /tmp/script.sh` 
16. Historico: Console Output OK
17. 

hecho hasta punto 2.5 inclusive