<template>
  <v-container>
    <v-row>
      <v-col>
        <v-text-field
          v-model="formData.text"
          clearable
          dense
        ></v-text-field>
      </v-col>
      <v-col cols="2">
        <v-btn
          icon
          color="primary"
          v-on:click="onSubmit()"
        >
          <v-icon>mdi-send</v-icon>
        </v-btn>
      </v-col>
    </v-row>
    <v-row>
      <v-bottom-navigation
        color="primary"
        v-model="value"
      >
        <v-btn>
          <span>Star</span>

          <v-icon>mdi-star</v-icon>
        </v-btn>

        <v-btn>
          <span>Heart</span>

          <v-icon>mdi-heart</v-icon>
        </v-btn>

        <v-btn>
          <span>ThumbUp</span>

          <v-icon>mdi-thumb-up</v-icon>
        </v-btn>
      </v-bottom-navigation>
    </v-row>
  </v-container>
</template>

<script>
const stickers =
[
  {
    packageId: "11539",
    stickerId: "52114116"
  },
  {
    packageId: "11539",
    stickerId: "52114111"
  },
  {
    packageId: "11539",
    stickerId: "52114115"
  }
]
export default {
  name: `Index`,
  data() {
    return {
      formData: {
        text: ``
      },
      value: 0,
      lineId: ``,
      user: {
        displayName: ``,
        pictureUrl: ``
      }
    }
  },
  mounted() {
    if (!this.canUseLIFF()) {
      return
    }

    window.liff.init({ liffId: `1656800308-xdn43o2Y` })
  },
  methods: {
    onSubmit() {
      console.log(stickers[this.value])
      console.log({
            type: `sticker`,
            packageId: stickers[this.value].packageId,
            stickerId: stickers[this.value].stickerId
          })
      if (!this.canUseLIFF()) {
        return
      }

      window.liff
        .sendMessages([
          {
            type: `sticker`,
            packageId: stickers[this.value].packageId,
            stickerId: stickers[this.value].stickerId
          },
          {
            type: `text`,
            text: `${this.formData.text}`
          }
        ])
        .then(() => {
          window.liff.closeWindow()
        })
        .catch(e => {
          window.alert(`Error sending message: ${e}`)
          window.alert(`${stickers[this.value].packageId}, ${stickers[this.value].stickerId}`)
        })
    },
    handleCancel() {
      if (!this.canUseLIFF()) {
        return
      }
      window.liff.closeWindow()
    },
    canUseLIFF() {
      return navigator.userAgent.indexOf(`Line`) !== -1 && window.liff
    }
  }
}
</script>
