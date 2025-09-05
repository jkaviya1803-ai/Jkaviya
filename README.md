<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width,initial-scale=1.0">
  <title>Student Portfolio + Quiz + Blog</title>
  <link rel="stylesheet" href="style.css">

  <!-- EmailJS SDK -->
  <script type="text/javascript" src="https://cdn.emailjs.com/dist/email.min.js"></script>
  <script>
    (function () {
      emailjs.init("YOUR_PUBLIC_KEY"); // replace with your EmailJS Public Key
    })();
  </script>

  <style>
    /* Quiz Styles */
    .quiz-container {
      background: #fff;
      padding: 20px;
      border-radius: 12px;
      width: 400px;
      margin: 20px auto;
      box-shadow: 0px 5px 15px rgba(0,0,0,0.2);
      text-align: center;
    }
    .quiz-container h2 { margin-bottom: 15px; }
    .quiz-container .btn {
      display: block;
      width: 100%;
      margin: 8px 0;
      padding: 10px;
      background: #007BFF;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-size: 16px;
    }
    .quiz-container .btn:hover { background: #0056b3; }
    #next-btn { display: none; background: #28a745; }

    /* Blog Styles */
    .blog-container {
      max-width: 800px;
      margin: 20px auto;
      padding: 10px;
    }
    .post {
      background: white;
      padding: 15px;
      margin-bottom: 20px;
      border-radius: 8px;
      box-shadow: 0 0 5px rgba(0,0,0,0.1);
    }
    .post h2 { margin: 0 0 10px; }
    .post small { color: gray; font-size: 12px; }
  </style>
</head>
<body>

  <!-- Navbar -->
  <header>
    <nav class="navbar">
      <div class="logo">MyPortfolio</div>
      <ul class="nav-links" id="nav-links">
        <li><a href="#home">Home</a></li>
        <li><a href="#about">About</a></li>
        <li><a href="#projects">Projects</a></li>
        <li><a href="#quiz">Quiz</a></li>
        <li><a href="#blog">Blog</a></li>
        <li><a href="#contact">Contact</a></li>
      </ul>
      <div class="menu-toggle" id="menu-toggle">&#9776;</div>
    </nav>
  </header>

  <!-- Hero -->
  <section id="home" class="hero">
    <div class="hero-content">
      <h1>Hello, I'm <span>Ms kaviya J</span></h1>
      <p>I am a student</p>
      <a href="#projects" class="btn">View My Work</a>
    </div>
  </section>

  <!-- About -->
  <section id="about" class="about">
    <h2>About Me</h2>
    <div class="about-content">
      <img src="IMG_20250905-WA0018.jpg" alt="Profile Photo">
      <p>
        I am a student enthusiastic about technology and design. I love building interactive,
        user-friendly web applications and exploring new tools in the tech world.
      </p>
    </div>
  </section>

  <!-- Projects -->
  <section id="projects" class="projects">
    <h2>My Projects</h2>
    <div class="project-grid">
      <div class="project-card">
        <h3>Project 1</h3>
        <p>A simple responsive website.</p>
      </div>
      <div class="project-card">
        <h3>Project 2</h3>
        <p>JavaScript-based quiz app.</p>
      </div>
      <div class="project-card">
        <h3>Project 3</h3>
        <p>Personal blog with CMS.</p>
      </div>
    </div>
  </section>

  <!-- Quiz Section -->
  <section id="quiz" class="quiz">
    <h2 style="text-align:center;">GK History Quiz</h2>
    <div class="quiz-container">
      <h2 id="question">Question text</h2>
      <div id="answer-buttons"></div>
      <button id="next-btn" class="btn">Next</button>
    </div>
  </section>

  <!-- Blog Section -->
  <section id="blog" class="blog">
    <h2 style="text-align:center;">My Blog</h2>
    <div class="blog-container" id="postsContainer"></div>
  </section>

  <!-- Contact -->
  <section id="contact" class="contact">
    <h2>Contact Me</h2>
    <form id="contact-form">
      <input type="text" id="name" name="user_name" placeholder="Your Name" required>
      <input type="email" id="email" name="user_email" placeholder="Your Email" required>
      <textarea id="message" name="message" placeholder="Your Message" required></textarea>
      <button type="submit">Send</button>
    </form>
    <p id="form-status"></p>
  </section>

  <!-- Footer -->
  <footer>
    <p>&copy; 2025 Ms kaviya J | All rights reserved</p>
  </footer>

  <!-- Quiz Script -->
  <script>
    const questions = [
      {
        question: "Who was the first President of India?",
        answers: [
          { text: "Dr. Rajendra Prasad", correct: true },
          { text: "Jawaharlal Nehru", correct: false },
          { text: "Mahatma Gandhi", correct: false },
          { text: "Sardar Patel", correct: false }
        ]
      },
      {
        question: "In which year did India gain Independence?",
        answers: [
          { text: "1942", correct: false },
          { text: "1947", correct: true },
          { text: "1950", correct: false },
          { text: "1935", correct: false }
        ]
      },
      {
        question: "Who was the founder of the Maurya Empire?",
        answers: [
          { text: "Ashoka", correct: false },
          { text: "Chandragupta Maurya", correct: true },
          { text: "Bindusara", correct: false },
          { text: "Bimbisara", correct: false }
        ]
      },
      {
        question: "Who is known as the Father of the Nation in India?",
        answers: [
          { text: "Mahatma Gandhi", correct: true },
          { text: "Subhas Chandra Bose", correct: false },
          { text: "Bhagat Singh", correct: false },
          { text: "Jawaharlal Nehru", correct: false }
        ]
      }
    ];

    const questionElement = document.getElementById("question");
    const answerButtons = document.getElementById("answer-buttons");
    const nextButton = document.getElementById("next-btn");

    let currentQuestionIndex = 0;
    let score = 0;

    function startQuiz() {
      currentQuestionIndex = 0;
      score = 0;
      nextButton.innerHTML = "Next";
      showQuestion();
    }

    function showQuestion() {
      resetState();
      let currentQuestion = questions[currentQuestionIndex];
      questionElement.innerHTML = currentQuestion.question;

      currentQuestion.answers.forEach(answer => {
        const button = document.createElement("button");
        button.innerHTML = answer.text;
        button.classList.add("btn");
        answerButtons.appendChild(button);
        button.addEventListener("click", () => selectAnswer(answer));
      });
    }

    function resetState() {
      nextButton.style.display = "none";
      while (answerButtons.firstChild) {
        answerButtons.removeChild(answerButtons.firstChild);
      }
    }

    function selectAnswer(answer) {
      if (answer.correct) {
        score++;
        alert("Correct ✅");
      } else {
        alert("Wrong ❌");
      }
      nextButton.style.display = "block";
    }

    function showScore() {
      resetState();
      questionElement.innerHTML = `Quiz Finished! 🎉<br> Your Score: ${score}/${questions.length}`;
      nextButton.innerHTML = "Play Again";
      nextButton.style.display = "block";
    }

    function handleNextButton() {
      currentQuestionIndex++;
      if (currentQuestionIndex < questions.length) {
        showQuestion();
      } else {
        showScore();
      }
    }

    nextButton.addEventListener("click", () => {
      if (currentQuestionIndex < questions.length) {
        handleNextButton();
      } else {
        startQuiz();
      }
    });

    startQuiz();
  </script>

  <!-- Blog Script -->
  <script>
    function loadPosts() {
      const postsContainer = document.getElementById("postsContainer");
      const posts = JSON.parse(localStorage.getItem("blogPosts")) || [];
      postsContainer.innerHTML = "";
      posts.reverse().forEach(post => {
        const postEl = document.createElement("div");
        postEl.classList.add("post");
        postEl.innerHTML = `
          <h2>${post.title}</h2>
          <small>${new Date(post.date).toLocaleString()}</small>
          <p>${post.content}</p>
        `;
        postsContainer.appendChild(postEl);
      });
    }
    loadPosts();
  </script>

</body>
</html>
