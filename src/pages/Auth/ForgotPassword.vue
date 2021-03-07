<template>
  <div class="fullscreen row">
    <div class="col">
    </div>
    <div class="col bg-primary">
    </div>
    <div class="fullscreen flex flex-center">
      <q-card class="q-pa-md" style="width:400px;">
        <div class="text-weight-bold text-center q-pb-lg q-gutter-sm">
          <q-img class="q-mb-md" src="~assets/krenovator.png"/>
          <div class="text-h4 text-primary">Oops!!</div>
          <div class="text-caption">Enter email address to reset your password</div>
        </div>

        <div class="q-gutter-sm">
          <q-input
            filled
            standout
            label="Email"
            color="text-white"
            v-model.trim="form.email"
            ref="email"
            :error="$v.form.email.$error"
            error-message="Please enter a valid email address"
          />

          <div class="q-gutter-sm flex flex-center">
            <q-btn
              color="primary"
              text-color="white"
              label="Submit"
              no-caps
              @click="onSubmit"
            />
          </div>
          <div class="q-gutter-sm flex flex-center">
            <q-btn
              out
              color="primary"
              text-color="primary"
              flat
              to="/login"
              label="Back to Login"
              no-caps
            />
          </div>
        </div>

      </q-card>
    </div>
  </div>
</template>

<script>
import { required, email } from 'vuelidate/lib/validators'

export default {
  name: 'ForgotPassword',
  data() {
    return {
      form: {
        email: ''
      },
    }
  },

  validations: {
    form: {
      email: { required, email },
    }
  },

  methods: {
    onSubmit() {
      this.$v.form.$touch()

      if (this.$v.form.$invalid) {
        return
      }
      // sent email to endpoint
      this.$store.dispatch('ForgotPassword', this.form)
        .then(res => {
          this.$q.notify({
            type: 'info',
            message: 'Please check your email for a redirect link',
            position: 'top',
            timeout: 2000
          })
        })
        .catch(err => {
          this.$q.notify({
            type: 'negative',
            message: `User email doesn't exist`,
            position: 'top',
            timeout: 2000
          })
          console.log(err)
        })
    }
  }
}
</script>
