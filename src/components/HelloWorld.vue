<template>
  <div class="hello">
    <img src="img/icons/Xenon-Logo.png" />
  </div>
  <div class="form">
    <div class="form-body">
      <input type="password" name="password" v-model="password"
        oninput="this.value = this.value.replace(/[^0-9.]/g, '').replace(/(\..*?)\..*/g, '$1').replace(/^0[^.]/, '0');"
        placeholder="* * * *" maxlength=4 />
    </div>
    <div class="form-action">
      <button @click="openTheDoor()">OPEN DOOR</button>
    </div>
    <div v-if="isPasswordWrong" class="form-message">
      <p>Şifre hatalı!</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      password: '',
      isPasswordWrong: false
    }
  },
  props: {
    msg: String
  },
  methods: {
    async openTheDoor () {
      console.log(this.password)
      if (this.password === '1234') {
        try {
          // const response = await fetch('https://57.128.171.246:8880/76439485765479374', {
          //   method: 'GET',
          //   headers: {
          //     'Content-Type': 'application/json'
          //   }
          // })
          // const result = await response.json()
          // console.log('Success:', result)
          const xhr = new XMLHttpRequest()
          xhr.open('GET', 'https://57.128.171.246:8880/76439485765479374')
          xhr.send()
        } catch (error) {
          console.error('Error:', error)
          // window.open('https://57.128.171.246:8880/76439485765479374')
          // window.location.href = 'https://57.128.171.246:8880/76439485765479374'
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
.form-message p {
  color: red;
}

input {
  /* margin-top: 80px; */
  display: flex;
  margin: 30px auto;
}

input[placeholder] {

  text-align-last: justify;

}

button {
  border-radius: 8px;
  display: flex;
  /* margin-top: 40px; */
  margin: 0px auto;
  border: 1px solid transparent;
  padding: 0.6em 1.2em;
  font-size: 1em;
  font-weight: 500;
  font-family: inherit;
  color: #ffffff;
  background-color: #000000;
  cursor: pointer;
  transition: border-color 0.25s;
}</style>
