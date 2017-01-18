<template>
  <div class="password">
      <div class="password-group">
        <input
          type="password"
          :id="id"
          :class="[defaultClass]"
          :placeholder="placeholder"
          :name="name"
          :required="required"
          v-model="password"
        >
        <div class="password-badge"
          :class="[isSecure ? successClass: '', !isSecure && isActive ? errorClass : '']"
          v-cloak
          v-if="badge"
          v-text="passwordCount"
        >
        </div>
      </div>
      <div v-bind:class="[strengthMeterClass]">
        <div v-bind:class="[strengthMeterFillClass]"
          :data-score="passwordStrength">
        </div>
      </div>
  </div>
</template>

<script>
import zxcvbn from 'zxcvbn'

export default {
  name: 'hello',
  props: {
    id: {
      type: String,
      default: 'password'
    },
    defaultClass: {
      type: String,
      default: 'form-control'
    },
    placeholder: {
      type: String,
      default: 'Type your password'
    },
    name: {
      type: String,
      default: 'password'
    },
    required: {
      type: Boolean,
      default: true
    },
    secureLength: {
      type: Number,
      default: 7
    },
    badge: {
      type: Boolean,
      default: true
    },
    errorClass: {
      type: String,
      default: 'error'
    },
    successClass: {
      type: String,
      default: 'success'
    },
    strengthMeterClass: {
      type: String,
      default: 'password-strength-meter'
    },
    strengthMeterFillClass: {
      type: String,
      default: 'password-strength-meter-fill'
    }

  },
  data () {
    return {
      password: null
    }
  },
  computed: {
    passwordStrength () {
      return this.password ? zxcvbn(this.password).score : null
    },
    isSecure () {
      return this.password ? this.password.length >= this.secureLength : null
    },
    isActive () {
      return this.password && this.password.length > 0
    },
    passwordCount () {
      return this.password && (this.password.length > this.secureLength ? `${this.secureLength}+` : this.password.length)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  [v-cloak] {
    display: none;
  },
  .password {
    max-width: 400px;
    margin: 0 auto;
  }
  .password-groupd {
    position: relative;
  }
  .password-strength-meter {
    position: relative;
    height: 3px;
    background: #DDD;
    margin: 10px auto 20px;
    border-radius: 3px;
  }
  .password-strength-meter:before, .password-strength-meter:after {
    content:  '';
    height: inherit;
    background: transparent;
    display: block;
    border-color: #FFF;
    border-style: solid;
    border-width: 0 5px 0 5px;
    position: absolute;
    width: 80px;
    z-index: 10;
  }
  .password-strength-meter:before{
    left: 70px;
  }
  .password-strength-meter:after {
    right: 70px;
  }
  .password-strength-meter-fill {
    background: transparent;
    height: inherit;
    position: absolute;
    width: 0;
    border-radius: inherit;
    transition: width 0.5s ease-in-out, background 0.25s;
  }
  .password-strength-meter-fill[data-score='0'] {
    background: darkred;
    width: 20%;
  }
    .password-strength-meter-fill[data-score='1'] {
      background: orangered;
      width: 40%;
    }
    .password-strength-meter-fill[data-score='2'] {
      background: orange;
      width: 60%;
    }
    .password-strength-meter-fill[data-score='3'] {
      background: yellowgreen;
      width: 80%;
    }
    .password-strength-meter-fill[data-score='4'] {
      background: green;
      width: 100%;
    }
    .password {
      background-color: #f1f1f1;
      border: 1px solid #f1f1f1;
      border-radius: 2px;
      box-sizing: border-box;
      font-size: 14px;
      padding: 13px;
      width: 100%;
    }
    .password-badge {
      float: right;
      position: relative;
      bottom: 33px;
      right: 10px;
      color: white;
      border-radius: 6px;
      padding: 3px;
      width: 30px;
      height: 15px;
      line-height: 1;
    }
    .error {
      background: red;
    }
    .success {
      background: #1bbf1b;
    }
</style>
