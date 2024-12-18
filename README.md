# API de Gestión de Usuarios y Peleadores de la UFC

Este proyecto es una API desarrollada en Laravel para la gestión de usuarios y peleadores de la UFC. Incluye funcionalidades para realizar operaciones CRUD sobre ambas entidades, utilizando programación moderna con Laravel.

---

## Instalación y Configuración

### Requisitos
- PHP >= 8.1
- Composer
- MySQL o cualquier otra base de datos compatible con Laravel
- Node.js y npm (opcional, si se utiliza Laravel Mix para frontend)

### Pasos para instalar

1. **Clonar el Repositorio**
```bash
git clone <url-del-repositorio>
cd <nombre-del-repositorio>
```

2. **Instalar Dependencias con Composer**
```bash
composer install
```

3. **Configurar el Archivo `.env`**
Copia el archivo de ejemplo y configúralo:
```bash
cp .env.example .env
```

Configura las variables de entorno en `.env`:
```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=nombre_de_base_de_datos
DB_USERNAME=usuario_mysql
DB_PASSWORD=contraseña_mysql
```

4. **Ejecutar Migraciones y Rellenar la Base de Datos**
```bash
php artisan migrate --seed
```

5. **Iniciar el Servidor de Desarrollo**
```bash
php artisan serve
```
El servidor estará disponible en `http://127.0.0.1:8000`.

6. **Verificar las Rutas Disponibles**
```bash
php artisan route:list
```

---

## Estructura del Proyecto

### Migraciones

#### Tabla: `users`
Campos principales:
- `id`
- `name`
- `email`
- `password`
- Timestamps (creado/actualizado)

#### Tabla: `fighters`
Campos principales:
- `id`
- `name`
- `nickname`
- `division`
- `wins`
- `losses`
- `knockouts`
- `submissions`
- `gender` (`Masculino` o `Femenino`)
- `image` (opcional)
- Timestamps (creado/actualizado)

### Modelos

#### Modelo: `User`
Gestión de usuarios, implementando autenticación con tokens API.

```php
class User extends Authenticatable
{
    use HasApiTokens, HasFactory, Notifiable;

    protected $fillable = ['name', 'email', 'password'];

    protected $hidden = ['password', 'remember_token'];
}
```

#### Modelo: `Fighter`
Gestión de peleadores, incluye estadísticas y detalles.

```php
class Fighter extends Model
{
    use HasFactory;

    protected $fillable = [
        'name', 'nickname', 'division', 'wins', 'losses',
        'knockouts', 'submissions', 'gender', 'image'
    ];
}
```

---

## Endpoints Disponibles

### Autenticación

| Método | Endpoint      | Descripción            |
|--------|---------------|------------------------|
| POST   | `/register`   | Registro de usuarios  |
| POST   | `/login`      | Inicio de sesión       |
| POST   | `/logout`     | Cierre de sesión       |

### Gestión de Usuarios

| Método | Endpoint      | Descripción                     |
|--------|---------------|---------------------------------|
| GET    | `/users`      | Listar usuarios                |
| POST   | `/users`      | Crear un nuevo usuario         |
| GET    | `/users/{id}` | Mostrar detalles de un usuario |
| PUT    | `/users/{id}` | Actualizar un usuario          |
| DELETE | `/users/{id}` | Eliminar un usuario            |

### Gestión de Peleadores

| Método | Endpoint          | Descripción                          |
|--------|-------------------|--------------------------------------|
| GET    | `/fighters`       | Listar peleadores                   |
| POST   | `/fighters`       | Crear un nuevo peleador             |
| GET    | `/fighters/{id}`  | Mostrar detalles de un peleador     |
| PUT    | `/fighters/{id}`  | Actualizar un peleador              |
| DELETE | `/fighters/{id}`  | Eliminar un peleador                |

---

## Controladores

### `FighterController`

Ejemplo de métodos implementados:

- **Listar Peleadores**:
```php
public function index()
{
    return response()->json(Fighter::all(), 200);
}
```

- **Crear un Nuevo Peleador**:
```php
public function store(Request $request)
{
    $data = $request->validate([
        'name' => 'required|string|max:255',
        'nickname' => 'required|string|max:255',
        'division' => 'required|string',
        'wins' => 'integer|min:0',
        'losses' => 'integer|min:0',
        'knockouts' => 'integer|min:0',
        'submissions' => 'integer|min:0',
        'gender' => 'required|in:Masculino,Femenino',
        'image' => 'nullable|url',
    ]);

    $fighter = Fighter::create($data);
    return response()->json($fighter, 201);
}
```

- **Actualizar un Peleador**:
```php
public function update(Request $request, Fighter $fighter)
{
    $data = $request->validate([
        'name' => 'string|max:255',
        'nickname' => 'string|max:255',
        'division' => 'string',
        'wins' => 'integer|min:0',
        'losses' => 'integer|min:0',
        'knockouts' => 'integer|min:0',
        'submissions' => 'integer|min:0',
        'gender' => 'in:Masculino,Femenino',
        'image' => 'url',
    ]);

    $fighter->update($data);
    return response()->json($fighter);
}
```

---

## Resultado Final

Con esta API puedes:
- Gestionar usuarios (registro, inicio de sesión y CRUD completo).
- Gestionar peleadores (listar, crear, actualizar y eliminar).

---

## Contribución
Si deseas contribuir a este proyecto, abre un **Pull Request** o envía un **Issue** con tus sugerencias.

---

## Licencia
Este proyecto está bajo la Licencia MIT. Consulta el archivo `LICENSE` para más detalles.
