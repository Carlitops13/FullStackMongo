# FullStackMongo - Sistema de Autenticación Completo

Un sistema completo de autenticación Full Stack construido con React, Express y MongoDB. Incluye registro de usuarios, verificación de email, recuperación de contraseña y autenticación segura con JWT.

##  Características Principales

###  Autenticación y Seguridad
- **Registro de usuarios** - Creación de nuevas cuentas con validación
- **Verificación de email** - Confirmación de correo electrónico mediante token JWT
- **Recuperación de contraseña** - Solicitud de recuperación con token temporal
- **Reset de contraseña** - Cambio seguro de contraseña con token de tiempo limitado
- **Hashing de contraseñas** - Utiliza bcryptjs para encriptar contraseñas
- **Tokens JWT** - Autenticación basada en JSON Web Tokens

###  Base de Datos
- **MongoDB** - Base de datos NoSQL para almacenar usuarios
- **Mongoose** - ODM para modelado de datos

###  Frontend
- **React 19** - Framework UI moderno con hooks
- **React Router 7** - Enrutamiento del lado del cliente
- **Vite** - Build tool rápido y moderno
- **Axios** - Cliente HTTP para comunicarse con la API

### Backend
- **Express 5** - Framework web rápido y minimalista
- **Node.js** - Runtime JavaScript del lado del servidor
- **Nodemailer** - Envío de correos electrónicos (opcional)
- **CORS** - Manejo de solicitudes entre dominios

---

##  Estructura del Proyecto

```
FormularioFullStack/
├── backend/                          # Servidor Express y API
│   ├── controllers/
│   │   └── auth.controller.js        # Lógica de autenticación
│   ├── models/
│   │   └── User.js                   # Esquema de usuario en MongoDB
│   ├── routes/
│   │   └── auth.routes.js            # Rutas de autenticación
│   ├── server.js                     # Configuración principal del servidor
│   ├── package.json                  # Dependencias del backend
│   └── .env                          # Variables de entorno (no versionado)
│
├── frontend/                         # Aplicación React + Vite
│   ├── src/
│   │   ├── pages/
│   │   │   ├── Register.jsx          # Página de registro
│   │   │   ├── VerifyAccount.jsx     # Página de verificación de email
│   │   │   ├── ForgotPassword.jsx    # Página de recuperación
│   │   │   └── ResetPassword.jsx     # Página de reset de contraseña
│   │   ├── services/
│   │   │   └── authService.js        # Cliente API para autenticación
│   │   ├── App.jsx                   # Componente raíz con rutas
│   │   ├── App.css                   # Estilos de la aplicación
│   │   ├── index.css                 # Estilos globales
│   │   └── main.jsx                  # Punto de entrada
│   ├── package.json                  # Dependencias del frontend
│   ├── vite.config.js               # Configuración de Vite
│   ├── eslint.config.js             # Configuración de ESLint
│   └── index.html                    # HTML principal
│
└── README.md                         # Este archivo
```

---

##  Instalación y Configuración

### Requisitos Previos
- **Node.js** (v14 o superior)
- **npm** o **yarn**
- **MongoDB** (local o Atlas)
- **Git**

### 1️⃣ Backend

#### Instalación
```bash
cd backend
npm install
```

#### Variables de Entorno
Crea un archivo `.env` en la carpeta `backend`:

```env
MONGO_URI=mongodb+srv://usuario:contraseña@cluster.mongodb.net/nombreDB
JWT_SECRET=tu_secreto_super_seguro_aqui
PORT=3000
```

#### Ejecución
```bash
# Modo desarrollo (con nodemon)
npm run dev

# Modo producción
npm start
```

El servidor se ejecutará en `http://localhost:3000`

### 2️⃣ Frontend

#### Instalación
```bash
cd frontend
npm install
```

#### Ejecución
```bash
# Modo desarrollo (con Vite)
npm run dev

# Build para producción
npm run build

# Vista previa de producción
npm preview
```

La aplicación se ejecutará en `http://localhost:5173` (por defecto con Vite)

---

##  Flujo de Autenticación

###  Registro
1. Usuario ingresa nombre, email y contraseña
2. Backend valida los datos
3. Se verifica que el email no esté registrado
4. Contraseña se hashea con bcryptjs
5. Se genera un token de verificación JWT (24h)
6. Usuario se guarda en MongoDB
7. Frontend recibe token para verificación

###  Verificación de Email
1. Usuario recibe enlace con token (simula email)
2. Accede a `/verify/:token`
3. Backend valida el token JWT
4. Si es válido, marca `isVerified: true`
5. Usuario puede ahora usar todas las funcionalidades

###  Recuperación de Contraseña
1. Usuario ingresa su email en `/forgot-password`
2. Backend busca el usuario por email
3. Si existe, genera token de reseteo (15min)
4. Retorna el token al frontend (en desarrollo)
5. Usuario puede proceder al reset

###  Reset de Contraseña
1. Usuario accede a `/reset-password/:token`
2. Ingresa la nueva contraseña
3. Backend valida el token
4. Contraseña nueva se hashea
5. `resetToken` se limpia en la BD
6. Usuario redirigido a registro

---

##  API Endpoints

### Base URL
`http://localhost:3000/api/auth`

### Endpoints

####  POST `/register`
Registra un nuevo usuario.

**Request Body:**
```json
{
  "name": "Juan Pérez",
  "email": "juan@example.com",
  "password": "miContraseña123"
}
```

**Response (201):**
```json
{
  "message": "Usuario registrado correctamente",
  "userId": "507f1f77bcf86cd799439011",
  "verificationToken": "eyJhbGciOiJIUzI1NiIs..."
}
```

####  GET `/verify/:token`
Verifica la cuenta del usuario.

**Response:**
```json
{
  "message": "Cuenta verificada correctamente"
}
```

####  POST `/forgot-password`
Solicita recuperación de contraseña.

**Request Body:**
```json
{
  "email": "juan@example.com"
}
```

**Response:**
```json
{
  "message": "Token de recuperación generado",
  "resetToken": "eyJhbGciOiJIUzI1NiIs..."
}
```

####  POST `/reset-password/:token`
Resetea la contraseña.

**Request Body:**
```json
{
  "newPassword": "nuevaContraseña456"
}
```

**Response:**
```json
{
  "message": "Contraseña actualizada correctamente"
}
```

---

##  Dependencias Principales

### Backend
- `express` - Framework web
- `mongoose` - ODM para MongoDB
- `bcryptjs` - Hashing de contraseñas
- `jsonwebtoken` - Tokens JWT
- `cors` - CORS middleware
- `dotenv` - Gestión de variables de entorno
- `nodemailer` - Envío de emails
- `nodemon` - Dev tool para auto-restart

### Frontend
- `react` - Biblioteca UI
- `react-router-dom` - Enrutamiento
- `axios` - Cliente HTTP
- `vite` - Build tool
- `react-dom` - Renderizado DOM

---

##  Desarrollo

### Scripts Disponibles

**Backend:**
```bash
npm run dev    # Inicia modo desarrollo con nodemon
npm start      # Inicia en producción
npm test       # Ejecuta tests
```

**Frontend:**
```bash
npm run dev    # Inicia servidor de desarrollo
npm run build  # Build para producción
npm run lint   # Análisis de código
npm run preview # Previsualiza build
```

---

##  Seguridad

### Mejores Prácticas Implementadas
-  Hashing de contraseñas con bcryptjs (10 rounds)
-  Tokens JWT con expiración
-  Validación de entrada en backend
-  CORS configurado
-  Variables de entorno para secretos

### Recomendaciones Adicionales
-  Usar HTTPS en producción
-  Implementar rate limiting
-  Validación más estricta en frontend
-  Usar httpOnly cookies para tokens
-  Implementar refresh tokens
-  Auditoría de intentos fallidos

---



## 👤 Autor

Carlitops13 - GitHub

---


##  Errores Comunes

### Error: "Cannot find module 'dotenv'"
```bash
npm install dotenv
```

### Error: "MongoDB connection failed"
- Verificar `MONGO_URI` en `.env`
- Verificar credenciales de MongoDB
- Permitir acceso desde tu IP en MongoDB Atlas

### Error: "Port 3000 already in use"
```bash
# Cambiar puerto en .env
PORT=3001
```

### Frontend no se conecta a Backend
- Verificar que backend esté corriendo
- Comprobar URL en `authService.js`
- Verificar CORS en `server.js`

---


