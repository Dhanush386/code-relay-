# CodeExam - Secure Online Coding Examination Platform

A comprehensive, secure coding examination platform with role-based access for organizers and participants, supporting C, C++, and Python with sandboxed code execution.

##  Features

### For Organizers
-  Secure JWT-based authentication
-  Create and manage coding questions
- Add starter code for multiple languages
- Create visible and hidden testcases
- View all submissions with detailed results
-  Create and manage exams

### For Participants
-  Exam-based authentication with participant ID
-  View questions with sample testcases
-  Real-time code editor with syntax highlighting
-  Run code against visible testcases
-  Submit solutions for evaluation
-  View submission results

### Security Features
-  Role-based access control (RBAC)
-  Hidden testcases never exposed to participants
-  Sandboxed code execution (Docker)
-  Time and memory limits enforcement
-  Input validation and sanitization

##  Prerequisites

- Node.js (v18 or higher)
- PostgreSQL (v14 or higher)
- Docker (optional, for secure code execution)
- npm or yarn

##  Installation

### 1. Clone the Repository

```bash
cd c:\Users\dhanu\OneDrive\Desktop\CV
```

### 2. Backend Setup

```bash
cd backend

# Install dependencies
npm install

# Set up environment variables
# Copy .env.example to .env and update with your credentials
copy .env.example .env

# Update DATABASE_URL in .env with your PostgreSQL credentials:
# DATABASE_URL="postgresql://username:password@localhost:5432/coding_exam?schema=public"

# Generate Prisma Client
npx prisma generate

# Run database migrations
npx prisma migrate dev --name init

# Start the backend server
npm run dev
```

The backend will run on `http://localhost:5000`

### 3. Frontend Setup

```bash
cd ../frontend

# Install dependencies
npm install

# Start the development server
npm run dev
```

The frontend will run on `http://localhost:3000`

### 4. Docker Setup (Optional but Recommended)

For secure code execution, build the Docker image:

```bash
cd backend
docker build -f Dockerfile.executor -t coding-exam-executor .
```

If Docker is not available, the system will fall back to local code execution (less secure).

##  Usage

### Organizer Workflow

1. **Register/Login**: Navigate to `http://localhost:3000` and click "Login as Organizer"
2. **Create Exam**: In the dashboard, go to "Exams" tab and create a new exam with a unique code
3. **Create Question**:
   - Go to "Questions" tab
   - Click "Create Question"
   - Fill in details (title, description, constraints, time/memory limits)
   - Select the exam
4. **Add Starter Code**:
   - Click "Manage" on a question
   - Add starter code for C, C++, and Python
5. **Add Testcases**:
   - Add at least 2 visible testcases (shown to participants)
   - Add hidden testcases (for final evaluation)
6. **View Submissions**: Go to "Submissions" tab to see all participant submissions

### Participant Workflow

1. **Register/Login**: Click "Login as Participant"
   - Enter exam code (provided by organizer)
   - Enter your participant ID
2. **View Questions**: Browse available questions in the sidebar
3. **Write Code**:
   - Select a question
   - Choose programming language
   - Write your solution in the code editor
4. **Run Code**: Click "Run Code" to test against visible testcases
5. **Submit**: Click "Submit" to submit for final evaluation (runs against all testcases)

##  Environment Variables

### Backend (.env)

```env
DATABASE_URL="postgresql://username:password@localhost:5432/coding_exam?schema=public"
ORGANIZER_JWT_SECRET="your-secret-key-here"
PARTICIPANT_JWT_SECRET="your-secret-key-here"
PORT=5000
```

### Frontend (.env.local)

```env
NEXT_PUBLIC_API_URL=http://localhost:5000
```

##  Project Structure

```
CV/
├── backend/
│   ├── config/           # Configuration files
│   ├── middleware/       # Authentication middleware
│   ├── prisma/          # Database schema and migrations
│   ├── routes/          # API routes (organizer, participant)
│   ├── services/        # Code execution service
│   ├── tmp/            # Temporary files for code execution
│   ├── server.js       # Express server
│   └── package.json
│
└── frontend/
    ├── app/
    │   ├── organizer/    # Organizer pages
    │   ├── participant/  # Participant pages
    │   ├── globals.css   # Global styles
    │   ├── layout.js     # Root layout
    │   └── page.js       # Landing page
    ├── components/       # React components
    ├── next.config.js
    ├── tailwind.config.js
    └── package.json
```

##  Technology Stack

### Backend
- **Framework**: Node.js + Express
- **Database**: PostgreSQL with Prisma ORM
- **Authentication**: JWT (jsonwebtoken)
- **Security**: bcryptjs for password hashing
- **Code Execution**: Docker (with local fallback)

### Frontend
- **Framework**: Next.js 14 (React)
- **Styling**: Tailwind CSS
- **Code Editor**: Monaco Editor
- **HTTP Client**: Axios

##  Docker Image Details

The `coding-exam-executor` Docker image includes:
- Ubuntu 22.04 base
- GCC (for C)
- G++ (for C++)
- Python 3

##  Security Considerations

1. **Hidden Testcases**: Never sent to frontend; only used server-side during submission
2. **Code Execution**: Runs in isolated Docker containers with resource limits
3. **Authentication**: JWT tokens with role-based access
4. **Input Validation**: All user inputs validated on backend
5. **SQL Injection Prevention**: Prisma ORM prevents SQL injection

##  Database Schema

- **Organizer**: Stores organizer accounts
- **Exam**: Stores exam information with unique codes
- **Participant**: Stores participant credentials
- **Question**: Stores coding questions
- **StarterCode**: Language-specific starter code
- **Testcase**: Test inputs/outputs with visibility flag
- **Submission**: Code submissions with scores

##  API Endpoints

### Organizer Routes
- `POST /api/organizer/register` - Register organizer
- `POST /api/organizer/login` - Login organizer
- `POST /api/organizer/exams` - Create exam
- `GET /api/organizer/exams` - List exams
- `POST /api/organizer/questions` - Create question
- `GET /api/organizer/questions` - List questions
- `POST /api/organizer/questions/:id/starter-code` - Add starter code
- `POST /api/organizer/questions/:id/testcases` - Add testcase
- `GET /api/organizer/submissions` - View all submissions

### Participant Routes
- `POST /api/participant/register` - Register for exam
- `POST /api/participant/login` - Login to exam
- `GET /api/participant/questions` - Get questions (visible testcases only)
- `POST /api/participant/run` - Run code (visible testcases only)
- `POST /api/participant/submit` - Submit code (all testcases)
- `GET /api/participant/submissions` - View own submissions

##  Troubleshooting

### Database Connection Issues
- Ensure PostgreSQL is running
- Check `DATABASE_URL` in `.env`
- Run `npx prisma generate` and `npx prisma migrate dev`

### Docker Not Available
- The system will use local execution as fallback
- For production, Docker is highly recommended for security

### Port Already in Use
- Backend: Change `PORT` in `.env`
- Frontend: Next.js uses port 3000 by default

##  License

This project is for educational purposes.

##  Support

For issues or questions, please refer to the implementation plan or contact the development team.

---

**Built with  using Next.js, Express, and Prisma**
