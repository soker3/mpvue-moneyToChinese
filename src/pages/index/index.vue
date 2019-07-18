<template>
  <div>
    <i-row>
      <i-col span="24" i-class="col-class">
        <i-input
          type="digit"
          right
          title="￥"
          mode="wrapped"
          @change="changeMoney"
		      autofocus
          i-class="my-class"
        />
      </i-col>
    </i-row>
    <i-row>
      <i-col span="24" i-class="col-class">
        <i-alert i-class="my-class">{{chineseMoney}}</i-alert>
      </i-col>
    </i-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      money: '',
      chineseList: ["零", "壹", "贰", "叁", "肆", "伍", "陆", "柒", "捌", "玖"],
      chineseUnit: ["", "拾", "佰", "仟", "万", "拾", "佰", "仟", "亿","拾", "佰"]
    };
  },
  methods: {
    changeMoney(e) {
      let inputValue = e.mp.detail.detail.value
      this.money = inputValue
    }
  },
  computed: {
    chineseMoney() {
      if (this.money > 99999999999.99) {
        return "输入的金额超出了范围"
      }
      let moneyArrAll = []
      let moneyArr = [] //整数部分 
      let moneyArr2 = []//小数部分
      let chineseArr = []
      let chineseArr2 = []
      let moneyArrLength
      let t = 0
      moneyArrAll = this.money.toString().split(".")
      if (moneyArrAll[0] != undefined) {
        moneyArr= moneyArrAll[0].split("").reverse()
      }
      if (moneyArrAll[1] != undefined) {
        moneyArr2 = moneyArrAll[1].split("")
      }
      moneyArrLength = moneyArr.length
      if (moneyArrLength > 1 && moneyArr[moneyArrLength - 1] == 0) {
        return '输入的金额有误'
      } else if (moneyArrLength == 1 && moneyArr[moneyArrLength - 1] == 0) {
         return '零圆整'
      }
      //1.处理整数部分，从个位开始
      for (let i = 0; i < moneyArrLength; i++) {
        let temp = moneyArr[i]
        //处理单位（不等于0，或者单位为“万”，“亿”的时候才处理金额单位）
        if (temp != 0 || i == 4 || i == 8) {
          chineseArr[t] = this.chineseUnit[i]
          //“亿”后面如果为“万”，“万”不显示
          if (i == 8 && chineseArr[t - 1] == "万") {
            chineseArr[t - 1] = ""
          }
          t++
        }
        //处理数字（针对0的处理：个/万/亿位为0 或者 上一位也为0的不做处理）
        if (temp == 0 && (i == 0 || i == 4 || i == 8 ||moneyArr[i - 1] == 0)) {
          continue
        } else {
          chineseArr[t] = this.chineseList[temp];
          t++
        }
      }
      chineseArr.reverse()
      //2.处理小数部分 从“分”开始
      for (let i = 0; i < moneyArr2.length; i++) {
        let temp = moneyArr2[i]
        if (i == 2) {
          break
        }
        if (i == 0 && temp != 0) {
          chineseArr2[i] = this.chineseList[temp] + '角'
        }
        if (i == 1 && temp != 0){
          if (moneyArr2[i-1] == 0){
            chineseArr2[i] = '零' + this.chineseList[temp] + '分'
          } else {
            chineseArr2[i] = this.chineseList[temp] + '分'
          }
        }
      }
      //3.返回大写金额
      if (chineseArr2.length > 0) {
        return chineseArr.join("") + "圆" +chineseArr2.join("")
      } else {
        return chineseArr.join("") + "圆整"
      }
    }
  }
}
</script>

<style scoped>
div>>>.my-class {
  font-size: 20px;
}

</style>
