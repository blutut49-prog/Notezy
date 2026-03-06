studysnap-mvp
 ├ index.html
 ├ script.js
 └ style.css<!DOCTYPE html>
<html>
<head>
<title>StudySnap MVP</title>
<link rel="stylesheet" href="style.css">
</head>

<body>

<h1>StudySnap</h1>
<p>Turn notes into flashcards instantly</p>

<input type="file" id="imageUpload">

<button onclick="analyzeNotes()">Generate Study Kit</button>

<h2>Summary</h2>
<div id="summary"></div>

<h2>Flashcards</h2>
<div id="flashcards"></div>

<h2>Quiz</h2>
<div id="quiz"></div>

<script src="script.js"></script>

</body>
</html>body{
font-family: Arial;
max-width: 600px;
margin: auto;
padding: 20px;
background: #f4f4f4;
}

button{
padding:10px;
font-size:16px;
margin-top:10px;
}

div{
background:white;
padding:10px;
margin-top:10px;
border-radius:6px;
}async function analyzeNotes() {

const fileInput = document.getElementById("imageUpload");
const file = fileInput.files[0];

const reader = new FileReader();

reader.onload = async function() {

const base64Image = reader.result.split(",")[1];

const response = await fetch("https://api.openai.com/v1/chat/completions", {
method: "POST",
headers: {
"Content-Type": "application/json",
"Authorization": "Bearer YOUR_API_KEY"
},
body: JSON.stringify({
model: "gpt-4.1",
messages: [
{
role: "user",
content: [
{
type: "text",
text: "Summarize these notes, make flashcards and a short quiz."
},
{
type: "image_url",
image_url: {
"url": `data:image/jpeg;base64,${base64Image}`
}
}
]
}
]
})
});

const data = await response.json();

const result = data.choices[0].message.content;

document.getElementById("summary").innerText = result;

};

reader.readAsDataURL(file);

}
