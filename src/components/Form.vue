<template>
  <ValidationObserver v-slot="{ handleSubmit }">
    <form @submit.prevent="handleSubmit(onSubmit)">
      <img src="@/assets/tigloo.webp" alt="">
      <fieldset v-if="currentStep === 1">
        <legend>Whitelist Our IP's</legend>
        <h3>Please make sure you whitelist our IP's</h3>
        <p v-for="(ips) in ipAddresses" :key="ips">{{ ips }}</p>
      </fieldset>

      <fieldset v-else-if="currentStep === 2">
        <legend>Step 2 - Traffic Flow</legend>
        <ValidationProvider name="whitelabel" rules="required" v-slot="{ errors }" class="spacebet">
          <label for="whitelabel" class="inputLabel">Who Sends the traffic?</label> <span class="redStar">*</span>
           <input type="text" v-model="trafficDetails.whitelabel" id="whitelabel">
          <div>
            <span class="invalid-feedback">{{ errors[0] }}</span>
          </div>
        </ValidationProvider>

        <ValidationProvider name="platform" rules="required" v-slot="{ errors }">
          <label for="platform" class="inputLabel">Which platform are you using?</label> <span class="redStar">*</span>
          <input type="text" v-model="trafficDetails.platform" id="platform"/>
          <div>
            <span class="invalid-feedback">{{ errors[0] }}</span>
          </div>
        </ValidationProvider>

        <ValidationProvider name="brand" rules="required" v-slot="{ errors }">
          <label for="brand" class="inputLabel">Who receive the traffic?</label> <span class="redStar">*</span>
          <input type="text" v-model="trafficDetails.brand" id="brand"/>
          <div>
            <span class="invalid-feedback">{{ errors[0] }}</span>
          </div>
        </ValidationProvider>

        <ValidationProvider name="country" rules="required" v-slot="{ errors }">
          <label for="country" class="inputLabel">From which country to test?</label> <span class="redStar">*</span>
          <input type="text" v-model="trafficDetails.country" id="country">
          <div>
            <span class="invalid-feedback">{{ errors[0] }}</span>
          </div>
        </ValidationProvider>

        <ValidationProvider name="campaignName" rules="required" v-slot="{ errors }">
          <label for="campaign" class="inputLabel">How to call the campaign?</label> <span class="redStar">*</span>
          <input type="text" v-model="trafficDetails.campaignName" id="campaign"/>
          <div>
            <span class="invalid-feedback">{{ errors[0] }}</span>
          </div>
        </ValidationProvider>
      </fieldset>

      <fieldset v-else-if="currentStep === 3">
        <legend>Step 3 - Integration Params</legend>
        <ValidationProvider name="namedField" rules="required" v-slot="{ errors }" v-for="(param, key, index) in integrationParams" :key="key">
            <label class="inputLabel" :for="key">Please fill in {{ key }} param</label> <span class="redStar">*</span>
            <input v-model="integrationParams[key]" :id="key" name="namedField">
            <div>
              <span class="invalid-feedback">{{ errors[0] }}</span>
            </div>
        </ValidationProvider>
      </fieldset>
      <button @click="goToPrevious(0)" v-if="currentStep === 2 || currentStep === 3">Previous</button>
      <button type="submit">Next</button>
<!--      <button @click="fillParams">Fill</button>-->
      <h3>Struggle with the form? Contact our support</h3>
    </form>
  </ValidationObserver>
</template>

<script>
import { ValidationProvider, extend , ValidationObserver } from 'vee-validate';
import { required } from 'vee-validate/dist/rules';


extend('required', {
  ...required,
  message: 'You must fill in the field'
})

export default {
  name: "Form",
  components: {
    ValidationProvider,
    ValidationObserver
  },
  data() {
    return {
      currentStep: 1,
      ipAddresses: ["34.242.60.225", "52.18.206.144", "82.81.51.192", "213.57.116.202", "82.81.48.111", "62.90.205.233"],
      trafficDetails: {
        brand: '',
        platform: '',
        whitelabel: '',
        country: '',
        campaignName: ''
      },
      integrationParams: {},

      jsonStatham: {
        "Trackbox": {
          "Username": "",
          "Password": "",
          "AffiliateID": "",
          "GI": "",
          "CI": "",
        },
        "Osher": {
          "Unique": "",
          "Unique2": "",
        }
      },
    }
  },
    methods: {
      fillParams() {
        this.trafficDetails.whitelabel = "Kalite"
        this.trafficDetails.platform = "Trackbox"
        this.trafficDetails.brand = "HashTrade"
        this.trafficDetails.country = "Israel"
        this.trafficDetails.campaignName = "TestCampaign"
      },
      onSubmit () {
        if (this.currentStep === 2){
          this.appendFields()
        }


        if (this.currentStep === 3) {
          alert('Form submitted!');
          return;
        }

        this.currentStep++;
      },

      goToPrevious(value){
        if(value == 0){
          this.currentStep--;
        }
      },

      appendFields(){
        //Only for this moment I'll put it right now
        if (this.jsonStatham.hasOwnProperty(this.trafficDetails.platform)) {
          for (const [key, value] of Object.entries(this.jsonStatham[this.trafficDetails.platform])) {
            this.integrationParams[key] = value;
          }
        }
      },
    },
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Ubuntu:wght@700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Questrial&display=swap');


html {
  height: 100%;
}
form {
  max-width: 340px;
  margin: 30px auto;
  background-color: #495057;
  padding: 50px;
  border-radius: 16px;
}

legend{
  font-family: Ubuntu;
  letter-spacing: 2px;
  color: #fff;
}

i {
  margin-right: 10px;
}

div{
  margin-bottom: 2px;
}

h2 {
  font-size: 20px;
  text-align: center;
  margin-bottom: 10px;
}
h3 {
  font-weight: 500;
  font-family: Questrial;
  letter-spacing: 1px;
  font-size: 16px;
  text-align: center;
  color: #fff;
  margin-bottom: 20px;
}

p {
  font-family: Questrial;
  color: #fff;
  margin: 0;
}
button {
  background-color: #9d4edd;
  padding: 10px 20px;
  margin-right: 10px;
  margin-top: 10px;
  border: none;
  text-align: center;
  color: white;
  font-family: Questrial;
  font-weight: 500;
  border-radius: 10px;
}


input {
  outline: none;
  border: 1px solid #343a40;
  font-size: 1em;
  padding: 5px 0;
  margin: 10px 0 5px 0;
  width: 100%;
  font-family: Questrial;
}
.redStar {
  color: red;
  font-size: 14px;
  font-weight: 500;
}
.inputLabel{
  color: #f8f9fa;
  font-weight: bold;
  text-align: left;
  font-size: 14px;
  font-family: Questrial;
  margin-top: 8px;
}
.invalid-feedback {
  color: red;
  font-family: Questrial;
  font-weight: 600;
}
</style>