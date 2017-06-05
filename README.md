```
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <link rel="stylesheet" href="css/style.css">
    <link rel="stylesheet" href="css/jigsaw.css">
    <style>
    </style>
</head>
<body>
    <div class="content">
        <div class="title"><h1>拼图</h1></div>
        <div class="setting">
            <span style="padding-left: 20px">步数：</span><span class="step">0</span>
            <span>用时：</span><span class="time">00'00"</span>
            <button class="start">开始</button>
        </div>
        <img class="model" src="images/jigsaw.jpg" alt="拼图">
        <div class="play-area"></div>
    </div>
    <script src="js/jquery-1.11.3.js"></script>
    <script>
        $(function () {
            var step = 0;//步数
            var reset = 0;//是否需要重置
            var successNum = 0;
            /* 在右边的方框用canvas画出左边图片 */
            var img=new Image()
            img.src="images/jigsaw.jpg";
            var h = img.height;
            var w = img.width;
            h /= 4;
            w /= 4;
            console.log(w,h);
            for(var i = 0;i<4;i++){
                for(var j = 0;j<4;j++){
                    $('<canvas id="block'+i + j+'" class="mycanvas" data="'+ successNum +'"></canvas>').appendTo($(".play-area"));
                    var cxt = document.getElementById("block"+ i + j).getContext("2d");
                    cxt.drawImage(img,j*w,i*h,w,h,0,0,300,150);
                    successNum++;
                };
            };
            var block = document.getElementsByClassName("mycanvas");
            var arr = [];
            for(var i = 0;i<block.length-1;i++){
                arr.push(block[i]);
            }
            $(".start").click(function () {
                if(reset == 1){//重置
                    alert("请刷新当前页面重置！")
                    return;
                };
                $(this).html("重置");
                reset = 1;
                arr.sort(function(){ return 0.5 - Math.random()});
                $(".play-area").empty();
                $('<canvas id="block" class="mycanvas blank" place="0"></canvas>').appendTo($(".play-area"));
                for(var i = 0;i<arr.length;i++){
                    $(arr[i]).attr("place",i+1).click(function () {
                        var $this = $(this);
                        move($this);
                    }).appendTo($(".play-area"));
                };
                /*启动定时器*/
                var time = 0;
                var min = 0;
                var s = 0;
                var start = setInterval(function () {
                    time++;
                    min = parseInt(time/60);
                    s = time%60;
                    s = doubleNum(s);
                    min = doubleNum(min);
                    $(".time").html(min + "\'" + s + "\"");
                },1000);
                function doubleNum(n) {
                    if(n>9){
                        return n;
                    }else{
                        n = "0" + n;
                        return n;
                    };
                };
            });
            function move(dom){
                var bPlace = parseInt($(".blank").attr("place"));
                var i =parseInt(dom.attr("place")) - bPlace;
                if(i == 0 || i == 2 || i == -2 || i == 3 || i == -3 || i>4 || i<-4 ) return;
                step += 1;
                $(".step").html(step);
                var nowTop = parseInt($(".blank").css("top"));
                var nowLeft = parseInt($(".blank").css("left"));
                var domTop = parseInt(dom.css("top"));
                var domLeft = parseInt(dom.css("left"));
                if(i == 1){
                    domLeft -= 100;
                    dom.css("left",domLeft);
                    nowLeft += 100;
                    $(".blank").css("left",nowLeft);
                };
                if(i == -1){
                    domLeft += 100;
                    dom.css("left",domLeft);
                    nowLeft -= 100;
                    $(".blank").css("left",nowLeft);
                };
                if(i == 4){
                    domTop -= 105;
                    dom.css("top",domTop);
                    nowTop += 105;
                    $(".blank").css("top",nowTop);
                };
                if(i == -4){
                    domTop += 105;
                    dom.css("top",domTop);
                    nowTop-= 105;
                    $(".blank").css("top",nowTop);
                };
                dom.attr("place",bPlace);
                var num = parseInt(dom.attr("data"));
                if(num == bPlace){
                    dom.addClass("success");
                }else{
                    dom.removeClass("success");
                };
                bPlace += i;
                $(".blank").attr("place",bPlace);
                if($(".success").length == 15){
                    clearInterval(start);
                    alert("恭喜通关！")
                };
            };
        });
    </script>
</body>
</html>
```
