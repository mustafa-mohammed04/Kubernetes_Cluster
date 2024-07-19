``` yaml

version: "3"
services:
  mysql_database:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: 1234
      MYSQL_DATABASE: wp_db
      MYSQL_USER: wp_depi
      MYSQL_PASSWORD: 1234
    volumes:
      - mysql_data:/var/lib/mysql

  wordpress:
    depends_on:
      - mysql_database
    image: wordpress:latest
    restart: always
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: mysql_database:3306
      WORDPRESS_DB_USER: wp_depi
      WORDPRESS_DB_PASSWORD: 1234
      WORDPRESS_DB_NAME: wp_db
    volumes:
      - wordpress_data:/var/www/html

volumes:
  mysql_data: {}
  wordpress_data: {}

```
