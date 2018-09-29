# clock
caves 实现一个手表

```
<canvas id="clock" style="background: black" width="500" height="500"></canvas>
const THEME = {
    "default": {
      clock_size: 500,                // 表盘轮廓大小
      clockBgColor: "#000000",       // 表盘颜色
      clockScaleColor: "#ffffff",     // 刻度颜色
      clockNumColor: "#ffffff",     // 表盘数字的颜色
      hourPointerColor: "#ffffff",      // 时指针颜色
      minutePointerColor: "#ffffff",   // 分指针颜色
      secondPointerColor: "#ff9f65",  // 秒指针颜色
      weekendColor: "#ffffff",  // 秒指针颜色
      dateColor: "#ff9f65",  // 秒指针颜色
    },
    "black": {
      clock_size: 500,                // 表盘轮廓大小
      clockBgColor: "#000000",       // 表盘颜色
      clockScaleColor: "#ffffff",     // 刻度颜色
      clockNumColor: "#ffffff",     // 表盘数字的颜色
      hourPointerColor: "#ffffff",      // 时指针颜色
      minutePointerColor: "#ffffff",   // 分指针颜色
      secondPointerColor: "#ff9f65",  // 秒指针颜色
      weekendColor: "#ffffff",  // 秒指针颜色
      dateColor: "#ff9f65",  // 秒指针颜色

    }
  }
  window.onload = function () {
    var clock = document.getElementById('clock');
    var clockCtx = clock.getContext('2d');

    function loop(clockCtx) {
      window.requestAnimationFrame(function () {
        loop(clockCtx);
      });
      // 跟上自己的样式
      renderClock(clockCtx, "black");
    }

    loop(clockCtx);
  };

  function renderClock(context, theme = "default") {
    var date = new Date();
    // 绘制一个矩形
    context.clearRect(0, 0, THEME[theme].clock_size, THEME[theme].clock_size);
    // 绘制表盘
    drawClockBackgroud(context, theme);
    // 绘制刻度
    drawTick(context, theme);
    // 绘制小刻度
    drawMinuteTick(context, theme);
    // 绘制表盘的数值
    drawNumTick(context, theme);
    // 绘制日期
    drawDate(context, date, theme)
    // 绘制小时
    drawHour(context, date, theme);
    // 绘制分
    drawMinute(context, date, theme);
    // 绘制秒
    drawSecond(context, date, theme);

  }

  // 绘制星期
  function drawDate(textctx, date, theme) {
    var day = date.getDay();
    var weekeed = {
      "0": "星期日",
      "1": "星期一",
      "2": "星期二",
      "3": "星期三",
      "4": "星期四",
      "5": "星期五",
      "6": "星期六",
    }
    textctx.fillStyle = THEME[theme].dateColor
    textctx.textAlign = 'center';//文本水平对齐方式
    textctx.textBaseline = 'middle';//文本垂直方向，基线位置

    textctx.font = "20px Georgia";
    textctx.fillStyle = THEME[theme].weekendColor;
    textctx.fillText(weekeed[day], THEME[theme].clock_size * 2 / 3, THEME[theme].clock_size / 2);
    var date = date.getDate();
    textctx.font = "20px Georgia";
    textctx.fillStyle = THEME[theme].dateColor;
    textctx.fillText(date, THEME[theme].clock_size * 6 / 8, THEME[theme].clock_size / 2);
  }


  function drawClockBackgroud(context, theme) {
    context.beginPath();
    context.strokeStyle = THEME[theme].clockBgColor;
    context.fillStyle = THEME[theme].clockBgColor;
    context.lineWidth = THEME[theme].clock_size / 25;
    context.arc(THEME[theme].clock_size / 2, THEME[theme].clock_size / 2, THEME[theme].clock_size / 2.2, 0, 2 * Math.PI);
    context.closePath();
    context.stroke();
    context.fill();
  }


  function drawNumTick(numctx, theme) {
    for (var i = 0; i < 12; i++) {
      numctx.font = "58px Georgia";
      numctx.fillStyle = THEME[theme].clockNumColor;
      numctx.textAlign = 'center';//文本水平对齐方式
      numctx.textBaseline = 'middle';//文本垂直方向，基线位置
      var x = Math.round(THEME[theme].clock_size / 2 + 17 / 25 * THEME[theme].clock_size / 2 * Math.sin(i / 12 * 2 * Math.PI));
      var y = Math.round(THEME[theme].clock_size / 2 - 17 / 25 * THEME[theme].clock_size / 2 * Math.cos(i / 12 * 2 * Math.PI));
      numctx.fillText((i == 0 ? "12" : i), x, y);
    }
  }

  function drawTick(context, theme) {
    var RADIUS = THEME[theme].clock_size / 2.2;
    for (var i = 0; i < 12; i++) {
      context.beginPath();
      context.lineJoin = "miter";
      context.lineWidth = THEME[theme].clock_size / 30;
      context.moveTo(THEME[theme].clock_size / 2 + (RADIUS - THEME[theme].clock_size / 20) * Math.sin(i / 12 * 2 * Math.PI), THEME[theme].clock_size / 2 - (RADIUS - THEME[theme].clock_size / 20 ) * Math.cos(i / 12 * 2 * Math.PI));
      context.lineTo(THEME[theme].clock_size / 2 + RADIUS * Math.sin(i / 12 * 2 * Math.PI), THEME[theme].clock_size / 2 - RADIUS * Math.cos(i / 12 * 2 * Math.PI));
      context.strokeStyle = THEME[theme].clockScaleColor;
      context.closePath();
      context.stroke();
    }
  }

  function drawMinuteTick(context, theme) {
    var RADIUS = THEME[theme].clock_size / 2.2;
    for (var i = 0; i < 60; i++) {
      context.beginPath();
      context.lineJoin = "miter";
      context.lineWidth = THEME[theme].clock_size / 60;
      context.moveTo(THEME[theme].clock_size / 2 + (RADIUS - THEME[theme].clock_size / 40) * Math.sin(i / 60 * 2 * Math.PI), THEME[theme].clock_size / 2 - (RADIUS - THEME[theme].clock_size / 40 ) * Math.cos(i / 60 * 2 * Math.PI));
      context.lineTo(THEME[theme].clock_size / 2 + RADIUS * Math.sin(i / 60 * 2 * Math.PI), THEME[theme].clock_size / 2 - RADIUS * Math.cos(i / 60 * 2 * Math.PI));
      context.strokeStyle = THEME[theme].clockScaleColor;
      context.closePath();
      context.stroke();
    }
  }


  function drawHour(hourctx, date, theme) {
    var RADIUS = THEME[theme].clock_size / 2.2;
    var h = date.getHours();
    var angle = (h > 12 ? (h - 12) : h) / 12 * 2 * Math.PI;
    var m = date.getMinutes();
    angle += m / 60 * 2 * Math.PI / 12;
    hourctx.lineJoin = "round";
    hourctx.strokeStyle = THEME[theme].hourPointerColor;
    hourctx.beginPath();
    hourctx.lineWidth = THEME[theme].clock_size * 2 / 50;
    hourctx.moveTo(THEME[theme].clock_size / 2, THEME[theme].clock_size / 2);
    hourctx.lineTo(THEME[theme].clock_size / 2 + 0.5 * RADIUS * Math.sin(angle), THEME[theme].clock_size / 2 - 0.5 * RADIUS * Math.cos(angle));
    hourctx.closePath();
    hourctx.stroke();
  }

  function drawMinute(minutectx, date, theme) {
    var RADIUS = THEME[theme].clock_size / 2.2;
    var m = date.getMinutes();
    var angle = m / 60 * 2 * Math.PI;
    minutectx.beginPath();
    minutectx.strokeStyle = THEME[theme].minutePointerColor;
    minutectx.lineWidth = THEME[theme].clock_size * 2 / 50;
    minutectx.moveTo(THEME[theme].clock_size / 2, THEME[theme].clock_size / 2);
    minutectx.lineTo(THEME[theme].clock_size / 2 + 0.75 * RADIUS * Math.sin(angle), THEME[theme].clock_size / 2 - 0.75 * RADIUS * Math.cos(angle));
    minutectx.closePath();
    minutectx.stroke();
  }
  function drawSecond(secondctx, date, theme) {
    var RADIUS = THEME[theme].clock_size / 2.2;
    var s = date.getSeconds();
    var angle = s / 60 * 2 * Math.PI;
    secondctx.beginPath();
    secondctx.lineWidth = THEME[theme].clock_size * 1 / 50;
    secondctx.strokeStyle = THEME[theme].secondPointerColor;
    secondctx.moveTo(THEME[theme].clock_size / 2, THEME[theme].clock_size / 2);
    secondctx.lineTo(THEME[theme].clock_size / 2 + 0.9 * RADIUS * Math.sin(angle), THEME[theme].clock_size / 2 - 0.9 * RADIUS * Math.cos(angle));
    secondctx.closePath();
    secondctx.stroke();
  }
  ```
