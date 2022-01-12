<template>
  <v-container>
    <v-row align="center"
              class="spacer"
              no-gutters>
      <v-col cols="2">
        <v-avatar size="30">
          <img
            v-bind:src="user.pictureUrl"
          >
        </v-avatar>
      </v-col>
      <v-col>
        {{user.displayName}}
      </v-col>
    </v-row>

    <v-row>
      <v-col cols="10">
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
      <v-bottom-navigation
        color="primary"
        grow
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
  </v-container>
</template>

<script>
const stickers =
[
  {
    packageId: "8515",
    stickerId: "16581252"
  },
  {
    packageId: "8515",
    stickerId: "16581253"
  },
  {
    packageId: "8515",
    stickerId: "16581242"
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
    window.liff.getProfile()
      .then(profile => {
        this.user.displayName = profile.displayName
        this.user.pictureUrl = profile.pictureUrl
      })
  },
  methods: {
    onSubmit() {
      console.log(stickers[this.value])
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
