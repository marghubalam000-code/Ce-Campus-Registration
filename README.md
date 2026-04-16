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
.title{text-align:center;font-size:28px;font-weight:800;background:linear-gradient(90deg,#ff512f,#dd2476,#ff00cc);-webkit-background-clip:text;color:transparent;text-shadow:0 0 10px rgba(255,0,150,0.4);} 
.subtitle{text-align:center;color:#2e7d32;margin-top:-10px;}
input,select{width:100%;padding:12px;margin:10px 0;border-radius:10px;border:1px solid #ccc;}
button{width:100%;padding:14px;border:none;border-radius:12px;font-size:16px;font-weight:bold;cursor:pointer;background:linear-gradient(135deg,#28a745,#00c853);color:white;}
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
<option>Class 10</option>
<option>Class 11</option>
<option>Class 12</option>
</select>

<select id="stream">
<option>Science</option>
<option>Commerce</option>
<option>Arts</option>
</select>

<label>Photo</label>
<input type="file" id="photo" required>

<button type="submit">🚀 Register Now</button>
</form>

<p id="msg"></p>
</div>

<script>
// 🔑 EmailJS init (paste your PUBLIC KEY below)
emailjs.init("YOUR_PUBLIC_KEY");

form.addEventListener("submit",function(e){
 e.preventDefault();

 let file=document.getElementById("photo").files[0];
 let reader=new FileReader();

 reader.onloadend=function(){
  let photo=reader.result;

  let data={
    name:name.value,
    father:father.value,
    mobile:mobile.value,
    class:document.getElementById("class").value,
    stream:document.getElementById("stream").value,
    date:new Date().toLocaleString()
  };

  // 📧 SEND EMAIL (service id added)
  emailjs.send("service_bnw6tan","YOUR_TEMPLATE_ID",data)
  .then(()=>{
    console.log("Email Sent");
  });

  // 📄 PDF
  const { jsPDF } = window.jspdf;
  let doc=new jsPDF();

  doc.setFontSize(16);
  doc.text("Catalyst Educational Campus",20,20);

  doc.setFontSize(12);
  doc.text(`Name: ${data.name}`,20,40);
  doc.text(`Father: ${data.father}`,20,50);
  doc.text(`Mobile: ${data.mobile}`,20,60);
  doc.text(`Class: ${data.class}`,20,70);
  doc.text(`Stream: ${data.stream}`,20,80);
  doc.text(`Date: ${data.date}`,20,90);

  doc.addImage(photo,'JPEG',140,40,40,40);

  doc.save(data.name+".pdf");

  document.getElementById("msg").innerText="✅ Registration + Email Sent + PDF Downloaded";
 };

 reader.readAsDataURL(file);
});
</script>

</body>
</html>
