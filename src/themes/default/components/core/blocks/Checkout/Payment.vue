<template>
  <div class="payment">
    <div class="row">
      <div class="col-md-12 mb15">
        <h3>Payment</h3>
      </div>
    </div>
    <div class="row" v-show="this.isActive">
      <div v-for="(method, index) in paymentMethods" :key="index" class="col-md-6 mb15">
        <label><input type="radio" :value="method.code" name="paymentmethod" v-model="payment.paymentMethod"> {{ method.name }} | {{ method.cost | price }} </label>
      </div>
      <span class="validation-error" v-if="!$v.payment.paymentMethod.required">Field is required</span>
      
      <div class="col-md-12 my30">
        <button-full @click.native="sendDataToCheckout" text="Go review the order"/>
      </div>
    </div>
  </div>
</template>

<script>
import { coreComponent } from 'lib/themes'
import EventBus from 'src/event-bus/event-bus'
import PaymentMethods from 'src/resource/payment_methods.json'

import ButtonFull from 'theme/components/theme/ButtonFull.vue'
import { required } from 'vuelidate/lib/validators'

export default {
  props: ['isActive'],
  validations: {
    payment: {
      paymentMethod: {
        required
      }
    }
  },
  data () {
    return {
      isFilled: false,
      paymentMethods: PaymentMethods,
      payment: {
        paymentMethod: 'cashondelivery'
      }
    }
  },
  methods: {
    sendDataToCheckout () {
      EventBus.$emit('checkout.payment', this.payment, this.$v)
    }
  },
  components: {
    ButtonFull
  },
  mixins: [coreComponent('core/blocks/Checkout/Payment')]
}
</script>

<style scoped>
.validation-error{
  color: red;
  display: block;
}
</style>
