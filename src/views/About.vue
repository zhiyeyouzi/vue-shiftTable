<template>
  <div class="week-home">
<!--    切换星期-->
    <div class="week-table-header">
      <el-tabs v-model="activeName" @tab-click="handleTabClick" style="width: 100%;">
        <el-tab-pane label="轮班" name="first">
          <p></p>
          <h1>轮班</h1>
        </el-tab-pane>
        <el-tab-pane label="资源" name="second">
          <!--星期视图表头-->
          <div class="week">
            <div class="week-day"></div>
            <div v-for="(item, index) in weekBox" :key="index" :class="item.day.num!== todayDate?'week-day':'week-day week-day-active'">
              {{ item.day.week }}
            </div>
          </div>
<!--          下方数据展示-->
          <div class="week-event">
            <div class="week-event-item" v-for="(item, index) in analogData" :key="index">
<!--              按区域区分-->
              <div class="week-event-area">{{item.area}}</div>
<!--              区域下的每个人的任务-->
              <div class="week-event-table" v-for="(itemA, indexA) in item.data" :key="indexA">
                <div class="week-item">
<!--                  左侧展示的每个人-->
                  <div class="week-item-day" >{{itemA.title}}</div>
<!--                  右侧的日历格子-->
                  <div class="week-item-day" v-for="(itemBox, indexBox) in weekBox"
                       :key="indexBox"
                       @click="handleGridDate(itemBox, itemA)">
<!--                    <div v-for="(itemB, indexB) in itemA.data"-->
<!--                         :key="indexB" class="week-item-order"-->
<!--                         v-show="itemBox.day.num === itemB.date">-->
<!--                        {{itemB.title}}-->
<!--                    </div>-->
                    <div v-for="(itemB, indexB) in itemA.data" :key="indexB" class="week-item-order">
                      <template
                          v-if="itemBox.day.num === itemB.date">
                        {{itemB.title}}
                      </template>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </el-tab-pane>
      </el-tabs>
<!--      右上角切换按钮-->
      <div class="btn-box" v-if="false">
        <el-button size="small" @click="gotoWeek(-1)">
          <i class="el-icon-arrow-left"></i>
        </el-button>
        <el-button size="small" @click="gotoWeek(1)" style="margin-left: 0;">
          <i class="el-icon-arrow-right"></i>
        </el-button>
        <el-button size="small" @click="gotoWeek(0)">
          今天
        </el-button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "About",
  data() {
    return {
      // 模拟数据
      analogData: [
        {
          area: '大陆',
          data: [
            {
              title: "未分配",
              data: []
            },
            {
              title: '张珊',
              data: [
                {date: '2021-02-05', title: '迪'},
                {date: '2021-02-05', title: '加'},
                {date: '2021-02-03', title: '阿'},
                {date: '2021-02-04', title: '他'},
                {date: '2021-02-07', title: '马'},
              ]
            },
            {
              title: '李思',
              data: [
                {date: '2021-02-08', title: 'xixix1'},
                {date: '2021-02-01', title: 'xixix2'},
                {date: '2021-02-03', title: 'xixix3'},
                {date: '2021-02-04', title: 'xixix4'},
                {date: '2021-02-07', title: 'xixix5'},
              ]
            },
          ]
        },
        // {
        //   area: '港澳台',
        //   data: [
        //     {
        //       title: '绝望',
        //       data: [
        //         {date: '2021-2-5', title: 'hahhah'},
        //         {date: '2021-2-1', title: 'hahhah'},
        //         {date: '2021-2-3', title: 'hahhah'},
        //         {date: '2021-2-4', title: 'hahhah'},
        //         {date: '2021-2-7', title: 'hahhah'},
        //       ]
        //     },
        //     {
        //       title: '太难了',
        //       data: [
        //         {date: '2021-2-5', title: '111ah'},
        //         {date: '2021-2-1', title: '111ah'},
        //         {date: '2021-2-3', title: '111ah'},
        //         {date: '2021-2-4', title: '111ah'},
        //         {date: '2021-2-7', title: '111ah'},
        //       ]
        //     },
        //   ]
        // },
      ],
      activeName: 'second',
      // 今天
      todayDate: '',
      // 当前日期
      currentFirstDate: null,
      // 星期日期展示
      weekBox: [
        { id: 0, day: {}},
        { id: 1, day: {}},
        { id: 2, day: {}},
        { id: 3, day: {}},
        { id: 4, day: {}},
        { id: 5, day: {}},
        { id: 6, day: {}},
      ],
    }
  },
  methods: {
    handleClickEvent() {

    },
    // 点击格子
    handleGridDate(week, item) {
      this.$message.info(item.title +'——————————'+ week.day.num)
      // console.log(week)
      // console.log(item)
    },
    // 切换视图
    handleTabClick(tab, event) {
      console.log(tab, event);
    },
    // 切换时间
    gotoWeek(flag) {
      if (flag === -1) {
        this.handleDate(this.addDate(this.currentFirstDate,-7))
      } else if (flag === 1 ) {
        this.handleDate(this.addDate(this.currentFirstDate,7));
      } else if (flag === 0 ) {
        let d = new Date()
        this.handleDate(this.addDate(d, -1))
      }
    },
    // 格式化时间
    formatDate(date){
      // let year = date.getFullYear()+'年';
      let month = (date.getMonth()+1)+'月';
      let day = date.getDate()+'日';
      let week = ['周日','周一','周二','周三','周四','周五','周六'][date.getDay()];

      let yy = date.getFullYear()
      let mm =
          date.getMonth() + 1 < 10
              ? '0' + (date.getMonth() + 1)
              : date.getMonth() + 1
      let dd = date.getDate() < 10 ? '0' + date.getDate() : date.getDate()

      let obj = {
        week: month + day + ' ' + week,
        num: yy + '-' + mm + '-' + dd
      }

      return obj
    },
    // 处理时间
    addDate(date, n) {
      date.setDate(date.getDate() + n);
      return date;
    },
    // 处理时间
    handleDate(date) {
      let week = date.getDay() - 1;
      date = this.addDate(date, week * -1);
      this.currentFirstDate = new Date(date);
      this.weekBox.forEach((item, index)=>{
        item.day = this.formatDate(index === 0 ? date : this.addDate(date,1))
      })
    },
    // 获取当天日期
    getToday() {
      let dd = new Date()
      let year = dd.getFullYear()
      let month =
          dd.getMonth() + 1 < 10
              ? '0' + (dd.getMonth() + 1)
              : dd.getMonth() + 1
      let day = dd.getDate() < 10 ? '0' + dd.getDate() : dd.getDate()

      this.todayDate = year + '-' + month + '-' + day
    },
  },
  mounted() {
    // 初始化时间
    let d = new Date()
    this.currentFirstDate = d
    this.handleDate(this.addDate(d, -1))
    this.getToday()
  },
  created() {
  }
}
</script>

<style scoped lang="scss">

.week-table-header{
  width: 100%;
  position: relative;
}

.btn-box{
  position: absolute;
  right: 10px;
  top: 0;
  display: flex;
  justify-content: flex-end;
}

.week {
  display: flex;
  justify-content: space-around;
  width: 100%;

  .week-day, .week-day-active{
    width: 12.5%;
    border: 1px solid #DEDCDA;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 50px;
    font-size: 12px;
    border-right: none;
  }
  .week-day {
    //background-color: pink;
    color: #3e3e3c;
  }
  .week-day-active{
    //background-color: lightblue;
    color: #1589ee;
  }
  .week-day:last-child{
    border-right: 1px solid #DEDCDA;
  }
}

.week-event{
  .week-event-item{
    .week-event-area{
      background: #f3f2f2;
      width: 100%;
      height: 32px;
      line-height: 32px;
      padding: 0 8px;
      text-align: left;
      font-size: 14px;
      color: #080707;
      border: 1px solid #DEDCDA;
    }
    .week-event-area:hover{
      cursor: pointer;
      background: #eef1f6;
    }

    .week-event-table{
      .week-item{
        display: flex;
        justify-content: space-around;
        width: 100%;
        .week-item-day-left{
          width: 12.5%;
          border: 1px solid #DEDCDA;
          display: flex;
          align-items: center;
          justify-content: center;
          min-height: 50px;
          font-size: 12px;
          border-right: none;
          padding: 5px;
          box-sizing: border-box;
        }
        .week-item-day{
          width: 12.5%;
          border: 1px solid #DEDCDA;
          min-height: 50px;
          font-size: 12px;
          border-right: none;
          padding: 5px;
          box-sizing: border-box;
          .week-item-order{
            width: 100%;
            height: 46px;
            background-color: pink;
            border: 1px solid #DEDCDA;
            border-radius: 3px;
            margin-bottom: 10px;
            color: #1589ee;
          }
        }
        .week-item-day:last-child{
          border-right: 1px solid #DEDCDA;
        }
      }
    }
  }

}

/deep/.el-tabs__header{
  margin: 0;
}
</style>
