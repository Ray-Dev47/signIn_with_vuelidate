<template>
  <div>
    <form class="my-form" @submit.prevent="submitForm" novalidate="true">
      <div class="container">
        <h5 class="container__header">Create a new account</h5>
        <ul>
          <li>
            <label class="my-form__label" for="">Email</label>
            <div class="grid">
              <input
                type="email"
                id="email"
                name="email"
                v-model="$v.form.email.$model"
                :class="{ 'is-invalid': submitted && $v.form.email.$error }"
                required
              />
            </div>
            <small class="error" v-if="submitted && !$v.form.email.required"
              >Email is required</small
            >
            <small class="error" v-if="submitted && !$v.form.email.email"
              >Please enter a valid email.</small
            >
            <small class="error" v-if="submitted && !$v.form.email.maxLength"
              >Email cannot be greater than 50</small
            >
          </li>
          <li>
            <label class="my-form__label" for="emailConfirm"
              >Confirm Email</label
            >
            <div class="grid">
              <input
                type="email"
                id="emailConfirm"
                name="emailConfirm"
                v-model="$v.form.emailConfirm.$model"
                :class="{
                  'is-invalid': submitted && $v.form.emailConfirm.$error,
                }"
                required
              />
            </div>
            <span v-if="submitted && $v.form.emailConfirm.$error" class="error">
              <small class="error" v-if="!$v.form.emailConfirm.required"
                >Email is required</small
              >
              <br />
              <small class="error" v-if="!$v.form.emailConfirm.sameAsEmail"
                >Emails do not match.</small
              >
            </span>
          </li>
          <li>
            <label class="my-form__label" for="userName">Username</label>
            <div class="grid">
              <input
                type="text"
                id="userName"
                name="userName"
                v-model.lazy="$v.form.userName.$model"
                :class="{ 'is-invalid': submitted && $v.form.userName.$error }"
              />
              <small
                class="error"
                v-if="submitted && !$v.form.userName.required"
                >Username is required</small
              >
              <small
                class="error"
                v-if="submitted && !$v.form.userName.strongUsername"
                >Username requires 3 to 20 characters using letters, digits and
                optionally a single punctuation</small
              >
            </div>
          </li>
          <li>
            <label class="my-form__label" for="password">Password</label>
            <div class="grid">
              <input
                type="password"
                name="password"
                id="password"
                v-model="$v.form.password.$model"
                :class="{ 'is-invalid': submitted && $v.form.password.$error }"
              />
              <span v-if="submitted && $v.form.password.$error" class="error">
                <small class="error" v-if="!$v.form.password.required"
                  >Password is required</small
                >
                <br />
                <small class="error" v-if="!$v.form.password.strongPassword"
                  >Passwords must be at least 8 characters long, contain one
                  lowercase letter, contain one uppercase letter and include at
                  least one number 0-10.</small
                >
              </span>
            </div>
          </li>
          <li>
            <label class="my-form__label" for="passwordConfirm"
              >Confirm Password</label
            >
            <div class="grid">
              <input
                type="password"
                id="passwordConfirm"
                name="passwordConfirm"
                v-model="$v.form.passwordConfirm.$model"
                :class="{
                  'is-invalid': submitted && $v.form.passwordConfirm.$error,
                }"
              />
              <span
                v-if="submitted && $v.form.passwordConfirm.$error"
                class="error"
              >
                <small class="error" v-if="!$v.form.passwordConfirm.required"
                  >Confirm password is required</small
                >
                <br />
                <small
                  class="error"
                  v-if="!$v.form.passwordConfirm.sameAsPassword"
                  >Passwords do not match.</small
                >
              </span>
            </div>
          </li>
          <li>
            <label class="wrapper">
              I want to receive emails from VenewLive and its partners about
              upcoming shows and information.
              <input
                type="checkbox"
                name="subscription"
                id="subscription"
                v-model="form.subscription"
              />
              <span class="checkmark"></span>
            </label>
          </li>

          <li>
            <label class="wrapper">
              By submitting this form I agree to the
              <a href="#">Terms of Service</a> and
              <a href="#">Privacy Notice</a>
              <input
                type="checkbox"
                v-model="form.terms"
                :class="{ 'is-invalid': submitted && $v.form.terms.$error }"
              />
              <span class="checkmark"></span>
            </label>
            <small class="error" v-if="submitted && !$v.form.terms.required"
              >Terms of Service and Privacy Policy needs to be
              acknowledged.</small
            >
          </li>
        </ul>
        <div class="button">
          <button type="submit" class="btn">Submit</button>
        </div>
        <div class="login_link">
          <a href="#">Go to Log In</a>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import {
  required,
  email,
  sameAs,
  minLength,
  maxLength,
} from "vuelidate/lib/validators";

export default {
  data() {
    return {
      submitted: false,
      form: {
        email: "",
        emailConfirm: "",
        userName: "",
        password: "",
        passwordConfirm: "",
        subscription: false,
        terms: "",
      },
    };
  },
  validations: {
    form: {
      email: { required, email, maxLength: maxLength(50) },
      emailConfirm: { required, sameAsEmail: sameAs("email") },
      userName: {
        required,
        minLength: minLength(3),
        maxLength: maxLength(20),
        strongUsername(username) {
          return (
            /[a-z]/.test(username) &&
            /[0-9]/.test(username)
          );
        },
      },
      password: {
        required,
        strongPassword(password) {
          return (
            /[A-Z]/.test(password) &&
            /[a-z]/.test(password) &&
            /[0-9]/.test(password) &&
            /[#?!@$%^&*-]/.test(password) &&
            password.length >= 8
          );
        },
      },
      passwordConfirm: { required, sameAsPassword: sameAs("password") },
      terms: { required },
    },
  },
  methods: {
    submitForm() {
      this.submitted = true;

      this.$v.form.$touch();

      if (this.$v.form.$invalid) {
        return;
      }
      this.$emit('submit', this.form) 
    },
  },
};
</script>


<style lang="scss" >
@import "@/assets/styles/_form";
</style>
