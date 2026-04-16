<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Catalyst Educational Campus</title>

<script src="https://cdn.jsdelivr.net/npm/emailjs-com@3/dist/email.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>
body{
    font-family:Arial;
    background:linear-gradient(135deg,#00c9ff,#92fe9d);
    margin:0;
    padding:20px;
}

.container{
    max-width:520px;
    margin:auto;
    background:#fff;
    padding:25px;
    border-radius:15px;
    box-shadow:0 10px 25px rgba(0,0,0,0.2);
}

h2{
    text-align:center;
    color:#0a6d5c;
}

input,select{
    width:100%;
    padding:10px;
    margin:8px 0;
    border:1px solid #ccc;
    border-radius:8px;
}

button{
    background:#0a8f5a;
    color:white;
    padding:12px;
    width:100%;
    border:none;
    border-radius:8px;
    font-size:16px;
    cursor:pointer;
}

button:hover{
    background:#06724a;
}

.hidden{display:none;}

.slip{
    margin-top:15px;
    padding:15px;
    border:2px dashed green;
    background:#f0fff4;
    border-radius:10px;
}

.success{
    color:green;
    font-weight:bold;
    text-align:center;
}
</style>
</head>

<body>

<div class="container">
<h2>CATALYST EDUCATIONAL CAMPUS</h2>

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

<input type="file" id="photo" required>

<button type="submit">Register</button>
</form>

<p id="msg" class="success"></p>
<div id="slip"></div>

</div>

<script>
emailjs.init("q7WRi2qk3AUR725UG");

// FIXED FUNCTION
function showSubjects(){
    let stream = document.getElementById("stream").value;

    document.getElementById("scienceBox").style.display =
        (stream === "Science") ? "block" : "none";

    document.getElementById("artsBox").style.display =
        (stream === "Arts") ? "block" : "none";
}

document.getElementById("form").addEventListener("submit", function(e){
e.preventDefault();

let stream = document.getElementById("stream").value;
let subject = "";

if(stream === "Science"){
    subject = document.getElementById("scienceSubject").value;
    if(!subject){ alert("Select Science Subject"); return; }
}

if(stream === "Arts"){
    subject = document.getElementById("artsSubject").value;
    if(!subject){ alert("Select Arts Subject"); return; }
}

if(stream === "Commerce"){
    subject = "Commerce";
}

let file = document.getElementById("photo").files[0];
if(!file){ alert("Upload photo"); return; }

let reader = new FileReader();

reader.onload = function(event){

let photo = event.target.result;

let data = {
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

// EMAIL (safe check)
emailjs.send("service_bnw6tan","__ejs-test-mail-service__",data);

// POPUP
alert("🎉 Welcome to CE Campus, " + data.name);

// SLIP UI
document.getElementById("slip").innerHTML = `
<div class="slip" id="printArea">
<h3>Registration Slip</h3>
<b>Name:</b> ${data.name}<br>
<b>Class:</b> ${data.class}<br>
<b>Stream:</b> ${data.stream}<br>
<b>Subject:</b> ${data.subject}<br>
<b>Address:</b> ${data.address}<br>
<b>Aadhar:</b> ${data.aadhar}<br>
<b>Date:</b> ${data.date}<br><br>

<button onclick="printSlip()">🖨️ Print Slip</button>
</div>`;

// PDF
const { jsPDF } = window.jspdf;
let doc = new jsPDF();

doc.text("CATALYST EDUCATIONAL CAMPUS",20,20);
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

document.getElementById("msg").innerText =
"✅ Registration Successful";
};

reader.readAsDataURL(file);
});

// PRINT FUNCTION (NEW)
function printSlip(){
    let printContent = document.getElementById("printArea").innerHTML;
    let win = window.open('', '', 'width=800,height=600');
    win.document.write(`
        <html>
        <head><title>Print Slip</title></head>
        <body>${printContent}</body>
        </html>
    `);
    win.document.close();
    win.print();
}
</script>

</body>
</html>
