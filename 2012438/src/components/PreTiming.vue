<template>
    <div v-show="IsPreTimingVisible">
        <form id="pretiming">
            <ul>
                <li>
                    <label>时</label>
                    <input id="hour" placeholder="0" v-model="inputhour">
                </li>
                <li>
                    <label>分</label>
                    <input id="minute" placeholder="0" v-model="inputminute">
                </li>
                <li>
                    <label>秒</label>
                    <input id="second" placeholder="0" v-model="inputsecond" >
                </li>
                <li>
                    <button id="countup" class="default-button" type="button" @click="updatecountupmyselts">开始正计时</button>
                </li>
                <li>
                    <button id="countdown" class="default-button" type="button" @click="updatecountdownmyselts">开始倒计时</button>
                </li>
            </ul>
    </form>
    </div>
</template>

<style>
#pretiming {
  height: 70px;
  width: 1140px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

#pretiming>ul {
  display: flex;
  list-style: none;
}

#pretiming>ul>li {
  margin-right: 20px;
  position: relative;
}

#pretiming label {
  display: block;
  position: absolute;
  right: 9px;
  height: 40px;
  line-height: 40px;
  color: #222222;
  font-family: 'PingFangSC';
}

#pretiming input,
#pretiming button {
  box-sizing: border-box;
  font-family: 'PingFangSC';
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

#pretiming input {
  width: 70px;
  height: 40px;
  border-radius: 5px;
  border: none;
  padding: 7px auto;
  padding-left: 11px;
}

#pretiming button {
  height: 40px;
  width: 98px;
  border-radius: 5px;
  border: none;
  padding: 9px;
  font-size: 16px;
  color: #fff;
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
      inputhour: null,
      inputminute: null,
      inputsecond: null
    }
  },
  mounted () {
    document.addEventListener('keyup', function (keything) {
      if (keything.key === 'Enter') {
        document.getElementById('countup').click()
      }
    })
  },
  props: {
    IsPreTimingVisible: {
      type: Boolean,
      default: true
    },
    IsPreTimingClearInput: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    updatecountupmyselts () {
      this.$emit('pre-timing-click', this.inputhour, this.inputminute, this.inputsecond, true, true)
      this.inputhour = null
      this.inputminute = null
      this.inputsecond = null
    },
    updatecountdownmyselts () {
      this.$emit('pre-timing-click', this.inputhour, this.inputminute, this.inputsecond, false, false)
      this.inputhour = null
      this.inputminute = null
      this.inputsecond = null
    }
  }
}

</script>
