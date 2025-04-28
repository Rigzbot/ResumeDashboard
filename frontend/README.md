
Smart Resume Parser — Frontend

Welcome!

This is the React app for the Smart Resume Dashboard. It lets you upload a resume and shows you a simple dashboard with:

- Top matching jobs with details
- Skill match analysis
- Salary trends based on experience and location
- Benefits shown in a radar chart
- US map showing where the jobs are

---

Tech Stack

- React
- Material UI (MUI)
- React Router
- D3.js
- Docker (optional)

---

Getting Started (Local Development)

1. Install Node.js and npm

Download and install Node.js from https://nodejs.org/.

Verify the installation:

node -v
npm -v

2. Install Project Dependencies

cd dataviz-dashboard
npm install

3. Start the App

npm start

Visit the app in your browser at http://localhost:3000

---

Docker Deployment

1. Build Docker Image

docker build -t resume-frontend .

2. Run the App in Docker

docker run -it --rm -p 3000:3000 resume-frontend

Make sure your backend URL (in .env or API settings) is correct if the backend is running locally (example: http://localhost:8000).


License

MIT License
