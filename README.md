<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Catalyst Educational Campus</title>

<script src="https://cdn.jsdelivr.net/npm/emailjs-com@3/dist/email.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>
body{font-family:Arial;background:linear-gradient(135deg,#00c9ff,#92fe9d);} 
.container{max-width:500px;margin:auto;background:#fff;padding:20px;border-radius:15px;} 
button{background:green;color:white;padding:12px;width:100%;border:none;} 
.hidden{display:none;} 
.slip{background:#eaffea;padding:10px;margin-top:10px;border:1px solid green;} 
</style>
</head>

<body>

<div class="container">
<h2 style="text-align:center;">CATALYST EDUCATIONAL CAMPUS</h2>

<form id="form">
<input id="name" placeholder="Student Name" required>
<input id="father" placeholder="Father Name" required>
<input id="mobile" placeholder="Mobile" required>
<input id="address" placeholder="Address" required>
<input id="aadhar" placeholder="Aadhar Number" required>

<select id="class" required>
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

<br>
<input type="file" id="photo" required><br><br>

<button type="submit">Register</button>

</form>

<p id="msg"></p>
<div id="slip"></div>

</div>

<script>
emailjs.init("q7WRi2qk3AUR725UG");

function showSubjects(){
 let stream=document.getElementById("stream").value;
 document.getElementById("scienceBox").style.display = (stream==="Science")?"block":"none";
 document.getElementById("artsBox").style.display = (stream==="Arts")?"block":"none":"none";
 document.getElementById("artsBox").style.display = (stream==="Arts")?"block":"none";
}

document.getElementById("form").addEventListener("submit", function(e){
 e.preventDefault();

 let stream=document.getElementById("stream").value;
 let subject="";

 if(stream==="Science"){
   subject=document.getElementById("scienceSubject").value;
   if(!subject){ alert("Select Science Subject"); return; }
 }

 if(stream==="Arts"){
   subject=document.getElementById("artsSubject").value;
   if(!subject){ alert("Select Arts Subject"); return; }
 }

 if(stream==="Commerce"){
   subject="Commerce";
 }

 let file=document.getElementById("photo").files[0];
 if(!file){ alert("Upload photo"); return; }

 let reader=new FileReader();

 reader.onload = function(event){

  let photo = event.target.result;

  let data={
    name:document.getElementById("name").value,
    father:document.getElementById("father").value,
    mobile:document.getElementById("mobile").value,
    address:document.getElementById("address").value,
    aadhar:document.getElementById("aadhar").value,
    class:document.getElementById("class").value,
    stream:stream,
    subject:subject,
    date:new Date().toLocaleString()
  };

  // EMAIL
  emailjs.send("service_bnw6tan","__ejs-test-mail-service__",data);

  // POPUP
  alert("🎉 Welcome to CE Campus, " + data.name);

  // SLIP
  document.getElementById("slip").innerHTML = `
  <div class="slip">
  <h3>Registration Slip</h3>
  Name: ${data.name}<br>
  Class: ${data.class}<br>
  Stream: ${data.stream}<br>
  Subject: ${data.subject}<br>
  Address: ${data.address}<br>
  Aadhar: ${data.aadhar}<br>
  Date: ${data.date}
  </div>`;

  // PDF
  const { jsPDF } = window.jspdf;
  let doc=new jsPDF();

  doc.setFontSize(16);
  doc.text("CATALYST EDUCATIONAL CAMPUS",20,20);

  doc.setFontSize(12);
  doc.text("Name: "+data.name,20,40);
  doc.text("Father: "+data.father,20,50);
  doc.text("Mobile: "+data.mobile,20,60);
  doc.text("Address: "+data.address,20,70);
  doc.text("Aadhar: "+data.aadhar,20,80);
  doc.text("Class: "+data.class,20,90);
  doc.text("Stream: "+data.stream,20,100);
  doc.text("Subject: "+data.subject,20,110);

  doc.addImage(photo,'JPEG',140,40,40,40);

  doc.save(data.name+"_Slip.pdf");

  document.getElementById("msg").innerText="✅ Registration Successful";
 };

 reader.readAsDataURL(file);

});

</script>

</body>
</html>
