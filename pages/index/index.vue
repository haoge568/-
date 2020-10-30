<template>
<view class="content">
  <div class="usermoney">
    <span>您当前余额为:</span>
    <span class="usermoneyvalue"></span>
  </div>
  <div class="container">
    <div class="infored">
      <span id="redwin">0</span>
    </div>
    <div class="infoblack">
      <span id="blackwin">0</span>
    </div>
    <div class="time">
      <span id="time"></span>
    </div>
    <div class="item">
      <div class="oldred">
        <div class="oldreditem"></div>
        <div class="oldreditem"></div>
        <div class="oldreditem"></div>
        <div class="oldreditem"></div>
        <div class="oldreditem"></div>
      </div>
      <div class="oldblack">
        <div class="oldblackitem"></div>
        <div class="oldblackitem"></div>
        <div class="oldblackitem"></div>
        <div class="oldblackitem"></div>
        <div class="oldblackitem"></div>
      </div>
    </div>
    <div class="pokeritem">
      <div class="redpoker">
        <span>红色方的手牌是</span>
        <span class="pokerinfo"></span>
        <span class="pokertype"></span>
        <span class="iswin"></span>
      </div>
      <div class="blackpoker">
        <span>黑色方的手牌是</span>
        <span class="pokerinfo"></span>
        <span class="pokertype"></span>
        <span class="iswin"></span>
      </div>
      <div class="nowmoney">
        <div @click="redmoneygetvalue" class="redmoney">
          <span>当前奖池:</span>
          <span class="redmoneyval">0</span>
        </div>
        <div @click="blackmoneygetvalue" class="blackmoney">
          <span>当前奖池:</span>
          <span class="blackmoneyval">0</span>
        </div>
      </div>
    </div>
  </div>
  <div class="addmoney">
    <div class="addmoneyitem">10</div>
    <div class="addmoneyitem">30</div>
    <div class="addmoneyitem">50</div>
    <div class="addmoneyitem">100</div>
    <div class="addmoneyitem">300</div>
    <div class="addmoneyitem">500</div>
    <div class="addmoneyitem">1000</div>
    <div class="addmoneyitem">3000</div>
    <div class="addmoneyitem">5000</div>
    <div class="addmoneyitem">10000</div>
    <div class="addmoneyitem">30000</div>
    <div class="addmoneyitem">50000</div>
    <div class="addmoneyitem" style="width:5rem">100000</div>
  </div>
  <div class="historywin">
    <div class="historywinitem"></div>
    <div class="historywinitem"></div>
    <div class="historywinitem"></div>
    <div class="historywinitem"></div>
    <div class="historywinitem"></div>
    <div class="historywinitem"></div>
    <div class="historywinitem"></div>
    <div class="historywinitem"></div>
  </div>
  <button style="display:none" @click="action">123</button>
</view>
</template>

<script>
export default {
  data() {
    return {

    }
  },
  mounted() {
    this.addmoneycolor();
    this.settime();
    this.setstorageusermoney();
  },
  methods: {
    setusermoney: function (setwhat) {
      var usermoney = document.querySelector(".usermoneyvalue")
      usermoney.innerHTML = Number(setwhat)
      window.localStorage.setItem("usermoney", setwhat)
    },
    setstorageusermoney: function () {
      var usermoney = document.querySelector(".usermoneyvalue")
      if (window.localStorage.getItem("havemoney") == "1") {
        usermoney.innerHTML = window.localStorage.getItem("usermoney")
      } else {
        window.localStorage.setItem("havemoney", "1")
        usermoney.innerHTML = 100000
        window.localStorage.setItem("usermoney", usermoney.innerHTML)
      }
    },
    addmoneycolor: function () {
      var ami = document.querySelectorAll(".addmoneyitem")
      for (let i = 0; i < ami.length; i++) {
        ami[i].onclick = function () {
          for (let j = 0; j < ami.length; j++) {
            ami[j].style.background = "rgba(251, 218, 65, 0.5)"
          }
          ami[i].style.background = "rgba(251, 218, 65, 1)"
        }
      }
    },
    redmoneygetvalue: function () {
      var usermoney = document.querySelector(".usermoneyvalue")
      var ami = document.querySelectorAll(".addmoneyitem")
      var redmoneyval = document.querySelector(".redmoneyval")
      for (let i = 0; i < ami.length; i++) {
        if (ami[i].style.background == "rgb(251, 218, 65)") {
          if (Number(ami[i].innerHTML) > Number(usermoney.innerHTML)) {
            alert("您当前余额不足请及时充值QAQ")
            return false;
          }
          redmoneyval.innerHTML = Number(redmoneyval.innerHTML) + Number(ami[i].innerHTML)
          this.setusermoney(Number(usermoney.innerHTML) - Number(ami[i].innerHTML))
        }
      }
    },
    blackmoneygetvalue: function () {
      var usermoney = document.querySelector(".usermoneyvalue")
      var ami = document.querySelectorAll(".addmoneyitem")
      var blackmoneyval = document.querySelector(".blackmoneyval")
      for (let i = 0; i < ami.length; i++) {
        if (ami[i].style.background == "rgb(251, 218, 65)") {
          if (Number(ami[i].innerHTML) > Number(usermoney.innerHTML)) {
            alert("您当前余额不足请及时充值QAQ")
            return false;
          }
          blackmoneyval.innerHTML = Number(blackmoneyval.innerHTML) + Number(ami[i].innerHTML)
          this.setusermoney(Number(usermoney.innerHTML) - Number(ami[i].innerHTML))
        }
      }
    },
    settime: function () {
      var _self = this;
      var lesstime = 20;
      var waittime = 10;
      let timer = setInterval(function () {
        document.querySelector("#time").innerHTML = lesstime--;
        if (lesstime < 0) {
          lesstime = 20;
          _self.action();
          clearInterval(timer);
          clearInterval(waittimer);
          let waittimer = setInterval(function () {
            document.querySelector("#time").innerHTML = waittime--;
            if (waittime < 0) {
              waittime = 10;
              document.querySelectorAll(".pokerinfo")[0].innerHTML = ""
              document.querySelectorAll(".pokerinfo")[1].innerHTML = ""
              document.querySelectorAll(".pokertype")[0].innerHTML = ""
              document.querySelectorAll(".pokertype")[1].innerHTML = ""
              document.querySelectorAll(".iswin")[0].innerHTML = ""
              document.querySelectorAll(".iswin")[1].innerHTML = ""
              clearInterval(waittimer);
              _self.settime();
            }
          }, 1000)
        }
      }, 1000)
    },
    action: function () {
      //定义玩家的数量
      var playerNum = 2;
      //调用getPorker方法，获取一副被打乱的扑克牌
      var poker = this.getPorker();
      //调用deal方法，给每个玩家发牌，获取所有玩家的手牌
      var handPokers = this.deal(playerNum, poker);
      //调用comparePoints方法，比较出手牌得分最高的玩家，获取到手牌得分最高的玩家
      var winner = this.comparePoints(handPokers);
      //调用convertPoker方法，将扑克牌格式转换
      /*
      11 ---> J
      12 ---> Q
      13 ---> K
      14 ---> A
      */
      var newHandPokers = this.convertPoker(handPokers);
      //调用mesPrint方法，获取我们需要在浏览器弹框上输出的内容
      this.mesPrint(winner, newHandPokers)
    },
    //随机排序
    randomsort: function (a, b) {
      return Math.random() > 0.5 ? -1 : 1;
      //用Math.random()函数生成0~1之间的随机数与0.5比较，返回-1或1
    },

    //生成有序的扑克牌并打乱顺序
    getPorker: function () {
      var poker = []; //打乱后的扑克牌
      var orgPoker = []; //原始有序的扑克牌
      var num = [2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14];
      var color = ["♠", "❤", "♣", "♦"];
      //生成有序的扑克牌
      for (var i = 0; i < num.length; i++) {
        for (var j = 0; j < color.length; j++) {
          orgPoker.push(num[i] + color[j]);
        }
      }
      //将有序的扑克牌打乱，生成新的无序的扑克牌
      poker = orgPoker.sort(this.randomsort);
      //返回被打乱的扑克牌
      return poker;
    },

    //console.log(getPorker());

    //发牌，传进两个参数，玩家数量和洗好的牌，返回所有玩家的从大到小排序的手牌
    deal: function (playerNum, poker) {
      var handPokers = []; //所有玩家的手牌的数组
      for (var i = 0; i < playerNum; i++) {
        var handPoker = []; //单个玩家的手牌
        handPoker.push(poker[i], poker[i + playerNum], poker[i + (2 * playerNum)]);
        //讲单个玩家的手牌按照降序排列，然后放到所有玩家手牌的数组里面
        handPokers.push(handPoker.sort(function (a, b) {
          return b.slice(0, b.length - 1) - a.slice(0, a.length - 1);
        }));
      }
      //返回所有玩家的手牌
      return handPokers;
    },

    //获取扑克牌的点数
    getNum: function (str) {
      if (str.length == 2 || str.length == 3) {
        return str.slice(0, str.length - 1);
      } else {
        alert("扑克牌格式错误！");
      }
    },

    //获取扑克牌的花色
    getColor: function (str) {
      if (str.length == 2 || str.length == 3) {
        return str.slice(-1);
      } else {
        alert("扑克牌格式错误！");
      }
    },
    //给玩家的手牌进行打分
    /*
    豹子：100000000000000
    顺金：1000000000000
    金花：10000000000
    顺子：100000000
    对子：1000000
    单牌：10000
    */
    points: function (playerPokers, index) {
      let aaa = index;
      var playerPoker1 = playerPokers[0];
      var playerPoker2 = playerPokers[1];
      var playerPoker3 = playerPokers[2];
      if ((this.getNum(playerPoker1) == this.getNum(playerPoker2) && this.getNum(playerPoker1) == this.getNum(playerPoker3) && this.getNum(playerPoker2) == this.getNum(playerPoker3))) {
        //豹子
        document.querySelectorAll(".pokertype")[aaa].innerHTML = "豹子"
        this.yestodayitem(index, "豹")
        return 100000000000000 * this.getNum(playerPoker1);
      } else if ((this.getNum(playerPoker1) - this.getNum(playerPoker2) == 1) && (this.getNum(playerPoker2) - this.getNum(playerPoker3) ==
          1 && (this.getNum(playerPoker1) - this.getNum(playerPoker3) == 2))) {
        //首先肯定是个顺子
        if ((this.getColor(playerPoker1) == this.getColor(playerPoker2) == this.getColor(playerPoker3))) {
          //顺金
          document.querySelectorAll(".pokertype")[aaa].innerHTML = "同花顺"
          this.yestodayitem(index, "同")
          return 1000000000000 * this.getNum(playerPoker1);
        } else {
          //普通顺子
          document.querySelectorAll(".pokertype")[aaa].innerHTML = "顺子"
          this.yestodayitem(index, "顺")
          return 100000000 * this.getNum(playerPoker1);
        }
      } else if ((this.getColor(playerPoker1) == this.getColor(playerPoker2) == this.getColor(playerPoker3))) {
        //金花
        document.querySelectorAll(".pokertype")[aaa].innerHTML = "金花"
        this.yestodayitem(index, "金")
        return 10000000000 * this.getNum(playerPoker1) + 100000000 * this.getNum(playerPoker2) + 1000000 * this.getNum(playerPoker3);
      } else if ((this.getNum(playerPoker1) == this.getNum(playerPoker2)) || (this.getNum(playerPoker2) == this.getNum(playerPoker3)) || (
          this.getNum(playerPoker3) == this.getNum(playerPoker1))) {
        //对子
        document.querySelectorAll(".pokertype")[aaa].innerHTML = "对子"
        this.yestodayitem(index, "对")
        return 1000000 * this.getNum(playerPoker2) + 10000 * this.getNum(playerPoker3);
      } else {
        document.querySelectorAll(".pokertype")[aaa].innerHTML = "单张"
        this.yestodayitem(index, "单")
        return 10000 * this.getNum(playerPoker1) + 100 * this.getNum(playerPoker2) + 1 * this.getNum(playerPoker3);
      }
    },
    yestodayitem: function (index, item) {
      if (index == 0) {
        document.querySelectorAll(".oldreditem")[4].innerHTML = document.querySelectorAll(".oldreditem")[3].innerHTML
        document.querySelectorAll(".oldreditem")[3].innerHTML = document.querySelectorAll(".oldreditem")[2].innerHTML
        document.querySelectorAll(".oldreditem")[2].innerHTML = document.querySelectorAll(".oldreditem")[1].innerHTML
        document.querySelectorAll(".oldreditem")[1].innerHTML = document.querySelectorAll(".oldreditem")[0].innerHTML
        document.querySelectorAll(".oldreditem")[0].innerHTML = item
      } else if (index == 1) {
        document.querySelectorAll(".oldblackitem")[4].innerHTML = document.querySelectorAll(".oldblackitem")[3].innerHTML
        document.querySelectorAll(".oldblackitem")[3].innerHTML = document.querySelectorAll(".oldblackitem")[2].innerHTML
        document.querySelectorAll(".oldblackitem")[2].innerHTML = document.querySelectorAll(".oldblackitem")[1].innerHTML
        document.querySelectorAll(".oldblackitem")[1].innerHTML = document.querySelectorAll(".oldblackitem")[0].innerHTML
        document.querySelectorAll(".oldblackitem")[0].innerHTML = item
      }
    },
    checknexttype: function (aaa) {
      aaa++;
      if (aaa == 2) {
        aaa = 0;
        return aaa
      }
      return aaa
    },
    //比较手牌的分数的大小
    comparePoints: function (handPokers) {
      var allPoints = []; //所有玩家手牌的分数数组
      //遍历循环每个玩家的手牌
      for (var i = 0; i < handPokers.length; i++) {
        //调用points方法来计算玩家手牌的得分
        allPoints.push(this.points(handPokers[i], i));
      }
      //求出哪个玩家的手牌得分最高
      var maxPoints = allPoints[0]
      for (var j = 0; j < allPoints.length; j++) {
        maxPoints = maxPoints > allPoints[j] ? maxPoints : allPoints[j];
      }
      // console.log(maxPoints);
      // 返回得分最高的玩家
      return allPoints.indexOf(maxPoints) + 1;
    },

    //生成最后需要输出的内容
    mesPrint: function (winner, newHandPokers) {
      var redmanywin = Number(document.querySelector("#redwin").innerHTML)
      var blackmanywin = Number(document.querySelector("#blackwin").innerHTML)
      var usermoney = document.querySelector(".usermoneyvalue");
      var redmoneyval = document.querySelector(".redmoneyval");
      var blackmoneyval = document.querySelector(".blackmoneyval");
      var historywin = document.querySelectorAll(".historywinitem");
      document.querySelector("#redwin").innerHTML = redmanywin;
      document.querySelector("#blackwin").innerHTML = blackmanywin;
      var alertMes = "";
      for (var i = 0; i < newHandPokers.length; i++) {
        document.querySelectorAll(".pokerinfo")[i].innerHTML = newHandPokers[i];
        alertMes = alertMes + "玩家" + (i + 1) + "的手牌为：" + newHandPokers[i] + "\n";
      }
      if (winner == 1) {
        document.querySelectorAll(".iswin")[0].innerHTML = "红方胜利";
        document.querySelectorAll(".iswin")[1].innerHTML = "";
        redmanywin += 1;
        document.querySelector("#redwin").innerHTML = redmanywin;
        usermoney.innerHTML = Number(usermoney.innerHTML) + (Number(redmoneyval.innerHTML) * 2)
        redmoneyval.innerHTML = ""
        blackmoneyval.innerHTML = ""
        for (let i = 0; i < historywin.length - 1; i++) {
          historywin[i].style.background = historywin[i + 1].style.background
        }
        historywin[7].style.background = "red"
        this.setusermoney(Number(usermoney.innerHTML))
      } else {
        document.querySelectorAll(".iswin")[1].innerHTML = "黑方胜利";
        document.querySelectorAll(".iswin")[0].innerHTML = "";
        blackmanywin += 1;
        document.querySelector("#blackwin").innerHTML = blackmanywin;
        usermoney.innerHTML = Number(usermoney.innerHTML) + (Number(blackmoneyval.innerHTML) * 2)
        redmoneyval.innerHTML = ""
        blackmoneyval.innerHTML = ""
        for (let i = 0; i < historywin.length - 1; i++) {
          historywin[i].style.background = historywin[i + 1].style.background
        }
        historywin[7].style.background = "black"
        this.setusermoney(Number(usermoney.innerHTML))
      }
      alertMes = alertMes + "---------------------------------------\n玩家" + winner + "的手牌最大，手牌为：" + newHandPokers[
        winner - 1] + "\n";
      return alertMes;
    },

    //扑克牌格式转换
    /*
    11 ---> J
    12 ---> Q
    13 ---> K
    14 ---> A
    */
    convertPoker: function (handPokers) {
      for (var i = 0; i < handPokers.length; i++) {
        for (var j = 0; j < handPokers[i].length; j++) {
          handPokers[i][j] = handPokers[i][j].replace("11", "J").replace("12", "Q").replace("13", "K").replace("14",
            "A");
        }
      }
      return handPokers;
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  list-style: none;
  text-decoration: none;
}

.content {
  background: url("../image/wallhaven-5w23r8.jpg");
  background-size: 100% 100%;
}

.container {
  width: 30rem;
  height: 30rem;
  background: aqua;
  margin: 0 auto;
  background: url("../image/001.png");
  background-size: 100% 100%;
}

.infored {
  width: 30px;
  height: 10px;
  float: left;
  margin-left: 30%;
  margin-top: 10%;
  font-size: 30px;
  color: white;
  font-weight: bolder;
}

.infoblack {
  width: 30px;
  height: 10px;
  float: right;
  margin-right: 30%;
  margin-top: 10%;
  font-size: 30px;
  color: white;
  font-weight: bolder;
}

.time {
  width: 20px;
  height: 10px;
  margin: 0 auto;
  float: left;
  margin-top: 37%;
  margin-left: 12%;
  font-size: 20px;
  font-weight: bolder;
}

.time span {
  display: block;
  margin: 0 auto;
  text-align: center;
}

.item {
  width: 30rem;
  height: 2rem;
  float: left;
  margin-top: 4rem;
  padding-top: 0.5rem;
}

.oldred {
  padding-left: 1.5rem;
  width: 12rem;
  height: 2rem;
  float: left;
}

.oldblack {
  padding-right: 1.5rem;
  width: 12rem;
  height: 2rem;
  float: right;
}

.oldreditem {
  margin-left: 0.5rem;
  width: 1.5rem;
  height: 1.5rem;
  border-radius: 50%;
  border: 2px solid white;
  background: red;
  float: left;
  color: white;
  font-weight: bolder;
  text-align: center;
}

.oldblackitem {
  margin-right: 0.5rem;
  width: 1.5rem;
  height: 1.5rem;
  border-radius: 50%;
  border: 2px solid white;
  background: black;
  float: right;
  color: white;
  font-weight: bolder;
  text-align: center;
}

.pokeritem {
  width: 30rem;
  height: 10rem;
  float: left;
  margin-top: 1rem;
  font-size: 20px;
}

.redpoker {
  width: 10rem;
  height: 8.5rem;
  float: left;
  padding-left: 2rem;
  color: red;
}

.blackpoker {
  width: 10rem;
  height: 8.5rem;
  float: right;
  padding-right: 2rem;
}

.pokerinfo {
  display: block;
  margin-top: 1rem;
  font-size: 25px;
}

.iswin {
  display: block;
}

.addmoney {
  width: 25rem;
  height: 8rem;
  margin: 0 auto;
}

.addmoney>div {
  width: 4rem;
  height: 2rem;
  background: rgba(251, 218, 65, 0.5);
  float: left;
  margin-left: 0.5rem;
  margin-top: 0.5rem;
  text-align: center;
  font-size: 20px;
  font-weight: bolder;
  border-radius: 20%;
}

.nowmoney {
  width: 30rem;
  height: 3rem;
  float: left;
}

.redmoney {
  padding-left: 2rem;
  width: 12rem;
  height: 2rem;
  float: left;
  color: red;
}

.blackmoney {
  padding-left: 1rem;
  width: 12rem;
  height: 2rem;
  float: right;
}

.redmoneyval {
  font-size: 20px;
  color: white;
  font-weight: bolder;
}

.blackmoneyval {
  font-size: 20px;
  color: white;
  font-weight: bolder;
}

.usermoney {
  float: left;
  width: 10rem;
  height: 5rem;
  position: fixed;
  top: 15rem;
  left: 17rem;
  font-size: 20px;
  color: white;
  font-weight: bolder;
}

.historywin {
  position: fixed;
  right: 20rem;
  top: 5rem;
  width: 3rem;
  height: 23rem;
}

.historywin div {
  width: 2.5rem;
  height: 2.5rem;
  border: 2px solid white;
  border-radius: 50%;
  margin-top: 0.2rem
}
</style>
