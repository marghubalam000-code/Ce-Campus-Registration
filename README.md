<Design By Marghubur Rahman>
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
}

button:hover{background:#06724a;}

.hidden{display:none;}

.slip{
    margin-top:15px;
    padding:15px;
    border:2px dashed green;
    background:#f0fff4;
    border-radius:10px;
    text-align:center;
}

.design-by{
    position:fixed;
    right:10px;
    bottom:10px;
    background:#0a8f5a;
    color:white;
    padding:8px 12px;
    border-radius:8px;
    font-size:13px;
}

.success{
    text-align:center;
    color:green;
    font-weight:bold;
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

<input id="aadhar" placeholder="Aadhar (XXXX-XXXX-XXXX)" maxlength="14" required>

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

<div class="design-by">Design by Marghubur Rahman</div>

<script>
emailjs.init("q7WRi2qk3AUR725UG");

// SUBJECT
function showSubjects(){
    let stream=document.getElementById("stream").value;
    document.getElementById("scienceBox").style.display=
        (stream==="Science")?"block":"none";
    document.getElementById("artsBox").style.display=
        (stream==="Arts")?"block":"none";
}

// AADHAR AUTO FORMAT
document.getElementById("aadhar").addEventListener("input",function(e){
    let v=e.target.value.replace(/\D/g,"").substring(0,12);

    let f=v;
    if(v.length>4) f=v.substring(0,4)+"-"+v.substring(4);
    if(v.length>8) f=v.substring(0,4)+"-"+v.substring(4,8)+"-"+v.substring(8);

    e.target.value=f;
});

// SUBMIT
document.getElementById("form").addEventListener("submit",function(e){
e.preventDefault();

let stream=document.getElementById("stream").value;
let subject="";

if(stream==="Science"){
subject=document.getElementById("scienceSubject").value;
if(!subject){alert("Select Subject");return;}
}
if(stream==="Arts"){
subject=document.getElementById("artsSubject").value;
if(!subject){alert("Select Subject");return;}
}
if(stream==="Commerce") subject="Commerce";

let file=document.getElementById("photo").files[0];
if(!file){alert("Upload photo");return;}

// ================= STRONG AADHAR VALIDATION =================
let aadharInput=document.getElementById("aadhar").value;
let aadhar=aadharInput.replace(/\D/g,"");

if(aadhar.length!==12){
alert("❌ Invalid Aadhar! Must be exactly 12 digits");
document.getElementById("aadhar").focus();
return;
}

if(/^(\d)\1{11}$/.test(aadhar)){
alert("❌ Invalid Aadhar Number");
document.getElementById("aadhar").focus();
return;
}

let reader=new FileReader();

reader.onload=function(event){

let photo=event.target.result;

let data={
name:document.getElementById("name").value,
father:document.getElementById("father").value,
mobile:document.getElementById("mobile").value,
address:document.getElementById("address").value,
class:document.getElementById("class").value,
stream:stream,
subject:subject,
date:new Date().toLocaleString()
};

// EMAIL
emailjs.send("service_bnw6tan","template_ye9opt9",{
name:data.name,
father:data.father,
mobile:data.mobile,
address:data.address,
aadhar:aadhar,
class:data.class,
stream:data.stream,
subject:data.subject,
date:data.date
});

// WHATSAPP
let msg=`New Registration:
Name:${data.name}
Father:${data.father}
Mobile:${data.mobile}
Address:${data.address}
Aadhar:${aadhar}
Class:${data.class}
Stream:${data.stream}
Subject:${data.subject}
Date:${data.date}`;

window.open("https://wa.me/919263960341?text="+encodeURIComponent(msg),"_blank");

// SLIP
document.getElementById("slip").innerHTML=`
<div class="slip">

<img src="${photo}" style="width:120px;height:120px;border-radius:10px;border:2px solid green;"><br><br>

<b>Name:</b>${data.name}<br>
<b>Class:</b>${data.class}<br>
<b>Stream:</b>${data.stream}<br>
<b>Subject:</b>${data.subject}<br>
<b>Aadhar:</b>${aadhar}<br>
<b>Date:</b>${data.date}<br><br>

<button onclick="printSlip()">🖨️ Print Slip</button>
</div>`;

// PDF
const {jsPDF}=window.jspdf;
let doc=new jsPDF();

doc.setFillColor(10,143,90);
doc.rect(0,0,220,40,"F");

doc.setTextColor(255,255,255);
doc.setFontSize(22);
doc.text("CATALYST EDUCATIONAL CAMPUS",20,25);

doc.setFontSize(12);
doc.text("REGISTRATION CERTIFICATE",55,35);

doc.setDrawColor(0,150,100);
doc.rect(10,50,190,200);

doc.setTextColor(0,0,0);
doc.text("Name:"+data.name,15,70);
doc.text("Father:"+data.father,15,80);
doc.text("Mobile:"+data.mobile,15,90);
doc.text("Address:"+data.address,15,100);
doc.text("Aadhar:"+aadhar,15,110);
doc.text("Class:"+data.class,15,120);
doc.text("Stream:"+data.stream,15,130);
doc.text("Subject:"+data.subject,15,140);
doc.text("Date:"+data.date,15,150);

// PHOTO
doc.rect(145,60,50,50);
doc.addImage(photo,"JPEG",145,60,50,50);

doc.setFontSize(12);
doc.setTextColor(10,143,90);
doc.text("REGISTRATION SUCCESSFUL",40,190);

doc.save(data.name+"_Certificate.pdf");

document.getElementById("msg").innerText="✅ Registration Successful";

};

reader.readAsDataURL(file);
});

// PRINT
function printSlip(){
let w=window.open('');
w.document.write(document.body.innerHTML);
w.print();
}
</script>

</body>
</html>
