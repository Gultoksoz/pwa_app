<template>
  <!--Safaride otomatik zoomu engellemek için yazıldı-->
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=0"/>

  <div class="hello">
    <!--Sayfanın başındaki logo-->
    <img src="img/icons/Xenon-Logo.png"/>
  </div>
  <div class="form">
    <div class="form-body">
      <!--Şifrenin istendiği alan-->
      <input type="password" inputmode="numeric" name="password" v-model="password" oninput="this.value = this.value.replace(/[^0-9.]/g, '').replace(/(\..*?)\..*/g, '$1').replace(/^0[^.]/, '0');" placeholder="* * * *" maxlength=4 />
      <!--Girilen şifrede hata varsa yazdırılır-->
      <p v-for="error of v$.password.$errors" :key="error.$uid">
        <small>{{ error.$message }}</small>
      </p>
    </div>
    <div class="form-action">
      <!--Kapıyı aç butonu. Tıklandığında girilen şifreyi kontrol eden fonk çağırılır.-->
      <button type="button"  :class="loadingClassName" @click="validateForm()">
    <span class="button__text">OPEN DOOR</span>
</button>
  
    </div>
    <!--Girilen şifre doğru değilse hatalıdır yazar.-->
    <div v-if="isPasswordWrong" class="form-message">
      <p>Şifre hatalı!</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

import { useVuelidate } from '@vuelidate/core'
import { required,numeric,minLength,maxLength, helpers } from '@vuelidate/validators'

export default {
  name: 'OfficeDoor',

  setup: () => ({ v$: useVuelidate() }),
  data () {
    return {
      password: '',//şifreyi tuttuğumuz değişken
      isPasswordWrong: false,// şifrenin yanlış veya doğru olma durumunu tuttuğumuz değişken
      loadingClassName: 'button', //  button--loading. Butona basıldığında doğru şifre girildiyse kapı açıldı bildirimi gelene kadar kullanıcıya gösterilen butonun classı.
      //loading: false
    }
  },
  //validasyon işlemleri için kullandığımız özellikler ve hata mesajları.
  validations () {
    return {
      password: {
        required: helpers.withMessage("Bu alan boş bırakılamaz.",required),
        numeric:helpers.withMessage("Şifre sayılardan oluşmalı.",numeric),
        $autoDirty: true ,
        minLength:helpers.withMessage("Şifre 4 karakterden oluşmalı.",minLength(4)) ,
        maxLength:helpers.withMessage("Şifre 4 karakterden oluşmalı.",maxLength(4)) 
      }
    }
  },
  props: {
    msg: String
  },
  methods: {
    async validateForm(){
      
      const isFormCorrect = await this.v$.$validate() //validate() fonksiyonu, yazdığımız kuralları kontrol edip uygun veya hatalı durumunu gönderir.
      //Eğer şifre uygunsa openTheDoor() fonk çağırılır.
      if(isFormCorrect)
      {
        this.openTheDoor()
      }
    },
    async openTheDoor () {
      console.log(this.password)
      if (this.password === '1234') { //Girilen şifre doğru mu kontrol edilir.
        this.loadingClassName = 'button--loading button' //Şifrenin doğru olduğu durumda istek atılırken opendoor butonun classı değiştirilir.
        try {
          const headers = { "Content-Type": "application/json" } // isteğimizin headrını bildiriyoruz
          const response = await axios.get('https://officeapi.dumanrd.com/76439485765479374', { headers }) // İstek atılacak urlye axios ile get isteği atılır.
          const result = await response.status //gelen cevabın durumunu result değişkenine atıyoruz.
          console.log('Success:', result)
          this.$swal.fire({  // Door opened yazan bir pop-up oluşturuyoruz
            title: 'Door opened',
            icon: 'success',
            confirmButtonText: 'OK!',
            timer:3000

          })
          this.loadingClassName = 'button' // Pop-up gösterildikten sonra buton classı değiştiriliyor
        } catch (error) { //Bu işlemler yapılırken bir hata gerçekleşirse bu blok çalışır
          console.error('Error:', error) //hatayı konsola yazdırıyoruz
        }
      } else { //Şifrenin yanlış olma durumunda bu değişkeni true yapıyoruz.
        this.isPasswordWrong = true
      }
    }
  },
  watch: { //Şifrenin yanlış olma durumunda şifre hatalıdır yazısının 5sn ekranda durması için
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

.form-message p{ /*Şifre hatalıdır yazısı */
  color: red;
  font-family: Arial, Helvetica, sans-serif;
}

.form-body small{ /*Şifrenin uygunluk kuralları */
  color: red;
  display: inline-block;
  width: revert;
  height: 52px;
  font-family: Arial, Helvetica, sans-serif;
}
.form-body{ /*Şifre uyarıları ve input */
  width: revert;
  height: 52px;
  margin-bottom: 15px;
  margin-top: 50px;
}

input{
  display: flex;
  margin: 30px auto;
}
input[placeholder] { /* uyarılar çıktığında inputun durumu */
justify-items: center;
margin: auto;
}

.button {
  position: relative;
  padding: 15px 20px;
  background-color: #4cb151;
  border: none;
  outline: none;
  border-radius: 10px;
  cursor: pointer;
  font-family: Arial, Helvetica, sans-serif;
}

.button:active {
  background-color: #4cb151;
}

.button__text {
  font: 20px "Arial", san-serif;
  color: #ffffff;
  transition: all 0.2s;
}

.button--loading .button__text { /*Butona basıldığında success durumunda bildirim çıkana kadar butonun yazı görünümü  */
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

@keyframes button-loading-spinner { /*yükleniyor butonu animasyonu */
  from {
    transform: rotate(0turn);
  }

  to {
    transform: rotate(1turn);
  }
}
</style>