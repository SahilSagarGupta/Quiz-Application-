# 🔐 QuizApplication

A web-based **Quiz Application** built with Java Servlets, JSP, and MongoDB — developed as part of the **Advanced Internet Programming (AIP)**, focused on **InterServlet Communication** and HTTP Session Management.

---

## 📌 Overview

This application allows users to register, log in, and attempt a 10-question multiple-choice quiz on Servlet and JSP concepts. The system tracks scores across attempts and displays user statistics on a profile page. All pages are protected from unauthorized access using session validation.

---

## 🚀 Features

- ✅ User Registration (Sign Up) with MongoDB persistence
- ✅ User Authentication (Sign In) with session creation
- ✅ Session-protected pages (Home, Profile, Quiz, Result)
- ✅ 10-question MCQ Quiz on Servlet & JSP topics
- ✅ Automatic answer evaluation and score calculation
- ✅ Quiz statistics tracking (attempts, last score, best score)
- ✅ Profile page displaying user info and quiz history
- ✅ Client-side form validation (password match, min length)
- ✅ Responsive dark-themed UI (HTML + CSS)

---

## 🛠️ Tech Stack

| Layer       | Technology                          |
|-------------|--------------------------------------|
| Backend     | Java Servlets (Jakarta EE)           |
| View        | JSP (Jakarta Server Pages)           |
| Frontend    | HTML5, CSS3, JavaScript              |
| Database    | MongoDB (v7.x, localhost:27017)      |
| Server      | Apache Tomcat v11.0                  |
| JDK         | Java SE 23                           |
| IDE         | Eclipse IDE (Enterprise Edition)     |

---

## 📦 Dependencies (JAR Files)

Place these JARs inside `src/main/webapp/WEB-INF/lib/`:

| JAR File                          | Purpose                          |
|-----------------------------------|----------------------------------|
| `bson-4.11.1.jar`                 | MongoDB BSON document model      |
| `mongodb-driver-core-4.11.1.jar`  | Core MongoDB driver              |
| `mongodb-driver-sync-4.11.1.jar`  | Synchronous MongoDB Java driver  |

---

## 📁 Project Structure

```
QuizApplication/
│
├── src/main/java/
│   └── com.servlet/
│       ├── HomeServlet.java        → Session guard → forwards to HomePage.jsp
│       ├── ProfileServlet.java     → Session guard → forwards to ProfilePage.jsp
│       ├── QuizServlet.java        → GET: show quiz | POST: evaluate & score
│       ├── SignInServlet.java      → Authenticate user via MongoDB, create session
│       └── SignUpServlet.java      → Register new user, insert document in MongoDB
│
└── src/main/webapp/
    ├── WEB-INF/
    │   ├── web.xml                 → Deployment descriptor
    │   └── lib/
    │       ├── bson-4.11.1.jar
    │       ├── mongodb-driver-core-4.11.1.jar
    │       └── mongodb-driver-sync-4.11.1.jar
    │
    ├── Index.html                  → Landing page (Sign In / Sign Up links)
    ├── SignIn.html                 → Login form → POST to SignInServlet
    ├── SignUp.html                 → Registration form → POST to SignUpServlet
    ├── HomePage.jsp                → Welcome page (after successful login)
    ├── ProfilePage.jsp             → User info + quiz statistics
    ├── QuizPage.jsp                → 10 MCQ questions form
    └── ResultPage.jsp              → Quiz score result display
```

---

## ⚙️ Prerequisites

Before running the project, ensure the following are installed:

- [Java JDK 23](https://www.oracle.com/java/technologies/downloads/)
- [Apache Tomcat 11.0](https://tomcat.apache.org/download-11.cgi)
- [Eclipse IDE for Enterprise Java Developers](https://www.eclipse.org/downloads/)
- [MongoDB Community Edition](https://www.mongodb.com/try/download/community)
- [MongoDB Compass](https://www.mongodb.com/try/download/compass) *(optional, for GUI)*

---

## 🗃️ Database Setup

1. Start the MongoDB service:
   ```bash
   mongod
   ```

2. MongoDB will run on `mongodb://localhost:27017` by default.

3. The application automatically creates the database and collection on first use:
   - **Database:** `aip_lab`
   - **Collection:** `users`

4. Each user document has the following structure:
   ```json
   {
     "fullname"  : "Sahil Sagar Gupta",
     "email"     : "example@gmail.com",
     "username"  : "sahil01",
     "password"  : "121212",
     "createdAt" : "09 Apr 2026, 10:52 am"
   }
   ```

> ⚠️ **Note:** Passwords are currently stored in plain text. For production use, implement hashing (e.g., BCrypt).

---

## ▶️ Steps to Run the Project

1. **Clone or import the project** into Eclipse:
   - `File` → `Import` → `Existing Maven Projects` → select `QuizApplication` folder

2. **Add JARs to the build path:**
   - Place all three MongoDB JARs in `src/main/webapp/WEB-INF/lib/`
   - Right-click project → `Build Path` → `Configure Build Path` → confirm JARs are listed

3. **Configure Tomcat in Eclipse:**
   - Go to `Window` → `Preferences` → `Server` → `Runtime Environments`
   - Add `Apache Tomcat v11.0` and point it to your Tomcat installation directory

4. **Start MongoDB:**
   ```bash
   mongod
   ```

5. **Run the application:**
   - Right-click project → `Run As` → `Run on Server`
   - Select your configured Tomcat server

6. **Open in browser:**
   ```
   http://localhost:8080/QuizApplication/Index.html
   ```

---

## 🖥️ Application Flow

```
Index.html
   ├── Sign Up → SignUpServlet → MongoDB (insert user) → SignIn.html
   └── Sign In → SignInServlet → MongoDB (verify user) → HttpSession → HomePage.jsp
                                                                  │
                                              ┌───────────────────┼───────────────────┐
                                              ▼                   ▼                   ▼
                                       ProfileServlet       QuizServlet (GET)    HomeServlet
                                              │                   │
                                       ProfilePage.jsp      QuizPage.jsp
                                                                  │
                                                         QuizServlet (POST)
                                                         Evaluate answers
                                                         Update session scores
                                                                  │
                                                           ResultPage.jsp
```

---

## 📋 Quiz Details

- **Total Questions:** 10 MCQs
- **Topic:** Java Servlets & JSP concepts
- **Scoring:** 1 point per correct answer
- **Statistics tracked via session:**
  - `quizAttempts` — total number of attempts
  - `quizLastScore` — score from the most recent attempt
  - `quizBestScore` — highest score across all attempts
  - `quizTotal` — total number of questions (10)

---

## 🌐 URL Mapping

| URL                              | Servlet / Page        | Method |
|----------------------------------|-----------------------|--------|
| `/Index.html`                    | Static HTML           | GET    |
| `/SignIn.html`                   | Static HTML           | GET    |
| `/SignUp.html`                   | Static HTML           | GET    |
| `/SignInServlet`                 | SignInServlet         | POST   |
| `/SignUpServlet`                 | SignUpServlet         | POST   |
| `/HomeServlet`                   | HomeServlet           | GET    |
| `/ProfileServlet`                | ProfileServlet        | GET    |
| `/QuizServlet`                   | QuizServlet           | GET    |
| `/QuizServlet`                   | QuizServlet           | POST   |
| `/HomePage.jsp`                  | JSP View              | —      |
| `/ProfilePage.jsp`               | JSP View              | —      |
| `/QuizPage.jsp`                  | JSP View              | —      |
| `/ResultPage.jsp`                | JSP View              | —      |

---

## 👤 Author

**Sahil Sagar Gupta**
Advanced Internet Programming Lab

---

## 📄 License

This project is developed for academic/educational purposes only.
