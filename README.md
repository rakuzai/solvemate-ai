# SolveMate AI

SolveMate AI is an educational assistant chatbot powered by the LLaMA 3.3 70B model through Groq's API. It helps students understand concepts and solve problems through guided learning rather than providing direct answers.

## Project Presentation & Demo
[Video](https://drive.google.com/file/d/1mmgDiqfPHERuvn0aBMinNooHaiWxp5_m/view?usp=sharing)

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

## Project Core Structure

```
solvemate-ai/
├── backend/
│   └── main.py
│   ├── src/
│   │   ├── components/
│   │   │   └── Chat.vue
│   │   ├── App.vue
│   │   └── main.js
│   ├── public/
│   ├── index.html
```

## Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/rakuzai/solvemate-ai.git
cd solvemate-ai
```

### 2. Backend Setup

Create and activate a Conda environment:

```bash
conda create -n solvemate
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
npm install
```

### 4. Running the Application

Start the backend server:

```bash
# From the backend directory
python main.py
```

Start the frontend development server:

```bash
# Format the code first
npm run format

# Start the development server
npm run dev
```

The application will be available at:
- Frontend: http://localhost:5173
- Backend: http://localhost:5000

## API Endpoints

- `POST /api/chat`: Send a message to the AI
- `DELETE /api/chat/<session_id>`: Delete a chat session
- `GET /api/health`: Check backend health status

## Requirements.txt

The `requirements.txt` file should contain:

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
npm run dev
```
