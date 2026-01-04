
<img width="1490" height="832" alt="image" src="https://github.com/user-attachments/assets/b231f7bc-3878-4d61-83e2-6e73fa97898a" />

<img width="1749" height="840" alt="image" src="https://github.com/user-attachments/assets/dc6c3692-4e58-42b0-b8fe-211790b48f79" />


# B2B SaaS Task Board

A full-stack B2B SaaS application for task management with organization support, built with FastAPI and React.

## Features

- 🔐 **Authentication**: Clerk integration for secure user authentication
- 👥 **Organization Management**: Multi-tenant organization support with role-based permissions
- 📋 **Task Management**: Create, read, update, and delete tasks with Kanban board UI
- 🎯 **Permission-Based Access**: Role-based access control with granular permissions
- 💳 **Tiered Plans**: Free and Pro tier limits for task creation

## Tech Stack

### Backend

- **Framework**: FastAPI
- **Database**: SQLAlchemy ORM with SQLite
- **Authentication**: Clerk Backend API
- **API**: RESTful API with CORS support

### Frontend

- **Framework**: React 18
- **Build Tool**: Vite
- **UI Components**: Custom CSS with modular styling
- **Authentication**: Clerk React SDK
- **HTTP Client**: Fetch API

## Project Structure

```
B2B_SaaS/
├── backend/
│   ├── app/
│   │   ├── api/              # API routes
│   │   │   ├── tasks.py      # Task endpoints
│   │   │   └── webhooks.py   # Webhook handlers
│   │   ├── core/             # Core configuration
│   │   │   ├── auth.py       # Authentication logic
│   │   │   ├── clerk.py      # Clerk client
│   │   │   ├── config.py     # Configuration
│   │   │   └── database.py   # Database setup
│   │   ├── models/           # Database models
│   │   │   └── task.py       # Task model
│   │   ├── schemas/          # Pydantic schemas
│   │   │   └── task.py       # Task schemas
│   │   └── main.py           # FastAPI app
│   ├── start.py              # Server startup
│   └── pyproject.toml        # Dependencies
│
├── frontend/
│   ├── src/
│   │   ├── components/       # React components
│   │   │   ├── KanbanBoard.jsx
│   │   │   ├── TaskCard.jsx
│   │   │   └── ...
│   │   ├── pages/            # Page components
│   │   │   ├── DashboardPage.jsx
│   │   │   ├── HomePage.jsx
│   │   │   └── ...
│   │   ├── services/         # API service
│   │   │   └── api.js        # API calls
│   │   ├── styles/           # CSS styles
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── package.json
│   └── vite.config.js
│
└── README.md
```

## Getting Started

### Prerequisites

- Python 3.10+
- Node.js 16+
- npm or yarn
- Clerk account (for authentication)

### Backend Setup

1. **Navigate to backend directory**

   ```bash
   cd backend
   ```

2. **Create and activate virtual environment**

   ```bash
   python -m venv .venv
   .venv\Scripts\activate  # Windows
   source .venv/bin/activate  # macOS/Linux
   ```

3. **Install dependencies**

   ```bash
   pip install -r requirements.txt
   # or with uv:
   uv sync
   ```

4. **Set up environment variables**
   Create a `.env` file in the `backend/` directory:

   ```env
   CLERK_SECRET_KEY=your_clerk_secret_key
   CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
   CLERK_WEBHOOK_SECRET=your_webhook_secret
   CLERK_JWKS_URL=https://your-instance.clerk.accounts.dev/.well-known/jwks.json
   DATABASE_URL=sqlite:///./tasks.db
   FRONTEND_URL=http://localhost:5173
   ```

5. **Run the server**
   ```bash
   python start.py
   # or with uv:
   uv run start.py
   ```
   Server will be available at `http://localhost:8000`

### Frontend Setup

1. **Navigate to frontend directory**

   ```bash
   cd frontend
   ```

2. **Install dependencies**

   ```bash
   npm install
   ```

3. **Set up environment variables**
   Create a `.env.local` file in the `frontend/` directory:

   ```env
   VITE_CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
   VITE_API_URL=http://localhost:8000
   ```

4. **Run the development server**
   ```bash
   npm run dev
   ```
   Frontend will be available at `http://localhost:5173`

## API Endpoints

### Tasks

- `GET /api/tasks` - List all tasks for the organization
- `POST /api/tasks` - Create a new task
- `GET /api/tasks/{task_id}` - Get a specific task
- `PUT /api/tasks/{task_id}` - Update a task
- `DELETE /api/tasks/{task_id}` - Delete a task
- `PATCH /api/tasks/{task_id}/status` - Update task status

### Webhooks

- `POST /api/webhooks/clerk` - Handle Clerk webhook events

## Authentication & Permissions

Permissions are managed through Clerk:

- `org:task:view` - View tasks
- `org:task:create` - Create new tasks
- `org:task:edit` - Edit existing tasks
- `org:task:delete` - Delete tasks

Each user must have a role with appropriate permissions in their organization.

## Database Models

### Task

- `id`: Primary key (UUID)
- `title`: Task title
- `description`: Task description
- `status`: Task status (todo, in_progress, done)
- `org_id`: Organization ID (from Clerk)
- `created_by`: User ID (from Clerk)
- `created_at`: Timestamp
- `updated_at`: Timestamp

## Development

### Common Tasks

**Start both servers:**

```bash
# Terminal 1: Backend
cd backend && python start.py

# Terminal 2: Frontend
cd frontend && npm run dev
```

**Run linting:**

```bash
# Frontend
cd frontend && npm run lint

# Backend
cd backend && flake8 app/
```

**Build for production:**

```bash
# Frontend
cd frontend && npm run build

# Backend uses production ASGI server
# Example with Gunicorn:
gunicorn app.main:app --workers 4 --worker-class uvicorn.workers.UvicornWorker
```

## Troubleshooting

### CORS Errors

Ensure `FRONTEND_URL` is correctly set in backend `.env` and matches your frontend origin.

### Authentication Errors

- Verify Clerk keys are correctly set in both `.env` files
- Ensure user has a role with required permissions in Clerk dashboard
- Check that the organization is properly set up in Clerk

### 403 Forbidden Errors

- Verify the permission names match exactly (singular: `org:task:view`, not `org:tasks:view`)
- Ensure user role includes the required permissions
- Sign out and sign back in to refresh the authentication token

## Contributing

1. Create a feature branch
2. Make your changes
3. Test thoroughly
4. Submit a pull request

## License

MIT License - feel free to use this project for your own purposes.
