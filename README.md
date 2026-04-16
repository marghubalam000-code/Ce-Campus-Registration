<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Student Registration - Catalyst Educational Campus</title>
<style>
body { font-family: Arial; background:linear-gradient(135deg,#d4fc79,#96e6a1); padding:20px; }
.container { max-width:500px; margin:auto; background:white; padding:20px; border-radius:15px; box-shadow:0 0 15px #888; }
h2 { text-align:center; color:#2c7a2c; }
.logo { text-align:center; }
.logo img { width:120px; margin-bottom:10px; }
input, select { width:100%; padding:10px; margin:8px 0; border-radius:8px; border:1px solid #ccc; }
button { width:100%; padding:12px; background:#28a745; color:white; border:none; border-radius:8px; font-size:16px; cursor:pointer; }
button:hover { background:#218838; }
.success { color:green; text-align:center; margin-top:10px; font-weight:bold; }
.qr { text-align:center; margin:15px 0; }
.slip { background:#eaffea; padding:15px; margin-top:15px; border:1px solid green; border-radius:10px; }
.idcard { margin-top:15px; padding:15px; border:2px solid #28a745; text-align:center; border-radius:10px; background:#f0fff0; }
.idcard img { width:80px; height:80px; border-radius:50%; }
</style>
</head>
<body>

<div class="container">

<!-- LOGO -->
<div class="logo">
<img src="logo.png" alt="Logo">
</div>

<h2>Student Registration</h2>

<div class="qr">
<img src="qr.png" width="200">
<p><b>Scan & Pay ₹500 Registration Fee</b></p>
</div>

<form id="regForm">
<input type="text" id="name" placeholder="Student Name" required>
<input type="text" id="father" placeholder="Father Name" required>
<input type="number" id="mobile" placeholder="Mobile Number" required>
<input type="text" id="aadhar" placeholder="Aadhar Number" required>

<select id="class">
<option value="">Select Class</option>
<option>Class 8</option>
<option>Class 9</option>
<option>Class 10</option>
<option>Class 11</option>
<option>Class 12</option>
</select>

<select id="stream">
<option value="">Select Stream</option>
<option>Science</option>
<option>Commerce</option>
<option>Arts</option>
</select>

<label>Upload Profile Photo</label>
<input type="file" id="photo" accept="image/*" required>

<label>Upload Payment Screenshot</label>
<input type="file" required>

<button type="submit">Register</button>
</form>

<div class="success" id="msg"></div>
<div id="slip"></div>
<div id="idcard"></div>
</div>

<script>
document.getElementById("regForm").addEventListener("submit", function(e){
 e.preventDefault();

 const file = document.getElementById("photo").files[0];
 const reader = new FileReader();

 reader.onloadend = function(){
   const photoData = reader.result;

   const data = {
     name: document.getElementById("name").value,
     father: document.getElementById("father").value,
     mobile: document.getElementById("mobile").value,
     class: document.getElementById("class").value,
     stream: document.getElementById("stream").value,
     date: new Date().toLocaleString(),
     photo: photoData
   };

   let students = JSON.parse(localStorage.getItem("students")) || [];
   students.push(data);
   localStorage.setItem("students", JSON.stringify(students));

   document.getElementById("msg").innerText = "Registration Successful!";

   document.getElementById("slip").innerHTML = `
   <div class="slip">
   <h3>Registration Slip</h3>
   <p><b>Name:</b> ${data.name}</p>
   <p><b>Class:</b> ${data.class}</p>
   <p><b>Stream:</b> ${data.stream}</p>
   <p><b>Date:</b> ${data.date}</p>
   </div>`;

   document.getElementById("idcard").innerHTML = `
   <div class="idcard">
   <h3>Catalyst Educational Campus</h3>
   <img src="${data.photo}">
   <p><b>${data.name}</b></p>
   <p>Class: ${data.class}</p>
   <p>Stream: ${data.stream}</p>
   <p>Mobile: ${data.mobile}</p>
   </div>`;
 };

 reader.readAsDataURL(file);
});
</script>

</body>
</html>
