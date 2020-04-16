<template>
  <div class="chart" v-loading="chartLoading">
    <div class="tools">
      <ul class="times">
        <li v-for="item in timeList" :key="item.value" :class="timeValue === item.value ? 'is_li':''"
            @click="choiceTime(item)">
          {{item.label}}
        </li>
      </ul>
      <ul class="types">
        <li @click="choiceType('line')" :class="typeValue === 'line' ? 'is_li':''">Line</li>
        <li @click="choiceType('kline')" :class="typeValue === 'kline' ? 'is_li':''">KLine</li>
      </ul>
    </div>
    <div id="Kline" class="kling cb">
    </div>
  </div>

</template>

<script>
  import {init, dispose} from 'klinecharts'
  import {config} from './kLineConfig/config'

  export default {
    data() {
      return {
        dealInfo: {},//交易对信息
        timeList: [
          {value: 1, label: '1M'},
          {value: 5, label: '5M'},
          {value: 10, label: '10M'},
          {value: 30, label: '30M'},
          {value: 60, label: '1H'},
          {value: 360, label: '6H'},
          {value: 720, label: '12H'},
          {value: 1440, label: '1D'},
          {value: 7200, label: '5D'},
        ],//时间列表
        timeValue: 1,//时间选择
        typeValue: 'kline',//图形类型
        klinecharts: null,//K线图实例
        klineData: [
          {
            timestamp: 1587009743000,
            open: 159.06,
            high: 159.65,
            low: 158.94,
            close: 159.45,
            volume: 143.66688,
            turnover: 0
          }
        ],  //初始数据
        resolution: '1',//分辨率
        lang: 'en', //语言
        chartLoading: true, //加载动画
        chartInterval: null,//定时器
      }
    },
    created() {

    },
    mounted() {
      this.klinecharts = init(document.getElementById('Kline'));
      //成交量指标
      this.klinecharts.addTechnicalIndicator('VOL', 100);
      this.initData(this.dealInfo.hash, this.timeValue);

      setInterval(() => {
        let newData = {
          timestamp: (new Date()).getTime(),
          open: Math.floor(Math.random() * 100),
          high: Math.floor(Math.random() * 60),
          low: Math.floor(Math.random() * 80),
          close: Math.floor(Math.random() * 50),
          volume: Math.floor(Math.random() * 1000),
          turnover: 0
        };
        this.klinecharts.updateData(newData);
      }, this.timeValue * 6000)
    },
    destroyed() {
      dispose(document.getElementById('Kline'))
    },
    watch: {
      //todo 切换中英文
      lang: function (newData, oldData) {
        //console.log(newData, oldData);
        if (newData && newData !== oldData) {
          // 设置样式配置
          this.klinecharts.setStyleOptions(config);
        }
      },
    },
    methods: {

      /**
       * @disc: 初始数据
       * @date: 2020-04-14 12:44
       * @author: Wave
       */
      async initData() {
        // 设置样式配置
        this.klinecharts.setStyleOptions(config);
        //隐藏三根辅助线
        this.klinecharts.setTechnicalIndicatorParams("MA", [5, 10, 30]);
        // 设置精度
        this.klinecharts.setPrecision(4, 4);
        //筛入数据
        this.klinecharts.applyNewData(this.klineData, 0);
      },

      /**
       * @disc: 选择时间
       * @params: timeInfo
       * @date: 2020-04-14 15:48
       * @author: Wave
       */
      choiceTime(timeInfo) {
        this.timeValue = timeInfo.value;
        this.klinecharts.clearData();
        this.initData(this.dealInfo.hash, this.timeValue);
      },

      /**
       * @disc: 图标类型选择
       * @params: type
       * @date: 2020-04-14 15:52
       * @author: Wave
       */
      choiceType(type) {
        if (type === 'line') {
          this.klinecharts.setCandleStickChartType('real_time');
        } else {
          this.klinecharts.setCandleStickChartType('candle_stick');
        }
        this.typeValue = type
      },

    },

  }
</script>

<style>
  ol, ul {
    list-style: none;
  }

  .chart {
    height: 800px;
    width: 1200px;
    margin: 20px auto;
    background-color: #0b0c14;
  }

  .tools {
    height: 25px;
    width: 100%;
    background-color: #141627;
    margin: 10px 0 0 0;
  }

  .times li, .types li {
    float: left;
    font-size: 12px;
    padding: 2px 5px;
    color: #7d7f93;
    text-decoration: none;
  }

  .times li:hover, .types li:hover {
    cursor: pointer;
    color: #608FFF;
  }

  .is_li {
    color: #608FFF;
  }

  .times {
    float: left;
    margin: 2px 0 0 10px;
  }

  .types {
    float: right;
    margin: 2px 10px 0 0;
  }

  .kling {
    height: 700px;
    width: 100%;
  }

</style>
