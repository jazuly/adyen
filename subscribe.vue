<template>
  <div>
    <div id="payment-page">
      <div class="container">
        <div v-if="type === 'dropin'" id="dropin" ref="dropin"></div>
        <div v-else class="payment-container">
          <div id="card" class="payment" :ref="`${type}`"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

function handleSubmission(state, component, url) {
  callServer(url, state.data)
    .then(res => handleServerResponse(res, component))
    .catch(error => {
      throw Error(error);
    });
}
function callServer(url, data) {
  return fetch(url, {
    method: "POST",
    body: JSON.stringify(data),
    headers: {
      "Content-Type": "application/json"
    }
  }).then(res => res.json());
}
function handleServerResponse(res, component) {
  if (res.action) {
    component.handleAction(res.action);
  } else {
    switch (res.resultCode) {
      case "Authorised":
        window.location.href = "/success";
        break;
      case "Pending":
        window.location.href = "/pending";
        break;
      case "Refused":
        window.location.href = "/failed";
        break;
      default:
        window.location.href = "/error";
        break;
    }
  }
}
export default {
  head() {
    return {
      title: "Payment page"
    };
  },
  asyncData({ route }) {
    return { type: route.params.payment };
  },
  data() {
    return {
      clientKey: "",
      response: ""
    };
  },
  mounted() {
    $nuxt.$axios.get('/payment2/listmethods')
      .then(data => {
        this.clientKey = '';
        this.response = data.data;
        console.log(data)
        const configuration = {
          paymentMethodsResponse: this.response,
          clientKey: this.clientKey,
          locale: "en_US",
          environment: "test",
          showPayButton: true,
          paymentMethodsConfiguration: {
            ideal: {
              showImage: true
            },
            card: {
              hasHolderName: true,
              holderNameRequired: true,
              name: "Credit or debit card",
              amount: {
                value: 1000,
                currency: "EUR"
              }
            }
          },
          onSubmit: (state, component) => {
            handleSubmission(state, component, "/api/initiatePayment");
          },
          onAdditionalDetails: (state, component) => {
            handleSubmission(state, component, "/api/submitAdditionalDetails");
          }
        };
        const checkout = new AdyenCheckout(configuration);
        const integration = checkout
          .create(this.type)
          .mount(this.$refs[this.type]);
      });
  }
};
</script>
