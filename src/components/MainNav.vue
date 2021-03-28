<template>
  <div class="navBg">
    <div class="navBox">
      <a href="index.html" class="logo"
        ><img src="../assets/images/logo2.png" alt=""
      /></a>
      <ul class="nav">
        <li>
          <a
            href="https://special.zhaopin.com/campus/2021/shz/sfsy031328/index.html"
            class="road-a"
            >首页</a
          >
        </li>
        <li>
          <a
            href="https://special.zhaopin.com/campus/2021/shz/sfsy031328/rule.html"
            class="road-a"
            >赛事介绍</a
          >
        </li>
        <li>
          <a
            href="https://special.zhaopin.com/campus/2021/shz/sfsy031328/job.html"
            class="road-a"
            >赛事动态</a
          >
        </li>
        <li>
          <a
            href="https://special.zhaopin.com/campus/2021/shz/sfsy031328/ssql.html"
            class="road-a red"
            >赛事群聊
          </a>
        </li>
        <li class="on">
          <a href="https://sf.shangzhaopin.com/login" class="road-a red"
            >我要参赛</a
          >
        </li>
        <li>
          <a
            href="https://special.zhaopin.com/campus/2021/shz/sfsy031328/contact.html"
            class="road-a"
            >物流风采</a
          >
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  computed: {},
  data () {
    return {}
  },
  methods: {}
}
</script>

<style lang="scss" scoped>
/*nav*/
.navBg {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 20;
  height: 80px;
  width: 100%;
  background-color: #273368;
}
.navBox {
  width: 1000px;
  height: 100%;
  margin: 0 auto;
}
.navBox .logo {
  display: block;
  float: left;
  margin-left: 10px;
  margin-top: 6px;
}
.navBox .nav {
  float: right;
  height: 100%;
  line-height: 90px;
}
.navBox .nav li {
  float: left;
  margin-left: 10px;
  height: 100%;
}
.navBox .nav li.on,
.navBox .nav li:hover {
  background-color: #95d4df;
}
.navBox .nav li a {
  color: #ffffff;
  display: block;
  font-size: 20px;
  text-align: center;
  padding: 0 0px;
  line-height: 40px;
  width: 120px;
  box-sizing: border-box;
  height: 40px;
  font-weight: bold;
  margin-top: 20px;
}
.navBox .nav li.on > a {
  color: #273368;
}
</style>
