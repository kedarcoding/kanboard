# 📝 Task Management Board

A modern and interactive **Task Management App** that helps organize tasks into columns using a Kanban-style layout. Built with Laravel and Vue 3 using Inertia.js and Tailwind CSS.

---

## 🚀 Tech Stack

- 🔧 **Backend**: Laravel 12
- 💾 **Database**: MySQL
- 🌐 **Frontend**: Vue 3 (Composition API)
- 🎯 **SPA Framework**: Inertia.js
- 💅 **UI Styling**: Tailwind CSS
- 📦 **Drag & Drop**: vue-draggable-next
- ⚡ **Routing & SPA Behavior**: Inertia-powered navigation

---

## 📌 Features

- 🟦 Kanban-style 3-column board (To-do / In-Progress / Done)
- ✨ Drag & drop tasks between columns
- 🖊 Inline task editing with keyboard (Enter key) and blur support
- ➕ Add new tasks with title and status selector
- 🗑 Delete tasks instantly
- 🔄 Realtime status update after drag or edit
- 💾 Backend synced via Inertia & Laravel Routes

---

## ⚙️ Project Setup

> Make sure PHP, Composer, Node.js, and MySQL are installed on your machine.

### 🛠 Backend (Laravel)

```bash
git clone https://github.com/kedarcoding/kanboard.git
cd kanboard

# Install PHP dependencies
composer install

# Setup your .env file
cp .env.example .env

# Generate app key
php artisan key:generate

# Configure your database in `.env` and then run:
php artisan migrate


# Start Laravel server
php artisan serve

# In another terminal, start Vite dev server
npm run dev


🧑‍💻 Author
Kedar Golande
Laravel + Vue Developer
