<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <input type="password" name="password" v-model="password" oninput="this.value = this.value.replace(/[^0-9.]/g, '').replace(/(\..*?)\..*/g, '$1').replace(/^0[^.]/, '0');" placeholder="* * * *" maxlength=4 />  <br>
    <button :disabled="isButtonDisabled">OPEN DOOR</button>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      password: ''
    }
  },
  props: {
    msg: String
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
input{
  margin-top: 80px;
}
input[placeholder] {

text-align-last: justify;

}
button {
  border-radius: 8px;
  margin-top: 40px;
  border: 1px solid transparent;
  padding: 0.6em 1.2em;
  font-size: 1em;
  font-weight: 500;
  font-family: inherit;
  color: #ffffff;
  background-color: #000000;
  cursor: pointer;
  transition: border-color 0.25s;
}

</style>
