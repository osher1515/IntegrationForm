<template>
    <form @submit.prevent>
      <img src="@/assets/tigloo.webp" alt="">
      <fieldset v-if="currentStep === 1">
        <legend>Whitelist Our IP's</legend>
        <h3>Please make sure you whitelist our IP's</h3>
        <p v-for="(ips) in ipAddresses" :key="ips">{{ ips }}</p>
        <button @click="stepChanger(1), validationAndInputs">Next Stage</button>
      </fieldset>

      <fieldset v-else-if="currentStep === 2">
        <legend>Step 2 - Traffic Flow</legend>
          <div>
            <label for="whitelabel" class="inputLabel">Who Sends the traffic?</label> <span class="redStar">*</span>
            <input type="text" v-model="trafficDetails.whitelabel" id="whitelabel">
            <span v-if="submitted && !$v.trafficDetails.whitelabel.required" class="invalid-feedback">It's a required field, please fill it</span>
          </div>

          <div>
            <label for="platform" class="inputLabel">Which platform are you using?</label> <span class="redStar">*</span>
            <input type="text" v-model="trafficDetails.platform" id="platform"/>
            <span v-if="submitted && !$v.trafficDetails.whitelabel.required" class="invalid-feedback">It's a required field, please fill it</span>
          </div>

          <div>
            <label for="brand" class="inputLabel">Who receive the traffic?</label> <span class="redStar">*</span>
            <input type="text" v-model="trafficDetails.brand" id="brand"/>
            <span v-if="submitted && !$v.trafficDetails.whitelabel.required" class="invalid-feedback">It's a required field, please fill it</span>
          </div>

        <div>
          <label for="country" class="inputLabel">From which country to test?</label> <span class="redStar">*</span>
          <input type="text" v-model="trafficDetails.country" id="country">
          <span v-if="submitted && !$v.trafficDetails.whitelabel.required" class="invalid-feedback">It's a required field, please fill it</span>
        </div>

          <div>
            <label for="campaign" class="inputLabel">How to call the campaign?</label> <span class="redStar">*</span>
            <input type="text" v-model="trafficDetails.campaignName" id="campaign"/>
            <span v-if="submitted && !$v.trafficDetails.whitelabel.required" class="invalid-feedback">It's a required field, please fill it</span>
          </div>
        <button @click="stepChanger(0)">Previous Stage</button>
        <button @click="validationAndInputs">Next Stage</button>
      </fieldset>

      <fieldset v-else-if="currentStep === 3">
        <legend>Step 3 - Integration Params</legend>
        <div v-for="(param, key, index) in integrationParams" :key="key">
          <label class="inputLabel" :for="key">Please fill in {{ key }} param</label> <span class="redStar">*</span>
          <input v-model="integrationParams[key]" :id="key" name="namedField">
        </div>
        <button @click="stepChanger(0)">Previous Stage</button>
      </fieldset>
      <button @click="fillParams" v-if="currentStep === 2">Fill parameters</button>
      <h3>Struggling with the form? <br>Contact our <a href="https://join.skype.com/invite/pzc3tcenqags">support</a><br> </h3>
    </form>
</template>

<script>
import { required, minLength} from 'vuelidate/lib/validators'
import useVuelidate from '@vuelidate/core'


export default {
  name: "Form",
  data() {
    return {
      $v: useVuelidate(),
      currentStep: 1,
      ipAddresses: ["34.242.60.225", "52.18.206.144", "82.81.51.192", "213.57.116.202", "82.81.48.111", "62.90.205.233"],
      trafficDetails: {
        brand: '',
        platform: '',
        whitelabel: '',
        country: '',
        campaignName: ''
      },
      submitted: false,
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


  validations() {
    return {
      trafficDetails: {
        brand: {required, minLength: minLength(3)},
        platform: {required, minLength: minLength(3)},
        whitelabel: {required, minLength: minLength(3)},
        country: {required, minLength: minLength(2)},
        campaignName: {required, minLength: minLength(2)}
      }
    }
  },

  methods: {

    fillParams() {
      //Method to fill params to check the form, can be removed
      this.trafficDetails.whitelabel = "Kalite"
      this.trafficDetails.platform = "Trackbox"
      this.trafficDetails.brand = "HashTrade"
      this.trafficDetails.country = "Israel"
      this.trafficDetails.campaignName = "TestCampaign"
    },

    stepChanger(value) {
      if (value === 0) {
        this.currentStep--;
      }
      if (value === 1) {
        this.currentStep++;
      }
    },
    validationAndInputs(){
      this.submitted = true;
      // stop here if form is invalid
      this.$v.$touch();
      if (this.$v.$invalid) {
        return;
      }
      this.stepChanger(1)
      this.appendFields()
      this.submitted = false;
    },
    appendFields(){
      if (this.jsonStatham.hasOwnProperty(this.trafficDetails.platform)) {
        for (const [key, value] of Object.entries(this.jsonStatham[this.trafficDetails.platform])) {
          this.integrationParams[key] = value;
        }
      }
    },
  }
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

p, a {
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
  font-weight: 500;
}
</style>