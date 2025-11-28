This week focuses on adding security-oriented improvements to a small Express.js project. It demonstrates secure HTTP headers, input validation and sanitization, password hashing, and JWT-based authentication.

Overview
Tech stack: Node.js, Express
Security enhancements:
Helmet for secure HTTP headers
Input validation with validator
Input sanitization
Password hashing with bcrypt
JWT-based authentication
Project is organized with a single route file for authentication under a vulnerable-looking umbrella but implemented with best-practice steps (validation, sanitization, hashing, and tokenization).

Repository Structure
secure-app/
app.js
package.json
package-lock.json
node_modules/
routes/
auth.js
Getting Started
Prerequisites
Node.js (seems Node 14+ recommended)
npm (comes with Node.js)
Install
Clone the repo
git clone https://github.com/MaryamAmjad-tech/Fix-vulnerabilities.git
Navigate to the project
cd secure-app
Install dependencies
npm install
Run the Server
Start the server
node app.js
Default port: 3000
You should see: "Server is running on port 3000"
Endpoints
POST /auth/register

Registers a new user
Body (JSON): { "email": "...", "password": "..." }
Validation: email must be valid
Sanitization: email normalized, password escaped
Password storage: hashed with bcrypt
POST /auth/login

Logs in a user
Body (JSON): { "email": "...", "password": "..." }
Finds user by email, compares password with bcrypt
On success, returns a JWT token: { token: "..." }
Note: This is a minimal, educational example. The user database is an in-memory array (not persistent). In production, replace with a real database and secure storage of secret keys.

Security Highlights
Helmet: Adds secure HTTP headers to protect against common vulnerabilities.
express.json(): Parses JSON request bodies safely.
Input validation:
Email format validated with validator.isEmail
Input sanitization:
Email normalized with validator.normalizeEmail
Password escaped with validator.escape to prevent basic injection vectors
Password hashing:
bcrypt.hash(password, 10) and bcrypt.compare for verification
JWT:
Sign a token with a secret key (replace 'your-secret-key' with a strong, environment-based value)
Token expires in 1 hour
Security notes:

Do not hardcode secret keys in code. Use environment variables (e.g., process.env.JWT_SECRET).
In production, store users in a real database and implement proper session management, rate limiting, and secure cookie transport if applicable.
Consider adding CSRF protection, input rate limiting, and better error handling for production readiness.
Testing
Use Thunder Client (or any API testing tool) to test:
POST http://localhost:3000/auth/register
Body: { "email": "test@example.com", "password": "secret123" }
POST http://localhost:3000/auth/login
Body: { "email": "test@example.com", "password": "secret123" }
Example Code Snippets
app.js (highlights)

const helmet = require('helmet');
app.use(helmet()); // Secure headers
app.use(express.json()); // Parse JSON bodies
app.use('/auth', authRoutes);
routes/auth.js (highlights)

Email validation: validator.isEmail(email)
Email sanitization: validator.normalizeEmail(email)
Password sanitization: validator.escape(password)
Password hashing: bcrypt.hash(cleanPassword, 10)
JWT generation:
jwt.sign({ email: user.email }, 'your-secret-key', { expiresIn: '1h' })
Troubleshooting
If the server doesn’t start, ensure Node.js and npm are installed and dependencies are installed (npm install).
If there’s a port conflict, change the PORT in app.js or kill the process using that port.
Future Improvements
Replace in-memory user store with a persistent database (e.g., MySQL, PostgreSQL, MongoDB).
Move secret keys to environment variables (e.g., using dotenv).
Add user input validation for password strength.
Implement protected routes requiring valid JWT.
Add unit/integration tests for registration, login, and token verification.
