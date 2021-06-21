<template>
  <form @submit.prevent="handleSubmit">
    <div class="row">
        <div class="form-group col-md-12">
            <label>Email address:</label>
            <input type="email" class="form-control" required v-model="values.email">
            <span v-if="(!$v.values.email.required || !$v.values.email.email) && $v.values.email.$dirty" class="text-danger">A valid email address is required.</span>
        </div>
    </div>
    
    <div class="row">
        <div class="form-group col-md-4">
            <label>Postal code:</label>
            <input type="text" pattern="[0-9]{4}" class="form-control" required v-model="values.postcode">
            <span v-if="(!$v.values.postcode.required || !$v.values.postcode.validatePostcode) && $v.values.postcode.$dirty" class="text-danger">A danish postal code is required.</span>
        </div>

        <div class="form-group col-md-8">
            <label>City:</label>
            <input type="text" class="form-control" required v-model="values.city">
            <span v-if="(!$v.values.city.required || !$v.values.city.validateCity) && $v.values.city.$dirty" class="text-danger">A valid city is required.</span>
        </div>
    </div>
    <div class="submit">
          <button>Submit</button>
    </div>
  </form>
</template>

<script>
import { required, email } from 'vuelidate/lib/validators'
import { helpers } from 'vuelidate/lib/validators'
export default {
    name:"Form",
    data(){
        return {
            values: {
                email: null,
                postcode: null,
                city: null,
            }
        }
    },

    validations: {
    values: {
            email: {
                required,
                email,
            },

            postcode: {
                required,
                validatePostcode: helpers.regex('validatePostcode', /^(?:[1-24-9]\d{3}|3[0-8]\d{2})$/),
            },

            city: {
                required,
                validateCity: helpers.regex('validateCity', /^[a-zA-Z æøåÆØÅ]*$/),

            }
    }
    },

    methods: {
            handleSubmit() {
                this.$v.$touch();
                   if(!this.$v.$invalid) {
                        console.log(
                            'Email: ',this.values.email, ', Postal code: ', this.values.postcode, ', City: ', this.values.city
                        );
                        alert('Form submitted.');
                    }
                },

    }
}

</script>

<style>
form {
    max-width: 520px;
    margin: 30px auto;
    background: white;
    text-align: left;
    padding: 40px;
    border-radius: 5px;
}
label {
    color: #aaa;
    display: inline-block;
    margin: 25px 0 15px;
    font-size: 0.6em;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: bold;
}
input.form-control {
    display: block;
    padding: 10px 6px;
    width: 100%;
    box-sizing: border-box;
    border: none;
    border-bottom: 1px solid #ddd;
    color: #555;
}
.text-danger {
    font-size: 0.6em;
}
button {
    background: #0f089a;
    border: 0;
    padding: 10px 20px;
    margin-top: 20px;
    color: white;
    border-radius: 5px;
    text-transform: uppercase;
    letter-spacing: 1px;
}
.submit {
    text-align: center;
    font-size: 0.8em;
}
</style>