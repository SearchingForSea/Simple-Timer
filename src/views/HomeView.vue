<template>
  <div class="home">
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <header>
      <pre-timing-vue id="pretimingvue" @pre-timing-click="changeCountTimingIsDisplay"
        :is-pre-timing-visible="PreTimingIsDisplay"></pre-timing-vue>
      <now-timing ref="timing" id="nowtiming" @pre-timing-click="changeCountTimingIsDisplay"
        :is-now-timing-visible="NowTimingIsDisplay" :is-now-timing-resume-visible="changeNowTiming" @clear-time="clearTime"
        @pause-time="pauseTime" @restart-time="restartTime"
        :is-now-timing-position="positionCount" :time-count-down="timerofnowtiming"
        :is-now-timing-stop="isstop"></now-timing>
    </header>
    <p id="time" class="time">
      {{ timer }}
    </p>
  </div>
</template>

<style scoped>
* {
  padding: 0;
  margin: 0;
}

body {
  background: #F2F4F7;
  font-size: 16px;
}

header {
  background-color: #97a5bc;
  width: 100%;
  height: 70px;
  box-sizing: border-box;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.time {
  width: 100%;
  height: 200px;
  margin-top: 115px;
  text-align: center;
  box-sizing: border-box;
  font-size: 200px;
  color: #333;
  font-family: 'PT Mono';
  font-size: 200px;
}
</style>

<script>
// @ is an alias to /src
import PreTimingVue from '@/components/PreTiming'
import nowTiming from '@/components/NowTiming'
export default {
  components: {
    PreTimingVue,
    nowTiming
  },
  data () {
    // eslint-disable-next-line no-labels, no-unused-expressions
    return {
      PreTimingIsDisplay: true,
      NowTimingIsDisplay: false,
      inputhour: 0,
      inputminute: 0,
      inputsecond: 0,
      timer: '00:00:00',
      timerofnowtiming: '00:00:00',
      totalTime: 0,
      nowcount: 0, // 记录现在的时间,
      nowtime: 0,
      startTime: true,
      pauseIsStop: true,
      iscountup: true,
      timecon: null,
      changeNowTiming: false, // 是否改变nowtiming界面
      positionCount: true, // 正计时标记
      isstop: false
    }
  },
  mounted () {
    console.log(this.$refs.timing)
  },
  methods: {
    changeCountTimingIsDisplay (datahour, dataminute, datasecond, iscountUp, ispositiondown) {
      // eslint-disable-next-line no-undef, no-unused-expressions, no-sequences
      if (!ispositiondown) {
        this.positionCount = false
        this.unPositionCount = true
      }

      this.PreTimingIsDisplay = false
      this.NowTimingIsDisplay = true
      this.inputhour = datahour
      this.inputminute = dataminute
      this.inputsecond = datasecond
      this.iscountup = iscountUp
      if (this.inputhour === null) {
        this.inputhour = 0
      }
      if (this.inputminute === null) {
        this.inputminute = 0
      }
      if (this.inputsecond === null) {
        this.inputsecond = 0
      }
      if (parseInt(this.inputminute) >= 60) {
        this.inputminute = 60
        console.log(this.inputminute)
      }
      if (parseInt(this.inputhour) >= 60) {
        this.inputhour = 60
      }
      if (parseInt(this.inputsecond) >= 60) {
        this.inputsecond = 60
      }
      if (parseInt(this.inputhour) < 0) {
        this.inputhour = 0
      }
      if (parseInt(this.inputminute) < 0) {
        this.inputminute = 0
      }
      if (parseInt(this.inputsecond) < 0) {
        this.inputsecond = 0
      }
      this.totalTime = Math.round(parseInt(this.inputhour)) * 3600 + Math.round(parseInt(this.inputminute)) * 60 + Math.round(parseInt(this.inputsecond))

      this.timerofnowtiming = ''
      this.timerofnowtiming += parseInt(this.inputhour) <= 9 ? ('0' + this.inputhour + '') : (this.inputhour + '')
      this.timerofnowtiming += ':'
      this.timerofnowtiming += parseInt(this.inputminute) <= 9 ? ('0' + this.inputminute + '') : (this.inputminute + '')
      this.timerofnowtiming += ':'
      this.timerofnowtiming += parseInt(this.inputsecond) <= 9 ? ('0' + this.inputsecond + '') : (this.inputsecond + '')

      if (!this.iscountup) {
        this.nowtime = this.totalTime * 10000 - 10000
        const temTime = this.nowtime / 10000
        if (temTime % 1 === 0) {
          if (temTime >= 0) {
            this.timer = ''
            if (temTime / 3600 <= 9) {
              this.timer += '0' + parseInt(temTime / 3600) + ':'
            } else {
              this.timer += parseInt(temTime / 3600) + ':'
            }
            if ((temTime % 3600) / 60 <= 9) {
              this.timer += '0' + parseInt((temTime % 3600) / 60) + ':'
            } else {
              this.timer += parseInt((temTime % 3600) / 60) + ':'
            }
            if (temTime % 60 <= 9) {
              this.timer += '0' + parseInt(temTime % 60)
            } else {
              this.timer += parseInt(temTime % 60)
            }
          }
        }
      }
      // eslint-disable-next-line no-undef
      this.timecon = setInterval(this.nowcounttimer, 100)
    },
    clearTime () {
      // eslint-disable-next-line no-unused-expressions, no-sequences
      this.PreTimingIsDisplay = true
      this.NowTimingIsDisplay = false
      this.nowtime = 0
      this.startTime = true
      this.pauseIsStop = true
      clearInterval(this.timecon)
      this.timer = '00:00:00'
      this.changeNowTiming = false
      this.isstop = false
    },
    pauseTime () {
      if (this.pauseIsStop) {
        this.changeNowTiming = true
        this.NowTimingIsDisplay = false
        // const time = document.getElementById('describecountDown')
        // time.innerHTML = "暂停倒计时&nbsp";
        // pauseBtn.innerHTML = "恢复计时器";
        this.startTime = false
        this.pauseIsStop = false
        // this.pauseBtn.style.display = 'none'
      } else {
        // const time = document.getElementById("describecountDown");
        // time.innerHTML = "正在倒计时&nbsp";
        // pauseBtn.innerHTML = "暂时计时器";
        this.changeNowTiming = false
        this.NowTimingIsDisplay = true
        this.startTime = true
        this.pauseIsStop = true
        // this.pauseBtn.style.display = 'flex'
      }
    },
    restartTime () {
      if (this.iscountup) {
        this.nowtime = 0
        this.startTime = true
        this.pauseIsStop = true
        this.timer = '00:00:00'
        clearInterval(this.timecon)
        this.timecon = setInterval(this.nowcounttimer, 100)
      }
      if (!this.iscountup) {
        this.nowtime = this.totalTime * 10000 - 10000
        this.startTime = true
        this.pauseIsStop = true
        this.timer = ''
        const temTime = this.nowtime / 10000
        if (temTime / 3600 <= 9) {
          this.timer += '0' + parseInt(temTime / 3600) + ':'
        } else {
          this.timer += parseInt(temTime / 3600) + ':'
        }
        if ((temTime % 3600) / 60 <= 9) {
          this.timer += '0' + parseInt((temTime % 3600) / 60) + ':'
        } else {
          this.timer.innerHTML += parseInt((temTime % 3600) / 60) + ':'
        }
        if (temTime % 60 <= 9) {
          this.timer += '0' + parseInt(temTime % 60)
        } else {
          this.timer += parseInt(temTime % 60)
        }
        clearInterval(this.timecon)
        this.timecon = setInterval(this.nowcounttimer, 100)
      }
    },
    // eslint-disable-next-line vue/no-dupe-keys
    nowcounttimer () {
      if (this.startTime) {
        if (this.iscountup) {
          this.nowtime += 1000
          const temTime = this.nowtime / 10000
          if (temTime % 1 === 0) {
            if (temTime - this.totalTime <= 0) {
              this.timer = ''
              if (temTime / 3600 <= 9) {
                this.timer += '0' + parseInt(temTime / 3600) + ':'
              } else {
                this.timer += parseInt(temTime / 3600) + ':'
              }
              if ((temTime % 3600) / 60 <= 9) {
                this.timer += '0' + parseInt((temTime % 3600) / 60) + ':'
              } else {
                this.timer += parseInt((temTime % 3600) / 60) + ':'
              }
              if (temTime % 60 <= 9) {
                this.timer += '0' + parseInt(temTime % 60)
              } else {
                this.timer += parseInt(temTime % 60)
              }
              if (temTime - this.totalTime === 0) {
                const pretimingUIinput = document.getElementsByTagName('input')
                for (let index = 0; index < pretimingUIinput.length; index++) {
                  pretimingUIinput[index].innerHTML = ''
                }
                this.isstop = true
                // eslint-disable-next-line no-undef
                clearInterval(this.timecon)
              }
            }
          }
        } if (!this.iscountup) {
          this.nowtime -= 1000
          // const timer = document.getElementById("time");
          const temTime = this.nowtime / 10000
          if (temTime % 1 === 0) {
            if (temTime >= 0) {
              this.timer = ''
              if (temTime / 3600 <= 9) {
                this.timer += '0' + parseInt(temTime / 3600) + ':'
              } else {
                this.timer += parseInt(temTime / 3600) + ':'
              }
              if ((temTime % 3600) / 60 <= 9) {
                this.timer += '0' + parseInt((temTime % 3600) / 60) + ':'
              } else {
                this.timer += parseInt((temTime % 3600) / 60) + ':'
              }
              if (temTime % 60 <= 9) {
                this.timer += '0' + parseInt(temTime % 60)
              } else {
                this.timer += parseInt(temTime % 60)
              }
            }
            if (temTime === -1) {
              const pretimingUIinput = document.getElementsByTagName('input')
              for (let index = 0; index < pretimingUIinput.length; index++) {
                pretimingUIinput[index].innerHTML = ''
              }
              this.isstop = true
              // eslint-disable-next-line no-undef
              this.startTime = false
              clearInterval(this.timecon)
            }
          }
        }
      }
    }
  }
}
</script>
