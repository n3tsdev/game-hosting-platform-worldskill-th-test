<div align="center">

# game-housting-platfrom-worldskill-th-test — Game Hosting Platform

**A full-stack web application for hosting, sharing, and playing browser-based games**

[![PHP](https://img.shields.io/badge/PHP-8.1+-777BB4?style=for-the-badge&logo=php&logoColor=white)](https://www.php.net/)
[![Laravel](https://img.shields.io/badge/Laravel-10.x-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)](https://laravel.com/)
[![MySQL](https://img.shields.io/badge/MySQL-Database-4479A1?style=for-the-badge&logo=mysql&logoColor=white)](https://www.mysql.com/)
[![JWT](https://img.shields.io/badge/JWT-Auth-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white)](https://jwt.io/)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

</div>

---

## Overview

**Module_A** is a full-stack game hosting platform built with **Laravel 10**. The platform allows developers to upload and publish browser-based games (packaged as `.zip` files), while players can browse, play, and compete on leaderboards — all from a single web application.

The project was developed as part of a practical exercise to demonstrate backend architecture skills including RESTful API design, session-based authentication, file handling, version management, and role-based access control.

---

## Features

### User System
- **Register & Login** — Session-based authentication with password hashing (bcrypt)
- **User Profiles** — Public profile page per user (`/user/{username}`)
- **Account Blocking** — Admins can block/unblock users with a stated reason; blocked users cannot access the platform

### Game Management
- **Upload Games** — Developers upload a game as a `.zip` file alongside a `.jpg` thumbnail
- **Version Control** — Each game supports multiple versions; new uploads create a new `GameVersion` record automatically
- **Game Browser** — Homepage lists all available games for players to browse
- **Slug-based Routing** — Each game is accessible via a human-readable URL slug (e.g., `/game/my-awesome-game`)

### Scoring & Leaderboard
- **Auto Score Tracking** — Every time a user plays a game, their score is automatically incremented
- **Per-Version Leaderboard** — Scores are tracked per game version, ensuring fairness when a game is updated
- **Ranked Leaderboard** — Scores are sorted in descending order for each game version

### Admin Dashboard
- **User Management** — View all users, block/unblock accounts with reason
- **Game Management** — View all games, soft-delete games from the platform
- **Score Management** — View leaderboards per game, reset top scores, delete individual scores, or wipe all scores for a user
- **Game Search** — Search games by title keyword

### REST API
A versioned REST API (`/api/v1/`) is also provided for external client integration:
- `POST /api/v1/user/signup` — Register a new user
- `POST /api/v1/user/signin` — Authenticate and receive a token
- `POST /api/v1/user/signout` — Invalidate session token
- `GET /api/v1/user/{username}` — Get public user profile
- `GET /api/v1/games` — List all games
- `GET /api/v1/games/uploaded` — List games uploaded by the authenticated user
- `POST /api/v1/games` — Upload a new game
- `POST /api/v1/games/update` — Push a new version to an existing game

---

## Architecture

```
Module_A/
├── app/
│   ├── Http/
│   │   ├── Controllers/        # Web controllers (HTML responses)
│   │   │   ├── AuthController.php
│   │   │   ├── GameController.php
│   │   │   ├── AdminController.php
│   │   │   ├── AdminAuthController.php
│   │   │   └── UserController.php
│   │   ├── Controllers/Api/    # REST API controllers (JSON responses)
│   │   │   ├── AuthController.php
│   │   │   ├── GameController.php
│   │   │   └── UserController.php
│   │   └── Middleware/
│   └── Models/
│       ├── User.php
│       ├── Admin.php
│       ├── Game.php            # Belongs to User, has many GameVersions
│       ├── GameVersion.php     # Has many Scores
│       ├── Score.php
│       └── Session.php
├── database/
│   └── migrations/             # Database schema definitions
├── resources/
│   └── views/                  # Blade templates
│       ├── game/               # Game browse, play, upload, update views
│       ├── user/               # Sign in, sign up, profile views
│       └── admin/              # Admin dashboard views
└── routes/
    ├── web.php                 # Traditional web routes
    └── api.php                 # REST API routes
```

### Entity Relationship Summary

```
User ──< Game ──< GameVersion ──< Score >── User
                                 (leaderboard per version)
```

---

## Tech Stack

| Layer | Technology |
|---|---|
| **Framework** | Laravel 10 (PHP 8.1+) |
| **Database** | MySQL |
| **Auth (Web)** | Custom session-based auth with bcrypt |
| **Auth (API)** | JWT (`tymon/jwt-auth`) + Laravel Sanctum |
| **File Storage** | Local disk — ZipArchive for game extraction |
| **Frontend** | Blade templates + Vite asset pipeline |
| **Testing** | PHPUnit 10 |
| **HTTP Client** | Guzzle 7 |

---

## Getting Started

### Prerequisites

- PHP 8.1 or higher
- Composer
- MySQL
- Node.js & npm (for asset compilation)

### Installation

**1. Clone the repository**
```bash
git clone https://github.com/your-username/Module_A.git
cd Module_A
```

**2. Install PHP dependencies**
```bash
composer install
```

**3. Install Node dependencies**
```bash
npm install
```

**4. Configure environment**
```bash
cp .env.example .env
php artisan key:generate
```

Edit `.env` with your database credentials:
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=module_a
DB_USERNAME=root
DB_PASSWORD=your_password
```

**5. Run database migrations**
```bash
php artisan migrate
```

**6. Create storage symlink**
```bash
php artisan storage:link
```

**7. Build frontend assets & start the server**
```bash
npm run dev
php artisan serve
```

Visit `http://localhost:8000` in your browser.

---

## Environment Variables

| Variable | Description |
|---|---|
| `APP_KEY` | Application encryption key (auto-generated) |
| `DB_DATABASE` | MySQL database name |
| `DB_USERNAME` | MySQL username |
| `DB_PASSWORD` | MySQL password |
| `FILESYSTEM_DISK` | Storage disk (`local` by default) |

---

## Running Tests

```bash
php artisan test
```

---

## License

This project is open-sourced under the [MIT License](https://opensource.org/licenses/MIT).

---

<div align="center">

</div>
