<template>
  <div class="Balllistnext">
    <div class="bl-nav">
      <div class="bl-nav-left">
        <i class="iconfont icon-jiantou" id="turnback" @click="backBalllist"></i>
      </div>
      <div class="bl-nav-title">
        <p>福彩3开奖详情</p>
      </div>
      <div class="bl-nav-right">
      </div>
    </div>
    <li class="lottery-info">
      <header class="lottery-time">
        <span>福彩3</span>
        <b>第{{ssq['IssueName']}}期 {{EndTime}}</b>
        <p class="lottery-number">
          <strong v-for="everyNumber in WinNumber">{{everyNumber}}</strong>
        </p>
      </header>
      <p class="moneyinfo">
        <span>
           <strong>销量：<b>{{saleMoney}}</b>元</strong>
        </span>
      </p>
    </li>
    <ul class="money-nav">
      <li>
        <span ref="titleDetail" class="active">开奖详情</span>
      </li>
    </ul>
    <table>
      <tr>
        <th><span>奖项</span></th>
        <th><span>注数</span></th>
        <th><span>每注奖金</span></th>
      </tr>
      <tr v-for="item in items" id="lottery-level">
        <td><span>{{item['winLevel']}}</span></td>
        <td><span>{{item['winCount']}}</span></td>
        <td><span>{{item['winMoney']}}</span></td>
      </tr>
    </table>
    <button @click="buy3D">
      购买福彩3D
    </button>
  </div>
</template>

<script>
  import Test from '../test'

  export default {
    name: '',
    data () {
      return {
        WinNumber: [],
        ssq: {},
        EndTime: '',
        items: [],
        saleMoney: '',
        totalMoney: '',
        localId: 466380
      }
    },
    methods: {
      testData () {
        this.$request({
          type: 'get',
          url: 'api/data/Handler.ashx?action=601&params={%22IssueID%22:' + this.localId + '}',
          headers: {},
          params: {},
          success: function (res) {
            this.ssq = res.data.data
            this.items = res.data.data.item
            // 销量
            this.saleMoney = res.data.data['SaleMoney']
            // 奖池
            this.totalMoney = res.data.data['TotalMoney']
            this.WinNumber = Test.cutFc3D(this.ssq['WinNumber'])
            this.EndTime = Test.cutTime(this.ssq['EndTime'])
            // 奖项
          },
          failed: function (err) {
            console.log(err)
          }
        })
      },
      backBalllist () {
        this.$router.push({path: '/fc3D'})
      },
      myhandle (val) {
        this.localId = val['changeID']
      },
      // 点击按钮购买彩票
      buy3D () {
        this.$router.push({path: '/fucai3d'})
      }
    },
    mounted () {
      this.testData()
    },
    created () {
      this.$bus.on('get', this.myhandle)
    },
    beforeDestroy () {
      this.$bus.off('get', this.myhandle)
    }
  }
</script>

<style scoped lang="less">
  @import "../../../common/css/style";

  .Balllistnext {
    width: 100%;
    height: 100%;
    background-color: @color-background-white;
    font-family: -apple-system, BlinkMacSystemFont, PingFang SC,
    Helvetica Neue, STHeiti, Microsoft Yahei, Tahoma, Simsun, sans-serif;
  }

  /*头部*/
  .bl-nav {
    width: 100%;
    height: 12vmin;
    background-color: @color-red;
    display: flex;
    justify-content: space-between;
  }

  .bl-nav > div {
    display: inline-block;
  }

  .bl-nav-left {
    width: 20%;
    height: 100%;
    // background-color: green;
    margin-left: 2.8vmin;
    overflow: hidden;
  }

  .bl-nav-right {
    width: 20%;
    height: 100%;
    // background-color: blue;
  }

  .bl-nav-title {
    height: 100%;
    margin: 0 auto;
    overflow: hidden;
  }

  .bl-nav-title p {
    text-align: center;
    font-size: 5.4vmin;
    font-weight: 700;
    color: white;
    line-height: 12vmin;
  }

  /*返回键*/
  #turnback {
    width: 5.86667vmin;
    height: 5.86667vmin;
    color: @color-background-white;
    font-weight: bold;
    font-size: 3.8vmin;
    display: inline-block;
    margin-left: 10px;
    margin-top: 3.7vmin;
  }

  header {
    padding: 3.46667vmin 0 4vmin 4.26667vmin;
  }

  // 获奖信息
  .lottery-time span:first-child {
    font-size: 4.26667vmin;
    font-weight: 400;
  }

  .lottery-time b {
    font-size: 3.46667vmin;
    font-weight: 400;
    margin-left: 1.06667vmin;
    color: @color-text-gray;
  }

  .lottery-number {
    margin-top: 3.73333vmin;
  }

  .lottery-number strong {
    width: 8.26667vmin;
    height: 8.26667vmin;
    line-height: 8.26667vmin;
    border-radius: 50%;
    color: #fff;
    text-align: center;
    display: inline-block;
    font-size: 4vmin;
    font-weight: 700;
    margin-right: 1.6vmin;
    box-sizing: border-box;
    background: #ff5f5f;
  }

  // 奖池和销量
  .moneyinfo {
    font-size: 3.73333vmin;
    border-bottom: 1px solid #e6e6e6;
    height: 10.66667vmin;
    line-height: 10.66667vmin;
    padding-left: 4vmin;
    color: #666;
  }

  .moneyinfo strong {
    margin: 4.53333vmin 0 2.4vmin;
    display: block;
    color: #999;
    font-weight: 400;
    line-height:10.66667vmin;
    font-size: 3.73333vmin;
  }

  .moneyinfo strong b {
    color: #ff5f5f;
    font-size: 3.73333vmin;
  }
  .moneyinfo>span strong:last-child{
    margin:0 0;
  }

  // tab切换奖金信息
  .money-nav {
    display: flex;
  }

  .money-nav li {
    text-align: center;
    line-height: 9.33333vmin;
    align-items: center;
    box-flex: 1;
    margin:0 auto;
  }

  .money-nav li span {
    font-size: 3.73333vmin;
    display: inline-block;
    line-height: 6.66667vmin;
  }
  .active{
    color:#ff5f5f;
    border-bottom: .53333vmin solid #ff5f5f;
  }

  // 奖金表格
  table{
    width: 100%;
  }
  th{
    width: 33.3333333%;
    text-align: center;
    height: 7.2vmin;
    border: 1px solid #e6e6e6;
    display: table-cell;
    vertical-align: inherit;
    line-height: 1.5;
  }
  td{
    width: 33.3333333%;
    text-align: center;
    height: 7.2vmin;
    border: 1px solid #e6e6e6;
    display: table-cell;
    vertical-align: inherit;
    line-height: 1.5;
  }
  th span{
    font-weight: 400;
    font-size: 3.46667vmin;
    color: @color-text-gray;
    padding-top: 1.06667vmin;
    display: block;
  }
  td span{
    font-weight: 400;
    font-size: 3.46667vmin;
    padding-top: 1.06667vmin;
    display: block;
    text-align: center;
    color:#666;
  }
  i{
    width: 2.66667vmin;
    height: 2.66667vmin;
    border-radius: 50%;
    background: #ff5f5f;
    display: inline-block;
  }
  // 购买按钮
  button{
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 10.66667vmin;
    background: #ff5f5f;
    color: #fff;
    font-size: 4.26667vmin;
    border: none;
    text-align: center;
    line-height: 10.66667vmin;
    outline: none;
  }
</style>

