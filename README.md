
<head>
   
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
    #camera--output{
         position : relative;
         display: none;
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
        border-top: 4px solid #bc0c0c;
        border-left: 4px solid #bc0c0c;
        margin-left: 33px;
        margin-top: 30px;
        z-index: 10;
    }
    #camera--trigger:after {
        display: block;
        content: "";
        width: 20px;
        height: 20px;
        position: absolute;
        top: 1px;
        right: 1px;
        border-top: 4px solid #bc0c0c;
        border-right: 4px solid #bc0c0c;
        margin-right: 50px;
        margin-top: 30px;
        z-index: 10;
    }
    #camera--trigger span:before {
        display: block;
        content: "";
        width: 20px;
        height: 20px;
        position: absolute;
        bottom: 1px;
        left: 1px;
        border-bottom: 4px solid #bc0c0c;
        border-left: 4px solid #bc0c0c;
        margin-left: 33px;
        margin-bottom: 20px;
        margin-right: 40px;
        z-index: 10;
    }
    #camera--trigger span:after {
        display: block;
        content: "";
        width: 20px;
        height: 20px;
        position: absolute;
        bottom: 1px;
        right: 1px;
        border-bottom: 4px solid #bc0c0c;
        border-right: 4px solid #bc0c0c;
        margin-left: 33px;
        margin-bottom: 20px;
        margin-right: 50px;
        z-index: 10;
    }

    .taken{
        transition: all 0.5s ease-in;
        height: 150px !important;
        margin-top: 6px;
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
        <!-- Camera trigger -->
        <div id="camera--trigger"><span>Take a picture</span>
                <img src="//:0" alt="" id="camera--output">

        </div>


    </main>
    <!-- Reference to your JavaScript file -->
    <script>
        // Set constraints for the video stream
        //var constraints = { video: { facingMode: "user" }, audio: false };
        //var constraints = { video: { facingMode: "user" }, audio: false };
        
        var constraints = { video: { facingMode: "environment"  }, audio: false };
        
        // Define constants
        const cameraView = document.querySelector("#camera--view"),
            cameraOutput = document.querySelector("#camera--output"),
            cameraSensor = document.querySelector("#camera--sensor"),
            cameraTrigger = document.querySelector("#camera--trigger")
        // Access the device camera and stream to cameraView
        function cameraStart() {
            const supported = 'mediaDevices' in navigator;
            if (!supported) {
                alert('device not supported')
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
            //cameraView.style.display = 'none';
            cameraOutput.style.display = 'block';

        };
        // Start the video stream when the window loads
        window.addEventListener("load", cameraStart, false);
    </script>
