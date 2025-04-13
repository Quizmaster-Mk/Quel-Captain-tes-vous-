<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Quel Captain America êtes-vous ?</title>
  <style>
    body {
      font-family: 'Courier New', monospace;
      background-color: #001f3f; /* Bleu Marine */
      color: #fff;
      margin: 0;
      padding: 20px;
      text-align: center;
    }

    h1 {
      color: #ff4136; /* Rouge Captain America */
      text-shadow: 2px 2px 5px #0074D9; /* Bleu clair */
      font-size: 40px;
      margin-bottom: 20px;
    }

    img.logo {
      max-width: 200px;
      margin-bottom: 20px;
    }

    .question {
      margin: 20px 0;
      background-color: #0074D9; /* Bleu clair */
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(255, 65, 54, 0.7);
    }

    .options button {
      background-color: #ff4136;
      color: #fff;
      padding: 10px 20px;
      border: none;
      margin: 5px;
      font-size: 18px;
      cursor: pointer;
      border-radius: 5px;
      transition: all 0.3s ease;
    }

    .options button:hover {
      background-color: #2ECC40; /* Vert clair patriotique */
      transform: scale(1.1);
    }

    .result {
      background-color: #0074D9;
      padding: 20px;
      border-radius: 10px;
      margin-top: 20px;
      display: none;
      box-shadow: 0 0 10px rgba(255, 65, 54, 0.7);
    }

    .score {
      font-size: 18px;
      margin-top: 10px;
      color: #7FDBFF;
    }
  </style>
</head>
<body>
  <img class="logo" src="https://zupimages.net/up/21/41/1ypk.png" alt="Logo Captain America">
  <h1>Quel Captain America êtes-vous ?</h1>

  <div id="quiz-container">
    <div class="question" id="question-container">
      <h2 id="question-text"></h2>
      <div class="options" id="options-container"></div>
    </div>
  </div>

  <div class="result" id="result-container">
    <h2>Vous êtes...</h2>
    <p id="result-text"></p>
    <p class="score">Votre résultat est basé sur vos choix !</p>
    <button onclick="startQuiz()">Recommencer le quiz</button>
  </div>

  <script>
    const questions = [
      { question: "Quelle est votre plus grande qualité ?", options: ["Courage", "Loyauté", "Altruisme", "Détermination"], result: [0, 1, 2, 3] },
      { question: "Quel type de mission préférez-vous ?", options: ["Leadership", "Infiltration", "Sauvetage", "Stratégie"], result: [4, 5, 6, 7] },
      { question: "Comment réagissez-vous face au danger ?", options: ["Avec sang-froid", "En première ligne", "Avec intelligence", "Avec passion"], result: [8, 9, 0, 1] },
      { question: "Votre ennemi juré est...", options: ["Hydra", "Crâne Rouge", "Vous-même", "L’injustice"], result: [2, 3, 4, 5] },
      { question: "Quel est votre symbole ?", options: ["Bouclier", "Étoile", "Justice", "Peuple"], result: [6, 7, 8, 9] },
      { question: "Votre époque préférée ?", options: ["Années 40", "Temps modernes", "Futur incertain", "Peu importe, tant que je me bats pour le bien"], result: [0, 1, 2, 3] },
      { question: "Comment les autres vous perçoivent ?", options: ["Héros", "Relique", "Leader", "Frère d’armes"], result: [4, 5, 6, 7] },
      { question: "Quelle valeur vous guide ?", options: ["Liberté", "Justice", "Responsabilité", "Honneur"], result: [8, 9, 0, 1] },
      { question: "Votre allié de cœur ?", options: ["Bucky", "Sam", "Nick Fury", "Les Avengers"], result: [2, 3, 4, 5] },
      { question: "Votre plus grand combat est contre...", options: ["La peur", "La trahison", "La guerre", "Le doute"] , result: [6, 7, 8, 9] }
    ];

    const characters = [
      { name: "Steve Rogers", description: "Le Captain original, emblème de droiture, courage et sacrifice pour le plus grand nombre." },
      { name: "Captain America moderne", description: "Adapté à son époque, diplomate et charismatique, il inspire par son calme et sa force intérieure." },
      { name: "Nomad", description: "Le Captain sans patrie, rebelle à toute autorité corrompue, fidèle à ses valeurs plus qu'à un drapeau." },
      { name: "Cap du Multivers", description: "Héros alternatif prêt à tout pour préserver l'équilibre des mondes parallèles." },
      { name: "Sam Wilson", description: "Faucon devenu Captain, il incarne l'héritage tout en portant une nouvelle voix pour la justice." },
      { name: "Bucky Barnes", description: "Le Soldat de l'hiver réhabilité, protecteur, stratège et fidèle même dans l'ombre." },
      { name: "Agent Peggy Carter (What If)", description: "Combattante brillante et téméraire, incarnation d’un Captain inspirant venu d’une autre réalité." },
      { name: "Isaiah Bradley", description: "Héros oublié mais fondamental, il symbolise résilience et espoir pour les générations futures." },
      { name: "Captain Hydra", description: "Version corrompue du Captain, preuve que même les symboles peuvent être manipulés." },
      { name: "Captain Ultimates", description: "Plus militaire et rigide, il fait passer l’ordre et la mission avant tout." }
    ];

    let currentQuestion = 0;
    let userAnswers = [];

    function startQuiz() {
      currentQuestion = 0;
      userAnswers = [];
      document.getElementById('result-container').style.display = 'none';
      document.getElementById('quiz-container').style.display = 'block';
      showQuestion();
    }

    function showQuestion() {
      const question = questions[currentQuestion];
      document.getElementById('question-text').innerText = question.question;

      const optionsContainer = document.getElementById('options-container');
      optionsContainer.innerHTML = '';

      question.options.forEach((option, index) => {
        const button = document.createElement('button');
        button.innerText = option;
        button.onclick = () => {
          userAnswers.push(question.result[index]);
          currentQuestion++;
          if (currentQuestion < questions.length) {
            showQuestion();
          } else {
            showResult();
          }
        };
        optionsContainer.appendChild(button);
      });
    }

    function showResult() {
      const resultIndex = userAnswers.reduce((a, b) => a + b, 0) % characters.length;
      const result = characters[resultIndex];

      document.getElementById('result-text').innerText = `${result.name} - ${result.description}`;
      document.getElementById('quiz-container').style.display = 'none';
      document.getElementById('result-container').style.display = 'block';
    }

    startQuiz();
  </script>
</body>
</html>
