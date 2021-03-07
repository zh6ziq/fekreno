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
          <div class="text-h4 text-primary">New password</div>
          <div class="text-caption">Enter your new password</div>
        </div>

        <div class="q-gutter-sm">
          <q-input
            filled
            standout
            label="New Password"
            color="text-white"
            v-model.trim="form.password"
            ref="password"
            :error="$v.form.password.$error"
            error-message="Minimum 8 characters"
          />

          <q-input
            filled
            standout
            label="Confirm Password"
            color="text-white"
            v-model.trim="form.confirmPwd"
            ref="confirmPwd"
            :error="$v.form.confirmPwd.$error"
            error-message="Password doesn't match"
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
          <!-- <div class="q-gutter-sm flex flex-center">
            <q-btn
              out
              color="primary"
              text-color="primary"
              flat
              to="/login"
              label="Back to Login"
              no-caps
            />
          </div> -->
        </div>

      </q-card>
    </div>
  </div>
</template>

<script>
import { required, sameAs, minLength } from 'vuelidate/lib/validators'

export default {
  name: 'ResetPassword',
  data() {
    return {
      form: {
        password: '',
        confirmPwd: ''
      },
    }
  },

  validations: {
    form: {
      password: { required, minLength: minLength(8) },
      confirmPwd: { required, sameAsPassword: sameAs('password') }
    }
  },

  methods: {
    onSubmit() {
      this.$v.form.$touch()

      if (this.$v.form.$invalid) {
        return
      }
      // sent email to endpoint
      this.$q.notify({
        type: 'info',
        message: 'Password has been updated. Please login.',
        position: 'top',
        timeout: 2000
      })
    }
  }
}
</script>
