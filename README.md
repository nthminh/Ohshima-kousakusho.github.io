# Ohshima-kousakusho.github.io
<!DOCTYPE html>
<html>
  <head>
    <base target="_top">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bulma@0.9.2/css/bulma.min.css">
    <style>
      input:focus{background:pink; color:blue;}
      button:focus{background:pink; color:blue;}
      .button {
  display: inline-block;
  padding: 15px 30px;
  font-size: 30px;
  cursor: pointer;
  text-align: center;
  text-decoration: none;
  outline: none;
  color: #fff;
  background-color: purple;
  border: none;
  border-radius: 15px;
  box-shadow: 0 9px #999;
}

.button:hover {background-color: #58257b}

.button:active {
  background-color: #58257b;
  box-shadow: 0 5px #666;
  transform: translateY(4px);
}
    </style>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html5-qrcode/2.0.3/html5-qrcode.min.js"></script>
    
  </head>
  <body>
    
    
    <div class="field is-grouped">
        <p class="control">
          <button class="button is-info" onclick="ChangeName()">
            名前-NAME </button>
         </p>
        <p class="control">
          <button class="button  is-success" onclick="ChangeWS()">
            工程-WORK</button>
         </p>
        <p class="control">
          <button class="button is-warning" onclick="ChangeCode()">
            QR-Code </button>
        
          
      </div>

    <input class ="input is-normal input-name is fullwidth" placeholder="Nameー名前" id ="Name">
    <input class ="input is-normal input-ws is fullwidth" placeholder="Work stationー工程ー機械ー場所" id ="WS" >
    
    <input class ="input is-rounded is-large input-code is fullwidth" placeholder="Codeー図面コード" id="code">
    <input class="input is-normal is-warning input-Number "   placeholder="Number-数量" id="Number-code">

       
      
  <div class="field is-horizontal">
      <div class="field-label is-normal">
        <label class="label">不良/不足数</label>
      </div>
    <div class="field-body">
      <div class="field">
        <p class="control">
          <input class="input is-small input-NG " placeholder="不良数/不足数-Số hàng hỏng hoặc thiếu"  id="NG-code">
        </p>
      </div>
    </div>
  </div>
       <button class="button is-danger is-fullwidth " onclick="sendData()">Save-保存</button>
       <div style=" width: 800px; margin: 0 auto;" id="reader"></div>

       
  <div class = "container is-fluid">
      <section class ="hero is-small  is-primary">
        <div class ="hero-body">
          <p class = "title">
            大島工作所-電子日報
          </p>
    </div> 
      </section>      


    <script>
      function ChangeName(){
        document.querySelector('.input-name').value=''
        document.getElementById("Name").focus();
      }
      function ChangeWS(){
        document.querySelector('.input-ws').value=''
        document.getElementById("WS").focus();
      }
      function ChangeCode(){
        document.querySelector('.input-code').value=''
        document.getElementById("code").focus();
      }
      function IsEmpty() {
        if (document.querySelector('.input-ws').value === "") {
          alert("工程を入力してください-Hãy nhập nội dung công việc của bạn");
        return false;
      }
  return true;
}
      function sendData(){
        IsEmpty() ;
        if (document.querySelector('.input-code').value === "") {
          alert("QRコードを入力してください-Hãy nhập mã hàng");
        return false;
      }
        const API_URL = "https://script.google.com/macros/s/AKfycbxXZT25iaT352mj4qp1my24YvmrgIAO2mzMKHyDCzJDEvabwqEAUvVPln76jI_UfTyepg/exec"
        const options = {
          method: 'POST',
          contentType: 'application/json',
          body: JSON.stringify({name: document.querySelector('.input-name').value,ws: document.querySelector('.input-ws').value,Number: document.querySelector('.input-Number').value,NG: document.querySelector('.input-NG').value, class:document.querySelector('.input-code').value+"$"})
        }
        const response = fetch(API_URL, options)
        .then(response => response.json())
        .then(data=> console.log(data))
          document.querySelector('.input-code').value=''
          document.querySelector('.input-Number').value=''
          document.querySelector('.input-NG').value=''
          document.getElementById("code").focus();
        
        play();
      }

       const API_URL = "https://script.google.com/macros/s/AKfycbxXZT25iaT352mj4qp1my24YvmrgIAO2mzMKHyDCzJDEvabwqEAUvVPln76jI_UfTyepg/exec"
       let html5QrCode;
       const config={fps:5,qrbox: 450}
      function qrCodeSuccessCallback(message) {
        onScanSuccess(message)
      }
      function play() { 
            var audio = new Audio( 
'https://media.geeksforgeeks.org/wp-content/uploads/20190531135120/beep.mp3'); 

            audio.play(); 
            }
           
      function onScanSuccess(qrCodeMessage) {
       html5QrCode.stop()
        const options = {
          method: 'POST',
          contentType: 'application/json',
          body: JSON.stringify({name: document.querySelector('.input-name').value,ws: document.querySelector('.input-ws').value,Number: document.querySelector('.input-Number').value,NG: document.querySelector('.input-NG').value, class:qrCodeMessage+"$"})
        }
        const response = fetch(API_URL, options)
        .then(response => response.json())
        .then(data=> startScan())
          document.querySelector('.input-code').value=''
          document.querySelector('.input-Number').value=''
          document.querySelector('.input-NG').value='';
      }

      function startScan(){
        IsEmpty();
        document.getElementById('reader').innerHTML =''
        html5QrCode = new Html5Qrcode('reader')
        html5QrCode.start ({ facingMode: "environment"},config,qrCodeSuccessCallback)
        play();
      }

        startScan(); 
    </script>
  </body>
</html>
