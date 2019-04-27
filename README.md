        <!DOCTYPE html>
        <html>

        <head>
            <title>动力球</title>
            <meta charset="utf-8">
        </head>
        <style type="text/css">
        html,
        body {
            padding: 0;
            margin: 0;
            background-color: #f5f5f5;
        }

        #ball {
            background-color: #dc5947;
            height: 60px;
            width: 60px;
            border-radius: 50%;
            border: none;
            position: absolute;
        }
        </style>

        <body>
            <div id="ball"></div>
            <script type="text/javascript">

        /***
        offsetLeft 这里是相对于body定位的左侧偏移量;
        offsetTop 这里是相对于body定位的左侧偏移量;
        clientX、clientY  点击位置距离当前body可视区域的x，y坐标;
        pageX、pageY 对于整个页面来说，包括了被卷去的body部分的长度;
        screenX、screenY  点击位置距离当前电脑屏幕的x，y坐标;

        *******************************************/
        	//获取小球
            var oball = document.getElementById('ball');

            var lastX = oball.offsetLeft,
                lastY = oball.offsetTop;

            oball.onmousedown = function(e) {
                clearInterval(this.timer)
                var event = e || window.event;
                distX = e.clientX - this.offsetLeft;
                distY = e.clientY - this.offsetTop;
                console.log(distX, distY)
                var _self = this;

                var speedX = 0,
                    speedY = 0;

                //鼠标移动事件
                document.onmousemove = function(e) {
                    var newleft = e.clientX - distX,
                        newtop = e.clientY - distY;
                    console.log(newtop, newleft)

                    //计算两个点之间的距离
                    speedX = newleft - lastX;
                    speedY = newtop - lastY;

                    // 更新上一次的值
                    lastX = newleft;
                    lastY = newtop;
                    _self.style.left = newleft + 'px';
                    _self.style.top = newtop + 'px';
                }
                //鼠标松开事件
                document.onmouseup = function() {
                    document.onmousemove = null;
                    document.onmouseup = null;
                    oballMove(_self, speedX, speedY);
                }
            }

            //运动函数
            function oballMove(obj, speedX, speedY) {
                clearInterval(obj.timer)
                //给定一个加速度
                var g = 2;
                obj.timer = setInterval(function() {
                    speedY += g;
                    var newleft = obj.offsetLeft + speedX,
                        newtop = obj.offsetTop + speedY;
                    if (newtop >=document.documentElement.clientHeight - obj.offsetHeight) {
                        speedY *= -1;
                        speedY *= 0.8;
                        speedX *= 0.8;
                        newtop = document.documentElement.clientHeight - obj.offsetHeight;
                    }
                    if (newtop <= 0) {
                        speedY *= -1;
                        speedY *= 0.8;
                        speedX *= 0.8;
                        newtop = 0;
                    }
                    if (newleft >=document.documentElement.clientWidth - obj.offsetWidth) {
                    	console.log(newleft)
                        speedX *= -1;
                        speedY *= 0.8;
                        speedX *= 0.8;
                        newleft = document.documentElement.clientWidth - obj.offsetWidth;
                    }
                    if (newleft <= 0) {
                    	console.log(newleft)
                        speedX *= -1;
                        speedY *= 0.8;
                        speedX *= 0.8;
                        newleft = 0;
                    }

                    //判断停止

                    if (Math.abs(speedX) < 1) {
                        speedX = 0;
                    }
                    if (Math.abs(speedY) < 1) {
                        speedY = 0;
                    }
                    if (speedY == 0 && speedX == 0 && newtop == document.documentElement.clientHeight - obj.offsetHeight) {
                        clearInterval(obj.timer)
                        console.log('ball is still !!!')
                    }
                    obj.style.left = newleft + 'px';
                    obj.style.top = newtop + 'px';

                }, 30)
            }
            console.log(oball.offsetLeft + '' + oball.offsetTop)
            </script>
        </body>

        </html>
