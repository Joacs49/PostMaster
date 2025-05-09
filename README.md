
📌 PostMaster
PostMaster es una API desarrollada en Laravel para la gestión eficiente de publicaciones, productos y usuarios. Incluye un sistema seguro de autenticación, así como un panel de administración (dashboard) para controlar el contenido subido, visualizar usuarios y realizar modificaciones.

🌟 Características principales:
Publicación de contenido: Subida y gestión de publicaciones y productos con imágenes.

Autenticación segura: Implementada con Laravel Sanctum para proteger el acceso a la API.

Subida de imágenes: Integración con la API de Imgur para almacenar y gestionar archivos multimedia.

Dashboard interactivo: Gestión centralizada de publicaciones, productos y usuarios.

Gestión de usuarios: Visualización y control de usuarios registrados.

Documentación automatizada: Uso de Swagger para documentar los endpoints de la API.

Código estructurado: Implementación del Patrón Repository para mantener un código limpio y escalable.

🚀 Tecnologías utilizadas:
Backend: Laravel

Autenticación: Laravel Sanctum

Subida de imágenes: API de Imgur

Documentación: Swagger

Patrón de diseño: Repository

Base de datos: MySQL

💡 Uso:
Realiza el login para acceder al dashboard.

Publica nuevos posts o productos desde el panel.

Administra usuarios y contenido de manera eficiente.

Consulta la documentación completa en Swagger para más detalles sobre los endpoints.

```shell
# Documentar API con Swagger
php artisan l5-swagger:generate
http://localhost:8000/api/documentation#
composer update darkaonline/l5-swagger
```

```shell
# Limpiar cache
php artisan route:clear
php artisan config:clear
php artisan cache:clear
php artisan optimize:clear
php artisan route:list
```

````shell
#Documentacion para utilizar la api de Imgur (subida de imagenes)

Se tiene que verificar el archivo php.ini y quitar los ; de este archivo: 'curl.cainfo = "D:/Projects/Laragon-installer/7.0-W64/etc/ssl/cacert.pem"' 
para tener activado el trafico https entre la api de imgur con la api de tami, despues verificar la ruta del archivo ´cacert.pem´, si esta en otra
ubicacion modifiquelo.

Creacion de cuenta cliente: https://api.imgur.com/oauth2/addclient
Documentacion de la API: https://apidocs.imgur.com/#de179b6a-3eda-4406-a8d7-1fb06c17cb9c
````

````shell
# Ejecutar las pruebas unitarias
php artisan migrate:fresh --seed --env=testing
php artisan migrate:fresh --env=testing
php artisan migrate --env=testing
php artisan test
````

````shell
# Verificacion de la db
php artisan tinker --env=testing
DB::connection()->getDatabaseName();
````

```shell
# Crear un nuevo modelo con todos los archivos asociados: controlador, factory, migración, seeder, request
php artisan make:model NuevoModelo --all

# Crear un nuevo middleware
php artisan make:middleware NuevoMiddleware
```

## Patrón Repository

El Patrón Repository se utiliza para separar la lógica de acceso a datos de la lógica de la aplicación, facilitando la mantenibilidad y pruebas del código.

## Implementación

* Crear una interfaz en app/Repositories/Contracts/BaseRepositoryInterface.php:

namespace App\Repositories\Contracts;

interface BaseRepositoryInterface {
    public function getAll();
    public function find($id);
    public function create(array $data);
    public function update(array $data, $id);
    public function delete($id);
}

* Implementar la interfaz en un repositorio concreto en app/Repositories/BaseRepository.php:

namespace App\Repositories;

use App\Repositories\Contracts\BaseRepositoryInterface;
use Illuminate\Database\Eloquent\Model;

class BaseRepository implements BaseRepositoryInterface {
    protected $model;
    
    public function __construct(Model $model) {
        $this->model = $model;
    }
    
    public function getAll() {
        return $this->model->all();
    }
    
    public function find($id) {
        return $this->model->find($id);
    }
    
    public function create(array $data) {
        return $this->model->create($data);
    }
    
    public function update(array $data, $id) {
        $record = $this->model->findOrFail($id);
        $record->update($data);
        return $record;
    }
    
    public function delete($id) {
        return $this->model->destroy($id);
    }
}

* Usar el repositorio en un controlador:

namespace App\Http\Controllers;

use App\Repositories\BaseRepository;
use App\Models\User;

class UserController extends Controller {
    protected $userRepository;
    
    public function __construct(BaseRepository $userRepository) {
        $this->userRepository = new BaseRepository(new User());
    }
    
    public function index() {
        return response()->json($this->userRepository->getAll());
    }
}

* Agregar al Provider

Para vincular la interface con el repository se tiene que agregar en el AppServiceProvider

namespace App\Providers;

use App\Repositories\BaseRepository;
use App\Repositories\BaseRepositoryInterface;

use Illuminate\Support\ServiceProvider;

class AppServiceProvider extends ServiceProvider
{
    /**
     * Register any application services.
     */
    public function register(): void
    {
        $this->app->bind(BaseRepositoryInterface::class, BaseRepository::class);
    }

    /**
     * Bootstrap any application services.
     */
    public function boot(): void
    {
        //
    }
}

* Beneficios

Separación de responsabilidades: Mejora la organización del código.

Facilita las pruebas: Se pueden realizar pruebas unitarias sin depender directamente de Eloquent.

Reutilización de código: Un repositorio puede usarse en múltiples partes de la aplicación.

## Recursos

* [Documentación de Sanctum](https://laravel.com/docs/12.x/sanctum#main-content)

* [Instalacion de Swagger](https://salim-hosen.medium.com/how-to-document-your-laravel-api-using-swagger-5480044bd860)


---

<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400" alt="Laravel Logo"></a></p>

<p align="center">
<a href="https://github.com/laravel/framework/actions"><img src="https://github.com/laravel/framework/workflows/tests/badge.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>

