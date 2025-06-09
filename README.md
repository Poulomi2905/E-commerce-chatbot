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

BACKEND CODES :
app.py :
# backend/app.py
from flask import Flask, request, jsonify
from flask_jwt_extended import create_access_token, jwt_required, JWTManager, get_jwt_identity
from werkzeug.security import generate_password_hash, check_password_hash
from flask_cors import CORS
from datetime import datetime

# Import db from extensions.py and your models
from extensions import db
from models import User, Product, ChatInteraction # Ensure Product and ChatInteraction are imported!

def create_app():
    app = Flask(__name__)
    # Set a strong secret key for JWTs. In production, get this from environment variables.
    app.config["JWT_SECRET_KEY"] = "your-very-strong-and-secret-key-that-no-one-can-guess-1234567890"
    app.config["SQLALCHEMY_DATABASE_URI"] = "sqlite:///ecommerce_chatbot.db" # Database file name
    app.config["SQLALCHEMY_TRACK_MODIFICATIONS"] = False # Disable tracking modifications for performance

    # Initialize extensions with the app instance
    db.init_app(app)
    jwt = JWTManager(app) # Initialize JWTManager with the app

    # Configure CORS to allow requests from your React frontend.
    # Allowing both 3000 and 3002 ports for robustness in development.
    CORS(app, resources={r"/*": {"origins": ["http://localhost:3000", "http://localhost:3002"]}})

    # Create database tables and add dummy product data on application startup (development only).
    # This block ensures your database is set up with initial products every time the app runs,
    # but only if the product table is currently empty.
    with app.app_context():
        db.create_all() # Create tables if they don't already exist
        if Product.query.count() == 0: # Check if the Product table is empty
            print("Adding dummy products to the database...")
            # Define sample product data
            p1 = Product(
                name='Wireless Mouse',
                category='Electronics',
                description='Ergonomic wireless mouse with adjustable DPI and long battery life.',
                price=25.99,
                stock_quantity=100,
                image_url='https://placehold.co/150x150/lightblue/white?text=Mouse',
                brand='LogiTech',
                rating=4.5
            )
            p2 = Product(
                name='Mechanical Keyboard',
                category='Electronics',
                description='RGB mechanical keyboard with clicky switches and customizable macros.',
                price=79.99,
                stock_quantity=50,
                image_url='https://placehold.co/150x150/lightgreen/white?text=Keyboard',
                brand='Razer',
                rating=4.7
            )
            p3 = Product(
                name='Red Cotton T-Shirt',
                category='Apparel',
                description='Comfortable 100% cotton t-shirt in vibrant red for casual wear.',
                price=15.00,
                stock_quantity=200,
                image_url='https://placehold.co/150x150/lightcoral/white?text=Red+Shirt',
                brand='FashionCo',
                rating=4.2
            )
            p4 = Product(
                name='Blue Denim Jeans',
                category='Apparel',
                description='Classic fit blue denim jeans, durable and stylish for everyday use.',
                price=45.50,
                stock_quantity=150,
                image_url='https://placehold.co/150x150/lightsteelblue/white?text=Blue+Jeans',
                brand='DenimWorks',
                rating=4.0
            )
            p5 = Product(
                name='Laptop Backpack',
                category='Accessories',
                description='Water-resistant backpack designed for 15-inch laptops with multiple compartments.',
                price=39.99,
                stock_quantity=80,
                image_url='https://placehold.co/150x150/lightgray/white?text=Backpack',
                brand='TravelGear',
                rating=4.6
            )
            p6 = Product(
                name='Noise-Cancelling Headphones',
                category='Electronics',
                description='Over-ear headphones with active noise cancellation, perfect for travel or focus.',
                price=199.99,
                stock_quantity=30,
                image_url='https://placehold.co/150x150/lightseagreen/white?text=Headphones',
                brand='AudioPro',
                rating=4.8
            )
            # Add all dummy products to the session and commit them to the database
            db.session.add_all([p1, p2, p3, p4, p5, p6])
            db.session.commit()
            print("Dummy products added successfully!")
        else:
            print("Products already exist in the database.") # Inform if products are already present
    
    return app

# Create the Flask app instance
app = create_app()

# Initialize chatbot pipeline from Hugging Face Transformers.
# Using 'text-generation' as 'conversational' task might be deprecated or inconsistent.
# The model 'microsoft/DialoGPT-medium' is a good choice for general conversation.
try:
    from transformers import pipeline
    # max_new_tokens controls the length of the bot's response
    chatbot_pipeline = pipeline("text-generation", model="microsoft/DialoGPT-medium", max_new_tokens=100)
    print("Chatbot pipeline initialized successfully.")
except Exception as e:
    print(f"Failed to load chatbot pipeline: {e}. General chat functionality will be limited.")
    chatbot_pipeline = None # Set to None if loading fails, to prevent further errors

# --- API Routes ---

@app.route('/register', methods=['POST'])
def register():
    """
    Handles user registration. Hashes the password before storing.
    Requires: username, password (and optional email) in JSON body.
    """
    data = request.get_json()
    username = data.get('username')
    password = data.get('password')
    email = data.get('email')

    if not username or not password:
        return jsonify({"message": "Username and password are required"}), 400

    # Check if username already exists
    if User.query.filter_by(username=username).first():
        return jsonify({"message": "Username already exists"}), 409

    # Hash the password for security
    hashed_password = generate_password_hash(password).decode('utf-8') # Decode to string for storage

    # Create new user and add to database
    new_user = User(username=username, password_hash=hashed_password, email=email)
    db.session.add(new_user)
    db.session.commit()

    return jsonify({"message": "User registered successfully"}), 201

@app.route('/login', methods=['POST'])
def login():
    """
    Handles user login. Verifies password and issues a JWT access token.
    Requires: username, password in JSON body.
    """
    data = request.get_json()
    username = data.get('username')
    password = data.get('password')

    # Find user by username
    user = User.query.filter_by(username=username).first()

    # Verify user exists and password is correct
    if user and check_password_hash(user.password_hash, password):
        # Create a JWT access token with the user's ID as identity
        access_token = create_access_token(identity=user.id)
        return jsonify(
            message="Login successful",
            access_token=access_token,
            user_id=user.id,
            username=user.username
        ), 200
    else:
        return jsonify({"message": "Invalid credentials"}), 401

@app.route('/chat/ask', methods=['POST'])
@jwt_required() # This route requires a valid JWT token
def chat_ask():
    """
    Handles chat interactions. Attempts to answer product-related queries
    or falls back to a general chatbot. Logs both user and chatbot messages.
    Requires: 'message' in JSON body.
    """
    current_user_id = get_jwt_identity() # Get user ID from the JWT token
    user = User.query.get(current_user_id) # Fetch user object (optional, for contextual info)

    if not request.is_json:
        return jsonify({"msg": "Missing JSON in request"}), 400

    user_message = request.json.get('message', None)
    if not user_message:
        return jsonify({"msg": "Missing message parameter"}), 400

    chatbot_response = ""
    user_message_lower = user_message.lower()

    # Define keywords to trigger product search
    product_keywords = ["product", "item", "looking for", "buy", "sell", "have", "show me", "search for", "find",
                        "shirt", "t-shirt", "mouse", "keyboard", "jeans", "backpack", "headphones",
                        "electronics", "apparel", "accessories", "price", "stock", "brand", "category"]
    
    # Determine if the user's message is a product-related query
    is_product_query = any(keyword in user_message_lower for keyword in product_keywords)

    if is_product_query:
        # Simple extraction of potential search terms by removing common words
        # This can be made more sophisticated with NLP libraries if needed
        search_terms_raw = user_message_lower.replace("show me", "").replace("looking for", "").replace("any", "").replace("what kind of", "").strip().split()
        
        # Filter out very common, non-descriptive words from search terms
        filtered_terms = [
            term for term in search_terms_raw 
            if term not in ["a", "an", "the", "for", "me", "what", "do", "you", "to", "i", "want", "can", "find", "about", "of"]
        ]
        
        products = []
        if filtered_terms:
            # Construct a dynamic SQLAlchemy query to search across multiple fields
            query = db.session.query(Product)
            for term in filtered_terms:
                # Use .ilike for case-insensitive matching
                query = query.filter(
                    (Product.name.ilike(f'%{term}%')) |
                    (Product.description.ilike(f'%{term}%')) |
                    (Product.category.ilike(f'%{term}%')) |
                    (Product.brand.ilike(f'%{term}%'))
                )
            products = query.limit(5).all() # Limit results to the top 5
        else:
            # If no specific terms but it's a general product query (e.g., "show me products"),
            # return a few random products as suggestions
            products = Product.query.order_by(db.func.random()).limit(3).all()

        if products:
            # Format product search results into a user-friendly response
            response_parts = ["Here are some products I found:"]
            for p in products:
                response_parts.append(
                    f"- {p.name} ({p.category}, Brand: {p.brand if p.brand else 'N/A'}), Price: ${p.price:.2f}, Stock: {p.stock_quantity}. Rating: {p.rating if p.rating else 'N/A'}."
                )
            chatbot_response = " ".join(response_parts)
        else:
            chatbot_response = "I couldn't find any products matching your specific request. Please try searching for something else, or ask a general question."
    else:
        # Fallback to the general chatbot if no product-related keywords are found
        if chatbot_pipeline:
            # Prepare the prompt for the text-generation pipeline.
            # Add a "Chatbot:" prefix to encourage a response style.
            prompt = f"User: {user_message}\nChatbot:"
            
            # Use the text-generation pipeline.
            # The result is a list of dictionaries, where each dict has a 'generated_text' key.
            generated_output = chatbot_pipeline(prompt, num_return_sequences=1)
            
            # Extract the generated text and remove the original prompt from the response
            full_generated_text = generated_output[0]['generated_text']
            chatbot_response = full_generated_text.replace(prompt, '').strip()

            # Fallback if the generation is empty or just whitespace
            if not chatbot_response:
                chatbot_response = "I'm sorry, I'm having trouble generating a response. Can you rephrase your question?"
        else:
            chatbot_response = "I'm sorry, the general chatbot is not available at the moment. Please try again later or ask about products."

    # Log the user's message to the database
    new_chat_user = ChatInteraction(
        user_id=current_user_id,
        sender="User",
        message=user_message,
        timestamp=datetime.utcnow() # Record the current timestamp
    )
    db.session.add(new_chat_user)
    
    # Log the chatbot's response to the database
    new_chat_bot = ChatInteraction(
        user_id=current_user_id,
        sender="Chatbot",
        message=chatbot_response,
        timestamp=datetime.utcnow() # Record the current timestamp
    )
    db.session.add(new_chat_bot)
    db.session.commit() # Commit both messages in one transaction

    return jsonify({"chatbot_response": chatbot_response, "user_message": user_message}), 200

# Entry point for running the Flask application
if __name__ == '__main__':
    app.run(debug=True) # Run in debug mode during development for auto-reloading and diagnostics

extensions.py :
# backend/extensions.py
from flask_sqlalchemy import SQLAlchemy
from flask_bcrypt import Bcrypt

db = SQLAlchemy()
bcrypt = Bcrypt()

models.py :
# backend/models.py
from extensions import db # To this
from datetime import datetime

class Product(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(100), nullable=False)
    category = db.Column(db.String(50), nullable=False)
    description = db.Column(db.Text, nullable=True)
    price = db.Column(db.Float, nullable=False)
    stock_quantity = db.Column(db.Integer, nullable=False)
    image_url = db.Column(db.String(200), nullable=True)
    brand = db.Column(db.String(50), nullable=True)
    rating = db.Column(db.Float, nullable=True) # Max 5.0

    def __repr__(self):
        return f"<Product {self.name}>"

    def to_dict(self):
        return {
            'id': self.id,
            'name': self.name,
            'category': self.category,
            'description': self.description,
            'price': self.price,
            'stock_quantity': self.stock_quantity,
            'image_url': self.image_url,
            'brand': self.brand,
            'rating': self.rating
        }

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    password_hash = db.Column(db.String(128), nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=True)
    created_at = db.Column(db.DateTime, default=datetime.utcnow)

    def __repr__(self):
        return f"<User {self.username}>"

    def to_dict(self):
        return {
            'id': self.id,
            'username': self.username,
            'email': self.email,
            'created_at': self.created_at.isoformat()
        }

class ChatInteraction(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    user_id = db.Column(db.Integer, db.ForeignKey('user.id'), nullable=True) # Nullable if user is not logged in
    sender = db.Column(db.String(20), nullable=False) # "User" or "Chatbot"
    message = db.Column(db.Text, nullable=False)
    timestamp = db.Column(db.DateTime, default=datetime.utcnow)

    user = db.relationship('User', backref=db.backref('chat_interactions', lazy=True))

    def __repr__(self):
        return f"<Chat {self.id} from {self.sender}>"

    def to_dict(self):
        return {
            'id': self.id,
            'user_id': self.user_id,
            'sender': self.sender,
            'message': self.message,
            'timestamp': self.timestamp.isoformat()
        }

    # backend/seed_data.py
import os
from faker import Faker
from extensions import db, bcrypt
from models import Product, User, ChatInteraction
from app import app # Import the app instance

fake = Faker()

def generate_mock_products(num_products=120): # Generate slightly more than 100
    print(f"Generating {num_products} mock products (Category: Clothes)...")
    products = []
    clothing_types = [
        "T-Shirt", "Jeans", "Dress", "Sweater", "Jacket", "Skirt",
        "Hoodie", "Shirt", "Pants", "Shorts", "Blouse", "Coat",
        "Socks", "Underwear", "Pyjamas", "Activewear Set", "Swimsuit",
        "Cardigan", "Vest", "Leggings", "Romper", "Jumpsuit", "Maxi Dress"
    ]
    clothing_brands = [
        "Zara", "H&M", "Nike", "Adidas", "Levi's", "Gap", "Uniqlo",
        "Calvin Klein", "Tommy Hilfiger", "Forever 21", "ASOS", "Mango",
        "Puma", "Reebok", "American Eagle", "Old Navy", "Patagonia"
    ]
    materials = [
        "Cotton", "Polyester", "Wool", "Silk", "Linen", "Denim",
        "Spandex", "Nylon", "Rayon", "Viscose", "Blend"
    ]
    colors = [
        "Red", "Blue", "Green", "Black", "White", "Gray", "Pink",
        "Yellow", "Purple", "Orange", "Brown", "Beige", "Navy"
    ]

    for i in range(num_products):
        clothing_type = fake.random_element(elements=clothing_types)
        brand = fake.random_element(elements=clothing_brands)
        material = fake.random_element(elements=materials)
        color = fake.random_element(elements=colors)

        product = Product(
            name=f"{brand} {color} {material} {clothing_type}",
            category="Clothes",
            description=f"A comfortable and stylish {color} {clothing_type} made from {material} for everyday wear.",
            price=round(fake.random_element(elements=[19.99, 24.99, 29.99, 34.99, 39.99, 49.99, 59.99, 69.99, 79.99, 89.99, 99.99, 109.99, 119.99, 129.99, 139.99]), 2),
            stock_quantity=fake.random_int(min=5, max=200),
            image_url=f"https://via.placeholder.com/150/{"".join(fake.hex_color().split('#')[1:])}?text={clothing_type.replace(' ', '+')}", # Placeholder image
            brand=brand,
            rating=round(fake.random_element(elements=[3.0, 3.5, 4.0, 4.2, 4.5, 4.8, 5.0]), 1)
        )
        products.append(product)
    return products

def generate_mock_user(username="testuser", password="testpassword"):
    print(f"Generating mock user: {username}")
    hashed_password = bcrypt.generate_password_hash(password).decode('utf-8')
    user = User(
        username=username,
        password_hash=hashed_password,
        email=fake.email()
    )
    return user

def seed_data():
    with app.app_context():
        # Clear existing data (optional, but good for re-seeding)
        db.session.query(Product).delete()
        db.session.query(User).delete()
        db.session.query(ChatInteraction).delete() # Also clear chat history
        db.session.commit()
        print("Cleared existing data from Product, User, ChatInteraction tables.")

        # Seed Products
        products = generate_mock_products()
        db.session.add_all(products)
        print(f"Added {len(products)} products to the database.")

        # Seed User
        user = generate_mock_user()
        db.session.add(user)
        print("Added test user to the database.")

        db.session.commit()
        print("Database seeding complete!")

if __name__ == '__main__':
    seed_data()

FRONTEND CODES :

CSS :
.App {
  text-align: center;
  font-family: Arial, sans-serif;
  max-width: 800px;
  margin: 20px auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  background-color: #f9f9f9;
}

.App-header {
  background-color: #282c34;
  padding: 20px;
  color: white;
  border-radius: 8px 8px 0 0;
  margin-bottom: 20px;
}

.App-header h1 {
  margin: 0;
  font-size: 1.8em;
}

.App-header button {
  background-color: #61dafb;
  color: #282c34;
  border: none;
  padding: 8px 15px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 0.9em;
  margin-left: 10px;
}

.auth-section, .chatbot-section {
  padding: 20px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
}

.auth-section h2, .chatbot-section h2 {
  color: #333;
  margin-top: 0;
}

form div {
  margin-bottom: 10px;
}

form label {
  display: inline-block;
  width: 100px;
  text-align: right;
  margin-right: 10px;
  color: #555;
}

form input[type="text"],
form input[type="password"],
form input[type="email"] {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  width: 200px;
}

form button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 10px;
  font-size: 1em;
}

form button:hover {
  background-color: #0056b3;
}

.auth-section p {
  margin-top: 15px;
  color: #d9534f; /* For error messages */
}

.auth-section p:empty {
    display: none; /* Hide if no message */
}

.chat-history {
  border: 1px solid #eee;
  padding: 10px;
  height: 250px;
  overflow-y: auto;
  margin-bottom: 15px;
  text-align: left;
  background-color: #f0f0f0;
  border-radius: 5px;
}

.chat-history p {
  margin: 5px 0;
  padding: 5px 10px;
  border-radius: 5px;
  word-wrap: break-word;
}

.chat-history p strong {
  color: #007bff;
}

.chat-history p:nth-child(even) {
  background-color: #e6e6e6; /* Chatbot messages background */
}

.chatbot-section input[type="text"] {
  width: calc(100% - 80px); /* Adjust width to fit button */
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin-right: 5px;
}

.chatbot-section button {
  background-color: #28a745;
  color: white;
  border: none;
  padding: 10px 15px;
  border-radius: 5px;
  cursor: pointer;
}

.chatbot-section button:hover {
  background-color: #218838;
}

.chatbot-section button:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}

JS :
import React, { useState } from 'react';
import './App.css'; // Assuming you still have App.css for basic styling

function App() {
  const [token, setToken] = useState(null); // To store JWT token
  const [userId, setUserId] = useState(null); // To store user ID
  const [username, setUsername] = useState(''); // To store username

  // State for login/register form
  const [loginUsername, setLoginUsername] = useState('');
  const [loginPassword, setLoginPassword] = useState('');
  const [loginEmail, setLoginEmail] = useState(''); // For registration
  const [isRegistering, setIsRegistering] = useState(false); // To toggle between login/register
  const [authMessage, setAuthMessage] = useState(''); // For auth feedback

  // State for chatbot
  const [chatInput, setChatInput] = useState('');
  const [chatHistory, setChatHistory] = useState([]);
  const [chatbotResponse, setChatbotResponse] = useState('');

  const API_BASE_URL = 'http://127.0.0.1:5000'; // Your Flask backend URL

  // --- Authentication Functions ---
  const handleAuth = async (endpoint) => {
    setAuthMessage(''); // Clear previous messages
    // THIS LINE WAS INCORRECT AND IS NOW FIXED:
    const url = `${API_BASE_URL}/${endpoint}`; // Correct template literal syntax
    const payload = {
      username: loginUsername,
      password: loginPassword,
    };
    if (endpoint === 'register') {
      payload.email = loginEmail;
    }

    try {
      const response = await fetch(url, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(payload),
      });

      const data = await response.json();

      if (response.ok) {
        setAuthMessage(data.message || 'Success!');
        if (endpoint === 'login') {
          setToken(data.access_token);
          setUserId(data.user_id);
          setUsername(data.username);
          // Clear form fields on successful login
          setLoginUsername('');
          setLoginPassword('');
          setLoginEmail('');
        }
      } else {
        setAuthMessage(data.message || 'An error occurred.');
      }
    } catch (error) {
      console.error('Authentication error:', error);
      setAuthMessage('Network error. Could not connect to the server.');
    }
  };

  const handleLogout = () => {
    setToken(null);
    setUserId(null);
    setUsername('');
    setChatHistory([]); // Clear chat history on logout
    setChatbotResponse('');
    setAuthMessage('Logged out successfully.');
  };


  // --- Chatbot Functions ---
  const handleChatSubmit = async (e) => {
    e.preventDefault(); // Prevent form default submission
    if (!chatInput.trim() || !token) return;

    const userMessage = chatInput;
    setChatInput(''); // Clear input immediately
    setChatHistory(prev => [...prev, { sender: 'User', message: userMessage }]); // Add user message to history

    try {
      const response = await fetch(`${API_BASE_URL}/chat/ask`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Authorization': `Bearer ${token}`
        },
        body: JSON.stringify({ message: userMessage }),
      });

      const data = await response.json();

      if (response.ok) {
        setChatbotResponse(data.chatbot_response);
        setChatHistory(prev => [...prev, { sender: 'Chatbot', message: data.chatbot_response }]);
      } else {
        setChatbotResponse(data.message || 'Error getting response from chatbot.');
      }
    } catch (error) {
      console.error('Chatbot API error:', error);
      setChatbotResponse('Network error. Could not connect to chatbot.');
    }
  };

  return (
    <div className="App">
      <header className="App-header">
        <h1>E-commerce Chatbot App</h1>
        {token && (
          <p>Welcome, {username}! <button onClick={handleLogout}>Logout</button></p>
        )}
      </header>

      <main>
        {!token ? (
          <section className="auth-section">
            <h2>{isRegistering ? 'Register' : 'Login'}</h2>
            <form onSubmit={(e) => {
              e.preventDefault();
              handleAuth(isRegistering ? 'register' : 'login');
            }}>
              <div>
                <label>Username:</label>
                <input
                  type="text"
                  value={loginUsername}
                  onChange={(e) => setLoginUsername(e.target.value)}
                  required
                />
              </div>
              <div>
                <label>Password:</label>
                <input
                  type="password"
                  value={loginPassword}
                  onChange={(e) => setLoginPassword(e.target.value)}
                  required
                />
              </div>
              {isRegistering && (
                <div>
                  <label>Email (optional):</label>
                  <input
                    type="email"
                    value={loginEmail}
                    onChange={(e) => setLoginEmail(e.target.value)}
                  />
                </div>
              )}
              <button type="submit">{isRegistering ? 'Register' : 'Login'}</button>
            </form>
            <p>{authMessage}</p>
            <button onClick={() => setIsRegistering(prev => !prev)}>
              {isRegistering ? 'Already have an account? Login' : 'Need an account? Register'}
            </button>
          </section>
        ) : (
          <section className="chatbot-section">
            <h2>Chat with the E-commerce Bot</h2>
            <div className="chat-history">
              {chatHistory.map((chat, index) => (
                <p key={index}><strong>{chat.sender}:</strong> {chat.message}</p>
              ))}
              {/* Display pending chatbot response if available */}
              {chatbotResponse && chatHistory.length > 0 && chatHistory[chatHistory.length - 1].sender === 'User' && (
                <p><strong>Chatbot:</strong> {chatbotResponse}</p>
              )}
            </div>
            <form onSubmit={handleChatSubmit}>
              <input
                type="text"
                value={chatInput}
                onChange={(e) => setChatInput(e.target.value)}
                placeholder="Ask the bot something..."
                disabled={!token}
              />
              <button type="submit" disabled={!token}>Send</button>
            </form>
          </section>
        )}
      </main>
    </div>
  );
}

export default App;

test.js :
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders learn react link', () => {
  render(<App />);
  const linkElement = screen.getByText(/learn react/i);
  expect(linkElement).toBeInTheDocument();
});


index.css :
body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
    'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

code {
  font-family: source-code-pro, Menlo, Monaco, Consolas, 'Courier New',
    monospace;
}

index.js :
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();


const reportWebVitals = onPerfEntry => {
  if (onPerfEntry && onPerfEntry instanceof Function) {
    import('web-vitals').then(({ getCLS, getFID, getFCP, getLCP, getTTFB }) => {
      getCLS(onPerfEntry);
      getFID(onPerfEntry);
      getFCP(onPerfEntry);
      getLCP(onPerfEntry);
      getTTFB(onPerfEntry);
    });
  }
};

export default reportWebVitals;


