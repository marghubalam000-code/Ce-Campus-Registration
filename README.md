<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Catalyst Educational Campus</title>

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;600;800&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/emailjs-com@3/dist/email.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>
body{font-family:'Poppins',sans-serif;background:linear-gradient(135deg,#00c9ff,#92fe9d);margin:0;}
.container{max-width:520px;margin:30px auto;background:#fff;padding:25px;border-radius:20px;box-shadow:0 15px 35px rgba(0,0,0,0.3);} 
.title{text-align:center;font-size:28px;font-weight:800;background:linear-gradient(90deg,#ff512f,#dd2476,#ff00cc);-webkit-background-clip:text;color:transparent;}
.subtitle{text-align:center;color:#2e7d32;margin-top:-10px;}
input,select{width:100%;padding:12px;margin:10px 0;border-radius:10px;border:1px solid #ccc;}
button{width:100%;padding:14px;border:none;border-radius:12px;font-size:16px;font-weight:bold;cursor:pointer;background:linear-gradient(135deg,#28a745,#00c853);color:white;}
.hidden{display:none;}
</style>
</head>

<body>

<div class="container">
<div class="title">Catalyst Educational Campus</div>
<div class="subtitle">Student Registration</div>

<form id="form">
<input id="name" placeholder="Student Name" required>
<input id="father" placeholder="Father Name" required>
<input id="mobile" placeholder="Mobile" required>

<select id="class">
<option value="">Select Class</option>
<option>Class 10</option>
<option>Class 11</option>
<option>Class 12</option>
</select>

<select id="stream" onchange="showSubjects()" required>
<option value="">Select Stream</option>
<option value="Science">Science</option>
<option value="Commerce">Commerce</option>
<option value="Arts">Arts</option>
</select>

<div id="scienceBox" class="hidden">
<select id="scienceSubject">
<option value="">Select Subject</option>
<option>Mathematics</option>
<option>Biology</option>
</select>
</div>

<div id="artsBox" class="hidden">
<select id="artsSubject">
<option value="">Select Subject</option>
<option>History</option>
<option>Geography</option>
<option>Political Science</option>
<option>Economics</option>
</select>
</div>

<label>Photo</label>
<input type="file" id="photo" required>

<button type="submit">🚀 Register Now</button>
</form>

<p id="msg"></p>
</div>

<script>
emailjs.init("q7WRi2qk3AUR725UG");

function showSubjects(){
 let stream=document.getElementById("stream").value;
 document.getElementById("scienceBox").style.display = (stream==="Science")?"block":"none";
 document.getElementById("artsBox").style.display = (stream==="Arts")?"block":"none":"none";
 document.getElementById("artsBox").style.display = (stream==="Arts")?"block":"none";
}

form.addEventListener("submit",function(e){
 e.preventDefault();

 let file=document.getElementById("photo").files[0];
 let reader=new FileReader();

 reader.onloadend=function(){
  let photo=reader.result;

  let stream=document.getElementById("stream").value;
  let subject="";

  if(stream==="Science") subject=document.getElementById("scienceSubject").value;
  if(stream==="Arts") subject=document.getElementById("artsSubject").value;
  if(stream==="Commerce") subject="Commerce";

  let data={
    name:name.value,
    father:father.value,
    mobile:mobile.value,
    class:document.getElementById("class").value,
    stream:stream,
    subject:subject,
    date:new Date().toLocaleString()
  };

  // EMAIL
  emailjs.send("service_bnw6tan","__ejs-test-mail-service__",data);

  // PDF FIXED DESIGN
  const { jsPDF } = window.jspdf;
  let doc=new jsPDF();

  // HEADER
  doc.setFontSize(18);
  doc.setTextColor(40,167,69);
  doc.text("CATALYST EDUCATIONAL CAMPUS",20,20);

  doc.setFontSize(12);
  doc.setTextColor(0,0,0);

  doc.text("Registration Slip",20,30);

  doc.text(`Name: ${data.name}`,20,45);
  doc.text(`Father: ${data.father}`,20,55);
  doc.text(`Mobile: ${data.mobile}`,20,65);
  doc.text(`Class: ${data.class}`,20,75);
  doc.text(`Stream: ${data.stream}`,20,85);
  doc.text(`Subject: ${data.subject}`,20,95);
  doc.text(`Date: ${data.date}`,20,105);

  // PHOTO
  doc.addImage(photo,'JPEG',140,40,40,40);

  doc.save(data.name+"_Slip.pdf");

  document.getElementById("msg").innerText="✅ Registration + PDF + Email Done";
 };

 reader.readAsDataURL(file);
});
</script>

</body>
</html>
