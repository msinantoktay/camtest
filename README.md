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
