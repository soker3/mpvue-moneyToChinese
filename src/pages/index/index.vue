<template>
  <div>
	<i-notice-bar icon="systemprompt" closable loop>
		{{alert}}
	</i-notice-bar>
	<i-row>
		<i-col span="24" i-class="col-class">
			<i-input type="number"  right title="￥" mode="wrapped" placeholder="输入金额"  @change="changeMoney"/>
		</i-col>
	</i-row>
	<i-row>
		<i-col span="24" i-class="col-class">
			<i-alert>{{chineseMoney}}</i-alert>
		</i-col>
	</i-row>
  </div>
</template>

<script>

export default {
  data () {
    return {
      money: 0,
	  chineseList: ['零','壹','贰','叁','肆','伍','陆','柒','捌','玖'],
	  chineseUnit: ['','拾','佰','仟','万','拾','佰','仟','亿'],
	  alert: '目前只支持整数'
    }
  },
  methods: {
	  changeMoney (e) {
		this.money = e.mp.detail.detail.value
		//console.log('this.inputValue='+ this.inputValue + '|||detail:' + e.mp.detail.detail.value)
	  }
  },
  computed: {
				chineseMoney () {
					var moneyArr = this.money.toString().split('').reverse()
					var chineseArr = []
					var j = 0
					var t = 0
					for (var i = 0; i < moneyArr.length; i++) {
						var temp = moneyArr[i]
						//个位为零的时候不处理
						if (i !== 0 || temp != 0) {
							if (temp != 0 || moneyArr[i-1] != 0) {
								chineseArr[t + 1]= this.chineseList[temp]
								//不等于零的时候才处理金额单位
								if (temp != 0){
									chineseArr[t] = this.chineseUnit[j]
								}
								
							}
						}
						
						t += 2
						//到【亿】的时候重新回到【拾】
						if (j == 7) {
							j = 1
						} else {
							j++
						}
					}
					chineseArr.reverse() 
					return chineseArr.join('') + '元整'
				}
			}
}
</script>

<style>

</style>
