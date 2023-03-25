
<head>
    <!-- This is just a bunch of hacked together code to see if it works. -->
    <meta charset="utf-8">
    <meta http-equiv="x-ua-compatible" content="ie=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Overlay test</title>
    <style media="screen">
      html, body{
        margin: 0;
        padding: 0;
        height: 100%;
        width: 100%;
    }
    #camera, #camera--view, #camera--sensor, #camera--output{
        position: fixed;
        height: 200px;
        width: 200px;
    }
    #camera--view, #camera--sensor, #camera--output{
        transform: scaleX(-1);
        filter: FlipH;
    }
    #camera--trigger {
        display: none;
        position: relative;
        width: 99%;
        height: 99%;
    }
    #camera--trigger:before {
        display: block;
        content: "";
        width: 20px;
        height: 20px;
        position: absolute;
        top: 1px;
        left: 1px;
        border-top: 4px solid #000;
        border-left: 4px solid #000;
        margin-left: 33px;
        margin-top: 30px;
    }
    #camera--trigger:after {
        display: block;
        content: "";
        width: 20px;
        height: 20px;
        position: absolute;
        top: 1px;
        right: 1px;
        border-top: 4px solid #000;
        border-right: 4px solid #000;
        margin-right: 50px;
        margin-top: 30px;
    }
    #camera--trigger span:before {
        display: block;
        content: "";
        width: 20px;
        height: 20px;
        position: absolute;
        bottom: 1px;
        left: 1px;
        border-bottom: 4px solid #000;
        border-left: 4px solid #000;
        margin-left: 33px;
        margin-bottom: 20px;
        margin-right: 40px;
    }
    #camera--trigger span:after {
        display: block;
        content: "";
        width: 20px;
        height: 20px;
        position: absolute;
        bottom: 1px;
        right: 1px;
        border-bottom: 4px solid #000;
        border-right: 4px solid #000;
        margin-left: 33px;
        margin-bottom: 20px;
        margin-right: 50px;
    }

    .taken{
        height: 100px!important;
        width: 100px!important;
        transition: all 0.5s ease-in;
        border: solid 3px white;
        box-shadow: 0 5px 10px 0 rgba(0,0,0,0.2);
        top: 20px;
        right: 20px;
        z-index: 2;
    }
    </style>
</head>
<body>
    <!-- Camera -->
    <main id="camera">
        <!-- Camera sensor -->
        <canvas id="camera--sensor"></canvas>
        <!-- Camera view -->
        <video id="camera--view" autoplay playsinline></video>
        <!-- Camera output -->
        <img src="//:0" alt="" id="camera--output">
        <!-- Camera trigger -->
        <div id="camera--trigger"><span>Take a picture</span></div>
    </main>
    <!-- Reference to your JavaScript file -->
    <script>
        // Set constraints for the video stream
        //var constraints = { video: { facingMode: "user" }, audio: false };
        //var constraints = { video: { facingMode: "user" }, audio: false };
        
        var constraints = { video: {  }, audio: false };
        
        // Define constants
        const cameraView = document.querySelector("#camera--view"),
            cameraOutput = document.querySelector("#camera--output"),
            cameraSensor = document.querySelector("#camera--sensor"),
            cameraTrigger = document.querySelector("#camera--trigger")
        // Access the device camera and stream to cameraView
        function cameraStart() {
            const supported = 'mediaDevices' in navigator;
            if (!supported) {
                alert('not supported')
                return;
            }
            navigator.mediaDevices
                .getUserMedia(constraints)
                .then(function(stream) {
                    cameraTrigger.style.display = 'block';
                    track = stream.getTracks()[0];
                    cameraView.srcObject = stream;
                })
                .catch(function(error) {
                    alert('Cant start');
                    console.error("Oops. Something is broken.", error);
                });
        }
        // Take a picture when cameraTrigger is tapped
        cameraTrigger.onclick = function() {
            cameraSensor.width = cameraView.videoWidth;
            cameraSensor.height = cameraView.videoHeight;
            cameraSensor.getContext("2d").drawImage(cameraView, 0, 0);
            cameraOutput.src = cameraSensor.toDataURL("image/webp");
            cameraOutput.classList.add("taken");

            cameraView.srcObject.getVideoTracks().forEach(track => track.stop());
            cameraSensor.style.display = 'none';
            cameraView.style.display = 'none';
        };
        // Start the video stream when the window loads
        window.addEventListener("load", cameraStart, false);
    </script>
