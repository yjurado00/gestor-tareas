# Gestor de Tareas

Aplicación web para gestionar tareas (To-Do) construida con Vue 3 + Laravel + PostgreSQL.

## Tecnologías
- Frontend: Vue 3, Vite, Vue Router
- Backend: Laravel 12, PHP 8.5
- Base de datos: PostgreSQL

## Requisitos
- PHP 8.1+
- Composer
- Node.js 18+
- PostgreSQL

## Instalación

### Backend
```bash
cd backend
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate --seed
php artisan serve
```

### Frontend
```bash
cd frontend
npm install
npm run dev
```

## Uso
- Backend corre en: http://localhost:8000
- Frontend corre en: http://localhost:5173

## Endpoints API
| Método | Endpoint | Acción |
|--------|----------|--------|
| GET | /api/tasks | Listar tareas |
| POST | /api/tasks | Crear tarea |
| PUT | /api/tasks/{id} | Actualizar tarea |
| DELETE | /api/tasks/{id} | Eliminar tarea |