<template>
  <div class="list-wrap">
    <div class="course-desc">
      <div class="course-desc__wrap">
        <div class="course-desc__store">无竞悦动智享馆.思北店</div>
        <div class="course-desc__info">
          <div class="course-desc__left">
            <img alt class="coach-avatar" src />
            <div class="course-detail">
              <div class="course-name">Zumba 尊巴</div>
              <div class="course-price-count">
                <div class="course-price">￥79</div>
                <div class="course-count"></div>
              </div>
            </div>
          </div>
          <div class="course-desc__right">
            <div class="course-desc__time">18:30-19:15</div>
            <div class="course-count">19人/20人</div>
          </div>
        </div>
      </div>
    </div>

    <div class="student-list__wrap">
      <div class="student-list">
        <div class="student-list__left">
          <div class="seat">1号位</div>
          <img alt class="student-avatar" src />
          <div class="student-info">
            <div class="student-info__name">黄跳跳</div>
            <div class="student-info__tele">15797679586</div>
          </div>
        </div>
        <div class="time">18:21:10</div>
      </div>
    </div>
  </div>
</template>

<script>
import "./style.stylus";
import param from "@utils/url-param";

export default {
  data() {
    return {
      studentList: []
    };
  },
  methods: {},
  created() {}
};
</script>