<template>
  <form @submit.prevent>
    <!--Integration Stage 1 -->
    <template v-if="currentStage === 1">
      <h2>Step 1 - IP's to whitelists</h2>
      <h3>Please make sure you whitelist our IP's</h3>
      <p v-for="(ips) in ipAddresses" :key="ips">{{ ips }}</p>
      <button @click="updateStage(1)">Traffic Details</button>
    </template>
    <!--Integration Stage 2 -->
    <template v-if="currentStage === 2">
      <h2>Step 2 - Traffic Flow</h2>
      <h3>Describe traffic flow</h3>
      <div class="inputGroup">
        <label for="whitelabel" class="inputLabel">Who Sends the traffic?</label> <span class="redStar">*</span>
        <input type="text" v-model="trafficDetails.whitelabel" id="whitelabel"/>
        <span v-if="submitted && !$v.trafficDetails.whitelabel.required" class="invalid-feedback">It's a required field, please fill it</span>
      </div>
      <div>
        <label for="platform" class="inputLabel">Which platform are you using?</label> <span class="redStar">*</span>
        <input type="text" v-model="trafficDetails.platform" id="platform"/>
        <span v-if="submitted && !$v.trafficDetails.platform.required" class="invalid-feedback">It's a required field, please fill it</span>
      </div>
      <div>
        <label for="brand" class="inputLabel">Who receive the traffic?</label> <span class="redStar">*</span>
        <input type="text" v-model="trafficDetails.brand" id="brand"/>
        <span v-if="submitted && !$v.trafficDetails.brand.required" class="invalid-feedback">It's a required field, please fill it</span>
      </div>
      <div>
        <label for="country" class="inputLabel">From which country to test?</label> <span class="redStar">*</span>
        <input type="text" v-model="trafficDetails.country" id="country">
        <span v-if="submitted && !$v.trafficDetails.country.required" class="invalid-feedback">It's a required field, please fill it</span>
      </div>
      <div>
        <label for="campaign" class="inputLabel">How to call the campaign?</label> <span class="redStar">*</span>
        <input type="text" v-model="trafficDetails.campaignName" id="campaign"/>
        <span v-if="submitted && !$v.trafficDetails.campaignName.required" class="invalid-feedback">It's a required field, please fill it</span>
      </div>
      <button @click="updateStage(0)">Previous Stage</button>
      <button @click="(e) => handleSubmitFirstBtn(e)">Integration Params</button>
    </template>
    <!-- Integration Stage 3 -->
    <template v-if="currentStage === 3">
      <h2>Step 3 - Additional Params</h2>
      <h3>Please fill all the parameters needed</h3>
      <div v-for="(param, key, index) in integrationParams" :key="key">
        <label class="inputLabel" :for="key">Please fill in {{ key }} param</label> <span class="redStar">*</span>
        <input v-model="integrationParams[key]" :id="key">
        <span v-if="submitted && !$v.integrationParams[key].required" class="invalid-feedback">It's a required field, please fill it</span>
        <!--        <span v-if="v$.integrationParams[key].$errors[0]" class="invalid-feedback"> {{ v$.integrationParams[key].$errors[0].$message }} </span>-->
      </div>
      <button @click="updateStage(0)">Previous Stage</button>
      <button @click="handleLastSubmit">Create Integration</button>
    </template>
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
      currentStage: 1,
      ipAddresses: ["34.242.60.225", "52.18.206.144", "82.81.51.192", "213.57.116.202", "82.81.48.111", "62.90.205.233"],
      trafficDetails: {
        brand: '',
        platform: '',
        whitelabel: '',
        country: '',
        campaignName: ''
      },
      submitted: false,

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
      integrationParams: {}
    }
  },
  validations() {
    return {
      trafficDetails: {
        brand: { required, minLength: minLength(3) },
        platform: { required, minLength: minLength(3) },
        whitelabel: { required, minLength: minLength(3) },
        country: { required, minLength: minLength(2) },
        campaignName: { required, minLength: minLength(2) }
      }
    }

  },


  methods: {
    updateStage(value) {
      //Move forward
      if (value === 1) {
        this.currentStage++;
        console.log(this.currentStage)
      } else { //Move backward
        this.currentStage--;
      }
    },
    handleSubmitFirstBtn(e) {
      this.submitted = true;

      // stop here if form is invalid
      this.$v.$touch();
      if (this.$v.$invalid) {
        return;
      }

      alert("SUCCESS!! :-)");
      this.updateStage(1)
      this.appendFields()
      this.submitted = false
    },

    handleLastSubmit(e){
      this.submitted = true;

      // stop here if form is invalid
      this.$v.$touch();
      if (this.$v.$invalid) {
        return;
      }
      alert("Form Submitted Successfully")
    },

    appendFields() {
      if (this.jsonStatham.hasOwnProperty(this.trafficDetails.platform)) {
        for (const [key, value] of Object.entries(this.jsonStatham[this.trafficDetails.platform])) {
          this.integrationParams[key] = value
        }
        console.log(this.integrationParams)
      }
    },
  }
}
</script>

<style scoped>
html {
  height: 100%;

}
form {
  max-width: 340px;
  margin: 30px auto;
  background-color: #dee2e6;
  padding: 40px;
  border-radius: 16px;
}
h2 {
  font-size: 20px;

  text-align: center;
  margin-bottom: 10px;
}
h3 {
  font-weight: bold;
  font-size: 16px;
  text-align: center;
  color: #343a40;
  margin-bottom: 20px;
}
h4 {
  font-weight: bold;
  font-size: 14px;
  color: #343a40;
  margin-bottom: 20px;
  font-family: 'Josefin Sans', sans-serif;
}
p {
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
  font-weight: bold;
  border-radius: 10px;
}
label {
  color: #aaa;
  display: inline-block;
  margin-top: 10px;
  text-align: left;
  justify-content: left;
  font-size: 12px;
  letter-spacing: 1px;
  font-weight: bold;
}
input {
  outline: none;
  border: 1px solid #343a40;
  font-size: 1em;
  padding: 5px 0;
  margin: 10px 0 5px 0;
  width: 100%;
}
.redStar {
  color: red;
  font-size: 14px;
  font-weight: 400;
}
.inputLabel{
  color: #343a40;
  font-weight: bold;
  text-align: left;
}
.invalid-feedback {
  color: red;
  font-weight: 500;
}
</style>