<template>
  <v-row>
    <v-col>
      <v-text-field
        v-model="formData.text"
        clearable
        solo
        dense
      ></v-text-field>
    </v-col>
    <v-col>
      <v-btn
        icon
        color="primary"
        v-on:click="onSubmit()"
      >
        <v-icon>mdi-send</v-icon>
      </v-btn>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      formData: {
        text: ''
      },
      lineId: null
    }
  },
  mounted() {
    if (!this.canUseLIFF()) {
      return
    }

    window.liff.init({ liffId: "1656800308-xdn43o2Y" })
  },
  methods: {
    onSubmit() {
      if (!this.canUseLIFF()) {
        return
      }

      window.liff
        .sendMessages([
          {
            type: 'text',
            text: `Hello!\n${this.formData.text}`
          }
        ])
        .then(() => {
          window.liff.closeWindow()
        })
        .catch(e => {
          window.alert('Error sending message: ' + e)
        })
    },
    handleCancel() {
      if (!this.canUseLIFF()) {
        return
      }
      window.liff.closeWindow()
    },
    canUseLIFF() {
      return navigator.userAgent.indexOf('Line') !== -1 && window.liff
    }
  }
}
</script>
