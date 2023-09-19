<template>
  <div class="hello">
    <img src="img/icons/Xenon-Logo.png"/>
  </div>
  <div class="form">
    <div class="form-body">
    <input type="password" name="password" v-model="password" oninput="this.value = this.value.replace(/[^0-9.]/g, '').replace(/(\..*?)\..*/g, '$1').replace(/^0[^.]/, '0');" placeholder="* * * *" maxlength=4 />
    </div>
    <div class="form-action">
      <button type="button"  :class="loadingClassName" @click="openTheDoor()">
    <span class="button__text">OPEN DOOR</span>
</button>
      <!-- <button @click="openTheDoor()" :disabled="isButtonDisabled">OPEN DOOR</button> -->
    </div>
    <div v-if="isPasswordWrong" class="form-message">
      <p>Şifre hatalı!</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'OfficeDoor',
  data () {
    return {
      password: '',
      isPasswordWrong: false,
      loadingClassName: 'button', //  button--loading
      loading: false
    }
  },
  props: {
    msg: String
  },
  methods: {
    async openTheDoor () {
      console.log(this.password)
      if (this.password === '1234') {
        this.loadingClassName = 'button--loading button'
        try {
          const response = await fetch('https://officeapi.dumanrd.com/76439485765479374', {
            method: 'GET',
            headers: {
              'Content-Type': 'application/json'
            }
          })
          const result = await response.json()
          console.log('Success:', result)
          this.$swal.fire({
            title: 'Door opened',
            icon: 'success',
            confirmButtonText: 'OK!'
          })
          this.loadingClassName = 'button'
        } catch (error) {
          console.error('Error:', error)
        }
      } else {
        this.isPasswordWrong = true
      }
    }
  },
  watch: {
    isPasswordWrong (val) {
      if (val) {
        setTimeout(() => {
          this.isPasswordWrong = false
        }, 5000)
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.form-message p{
  color: red;
}
input{
  /* margin-top: 80px; */
  display: flex;
  margin: 30px auto;
}
input[placeholder] {

text-align-last: justify;

}

.button {
  position: relative;
  padding: 8px 16px;
  background: #009579;
  border: none;
  outline: none;
  border-radius: 2px;
  cursor: pointer;
}

.button:active {
  background: #007a63;
}

.button__text {
  font: bold 20px "Quicksand", san-serif;
  color: #ffffff;
  transition: all 0.2s;
}

.button--loading .button__text {
  visibility: hidden;
  opacity: 0;
}

.button--loading::after {
  content: "";
  position: absolute;
  width: 16px;
  height: 16px;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;
  border: 4px solid transparent;
  border-top-color: #ffffff;
  border-radius: 50%;
  animation: button-loading-spinner 1s ease infinite;
}

@keyframes button-loading-spinner {
  from {
    transform: rotate(0turn);
  }

  to {
    transform: rotate(1turn);
  }
}
</style>
