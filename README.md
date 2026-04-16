# Ce-Campus-Registration
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Student Registration - Catalyst Educational Campus</title>
<style>
body { font-family: Arial; background:#f5f5f5; padding:20px; }
.container { max-width:500px; margin:auto; background:white; padding:20px; border-radius:10px; box-shadow:0 0 10px #ccc; }
h2 { text-align:center; }
input, select { width:100%; padding:10px; margin:8px 0; }
button { width:100%; padding:10px; background:green; color:white; border:none; cursor:pointer; }
.success { color:green; text-align:center; margin-top:10px; }
</style>
</head>
<body>

<div class="container">
<h2>Student Registration</h2>

<form id="regForm">
<input type="text" placeholder="Student Name" required>
<input type="text" placeholder="Father Name" required>
<input type="number" placeholder="Mobile Number" required>
<input type="text" placeholder="Aadhar Number" required>
<select required>
<option value="">Select Course</option>
<option>Class 8</option>
<option>Class 9</option>
<option>Class 10</option>
<option>Class 11</option>
<option>Class 12</option>
</select>
<input type="file" required>
<button type="submit">Register</button>
</form>

<div class="success" id="msg"></div>
</div>

<script>
document.getElementById("regForm").addEventListener("submit", function(e){
 e.preventDefault();
 document.getElementById("msg").innerText = "Registration Successful!";
});
</script>

</body>
</html>
