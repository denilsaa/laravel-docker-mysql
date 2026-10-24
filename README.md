# Laravel + Docker + MySQL — Inicio rápido

Este paquete permite crear un proyecto Laravel desde **una carpeta prácticamente vacía** usando Docker.

No necesitas instalar en Windows:

- PHP
- Composer
- Laravel Installer
- MySQL
- XAMPP / WAMP

Solo necesitas:

- Docker Desktop
- Visual Studio Code
- PowerShell

---

## 1. Extraer el ZIP

Extrae todos los archivos en una carpeta nueva, por ejemplo:

```text
C:\Users\TU_USUARIO\Desktop\mi_laravel
```

Al inicio debe contener:

```text
mi_laravel/
├── Dockerfile
├── compose.yaml
├── .dockerignore
└── README.md
```

Laravel todavía no tiene que existir. Docker lo creará automáticamente.

---

## 2. Abrir PowerShell en la carpeta

Ejemplo:

```powershell
cd "$HOME\Desktop\mi_laravel"
```

También puedes abrir la carpeta con VS Code:

```powershell
code .
```

---

## 3. Comprobar Docker

Ejecuta:

```powershell
docker --version
docker compose version
docker ps
```

Si `docker ps` aparece vacío, no es un error. Solo significa que todavía no hay contenedores ejecutándose.

---

## 4. Verificar la configuración

Ejecuta:

```powershell
docker compose config
```

Si muestra los servicios `app` y `mysql` sin errores, continúa.

---

## 5. Crear Laravel y levantar todo

Ejecuta:

```powershell
docker compose up -d --build
```

En la primera ejecución Docker hará automáticamente:

1. Construir el contenedor PHP.
2. Añadir Composer.
3. Descargar Laravel si todavía no existe.
4. Crear/configurar `.env`.
5. Levantar MySQL.
6. Esperar a que MySQL esté listo.
7. Ejecutar las migraciones.
8. Iniciar Laravel en el puerto 8000.

No necesitas ejecutar manualmente `composer create-project`.

---

## 6. Ver el proceso de instalación

Para mirar lo que está haciendo Laravel:

```powershell
docker compose logs -f app
```

Cuando aparezca que Laravel está ejecutándose en el puerto 8000, puedes salir de los logs con:

```text
Ctrl + C
```

Esto NO apaga los contenedores.

---

## 7. Comprobar que todo está activo

Ejecuta:

```powershell
docker compose ps
```

Debes ver:

```text
mi_laravel_app
mi_laravel_mysql
```

MySQL debería aparecer como `healthy`.

---

## 8. Abrir Laravel

En el navegador abre:

```text
http://localhost:8000
```

Si aparece Laravel, la instalación terminó correctamente.

---

## 9. Comprobar Laravel, PHP y Composer

Laravel:

```powershell
docker compose exec app php artisan --version
```

PHP:

```powershell
docker compose exec app php -v
```

Composer:

```powershell
docker compose exec app composer --version
```

---

## 10. Comprobar MySQL

Ejecuta:

```powershell
docker compose exec mysql mysql -ularavel -plaravel123 -e "SHOW DATABASES;"
```

Debes ver la base:

```text
laravel
```

---

## 11. Verificar las migraciones

```powershell
docker compose exec app php artisan migrate:status
```

Las migraciones iniciales deberían aparecer como ejecutadas.

---

# Uso diario

## Encender el proyecto

```powershell
docker compose up -d
```

Después abre:

```text
http://localhost:8000
```

## Apagar el proyecto

```powershell
docker compose down
```

Este comando conserva la base de datos.

## Reiniciar

```powershell
docker compose restart
```

---

# Comandos Laravel

Ejecutar migraciones:

```powershell
docker compose exec app php artisan migrate
```

Crear modelo y migración:

```powershell
docker compose exec app php artisan make:model Producto -m
```

Crear controlador:

```powershell
docker compose exec app php artisan make:controller ProductoController
```

Ver rutas:

```powershell
docker compose exec app php artisan route:list
```

Limpiar caché:

```powershell
docker compose exec app php artisan optimize:clear
```

---

# Comandos Composer

Instalar dependencias:

```powershell
docker compose exec app composer install
```

Instalar un paquete:

```powershell
docker compose exec app composer require NOMBRE_DEL_PAQUETE
```

---

# Ver errores / logs

Laravel:

```powershell
docker compose logs -f app
```

MySQL:

```powershell
docker compose logs -f mysql
```

Todos los servicios:

```powershell
docker compose logs
```

---

# IMPORTANTE: conservar o borrar la base de datos

Este comando es seguro y conserva MySQL:

```powershell
docker compose down
```

Este comando elimina también el volumen y BORRA la base de datos:

```powershell
docker compose down -v
```

Usa `-v` solamente cuando quieras reiniciar completamente la base.

---

# Si el puerto 8000 está ocupado

Abre `compose.yaml` y cambia:

```yaml
ports:
  - "8000:8000"
```

por ejemplo a:

```yaml
ports:
  - "8001:8000"
```

Después abre:

```text
http://localhost:8001
```

---

# Resumen rápido

Primera vez:

```powershell
docker --version
docker compose version
docker ps
docker compose config
docker compose up -d --build
docker compose logs -f app
```

Uso diario:

```powershell
docker compose up -d
```

Para terminar:

```powershell
docker compose down
```

Laravel:

```powershell
docker compose exec app php artisan
```

Composer:

```powershell
docker compose exec app composer
```
