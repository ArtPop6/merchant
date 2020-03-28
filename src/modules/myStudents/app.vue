<template>
  <div class="student-wrap">
    <div class="coach-info">
      <div class="coach-name">童壮壮教练</div>
      <div class="greet-word">你好，这是你近60天的课程</div>
    </div>

    <img alt class="barbell" src="./img/barbell.png" />

    <div class="date-pick__current">
      <div class="date-pick__year">{{currentYear}}</div>
      <div class="date-pick__title">年</div>
      <div class="date-pick__month">{{currentMonth}}</div>
      <div class="date-pick__title">月</div>
      <!-- <div class="curren">{{currentDate}}</div> -->
    </div>

    <div class="date-pick__smallwrap">
      <!-- :style="getTransform"
      @touchend="touchEnd"
      @touchmove="touchMove"
      @touchstart="touchStart"
      @transitionend="transitionEnd"
      @webkit-transition-end="transitionEnd"-->
      <!-- <div
        :style="getTransform"
        @touchend="touchEnd"
        @touchmove="touchMove"
        @touchstart="touchStart"
        @transitionend="transitionEnd"
        @webkit-transition-end="transitionEnd"
        class="date-pick__wrap"
        ref="sliders"
      >
        <div :key="index" class="date-pick" v-for="(item, index) in weekDates">
          <div :key="index2" @click="clickDate(index2)" class="date-pick__threeweeks" v-for="(week, index2) in item">
            <div :class="currentIndex === index2 ? 'today-week' : ''" class="week-day">{{chineseWeek(week.week)}}</div>
            <div
              :class="currentIndex === index2 ? 'today-date' : ''"
              class="date"
            >{{week.isToday ? '今' : week.date.substring(8)}}</div>
          </div>
        </div>
      </div>-->
      <div
        :style="getTransform"
        @touchend="touchEnd"
        @touchmove="touchMove"
        @touchstart="touchStart"
        @transitionend="transitionEnd"
        @webkit-transition-end="transitionEnd"
        class="date-pick__wrap"
        ref="sliders"
      >
        <div :key="index" class="date-pick" v-for="(item, index) in dates">
          <div
            :key="index2"
            @click="clickDate(index2)"
            class="date-pick__threeweeks"
            v-for="(week, index2) in getCurrentWeekDates(item.date)"
          >
            <div :class="currentIndex === index2 ? 'today-week' : ''" class="week-day">{{chineseWeek(week.week)}}</div>
            <div
              :class="currentIndex === index2 ? 'today-date' : ''"
              class="date"
            >{{week.isToday ? '今' : week.date.substring(8)}}</div>
          </div>
        </div>
      </div>
    </div>

    <div class="course-info__wrap">
      <div :key="index" class="course-date" v-for="(item, index) in courseData">
        <div class="course-store">
          <div class="store-name">{{item.store_name}}</div>
          <!-- <div class="hole">
            <div class="hole-one"></div>
            <div class="hole-one"></div>
            <div class="hole-one"></div>
          </div>-->
        </div>
        <div :key="indexs" @click="goStudentList()" class="course-date__class" v-for="(course, indexs) in item.course">
          <div class="course-seat">
            <img :src="course.coach_avatar" alt class="avatar" />
            <div class="course-desc">
              <div class="course-name">{{course.course_name}}</div>
              <div class="price-person">
                <div class="course-price">￥{{course.price}}</div>
                <div class="course-person">2人/5人</div>
              </div>
            </div>
          </div>
          <div class="course-right">
            <div class="student-list">学员列表</div>
            <div class="course-time">{{course.begin_time + '-' + course.end_time }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import "./style.stylus";
import param from "@utils/url-param";
import { go } from "@utils/goto";
import moment from "moment";

export default {
  data() {
    return {
      // new
      dates: [],
      currentWeek: [],
      // old
      currentDate: "",
      weekDates: [[], [], []],
      currentIndex: "",
      weekDay: [
        { date: "", week: 7 },
        { date: "", week: 1 },
        { date: "", week: 2 },
        { date: "", week: 3 },
        { date: "", week: 4 },
        { date: "", week: 5 },
        { date: "", week: 6 }
      ],
      courseData: [
        {
          store_id: 1,
          store_name: "思北",
          course: [
            {
              course_id: 1,
              course_name: "Zumba 尊巴",
              coach_avatar:
                "https://dss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=1457142803,4010601568&fm=26&gp=0.jpg",
              begin_time: "15:00",
              end_time: "16:00",
              space_name: "瑜伽室A",
              seat: 5,
              price: 79
            },
            {
              course_id: 1,
              course_name: "Zumba 尊巴",
              coach_avatar:
                "https://dss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=1457142803,4010601568&fm=26&gp=0.jpg",
              begin_time: "16:00",
              end_time: "17:00",
              space_name: "瑜伽室B",
              seat: 6,
              price: 135
            }
          ]
        },
        {
          store_id: 2,
          store_name: "新景",
          course: [
            {
              course_id: 1,
              course_name: "Zumba 尊巴",
              coach_avatar:
                "https://dss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=1457142803,4010601568&fm=26&gp=0.jpg",
              begin_time: "15:00",
              end_time: "16:00",
              space_name: "瑜伽室A",
              seat: 5
            },
            {
              course_id: 1,
              course_name: "Zumba 尊巴",
              coach_avatar:
                "https://dss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=1457142803,4010601568&fm=26&gp=0.jpg",
              begin_time: "16:00",
              end_time: "17:00",
              space_name: "瑜伽室B",
              seat: 6,
              price: 135
            }
          ]
        }
      ],
      endx: 0,
      actIndex: 1,
      sliderWidth: 375,
      distan: { x: 0, y: 0 },
      isAnimation: false,
      start: { x: 0, y: 0 },
      direction: "",
      clickIndex: ""
    };
  },
  computed: {
    getTransform() {
      // console.log("getTransform222");
      this.endx = -this.actIndex * this.sliderWidth + this.distan.x;
      // this.endx = 1500;
      const style = {};
      style["transform"] = "translateX(" + this.endx + "px)";
      // style["color"] = "red";
      style["transition"] = this.isAnimation
        ? "transform .5s ease-out"
        : "none";
      return style;
    },
    chineseWeek() {
      return week => {
        if (week === 1) {
          return "一";
        }
        if (week === 2) {
          return "二";
        }
        if (week === 3) {
          return "三";
        }
        if (week === 4) {
          return "四";
        }
        if (week === 5) {
          return "五";
        }
        if (week === 6) {
          return "六";
        }
        if (week === 7) {
          return "日";
        }
      };
    },
    currentYear() {
      // return this.dates[1].date.format("YYYY");
      return moment(this.currentDate).format("YYYY");
    },
    currentMonth() {
      return moment(this.currentDate).format("MM");
    }
  },
  watch: {},
  methods: {
    // new
    getDatesInfo() {
      this.dates[0] = { date: moment().subtract(7, "d") };
      this.dates[1] = { date: moment() };
      this.dates[2] = { date: moment().add(7, "d") };
      console.log(this.dates);
      this.judgeCurrentIndex();
      this.currentDate = this.dates[1].date.format("YYYY-MM-DD");
    },
    // old
    goStudentList() {
      go("./studentList.html");
    },
    // getDate() {
    //   const _currentWeek = Number(moment().format("E"));
    //   const _lastWeek = moment()
    //     .subtract(7, "d")
    //     .format("YYYY-MM-DD");
    //   console.log(_lastWeek);
    //   for (let i = 0; i < this.weekDay.length; i++) {
    //     if (this.weekDay[i].week === _currentWeek) {
    //       this.weekDay[i].date = moment()
    //         .format("YYYY-MM-DD")
    //         .substring(5);
    //     }
    //   }
    //   console.log(this.weekDay);
    // },
    getCurrentWeekDates(date) {
      const weekOfDate = Number(moment(date).format("E"));
      const weeks = [7, 1, 2, 3, 4, 5, 6];
      const today = moment();
      const arr = [];
      for (let i = 0; i < 7; i++) {
        const _theDate = moment(date).subtract(weekOfDate - i, "d");
        arr.push({
          date: _theDate.format("YYYY-MM-DD"),
          moDate: _theDate,
          week: weeks[i],
          isToday: _theDate.format("YYYY-MM-DD") === today.format("YYYY-MM-DD")
        });
      }
      // console.log(arr);
      return arr;
    },
    getThreeWeekDates() {
      // const _date = this.currentDate;
      this.weekDates[0] = this.getCurrentWeekDates(
        this.currentDate.subtract(7, "d")
      );
      // const _datetwo = this.currentDate;
      // console.log("dateTwo", _datetwo);
      this.weekDates[1] = this.getCurrentWeekDates(
        this.currentDate.add(7, "d")
      );
      // console.log("getThreeWeekDates", this.currentDate, this.weekDates[1]);
      // const _datethree = this.currentDate;
      // console.log("date3", _datethree);
      this.weekDates[2] = this.getCurrentWeekDates(
        this.currentDate.add(7, "d")
      );
      console.log(
        "getThreeWeekDates最终结果",
        this.currentDate,
        this.weekDates
      );
    },
    // getLastWeek() {
    //   this.weekDates = [];
    //   this.currentDate = this.currentDate.subtract(7, "d");
    //   this.getCurrentWeekDates(this.currentDate);
    // },
    // getNextWeek() {
    //   this.weekDates = [];
    //   this.currentDate = this.currentDate.add(7, "d");
    //   this.getCurrentWeekDates(this.currentDate);
    // },
    clickDate(index) {
      this.currentIndex = index;
      // const _currentWeek = this.weekDates[1]
      // this.currentDate = this.getCurrentWeekDates(
      //   this.dates[this.actIndex].date
      // );
      this.currentWeek = this.getCurrentWeekDates(
        this.dates[this.actIndex].date
      );
      this.currentDate = this.currentWeek[index].date;
      console.log(
        "那一周数据",
        this.getCurrentWeekDates(this.dates[this.actIndex].date)
      );

      // this.clickIndex = index;
    },
    // 选取默认选中的值
    judgeCurrentIndex() {
      if (Number(this.dates[this.actIndex].date.format("E")) === 7) {
        this.currentIndex = 0;
        return;
      }
      this.currentIndex = Number(this.dates[this.actIndex].date.format("E"));
    },
    // getTransform() {
    //   console.log("getTransform222");
    //   // this.endx = -this.actIndex * this.sliderWidth + this.distan.x;
    //   // this.endx = 1500;
    //   const style = {};
    //   style["transform"] = "translateX(" + this.endx + "px)";
    //   style["color"] = "red";
    //   style["transition"] = this.isAnimation
    //     ? "transform .5s ease-out"
    //     : "none";
    //   return style;
    // },
    touchStart(e) {
      // console.log("touchStart");
      this.start.x = e.touches[0].pageX;
      // console.log("start", this.weekDates);
    },
    touchMove(e) {
      // 这里需要过渡动画
      // console.log("touchMove", e);
      this.isAnimation = true;
      this.distan.x = e.touches[0].pageX - this.start.x;
      // 需要移动的容器
      const dom = this.$refs.sliders;
      // 向左
      this.endx = this.endx + this.distan.x;
      // this.endx = "350";
      dom.style["transform"] = "translateX(" + this.endx + "px)";
    },
    touchEnd(e) {
      console.log("touchEnd");
      this.isAnimation = true;
      this.distan.x = e.changedTouches[0].pageX - this.start.x;
      // 向右
      if (this.distan.x > 0) {
        this.direction = "right";
        this.actIndex = 0;
        // this.currentDate = this.currentDate.subtract(7, "d");
      } else if (this.distan.x < 0) {
        this.direction = "left";
        this.actIndex = 2;
        // this.currentDate = this.currentDate.add(7, "d");
      }
      this.distan.x = 0;
    },
    transitionEnd() {
      this.isAnimation = false;
      // console.log("transitionEnd");
      if (this.actIndex === 2) {
        // this.weekDates = [];
        // this.currentDate = this.currentDate.add(7, "d");
        // console.log("shift前", this.weekDates);
        this.dates.push({
          date: moment(this.dates[this.actIndex].date).add(7, "d")
        });
        this.dates.shift();
        const _currentWeek = this.getCurrentWeekDates(this.dates[1].date);
        this.currentDate = _currentWeek[this.currentIndex].date;
        // this.weekDates.shift();
        // this.weekDates[0] = this.weekDates[1];
        // this.weekDates[1] = this.weekDates[2];
        // this.weekDates[2] = [];

        // console.log("shift后", this.weekDates);

        console.log("shift后", this.weekDates);
        // this.getThreeWeekDates();

        this.actIndex = 1;
        // this.currentDate = this.weekDates[1][this.currentIndex].moDate;
        // this.weekDates[2] = this.getCurrentWeekDates(
        //   this.currentDate.add(7, "d")
        // );
        // console.log("transitionEnd", this.currentDate, this.weekDates);
        // this.dates.push({
        //   date: moment(this.dates[this.actIndex].date)
        //     .add(7, "d")
        //     .format("YYYY-MM-DD")
        // });
        // this.dates.shift();
        // this.actIndex = 1;
      } else if (this.actIndex === 0) {
        // this.currentDate = this.currentDate.subtract(14, "d");

        // this.currentDate = this.currentDate.subtract(7, "d");
        // this.getThreeWeekDates();
        // console.log("transitionEnd", this.weekDates);
        this.dates.unshift({
          date: moment(this.dates[this.actIndex].date).subtract(7, "d")
        });
        this.dates.pop();
        this.actIndex = 1;
        const _currentWeek = this.getCurrentWeekDates(this.dates[1].date);
        this.currentDate = _currentWeek[this.currentIndex].date;
        // this.currentDate = this.weekDates[1][this.currentIndex].moDate;
        // this.weekDates.unshift(this.getCurrentWeekDates(this.currentDate));
        // this.weekDates.pop();
        // console.log(this.weekDates);
        // console.log("transitionEnd", this.currentDate, this.weekDates);
        // this.dates.unshift({
        //   date: moment(this.dates[this.actIndex].date)
        //     .subtract(7, "d")
        //     .format("YYYY-MM-DD")
        // });
        // this.dates.pop();
        // this.actIndex = 1;
      }
    }
  },
  created() {
    // this.getDate();
    // this.currentDate = moment();
    // this.currentDate.subtract(7, "d");
    // console.log("生命周期测试", this.currentDate);
    // 获取当前一周的日期与星期
    // this.getCurrentWeekDates(moment());

    // this.getThreeWeekDates();
    // this.judgeCurrentIndex();

    this.getDatesInfo();
  }
};
</script>