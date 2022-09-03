<!-- eslint-disable no-undef -->
<template>
    <div>
        <form id="timing">
            <div id="hint" v-show="IsNowTimingVisible||IsNowTimingResumeVisible">
                <label id="positioncountDown" v-if="IsNowTimingPosition">正在正计时&nbsp;&nbsp;</label>
                <label id="unpositioncountDown" v-else>正在倒计时&nbsp;&nbsp;</label>
                <label id="timecountDown">{{TimeCountDown}}</label>
                <label id="ended" v-if="endedvision">&nbsp;&nbsp;&nbsp;&nbsp;已结束</label>
            </div>
            <button id="resume" type="button" @click="pauseTime" v-show="IsNowTimingResumeVisible">恢复计时器</button>
            <button id="pause" type="button" @click="pauseTime" v-show="IsNowTimingVisible">暂停计时器</button>
            <button id="clear" type="button" @click="$emit('clear-time')" v-show="IsNowTimingVisible||IsNowTimingResumeVisible">清空计时器</button>
            <button id="restart" type="button" @click="restartTime" v-show="IsNowTimingVisible||IsNowTimingResumeVisible">重新计时器</button>
        </form>
    </div>
</template>

<style>
* {
  padding: 0;
  margin: 0;
}
header #timing {
  height: 70px;
  width: 1180px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  padding-left: 40px;
}

#timing label {
  font-size: 16px;
  color: white;
  font-family: "PingFang SC";
}

#timing #pause {
  height: 40px;
  width: 98px;
  border-radius: 5px;
  border: none;
  padding: 9px;
  font-size: 16px;
  color: white;
  background-color: #2188E9;
  font-family: 'PingFangSC';
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-left: 20px;
}

#timing #resume {
  height: 40px;
  width: 98px;
  border-radius: 5px;
  border: none;
  padding: 9px;
  font-size: 16px;
  color: white;
  background-color: #2188E9;
  font-family: 'PingFangSC';
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-left: 20px;
}

#timing #clear {
  height: 40px;
  width: 98px;
  border-radius: 5px;
  border: none;
  padding: 9px;
  font-size: 16px;
  color: white;
  background-color: #DD2E1D;
  font-family: 'PingFangSC';
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  float: right;
  margin-left: 639px;
}

#timing #restart {
  height: 40px;
  width: 98px;
  border-radius: 5px;
  border: none;
  padding: 9px;
  font-size: 16px;
  color: white;
  background-color: #FFB020;
  font-family: 'PingFangSC';
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  float: right;
  margin-left: 20px;
}

button.default-button {
  background: #2188E9;
}
</style>

<script>

export default {
  data () {
    return {
      isdisplay: true,
      pauseIsStop: true,
      endedvision: false
    }
  },
  methods: {
    pauseTime () {
      this.$emit('pause-time')
      if (this.pauseIsStop) {
        if (this.IsNowTimingPosition) {
          const temcountDown = document.getElementById('positioncountDown')
          temcountDown.innerHTML = '暂停正计时&nbsp&nbsp'
        } else {
          const temcountDown = document.getElementById('unpositioncountDown')
          temcountDown.innerHTML = '暂停倒计时&nbsp&nbsp'
        }
        this.pauseIsStop = false
        document.getElementById('pause').style.display = 'none'
        // eslint-disable-next-line vue/no-mutating-props
        this.IsNowTimingResumeVisible = true
      } else {
        this.pauseIsStop = true
        // eslint-disable-next-line vue/no-mutating-props
        this.IsNowTimingResumeVisible = false
        document.getElementById('pause').style.display = 'flex'
        if (this.IsNowTimingPosition) {
          const time = document.getElementById('positioncountDown')
          time.innerHTML = '正在正计时&nbsp&nbsp'
        } else {
          const time = document.getElementById('unpositioncountDown')
          time.innerHTML = '正在倒计时&nbsp&nbsp'
        }
      }
    },
    restartTime () {
      this.$emit('restart-time')
      if (this.IsNowTimingPosition) {
        document.getElementById('positioncountDown').innerHTML = '正在正计时&nbsp;&nbsp;'
      } else {
        document.getElementById('unpositioncountDown').innerHTML = '正在倒计时&nbsp;&nbsp;'
      }
      this.endedvision = false
      // eslint-disable-next-line vue/no-mutating-props
      this.IsNowTimingResumeVisible = false
      document.getElementById('pause').style.display = 'flex'
    }
  },
  watch: {
    IsNowTimingStop (newVal, oldVal) {
      if (newVal) {
        let time
        if (this.IsNowTimingPosition) {
          time = document.getElementById('positioncountDown')
        } else {
          time = document.getElementById('unpositioncountDown')
        }
        time.innerHTML = '倒计时&nbsp&nbsp&nbsp;&nbsp;'
        this.endedvision = true
        const pauseBtn = document.getElementById('pause')
        pauseBtn.style.display = 'none'
      } else {
        let timeelse
        if (this.IsNowTimingPosition) {
          timeelse = document.getElementById('positioncountDown')
          timeelse.innerHTML = '正在正计时&nbsp;&nbsp;'
        } else {
          timeelse = document.getElementById('unpositioncountDown')
          timeelse.innerHTML = '正在倒计时&nbsp;&nbsp;'
        }
        this.endedvision = false
        const pauseBtn = document.getElementById('pause')
        pauseBtn.style.display = 'flex'
      }
    }
  },
  mounted () {
    document.addEventListener('keyup', function (keything) {
      if (keything.key === ' ') {
        document.getElementById('pause').click()
      }
    })
  },
  props: {
    IsNowTimingVisible: {
      type: Boolean,
      default: true
    },
    IsNowTimingResumeVisible: {
      type: Boolean
    },
    IsNowTimingPosition: {
      type: Boolean
    },
    TimeCountDown: {
      type: String
    },
    IsNowTimingStop: {
      type: Boolean,
      default: false
    }
  }
}
// const pauseBtn = document.getElementById('pause')

// // eslint-disable-next-line no-undef
// pauseBtn.addEventListener('click', pauseTime)
</script>
