# SolveMate AI

SolveMate AI is an educational assistant chatbot powered by the LLaMA 3.3 70B model through Groq's API. It helps students understand concepts and solve problems through guided learning rather than providing direct answers.

## Features

- Real-time chat interface
- Multiple chat sessions support
- Session management and persistence
- Responsive design for desktop and mobile
- Educational-focused AI responses
- Markdown support in messages

## Tech Stack

- Frontend:
  - Vue 3
  - Vite
  - Axios
  - Tailwind CSS
- Backend:
  - Python 3.8+
  - Flask
  - Flask-CORS
  - Groq API (LLaMA 3.3 70B Versatile model)

## Prerequisites

- Python 3.8 or higher
- Node.js 16 or higher
- Conda package manager
- Groq API key

## Setup Instructions

### 1. Clone the Repository

```bash
git clone <your-repository-url>
cd solvemate-ai
```

### 2. Backend Setup

Create and activate a Conda environment:

```bash
conda create -n solvemate python=3.8
conda activate solvemate
```

Install Python dependencies:

```bash
pip install -r requirements.txt
```

Create a `.env` file in the project root:

```env
GROQ_API_KEY=your_groq_api_key_here
```

### 3. Frontend Setup

Install Node dependencies:

```bash
cd frontend
npm install
```

### 4. Running the Application

Start the backend server:

```bash
# From the project root
python main.py
```

Start the frontend development server:

```bash
# From the frontend directory
npm run dev
```

The application will be available at:
- Frontend: http://localhost:5173
- Backend: http://localhost:5000

## Project Structure

```
solvemate-ai/
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   └── Chat.vue
│   │   └── App.vue
│   ├── package.json
│   └── vite.config.js
├── main.py
├── requirements.txt
├── .env
└── README.md
```

## API Endpoints

- `POST /api/chat`: Send a message to the AI
- `DELETE /api/chat/<session_id>`: Delete a chat session
- `GET /api/health`: Check backend health status

## Requirements.txt

Create a `requirements.txt` file with the following dependencies:

```txt
flask==2.0.1
flask-cors==3.0.10
python-dotenv==0.19.0
requests==2.26.0
```

## Environment Variables

Required environment variables:

- `GROQ_API_KEY`: Your Groq API key for accessing the LLaMA model

## Development

To start development:

1. Ensure you're in the correct Conda environment:
```bash
conda activate solvemate
```

2. Run the backend server in debug mode:
```bash
python main.py
```

3. In a separate terminal, run the frontend development server:
```bash
cd frontend
npm run dev
```
