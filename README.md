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
.logo img { width:130px; margin-bottom:10px; }
input, select { width:100%; padding:10px; margin:8px 0; border-radius:8px; border:1px solid #ccc; }
button { width:100%; padding:12px; background:#28a745; color:white; border:none; border-radius:8px; font-size:16px; cursor:pointer; margin-top:5px; }
button:hover { background:#218838; }
.success { color:green; text-align:center; margin-top:10px; font-weight:bold; }
.qr { text-align:center; margin:15px 0; padding:10px; background:#f8fff8; border-radius:10px; }
.slip { background:#eaffea; padding:15px; margin-top:15px; border:1px solid green; border-radius:10px; }
.idcard { margin-top:15px; padding:15px; border:2px solid #28a745; text-align:center; border-radius:10px; background:#f0fff0; }
.idcard img { width:80px; height:80px; border-radius:50%; }
.print-btn { background:#007bff; }
.print-btn:hover { background:#0056b3; }
</style>
</head>
<body>

<div class="container">

<!-- LOGO -->
<div class="logo">
<img src="logo.png" onerror="this.style.display='none'">
</div>

<h2>Student Registration</h2>

<!-- QR -->
<div class="qr">
<img src="qr.png" width="200" onerror="this.style.display='none'">
<p><b>type upi marghub0@ptyes & Pay ₹500 Registration Fee</b></p>
</div>

<form id="regForm">
<input type="text" id="name" placeholder="Student Name" required>
<input type="text" id="father" placeholder="Father Name" required>
<input type="number" id="mobile" placeholder="Mobile Number" required>
<input type="text" id="aadhar" placeholder="Aadhar Number" required>

<select id="class" required>
<option value="">Select Class</option>
<option>Class 8</option>
<option>Class 9</option>
<option>Class 10</option>
<option>Class 11</option>
<option>Class 12</option>
</select>

<select id="stream" required>
<option value="">Select Stream</option>
<option>Science</option>
<option>Commerce</option>
<option>Arts</option>
</select>

<label>Upload student Photo</label>
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

   // SLIP with PRINT
   document.getElementById("slip").innerHTML = `
   <div class="slip" id="printArea">
   <h3>Registration Slip</h3>
   <p><b>Name:</b> ${data.name}</p>
   <p><b>Class:</b> ${data.class}</p>
   <p><b>Stream:</b> ${data.stream}</p>
   <p><b>Date:</b> ${data.date}</p>
   <button class="print-btn" onclick="printSlip()">Print Slip</button>
   </div>`;

   // ID CARD
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

// PRINT FUNCTION
function printSlip(){
 var content = document.getElementById('printArea').innerHTML;
 var win = window.open('', '', 'width=600,height=600');
 win.document.write('<html><head><title>Print</title></head><body>' + content + '</body></html>');
 win.document.close();
 win.print();
}
</script>

</body>
</html>
<!DOCTYPE html>
<html>
<head>
<title>Registration</title>
<style>
body{font-family:Arial;background:#d4fc79;padding:20px;}
.container{background:white;padding:20px;border-radius:10px;max-width:500px;margin:auto;}
button{background:green;color:white;padding:10px;width:100%;}
</style>
</head>
<body>

<div class="container">
<h2>Student Registration</h2>

<form id="form">
<input id="name" placeholder="Name" required><br>
<input id="father" placeholder="Father Name" required><br>
<input id="mobile" placeholder="Mobile" required><br>
<input id="aadhar" placeholder="Aadhar" required><br>

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

<button type="submit">Register</button>
</form>

<p id="msg"></p>
</div>

<script>
document.getElementById("form").addEventListener("submit", function(e){
 e.preventDefault();

 fetch("PASTE_YOUR_SCRIPT_URL_HERE", {
   method: "POST",
   body: JSON.stringify({
     name: name.value,
     father: father.value,
     mobile: mobile.value,
     aadhar: aadhar.value,
     class: document.getElementById("class").value,
     stream: document.getElementById("stream").value
   })
 })
 .then(res => res.text())
 .then(data => {
   document.getElementById("msg").innerText = "Registration Successful!";
 });
});
</script>

</body>
</html>
