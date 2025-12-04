<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<title>Anime Quiz Oyunu</title>
<style>
body { font-family: Arial; background:#f0f0f0; text-align:center; padding:20px; }
.quiz-container { background:#fff; padding:20px; border-radius:10px; max-width:600px; margin:auto; box-shadow:0 5px 10px rgba(0,0,0,0.2); }
button { padding:10px 20px; margin:5px; border-radius:5px; border:none; cursor:pointer; }
.correct { background-color:#4CAF50; color:white; }
.wrong { background-color:#f44336; color:white; }
.premium { background-color:#ff9800; color:white; }
#hint { font-style:italic; color:#555; }
.ads { margin:20px 0; font-style:italic; color:#888; }
</style>
</head>
<body>

<div class="quiz-container">
<h1>Anime Quiz Oyunu</h1>
<div id="question"></div>
<div id="options"></div>
<p id="hint"></p>
<p id="score">Puan: 0</p>

<div class="ads">
Reklam alanı (Gerçek reklam için internete ve sunucuya ihtiyacınız var)
</div>

<button id="nextBtn">Sonraki Soru</button>
<div style="margin-top:20px;">
<a href="https://www.patreon.com/kullaniciadi" target="_blank">
<button>Destek Ol / Patreon</button>
</a>
</div>
</div>

<script>
// Premium kullanıcı kontrolü
let userIsPremium = false; // true yaparsan premium sorular açılır

// 100 soruluk quiz dizisi (tüm sorular burada olacak)
let quizQuestions = [
  {soru:"Naruto'da ana karakterin tam adı nedir?", seçenekler:["Naruto Uchiha","Naruto Uzumaki","Naruto Hatake","Naruto Haruno"], cevap:"Naruto Uzumaki", hint:"Şakacı ninja, sarı saçlı", zorluk:"Kolay", kategori:"Naruto", premium:false},
  {soru:"Dragon Ball’da Kamehameha tekniğini öğreten üstad kimdir?", seçenekler:["Goku","Vegeta","Master Roshi","Piccolo"], cevap:"Master Roshi", hint:"Yaşlı kaplumbağa", zorluk:"Kolay", kategori:"Dragon Ball", premium:false},
  {soru:"Death Note defterine adı yazılan kişi ne olur?", seçenekler:["Ölür","Zengin olur","Görünmez olur","Kahraman olur"], cevap:"Ölür", hint:"Ad + yüz düşerse ölüm", zorluk:"Orta", kategori:"Death Note", premium:true},
  // ... tüm 100 soruyu buraya ekle
];

// Dizi karıştırma fonksiyonu
function shuffleArray(array) {
  for (let i = array.length - 1; i > 0; i--) {
    let j = Math.floor(Math.random() * (i + 1));
    [array[i], array[j]] = [array[j], array[i]];
  }
}

// Premium filtreleme ve karıştırma
let filteredQuestions = quizQuestions.filter(q => userIsPremium || !q.premium);
shuffleArray(filteredQuestions);

// Quiz mantığı
let currentQuestion = 0;
let score = 0;

function showQuestion() {
  if(currentQuestion >= filteredQuestions.length){
    document.getElementById("question").innerHTML = `<h2>Oyun bitti!</h2><p>Toplam puanınız: ${score}</p><button onclick="restartQuiz()">Yeniden Oyna</button>`;
    document.getElementById("options").innerHTML = "";
    document.getElementById("hint").innerText = "";
    return;
  }

  let q = filteredQuestions[currentQuestion];
  document.getElementById("question").innerText = q.soru;
  document.getElementById("hint").innerText = "İpucu: " + q.hint;

  let optionsHTML = "";
  q.seçenekler.forEach(opt => {
    optionsHTML += `<button onclick="checkAnswer('${opt}')">${opt}</button>`;
  });
  document.getElementById("options").innerHTML = optionsHTML;
}

function checkAnswer(answer){
  let q = filteredQuestions[currentQuestion];
  let buttons = document.querySelectorAll("#options button");

  if(answer === q.cevap){
    score += 10;
    buttons.forEach(b => { if(b.innerText===answer) b.classList.add("correct"); });
  } else {
    buttons.forEach(b => {
      if(b.innerText===answer) b.classList.add("wrong");
      if(b.innerText===q.cevap) b.classList.add("correct");
    });
  }

  document.getElementById("score").innerText = "Puan: " + score;
}

document.getElementById("nextBtn").addEventListener("click", ()=>{
  currentQuestion++;
  showQuestion();
});

function restartQuiz(){
  currentQuestion = 0;
  score = 0;
  filteredQuestions = quizQuestions.filter(q => userIsPremium || !q.premium);
  shuffleArray(filteredQuestions);
  showQuestion();
}

// İlk soruyu göster
showQuestion();
</script>

</body>
</html>
