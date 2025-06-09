# E-commerce-chatbot
Overview
This project develops a full-stack e-commerce chatbot application designed to assist users with product inquiries and general conversational needs within an online shopping context. It features a secure user authentication system, an intelligent chatbot powered by a pre-trained language model, and basic product search functionality integrated with a database.

🚀 Key Features
User Authentication: Secure registration and login using JWT (JSON Web Tokens) and password hashing.

Intelligent Chatbot: Powered by a pre-trained microsoft/DialoGPT-medium model from Hugging Face Transformers for conversational interactions.

Product Search: Ability to search for products based on keywords, categories, descriptions, and brands from a simulated e-commerce catalog.

Chat History Persistence: All user-chatbot interactions are logged and stored in a SQLite database.

Scalable Architecture: Separate frontend (React) and backend (Flask) services for modular development.

🛠️ Technology Stack
Frontend:

React.js: Building dynamic user interfaces.

HTML/CSS: Core web structuring and styling.

npm: Package management.

Backend:

Flask: Python micro-framework for RESTful APIs.

Flask-SQLAlchemy: ORM for database interactions with SQLite.

Flask-JWT-Extended: JWT-based authentication.

Werkzeug.security: Secure password hashing.

Flask-CORS: Handling Cross-Origin Resource Sharing.

Hugging Face Transformers: AI model integration (microsoft/DialoGPT-medium).

SQLite: Lightweight database for user and product data.

Development Tools:

Python (3.8+)

Node.js & npm

Visual Studio Code (VS Code)

Python Virtual Environments (venv)

📁 Project Structure
ecommerce-chatbot-app/
├── backend/
│   ├── venv/                   # Python Virtual Environment
│   ├── ecommerce_chatbot.db    # SQLite Database file (generated)
│   ├── app.py                  # Main Flask application, API routes, chatbot logic
│   ├── models.py               # Database models (User, Product, ChatInteraction)
│   ├── extensions.py           # Database initialization (db object)
│   └── requirements.txt        # Backend Python dependencies
├── frontend/
│   ├── node_modules/           # Node.js dependencies
│   ├── public/                 # Public assets
│   ├── src/
│   │   ├── App.js              # Main React component, state management, API calls
│   │   ├── App.css             # Basic CSS styling
│   │   └── index.js            # React app entry point
│   ├── package.json            # Frontend Node.js dependencies
│   └── .env                    # Environment variables (if used)
└── README.md                   # Project README (this file)

⚙️ Setup and Installation
Follow these steps to get the application up and running on your local machine.

1. Prerequisites
Python 3.8+: Download & Install Python

Node.js and npm: Download & Install Node.js (npm is included)

Git (Optional): Download & Install Git (for cloning the repository)

2. Clone the Repository (Optional, if you have a local copy already)
If you haven't already, clone this repository:

git clone <repository-url>
cd ecommerce-chatbot-app

3. Backend Setup (Flask API)
Navigate to the Backend Directory:

cd backend

Create and Activate a Python Virtual Environment:

python -m venv venv
.\venv\Scripts\activate   # On Windows
# source venv/bin/activate # On macOS/Linux

Install Python Dependencies:
First, create a requirements.txt file if you don't have one. From your backend directory while venv is active:

pip freeze > requirements.txt

Then, install the dependencies:

pip install -r requirements.txt
# If for some reason `requirements.txt` is missing or problematic, you can install manually:
# pip install Flask Flask-SQLAlchemy Flask-JWT-Extended Werkzeug Flask-Cors transformers torch

Note: torch is a large dependency for transformers and may take some time to download and install.

Delete Existing Database File (Crucial for a Clean Start):
If you've run the backend before, a database file (ecommerce_chatbot.db or site.db) might exist. Ensure your Flask app is stopped (Ctrl+C in its terminal), then delete this file from the backend directory. This forces the application to recreate the database with the latest schema and populate it with dummy product data.

4. Run the Flask Backend Server
From the backend directory with your virtual environment active:

python app.py

Expected Output: You should see messages similar to:

Adding dummy products to the database...

Dummy products added successfully!

Chatbot pipeline initialized successfully.

* Running on http://127.0.0.1:5000

Keep this terminal window open and running.

5. Frontend Setup (React App)
Open a NEW Terminal Window:
Do not close the backend terminal.

Navigate to the Frontend Directory:

cd ../frontend # From the 'backend' directory
# or
# cd C:\Users\Poulomi\Desktop\ecommerce-chatbot-app\frontend # From anywhere

Install Node.js Dependencies:

npm install

This command will install all React-related libraries specified in package.json.

6. Run the React Development Server
From the frontend directory:

npm start

Expected Output: This will usually open your application automatically in your default web browser at http://localhost:3000 or http://localhost:3002.

Keep this terminal window open and running.

💻 How to Use the Application
Access the Application: Once both backend and frontend servers are running, open your web browser and navigate to the address shown in your frontend terminal (e.g., http://localhost:3002).

User Registration:

On the initial screen, provide a unique username, a strong password, and optionally an email address.

Click the "Register" button. You should receive a "User registered successfully" message.

User Login:

If you're on the registration screen, click "Already have an account? Login" to switch forms.

Enter the username and password of your newly registered user.

Click "Login". Upon successful authentication, the interface will seamlessly transition to the chatbot interaction area.

Chat with the E-commerce Bot:

Once logged in, type your queries into the input field at the bottom of the screen.

Product Search Examples:

"Show me electronics."

"Do you have a red t-shirt?"

"I'm looking for headphones."

"What kind of backpacks do you sell?"

"Tell me about the Wireless Mouse."

General Conversation Examples:

"Hello."

"How are you today?"

"Tell me a joke."

The chatbot's responses will appear in the chat history above the input field.

🔍 Learnings & Troubleshooting Common Issues
During the development of this project, several common pitfalls were encountered. Documenting these challenges and their solutions is crucial for future development and maintenance.

1. ModuleNotFoundError (e.g., transformers)
Problem: Python modules like transformers or deep learning backends (torch) were not found.

Solution: Ensure the Python virtual environment (venv) is activated (.\venv\Scripts\activate) and all dependencies are installed using pip install -r requirements.txt. For transformers, specifically ensure torch is installed (pip install torch).

2. bcrypt Installation/Usage Issues
Problem: Problems with bcrypt for password hashing due to underlying C library dependencies.

Solution: Switched to werkzeug.security (generate_password_hash, check_password_hash) for robust and simpler password hashing.

3. KeyError: 'Unknown task conversational' from Hugging Face Transformers
Problem: The transformers pipeline failed to initialize with the 'conversational' task, leading to errors in the backend and "Error getting response from chatbot" on the frontend. This was due to changes in the transformers library's API or compatibility.

Solution: Modified backend/app.py to use pipeline("text-generation", model="microsoft/DialoGPT-medium") which is more generally supported. The response extraction was adjusted to match the text-generation output format.

4. Cross-Origin Resource Sharing (CORS) Issues
Problem: The browser's Same-Origin Policy prevented communication between the React frontend (e.g., http://localhost:3000 or http://localhost:3002) and the Flask backend (http://127.0.0.1:5000). This showed as "Access to fetch... has been blocked by CORS policy" errors.

Solution: Configured Flask-CORS in backend/app.py to explicitly whitelist the frontend's development server origins: CORS(app, resources={r"/*": {"origins": ["http://localhost:3000", "http://localhost:3002"]}}). Performing a hard browser refresh was often necessary to ensure the latest CORS headers were recognized.

5. Incorrect API URL Construction in React Frontend
Problem: Frontend requests resulted in 404 Not Found errors and SyntaxError: Unexpected token '<' because API URLs were malformed (e.g., containing literal HTML tags from incorrect copy-pasting).

Solution: Ensured the API_BASE_URL constant (http://127.0.0.1:5000) was correctly prepended to all API endpoint calls in frontend/src/App.js using proper JavaScript template literals (${API_BASE_URL}/${endpoint}).

6. localhost vs 127.0.0.1 vs localhost:port Nuances
Problem: Even though localhost and 127.0.0.1 refer to the same machine, browsers can treat them as different origins for CORS. React's dev server might also pick different ports.

Solution: Maintained http://127.0.0.1:5000 as the API_BASE_URL in React for consistent backend targeting and ensured the Flask CORS configuration explicitly allowed all potential frontend development ports (3000 and 3002).

7. Database File Deletion for Schema Updates
Problem: Changes to models.py (e.g., adding new fields to Product) were not reflected in the existing SQLite .db file, causing database errors.

Solution: The robust practice involved stopping the Flask server, deleting the ecommerce_chatbot.db file (or site.db if named differently) from the backend directory, and then restarting the Flask server. This allowed db.create_all() to rebuild the database with the updated schema and re-populate dummy data.

💡 Future Enhancements
Advanced NLU: Integrate more sophisticated Natural Language Understanding (NLU) to handle complex, multi-turn conversations and identify fine-grained user intents and entities (e.g., "I want a red shirt in size L").

Contextual Memory: Implement a more robust conversation memory for the chatbot to maintain context across multiple turns and sessions.

Shopping Cart & Orders: Develop full e-commerce functionality, including adding items to a cart, viewing cart contents, and simulating order placement and tracking.

Rich UI/UX: Enhance the frontend's visual design, responsiveness for various devices, and user experience with dynamic loading indicators, animations, and potentially more interactive chat elements (e.g., product cards).

User Profiles: Allow users to manage their profiles, view past orders, and personalize preferences.

Deployment: Deploy the application to cloud platforms (e.g., Render, Heroku for Flask; Netlify, Vercel for React) for public accessibility and demonstration.

Automated Testing: Implement comprehensive unit and integration tests for both frontend and backend to ensure reliability and maintainability.
