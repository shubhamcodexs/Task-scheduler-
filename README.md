
# 📋 Dashboard Task Scheduler with Real-Time Notifications

A backend task scheduling system built with **Node.js**, **Express.js**, **MongoDB**, **Socket.IO**, and **node-cron**. Tasks are scheduled via REST APIs and trigger real-time notifications when executed — no page refresh needed.

---

## 🚀 Features

- ✅ **REST API** for full task CRUD operations
- ✅ **MongoDB** for persistent task storage
- ✅ **node-cron** scheduler that checks due tasks every minute
- ✅ **Socket.IO** real-time notifications pushed to all connected clients
- ✅ **Live Dashboard** at `http://localhost:5000` to see everything in action
- ✅ **Clean MVC structure** — config, models, controllers, routes, scheduler, middleware

---

## 🏗️ Project Structure

```
dashboard-task-scheduler/
├── public/
│   └── index.html              # Live real-time dashboard (browser)
├── src/
│   ├── config/
│   │   └── db.js               # MongoDB connection
│   ├── controllers/
│   │   └── taskController.js   # CRUD logic + Socket.IO emits
│   ├── middleware/
│   │   └── errorHandler.js     # Global error handling
│   ├── models/
│   │   └── Task.js             # Mongoose task schema
│   ├── routes/
│   │   └── taskRoutes.js       # Express route definitions
│   ├── scheduler/
│   │   └── taskScheduler.js    # node-cron job runner
│   └── app.js                  # Express app setup
├── .env.example                # Environment variable template
├── .gitignore
├── package.json
├── README.md
└── server.js                   # Entry point (HTTP + Socket.IO)
```

---

## ⚙️ Setup & Installation

### 1. Clone the repository
```bash
git clone https://github.com/your-username/dashboard-task-scheduler.git
cd dashboard-task-scheduler
```

### 2. Install dependencies
```bash
npm install
```

### 3. Configure environment variables
```bash
cp .env.example .env
```
Edit `.env`:
```
PORT=5000
MONGO_URI=mongodb://localhost:27017/task-scheduler
```

### 4. Start the server
```bash
# Development (auto-restart with nodemon)
npm run dev

# Production
npm start
```

### 5. Open the live dashboard
Visit `http://localhost:5000` in your browser.

---

## 🔌 API Endpoints

Base URL: `http://localhost:5000/api`

| Method | Endpoint         | Description              |
|--------|------------------|--------------------------|
| POST   | `/tasks`         | Create a new task        |
| GET    | `/tasks`         | Get all tasks            |
| GET    | `/tasks?status=pending` | Filter by status  |
| GET    | `/tasks/:id`     | Get a single task        |
| PUT    | `/tasks/:id`     | Update a task            |
| DELETE | `/tasks/:id`     | Delete a task            |
| GET    | `/health`        | Health check             |

### Task Object
```json
{
  "_id": "64abc123...",
  "title": "Send weekly report",
  "description": "Email the Q2 summary to the team",
  "scheduledTime": "2025-10-15T14:30:00.000Z",
  "status": "pending",
  "notified": false,
  "createdAt": "2025-10-15T12:00:00.000Z",
  "updatedAt": "2025-10-15T12:00:00.000Z"
}
```

**Status values:** `pending` → `running` → `completed` / `failed`

---

## 📡 Socket.IO Events

All connected clients receive these real-time events:

| Event               | Payload                              | Trigger                        |
|---------------------|--------------------------------------|--------------------------------|
| `welcome`           | `{ message, timestamp }`            | On client connection           |
| `task:created`      | `{ task, message }`                 | When a new task is created     |
| `task:updated`      | `{ task, message }`                 | When a task is updated         |
| `task:deleted`      | `{ taskId, message }`               | When a task is deleted         |
| `task:notification` | `{ taskId, title, status, message }`| When scheduler starts a task   |
| `task:completed`    | `{ taskId, title, status, message }`| When task finishes             |
| `task:failed`       | `{ taskId, title, status, message }`| When task execution fails      |

---

## 🧪 Testing with Postman

### Create a Task
```
POST http://localhost:5000/api/tasks
Content-Type: application/json

{
  "title": "Send weekly report",
  "description": "Email summary to team",
  "scheduledTime": "2025-10-15T14:00:00.000Z"
}
```

### Get All Tasks
```
GET http://localhost:5000/api/tasks
GET http://localhost:5000/api/tasks?status=pending
```

### Update a Task
```
PUT http://localhost:5000/api/tasks/:id
Content-Type: application/json

{
  "title": "Updated Title",
  "scheduledTime": "2025-10-15T15:00:00.000Z"
}
```

### Delete a Task
```
DELETE http://localhost:5000/api/tasks/:id
```

---

## 🔄 How the Scheduler Works

1. **node-cron** runs a job **every minute**
2. It queries MongoDB for tasks where `status = "pending"` AND `scheduledTime <= now`
3. Each due task is marked as `running` and a `task:notification` event is emitted via Socket.IO
4. After simulated execution, the task is marked `completed` and `task:completed` is emitted
5. The live dashboard updates **instantly** — no page refresh needed

---

## 🛠️ Tech Stack

| Technology   | Purpose                         |
|--------------|---------------------------------|
| Node.js      | JavaScript runtime              |
| Express.js   | HTTP server & REST API          |
| MongoDB      | Database for task storage       |
| Mongoose     | MongoDB ODM                     |
| node-cron    | Task scheduling (cron jobs)     |
| Socket.IO    | WebSocket real-time events      |
| dotenv       | Environment variable management |
| cors         | Cross-Origin Resource Sharing   |

---

## 📌 Success Criteria

- [x] Task Creation API Completed
- [x] Tasks Stored Successfully in MongoDB
- [x] Scheduler Working Correctly with node-cron
- [x] Notifications Triggered at Scheduled Time
- [x] Real-Time Updates Delivered Without Page Refresh
- [x] API Tested via Postman-compatible endpoints
- [x] Clean Code Structure Maintained (MVC pattern)
- [x] Project ready for GitHub submission
