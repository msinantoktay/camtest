
<head>
</head>
<body>
    <h1>Photo of A4 doc</h1>
    <div id="camera" style="position: relative;">
        <video id="video" width="640" height="480" autoplay></video>
        <canvas id="canvas" width="640" height="480"></canvas>
        <div id="guidelines" style="position: absolute; border: 2px solid red;"></div>
    </div>
    <button id="snap">Capture</button>

    <script>
        // Access the camera
        navigator.mediaDevices.getUserMedia({video: true})
            .then(function(stream) {
                var video = document.querySelector('#video');
                video.srcObject = stream;
                video.play();
                setGuidelines();
            })
            .catch(function(err) {
                console.log("An error occurred: " + err);
            });

        // Set the guidelines
        function setGuidelines() {
            var video = document.getElementById('video');
            var videoRect = video.getBoundingClientRect();
            var aspectRatio = 210 / 297; // A4 paper aspect ratio

            // Set the guidelines size and position
            var guidelines = document.getElementById('guidelines');
            var guidelinesWidth = videoRect.width * 0.50; // 90% of video width
            var guidelinesHeight = videoRect.width * 0.50;
            guidelines.style.width = guidelinesWidth + 'px';
            guidelines.style.height = guidelinesHeight + 'px';
            guidelines.style.left = (videoRect.left + (videoRect.width - guidelinesWidth) / 2) + 'px';
            guidelines.style.top =  (videoRect.top + (videoRect.width - guidelinesWidth) / 2)) + 'px';

        }

        // Capture the image
        var canvas = document.getElementById('canvas');
        var context = canvas.getContext('2d');
        var video = document.getElementById('video');
        var snap = document.getElementById('snap');

        snap.addEventListener('click', function() {
            context.drawImage(video, 0, 0, 640, 480);
        });
    </script>
</body>

