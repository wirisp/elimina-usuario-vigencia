# elimina-usuario-vigencia
Script que elimina los usuarios de la base de datos ,de acuerdo al tiempo que le quiera poner de vigencia, por ejemplo, eliminar los usuarios que despues del primer inicio lleven 10 dias de vigencia.

- Te muestro la lista de tablas que tengo
MariaDB [radius]> show tables;
```
+------------------------+
| Tables_in_radius       |
+------------------------+
| batch_history          |
| billing_history        |
| billing_merchant       |
| billing_paypal         |
| billing_plans          |
| billing_plans_profiles |
| billing_rates          |
| cui                    |
| detalle                |
| diarias                |
| dictionary             |
| fichas                 |
| hotspots               |
| invoice                |
| invoice_items          |
| invoice_status         |
| invoice_type           |
| lotes                  |
| nas                    |
| node                   |
| operators              |
| operators_acl          |
| operators_acl_files    |
| payment                |
| payment_type           |
| proxys                 |
| radacct                |
| radcheck               |
| radgroupcheck          |
| radgroupreply          |
| radhuntgroup           |
| radippool              |
| radpostauth            |
| radreply               |
| radusergroup           |
| realms                 |
| resumen                |
| totales                |
| usadas                 |
| userbillinfo           |
| userinfo               |
| wimax                  |
+------------------------+
```
- Las tablas de las cuales elimino mis fichas son `radcheck,radacct,radusergroup,userinfo` pero tu puedes buscar en donde se encuentran los usuarios a eliminar,por ejemplo un usuario llamado `E8RQ` , entonces lo busco en mis tablas con;

```
SELECT username FROM radcheck WHERE username = 'E8RQ';
SELECT username FROM radacct WHERE username = 'E8RQ';
SELECT username FROM radusergroup WHERE username = 'E8RQ';
SELECT username FROM userinfo WHERE username = 'E8RQ';
```
o todas las tablas una por una, para asi en el script coloques las tablas donde se eliminara, en la fila 19 del script.

```
mysql -uroot -e "use radius; DELETE FROM radcheck WHERE username = '$USERNAME';"
mysql -uroot -e "use radius; DELETE FROM radacct WHERE username = '$USERNAME';"
mysql -uroot -e "use radius; DELETE FROM radusergroup WHERE username = '$USERNAME';"
mysql -uroot -e "use radius; DELETE FROM userinfo WHERE username = ‘$USERNAME';"
```
- Cambia en el script el password de la base de datos y los nombres de las tablas, tambien he filtrado las fichas en la tabla `usadas` por grupos

- Este script fue tomado de `https://aacable.wordpress.com/tag/radius-expired-user/` y modificado para acomodarlo a mis tablas.

- Respaldo de la db
```
mysqldump -p -u root radius > dbname.sql
```
- Restaurar db
```
mysql -p -u root radius < dbname.sql
```
