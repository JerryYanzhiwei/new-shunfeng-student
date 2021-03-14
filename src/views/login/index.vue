<template>
  <div class="login_container"
    :style="{
      backgroundImage: `url(${bg})`
    }">
    <!-- 登录 -->
    <div v-if="isLogin && deviceType === 1" class="login_content" :style="{backgroundImage: `url(${denglu})`}">
      <p class="login_txt">登录</p>
      <div class="form_item">
        <el-input type="number" v-model="loginForm.phone" placeholder="手机号" size="mini" prefix-icon="el-icon-user"></el-input>
      </div>
      <div v-if="!loginType" class="form_item">
        <el-input type="password" v-model="loginForm.password" placeholder="密码" size="mini" prefix-icon="el-icon-lock"></el-input>
      </div>
      <div v-else class="form_item msg_input">
        <el-input v-model="loginForm.verificationCode" placeholder="验证码" prefix-icon="el-icon-mobile-phone" size="mini"></el-input>
        <span v-if="!showLoginCode" @click="sendLoginCode" class="send_msg">发送验证码</span>
        <span v-else class="send_msg">{{loginCode}}</span>
      </div>
      <div class="form_item2">
        <span @click="isLogin = false">注册</span>
        <span @click="changeLoginType">
          {{
            !loginType ? '验证码登录' : '密码登录'
          }}
        </span>
      </div>
      <div class="btn_contain">
        <el-button v-if="!loginType" @click="passwordLogin">登录</el-button>
        <el-button v-if="loginType" @click="codeLogin">登录</el-button>
      </div>
    </div>
    <!-- 注册 -->
    <div v-else class="registry_content">
      <p class="registry_txt">
        <span v-if="deviceType === 1" @click="isLogin = true">返回登录</span>
        注册
      </p>
      <div class="login_form_contain">
        <el-form
          :model="registryForm"
          ref="registryForm"
          :rules="rules"
          >
          <!-- 姓名 -->
          <el-form-item
            prop="name"
            label="姓名">
            <el-input
              v-model="registryForm.name"
              size="mini"
              >
            </el-input>
          </el-form-item>
          <!-- 性别 -->
          <el-form-item
            prop="gender"
            label="性别">
            <el-select
              size="mini"
              v-model="registryForm.gender"
              placeholder="">
              <el-option
                v-for="item in sexArr"
                :key="item.code"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </el-form-item>
          <!-- 手机号 -->
          <el-form-item
            prop="phone"
            label="手机号">
            <el-input
              type="number"
              v-model="registryForm.phone"
              size="mini"
              >
            </el-input>
          </el-form-item>
          <!-- 验证码 -->
          <el-form-item
            prop="verificationCode"
            label="验证码">
            <el-input
              v-model="registryForm.verificationCode"
              size="mini"
              >
            </el-input>
            <span v-if="!showCount" @click="getRegistryCode" class="get_code">获取验证码</span>
            <span v-if="showCount" class="get_code">{{count}}</span>
          </el-form-item>
          <!-- 密码 -->
          <el-form-item
            prop="password"
            label="密码">
            <el-input
              type="password"
              v-model="registryForm.password"
              size="mini"
              >
            </el-input>
          </el-form-item>
          <!-- 邮箱 -->
          <el-form-item
            prop="email"
            label="邮箱">
            <el-input
              v-model="registryForm.email"
              size="mini"
              >
            </el-input>
          </el-form-item>
          <!-- 学校 -->
          <el-form-item
            prop="school"
            label="学校">
            <el-input
              v-model="registryForm.school"
              size="mini"
              >
            </el-input>
          </el-form-item>
          <el-form-item
            label="学生证"
            >
            <span v-if="fileName">{{fileName}} <i @click="clickUploadBtn(2)" class="reload">重新上传</i></span>
            <el-button v-else size="mini" @click="clickUploadBtn(2)">上传</el-button>
            <input type="file" v-show="false" ref="file2" @change="fileChange">
          </el-form-item>
          <!-- 专业 -->
          <el-form-item
            prop="profession"
            label="专业">
            <el-input
              v-model="registryForm.profession"
              size="mini"
              >
            </el-input>
          </el-form-item>
          <!-- 年级 -->
          <el-form-item
            prop="grade"
            label="年级">
            <el-input
              v-model="registryForm.grade"
              size="mini"
              >
            </el-input>
          </el-form-item>
          <!-- 毕业时间 -->
          <el-form-item
            prop="educationalYear"
            label="毕业时间">
            <el-input
              v-model="registryForm.educationalYear"
              size="mini"
              placeholder="年/月，比如2021年6月"
              >
            </el-input>
          </el-form-item>
          <!-- 备注 -->
          <el-form-item
            label="备注">
            <el-input
              v-model="registryForm.described"
              size="mini"
              >
            </el-input>
          </el-form-item>
        </el-form>
      </div>
      <div class="btn_contain">
        <el-button @click="submitRegistry" size="mini">注册</el-button>
      </div>
    </div>
    <!-- 抽奖 -->
    <div v-if="drawVisibel" class="prize_contain">
      <p class="luck_draw_txt">点击抽奖</p>
      <div class="prize_items">
        <div @click="touchPrize" class="prize_detail" v-for="(item, index) in [1, 2, 3]" :key="index">
          <img class="prize_cover front" :src="prizeCover" alt="">
          <img class="prize_cover back" :src="prizeBack" alt="">
          <img class="hidden" :src="prizeCover" alt="">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapActions } from 'vuex'
import bg from '@/assets/login_bg.png'
import denglu from '@/assets/denglu.png'
export default {
  data () {
    return {
      logo: require('../../assets/images/title.jpg'),
      prizeCover: require('../../assets/images/cover1.png'),
      prizeBack: require('../../assets/images/cover2.png'),
      bg,
      denglu,
      showCount: false,
      count: 0,
      timer: null,
      loginTimer: null,
      showLoginCode: false,
      loginCode: 0,
      deviceType: null, // 1: PC 2: phone
      // 是否可抽奖
      canDraw: true,
      // 抽奖弹框
      drawVisibel: false,
      // true: 登录  false: 注册
      isLogin: true,
      // false: 密码登录  true: 验证码登录
      loginType: false,
      studentCard: null,
      fileName: '',
      loginForm: {
        phone: '',
        password: '',
        verificationCode: ''
      },
      registryForm: {
        name: '',
        phone: '',
        gender: '',
        verificationCode: '',
        password: '',
        email: '',
        school: '',
        profession: '',
        grade: '',
        described: '',
        educationalYear: ''
      },
      postForm: null,
      sexArr: [
        {
          label: '男',
          value: 1
        },
        {
          label: '女',
          value: 2
        }
      ]
    }
  },
  computed: {
    rules () {
      return {
        name: [
          { required: true, message: '', trigger: 'blur' }
        ],
        phone: [
          { required: true, message: '', trigger: 'blur' }
        ],
        gender: [
          { required: true, message: '', trigger: 'blur' }
        ],
        verificationCode: [
          { required: true, message: '', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '', trigger: 'blur' }
        ],
        email: [
          { required: true, message: '', trigger: 'blur' }
        ],
        school: [
          { required: true, message: '', trigger: 'blur' }
        ],
        profession: [
          { required: true, message: '', trigger: 'blur' }
        ],
        grade: [
          { required: true, message: '', trigger: 'blur' }
        ],
        educationalYear: [
          { required: true, message: '', trigger: 'blur' }
        ]
      }
    }
  },
  mounted () {
    this.deviceType = document.body.clientWidth > 500 ? 1 : 2
  },
  methods: {
    ...mapActions(['login', 'POST_GET_CODE', 'POST_REGISTRY', 'LOGIN_CODE', 'CODE_LOGIN']),
    clickUploadBtn (type) {
      console.log('上传类型', type)
      const ref = `file${type}`
      const dom = this.$refs[ref]
      dom && dom.click()
    },
    async fileChange (e) {
      const file = e.target.files[0]
      console.log('file:', file)
      const arr = file.name.split('.')
      const type = arr[arr.length - 1]
      const acceptTypes = ['png', 'jpg', 'jpeg']
      if (type && acceptTypes.indexOf(type) > -1) {
        this.fileName = file.name
        this.studentCard = file
        const dom = this.$refs.file2
        dom.value = ''
      } else {
        this.$message.error('只允许上传jpg,png,jpeg类型文件')
      }
    },
    // 获取登录验证码
    async sendLoginCode () {
      const res = await this.LOGIN_CODE({
        phone: this.loginForm.phone
      })
      if (res.result === '0' && res.data) {
        this.$message.success('发送成功')
        this.loginCode = 60
        this.showLoginCode = true
        this.loginTimer = setInterval(() => {
          this.loginCode--
          if (this.loginCode === 0) {
            clearInterval(this.loginTimer)
            this.loginTimer = null
            this.showLoginCode = false
            this.loginCode = 60
          }
        }, 1000)
      }
    },
    // 抽奖
    touchPrize ($el) {
      if (this.canDraw) {
        $el.target.style.transform = 'rotateY(-180deg)'
        $el.target.nextSibling.style.transform = 'rotateY(-360deg)'
        this.canDraw = false
        setTimeout(() => {
          location.reload()
        }, 3000)
      }
    },
    // 密码登录
    async passwordLogin () {
      if (!this.validatePhone(this.loginForm.phone)) {
        this.$message.error('请输入正确的手机号码')
        return
      }
      const res = await this.login(this.loginForm)
      if (res.result === '0') {
        this.$alert('选题阶段将于2021年3月19号正式开启，请耐心等待', '提示', {
          confirmButtonText: '确定',
          callback: () => {
            return false
          }
        })
        // sessionStorage.setItem('userInfo', JSON.stringify(res.data))
        // this.$router.push('/moduleSelect')
      }
    },
    async codeLogin () {
      const res = await this.CODE_LOGIN(this.loginForm)
      if (res.result === '0') {
        this.$alert('选题阶段将于2021年3月19号正式开启，请耐心等待', '提示', {
          confirmButtonText: '确定',
          callback: () => {
            return false
          }
        })
        // sessionStorage.setItem('userInfo', JSON.stringify(res.data))
        // this.$router.push('/moduleSelect')
      }
    },
    // 切换登录方式
    changeLoginType () {
      this.loginType = !this.loginType
      const dom = this.$refs.file2
      dom.value = ''
      this.studentCard = null
      this.fileName = ''
    },
    // 获取注册验证码
    async getRegistryCode () {
      const res = await this.POST_GET_CODE({
        phone: this.registryForm.phone
      })
      if (res.result === '0' && res.data) {
        this.$message.success('发送成功')
        this.count = 60
        this.showCount = true
        this.timer = setInterval(() => {
          this.count--
          console.log(this.count, 111)
          if (this.count === 0) {
            clearInterval(this.timer)
            this.timer = null
            this.showCount = false
            this.count = 60
          }
        }, 1000)
      }
    },
    // 注册账号
    submitRegistry () {
      // const width = document.body.clientWidth
      this.$refs.registryForm.validate(async (valid) => {
        if (valid) {
          if (!this.validatePhone(this.registryForm.phone)) {
            this.$message.error('请输入正确的手机号码')
            return
          }
          if (!this.validateEmail(this.registryForm.email)) {
            this.$message.error('请输入正确的邮箱')
            return
          }
          if (!this.studentCard) {
            this.$message.error('请上传学生证')
            return
          }
          this.postForm = new FormData()
          this.postForm.append('studentCardFile', this.studentCard)
          for (var key in this.registryForm) {
            this.postForm.append(key, this.registryForm[key])
          }
          const res = await this.POST_REGISTRY(this.postForm)
          if (res.result === '0' && res.data) {
            this.$message.success('注册成功')
            setTimeout(() => {
              location.reload()
            }, 300)
            // if (width < 500) {
            //   this.$alert('注册成功, 请在电脑中继续操作', 'TIP', {
            //     confirmButtonText: '确定',
            //     callback: action => {
            //       this.drawVisibel = true
            //     }
            //   })
            // } else {
            //   this.drawVisibel = true
            // }
          }
          console.log('通过', res)
        } else {
          console.log(21321)
        }
      })
    }
  }
}
</script>

<style lang="scss" scoped>
  .login_container{
    display: flex;
    align-items: center;
    justify-content: center;
    background-repeat: no-repeat;
    background-size: cover;

    width: 100%;
    height: 100vh;
    .reload {
      font-size: 12px;
      color: #409EFF;
      cursor: pointer;
    }
    .login_content {
      padding: 27px 60px 32px 60px;
      background-repeat: no-repeat;
      background-size: cover;
      margin-top: 18%;
      color: #fff;

      border-radius: 8px;
      .login_top {
        margin-bottom: 30px;
        text-align: center;
        img {
          width: 100%;
          height: auto;
          border-radius: 8px;
        }
      }
      .login_txt {
        text-align: center;
        font-size: 18px;
      }
      .form_item2 {
        display: flex;
        align-items: center;
        justify-content: space-between;
        width: 230px;
        padding: 10px 0;
        margin: 0 auto;
        cursor: pointer;
        span {
          color: #00b8ff;
          font-size: 12px;
        }
      }
      .form_item {
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative;

        margin-top: 10px;
        .label_title {
          width: 80px;
        }
        .send_msg {
          position: absolute;
          bottom: 5px;
          right: 40px;

          font-size: 12px;
          color: #00b8ff;
          cursor: pointer;
        }
      }
      .btn_contain {
        display: flex;
        justify-content: center;
      }
    }
    .registry_content {
      width: 400px;
      padding: 20px;

      background-color: #0c3c85;
      border-radius: 8px;
      margin-left: 40%;
      .btn_contain {
        display: flex;
        justify-content: center;
      }
      .registry_txt {
        position: relative;

        margin-bottom: 10px;

        text-align: center;
        font-size: 18px;
        color: #fff;
        span {
          position: absolute;
          left: 0;
          top: 5px;

          font-size: 12px;
          cursor: pointer;
        }
      }
    }
    .prize_contain {
      position: fixed;
      top: 0;
      left: 0;
      bottom: 0;
      right: 0;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;

      background-color: rgba(0, 0, 0, .8);
      .luck_draw_txt {
        margin-bottom: 10%;

        color: #fff;
        font-size: 18px;
        font-weight: bold;
      }
      .prize_items {
        display: flex;
        justify-content: space-around;
        flex-wrap: wrap;

        width: 100%;
        .prize_detail {
          position: relative;
          width: 20%;
          perspective: 500;
          img.prize_cover {
            position: absolute;
            top: 0;
            left: 0;
            transition: all 1s;
            backface-visibility: hidden;
            perspective: 1000;
            border-radius: 10px;
            cursor: pointer;
            width: 100%;
            height: auto;
          }
          .hidden {
            width: 100%;
            height: auto;
            margin-bottom: 10%;
            visibility: hidden;
          }
          .back {
            transform: rotateY(-180deg);
          }
          // &:hover .front {
          //   transform: rotateY(-180deg);
          // }
          // &:hover .back {
          //   transform: rotateY(-360deg);
          // }
        }
      }
    }
  }
  @media screen and (max-width: 500px) {
    .el-message-box {
      width: 100%;
    }
    .login_container .login_content {
      box-shadow: unset;
      width: 100%;
    }
    .login_container .registry_content {
      width: 100%;
      height: 100%;
      box-shadow: unset;
      border-radius: 0;
      margin: unset;
    }
    .registry_content .el-input--mini {
      width: 70%;
    }
    .login_form_contain .el-form-item .get_code {
      right: 6%;
    }
    .login_container .prize_contain .prize_items .prize_detail {
      width: 40%;
    }
  }
</style>

<style lang="scss">
  .el-input {
    width: 230px;
  }
  .msg_input {
    .el-input--mini .el-input__inner {
      padding-right: 80px;
    }
  }
  .login_container {
    .form_item {
      .el-input {
        &__inner {
          background-color: #14438f;
          border-color: #0b83c1;
          color: #fff;
        }
        &__prefix {
          color: #fff;
        }
        ::placeholder {
          color: #fff;
        }
      }
    }
    .login_content {
      .btn_contain {
        .el-button {
          padding: 6px 100px;
          background: linear-gradient(270deg, #009af2 0%, #0183e9 100%);
          color: #fff;
          border-color: #009af2;
        }
      }
    }
  }

  .registry_content {
    .btn_contain {
      .el-button--mini {
        padding: 7px 20px;
        border-color: #009af2;
        background-color: #009af2;
        color: #fff;
      }
    }
  }

  .login_form_contain {
    .el-input {
      &__inner {
        background-color: #14438f;
        border-color: #0b83c1;
        color: #fff;
      }
      &__prefix {
        color: #fff;
      }
      ::placeholder {
        color: #fff;
      }
    }
    .el-form-item {
      &__label {
        width: 80px;
        color: #fff;
      }
      &__content {
        .el-button--mini {
          padding: 4px 15px;
          border-color: #009af2;
          background-color: #009af2;
          color: #fff;
        }
      }
      position: relative;

      margin-bottom: 5px;
      .get_code {
        position: absolute;
        right: 60px;
        top: 2px;
        width: 60px;
        text-align: center;

        font-size: 12px;
        color: #409EFF;
        cursor: pointer;
      }
    }
  }
  .el-form-item__error {
    display: none;
  }
</style>
