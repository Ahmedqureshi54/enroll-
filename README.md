function enrollUser() {
faceio.enroll({
"locale": "auto",
"payload": {
"email": "user@example.com",
"name": "John Doe",
"employeeId": "EMP123"
}
}).then(userInfo => {
console.log("User Enrolled:", userInfo);
// Send enrollment data to server
fetch('/enroll-user', {
method: 'POST',
headers: {
'Content-Type': 'application/json'
},
body: JSON.stringify({
facialId: userInfo.facialId,
email: userInfo.payload.email,
name: userInfo.payload.name,
employeeId: userInfo.payload.employeeId
})
}).then(response => response.json())
.then(data => console.log("Enrollment saved:", data))
.catch(error => console.error("Error saving enrollment:", error));
}).catch(errCode => {
console.error("Enrollment failed:", errCode);
handleError(errCode);
});
}
function handleError(errCode) {
switch (errCode) {
case faceio.fioErrCode.PERMISSION_REFUSED:
alert("Camera access denied.");
break;
case faceio.fioErrCode.NO_FACES_DETECTED:
alert("No faces detected. Please try again.");
break;
default:
alert("Error: " + errCode);
}
}
