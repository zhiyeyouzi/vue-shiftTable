<template>
  <div class="week-home">
<!--    切换星期-->
    <div class="blank-space" >
      <h2>轮班表</h2>
    </div>
    <div class="week-table-header">
      <!-- <el-tabs v-model="activeName" @tab-click="handleTabClick" style="width: 100%;">
        <el-tab-pane label="轮班" name="first">
          <p></p>
          <h1>轮班</h1>
        </el-tab-pane>
        <el-tab-pane label="资源" name="second">
        </el-tab-pane>
      </el-tabs> -->
      <div class="header-option">
        <div class="tab-view">
          <div :class="activeName===0?'tab-view-item tab-view-item-active':'tab-view-item'" @click="handleTabClick(0)">轮班</div>
          <div :class="activeName===1?'tab-view-item tab-view-item-active':'tab-view-item'" @click="handleTabClick(1)">资源</div>
        </div>
        <!--      右上角切换按钮-->
        <div class="btn-box" v-if="true">
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
<!--      轮班-->
      <div v-show="activeName===0">
        <!--星期视图表头-->
        <div class="week">
          <div class="week-day"></div>
          <div v-for="(item, index) in weekBox" :key="index" :class="item.day.num!== todayDate?'week-day':'week-day week-day-active'">
            {{ item.day.week }}
          </div>
        </div>
      </div>
<!--      资源-->
      <div v-show="activeName===1">
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
            <div class="week-event-area" @click="tapAreaStatus(item, index)">{{item.area}}</div>
            <!--              区域下的的任务-->
            <div class="week-event-table" v-for="(itemA, indexA) in item.data" :key="indexA" v-show="item.openStatus">
              <div class="week-item">
                <!--                  左侧展示的每个人-->
                <div class="week-item-day-left">
                  <!--                    未分配-->
                  <div class="item-day-left-unallot" v-if="indexA === 0">
                    {{itemA.title}}
                  </div>
                  <!--                    已分配给人-->
                  <div class="item-day-left-alloted" v-else>
                    <img src="" alt="icon">
                    {{itemA.title}}
                  </div>
                </div>
                <!--                  右侧的日历格子-->
                <div class="week-item-day" v-for="(itemBox, indexBox) in weekBox"
                     :key="indexBox"
                     @click="handleGridDate(itemBox, itemA)">
                  <!--                    第一种-->
                  <!--                    <div v-for="(itemB, indexB) in itemA.data"-->
                  <!--                         :key="indexB" class="week-item-order"-->
                  <!--                         @click.stop="handleClickEvent"-->
                  <!--                         v-show="itemBox.day.num === itemB.date">-->
                  <!--                      <div class="week-item-order-t">{{itemB.date}}我你笑ID解散分开了较好的是功夫就可获得更好的福利卡改好了打分卡挂号费打卡记录挂号费大骨灰级阿卡丽</div>-->
                  <!--                      <div class="week-item-order-b">{{itemB.status + itemB.title}}撒京东快地方噶广东省防范大哥发的刮大风</div>-->
                  <!--                    </div>-->
                  <!--                    第二种-->
                  <!--                    <div v-for="(itemB, indexB) in itemA.data" :key="indexB" class="week-item-order">-->
                  <!--                      <template-->
                  <!--                          v-if="itemBox.day.num === itemB.date">-->
                  <!--                        <div class="week-item-order-t">{{itemB.date}}</div>-->
                  <!--                        <div class="week-item-order-b">{{itemB.status + itemB.title}}</div>-->
                  <!--                      </template>-->
                  <!--                    </div>-->
                </div>
              </div>
            </div>
          </div>
        </div>
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
          area: '中国',
          openStatus: true,
          data: [
            {
              title: "未分配",
              data: []
            },
            {
              title: '张珊',
              data: [
                {date: '2021-02-05', title: '迪', status: "pending"},
                {date: '2021-02-05', title: '加', status: "pending"},
                {date: '2021-02-03', title: '阿', status: "pending"},
                {date: '2021-02-04', title: '他', status: "pending"},
                {date: '2021-02-07', title: '马', status: "pending"},
              ]
            },
            {
              title: '李思',
              data: [
                {date: '2021-02-08', title: 'xixix1', status: "accepted"},
                {date: '2021-02-01', title: 'xixix2', status: "accepted"},
                {date: '2021-02-03', title: 'xixix3', status: "accepted"},
                {date: '2021-02-04', title: 'xixix4', status: "accepted"},
                {date: '2021-02-07', title: 'xixix5', status: "accepted"},
              ]
            },
          ]
        },
        {
          area: '日本',
          openStatus: true,
          data: [
            {
              title: "未分配",
              data: []
            },
            {
              title: '王舞',
              data: [
                {date: '2021-02-05', title: 'hahhah'},
                {date: '2021-02-01', title: 'hahhah'},
                {date: '2021-02-03', title: 'hahhah'},
                {date: '2021-02-04', title: 'hahhah'},
                {date: '2021-02-07', title: 'hahhah'},
              ]
            },
            {
              title: '赵琉',
              data: [
                {date: '2021-02-05', title: '111ah'},
                {date: '2021-02-01', title: '111ah'},
                {date: '2021-02-03', title: '111ah'},
                {date: '2021-02-04', title: '111ah'},
                {date: '2021-02-07', title: '111ah'},
              ]
            },
          ]
        },{
          area: '美国',
          openStatus: true,
          data: [
            {
              title: "未分配",
              data: []
            },
            {
              title: '张珊',
              data: [
                {date: '2021-02-05', title: '迪', status: "pending"},
                {date: '2021-02-05', title: '加', status: "pending"},
                {date: '2021-02-03', title: '阿', status: "pending"},
                {date: '2021-02-04', title: '他', status: "pending"},
                {date: '2021-02-07', title: '马', status: "pending"},
              ]
            },
            {
              title: '李思',
              data: [
                {date: '2021-02-08', title: 'xixix1', status: "accepted"},
                {date: '2021-02-01', title: 'xixix2', status: "accepted"},
                {date: '2021-02-03', title: 'xixix3', status: "accepted"},
                {date: '2021-02-04', title: 'xixix4', status: "accepted"},
                {date: '2021-02-07', title: 'xixix5', status: "accepted"},
              ]
            },
          ]
        },
        {
          area: '韩国',
          openStatus: true,
          data: [
            {
              title: "未分配",
              data: []
            },
            {
              title: '王舞',
              data: [
                {date: '2021-02-05', title: 'hahhah'},
                {date: '2021-02-01', title: 'hahhah'},
                {date: '2021-02-03', title: 'hahhah'},
                {date: '2021-02-04', title: 'hahhah'},
                {date: '2021-02-07', title: 'hahhah'},
              ]
            },
            {
              title: '赵琉',
              data: [
                {date: '2021-02-05', title: '111ah'},
                {date: '2021-02-01', title: '111ah'},
                {date: '2021-02-03', title: '111ah'},
                {date: '2021-02-04', title: '111ah'},
                {date: '2021-02-07', title: '111ah'},
              ]
            },
          ]
        },{
          area: '英国',
          openStatus: true,
          data: [
            {
              title: "未分配",
              data: []
            },
            {
              title: '张珊',
              data: [
                {date: '2021-02-05', title: '迪', status: "pending"},
                {date: '2021-02-05', title: '加', status: "pending"},
                {date: '2021-02-03', title: '阿', status: "pending"},
                {date: '2021-02-04', title: '他', status: "pending"},
                {date: '2021-02-07', title: '马', status: "pending"},
              ]
            },
            {
              title: '李思',
              data: [
                {date: '2021-02-08', title: 'xixix1', status: "accepted"},
                {date: '2021-02-01', title: 'xixix2', status: "accepted"},
                {date: '2021-02-03', title: 'xixix3', status: "accepted"},
                {date: '2021-02-04', title: 'xixix4', status: "accepted"},
                {date: '2021-02-07', title: 'xixix5', status: "accepted"},
              ]
            },
          ]
        },
        {
          area: '法国',
          openStatus: true,
          data: [
            {
              title: "未分配",
              data: []
            },
            {
              title: '王舞',
              data: [
                {date: '2021-02-05', title: 'hahhah'},
                {date: '2021-02-01', title: 'hahhah'},
                {date: '2021-02-03', title: 'hahhah'},
                {date: '2021-02-04', title: 'hahhah'},
                {date: '2021-02-07', title: 'hahhah'},
              ]
            },
            {
              title: '赵琉',
              data: [
                {date: '2021-02-05', title: '111ah'},
                {date: '2021-02-01', title: '111ah'},
                {date: '2021-02-03', title: '111ah'},
                {date: '2021-02-04', title: '111ah'},
                {date: '2021-02-07', title: '111ah'},
              ]
            },
          ]
        },
        {
          area: '德国',
          openStatus: true,
          data: [
            {
              title: "未分配",
              data: []
            },
            {
              title: '王舞',
              data: [
                {date: '2021-02-05', title: 'hahhah'},
                {date: '2021-02-01', title: 'hahhah'},
                {date: '2021-02-03', title: 'hahhah'},
                {date: '2021-02-04', title: 'hahhah'},
                {date: '2021-02-07', title: 'hahhah'},
              ]
            },
            {
              title: '赵琉',
              data: [
                {date: '2021-02-05', title: '111ah'},
                {date: '2021-02-01', title: '111ah'},
                {date: '2021-02-03', title: '111ah'},
                {date: '2021-02-04', title: '111ah'},
                {date: '2021-02-07', title: '111ah'},
              ]
            },
          ]
        },
        {
          area: '俄罗斯',
          openStatus: true,
          data: [
            {
              title: "未分配",
              data: []
            },
            {
              title: '王舞',
              data: [
                {date: '2021-02-05', title: 'hahhah'},
                {date: '2021-02-01', title: 'hahhah'},
                {date: '2021-02-03', title: 'hahhah'},
                {date: '2021-02-04', title: 'hahhah'},
                {date: '2021-02-07', title: 'hahhah'},
              ]
            },
            {
              title: '赵琉',
              data: [
                {date: '2021-02-05', title: '111ah'},
                {date: '2021-02-01', title: '111ah'},
                {date: '2021-02-03', title: '111ah'},
                {date: '2021-02-04', title: '111ah'},
                {date: '2021-02-07', title: '111ah'},
              ]
            },
          ]
        },
      ],
      activeName: 1,
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
    // 切换区域状态
    tapAreaStatus(item, index) {
      this.analogData[index].openStatus = !item.openStatus
    },
    // 阻止事件穿透
    handleClickEvent() {

    },
    // 点击格子
    handleGridDate(week, item) {
      this.$message.info(item.title +'——————————'+ week.day.num)
      // console.log(week)
      // console.log(item)
    },
    // 切换视图
    handleTabClick(tab) {
      this.activeName = tab
    },
    // 切换时间
    gotoWeek(flag) {
      console.log(flag);
      // if (flag === -1) {
      //   this.handleDate(this.addDate(this.currentFirstDate,-7))
      // } else if (flag === 1 ) {
      //   this.handleDate(this.addDate(this.currentFirstDate,7));
      // } else if (flag === 0 ) {
      //   let d = new Date()
      //   this.handleDate(this.addDate(d, -1))
      // }
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

.week-home{
  padding:  0 10px 10px 10px;
}

//占位用
.blank-space{
  height: 78px;
  width: calc(100% - 20px);
  background: #ffffff;
  z-index: 101;
  position: fixed;
  display: flex;
  align-items: center;
  justify-content: center;
}

.week-table-header{
  width: 100%;
  position: relative;
}

.header-option{
  display: flex;
  justify-content: space-between;
  align-items: center;
  position: fixed;
  top: 60px;
  background-color: #fff;
  width: calc(100% - 42px);
  height: 40px;
  z-index: 101;
  border: 1px solid #DEDCDA;
  padding: 0 10px;
  .tab-view{
    display: flex;
    height: 40px;
    .tab-view-item{
      cursor: pointer;
      padding: 0 5px;
      font-size: 14px;
      color: #333333;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .tab-view-item:first-child{
      margin-right: 30px;
    }
    .tab-view-item:hover{

    }
    .tab-view-item-active{
      color: #080707;
      font-weight: bold;
      border-bottom: 2px solid #006DCC;
    }
  }
}
.btn-box{
  display: flex;
  justify-content: flex-end;
}

.week {
  display: flex;
  justify-content: space-around;
  width: calc(100% - 20px);
  background-color: #fff;
  position: fixed;
  top: 100px;
  z-index: 101;
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
  padding-top: 152px;
  .week-event-item{
    .week-event-area{
      position: sticky;
      top: 152px;
      background: #f3f2f2;
      //width: 100%;
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
          min-height: 50px;
          font-size: 12px;
          border-right: none;
          padding: 5px;
          box-sizing: border-box;
          .item-day-left-unallot, .item-day-left-alloted{
            font-size: 12px;
            line-height: 16px;
            padding-left: 10px;
          }
          .item-day-left-unallot{
            font-weight: bold;
            color: #080707;
          }
          .item-day-left-alloted{
            color: #006DCC;
            text-decoration:underline;
            cursor: pointer;
            img{
              width: 20px;
              height: 20px;
            }
          }
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
            border: 1px solid #DEDCDA;
            border-radius: 3px;
            margin-bottom: 10px;
            border-left: 3px solid red;
            padding: 6px 10px;
            box-sizing: border-box;
            .week-item-order-t, .week-item-order-b{
              text-align: left;
              font-size: 12px;
              line-height: 16px;
              overflow: hidden;
              white-space: nowrap;
              text-overflow: ellipsis;
            }
            .week-item-order-t{
              color: #080707;
              font-weight: bold;
              cursor: pointer;
            }
            .week-item-order-t:hover{ text-decoration:underline;}
            .week-item-order-b{
              color: #333333;
            }
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
