<template>
  <v-container fluid fill-height>
    <v-layout
      text-xs-center
      wrap
      justify-center row align-center
    >
      <v-flex
        mb-5
        xs12
      >
        <v-flex>
          <v-btn round color="blue" class="white--text" @click="onDownload()">DOWNLOAD FROM GOOGLE DRIVE</v-btn>
        </v-flex>
        <v-flex>
          <v-btn round color="orange" class="white--text" @click="onUpload()">UPLOAD TO GOOGLE DRIVE</v-btn>
        </v-flex>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
/* eslint-disable */
export default {
  data () {
    return {
      type: null,
      config: {
        developerKey: 'XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX', // CHANGE TO YOUR KEY
        clientId: 'XXXXXXXXXXXX-XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX.apps.googleusercontent.com', // CHANGE YOUR CLIENT ID
        scope: 'https://www.googleapis.com/auth/drive.file',
        appId: 'XXXXXXXXXXXX' // CHANGE YOUR PROJECT NUMBER
      },
      pickerApiLoaded: false,
      oAuthToken: null
    }
  },
  mounted () {
    if (!window.gapi) {
      return
    }
    window.gapi.load('auth2', () => {
      window.gapi.load('picker', () => {
        this.pickerApiLoaded = true
      })
    })
  },
  methods: {
    handleAuthResult (authResult) {
      if (authResult && !authResult.error) {
        this.oAuthToken = authResult.access_token
        this.createPicker()
      } else {
        return console.warn(authResult.details)
      }
    },
    onDownload () {
      this.type = 'download'
      this.handleButton()
    },
    onUpload () {
      this.type = 'upload'
      this.handleButton()
    },
    handleButton () {
      if (!this.oAuthToken) {
        window.gapi.auth2.authorize({
          client_id: this.config.clientId,
          scope: this.config.scope
        }, this.handleAuthResult)
      } else {
        this.createPicker()
      }
    },
    createPicker () {
      if (this.pickerApiLoaded && this.oAuthToken) {
        var view = new google.picker.DocsView()
        view.setIncludeFolders(true)
        if (this.type === 'upload')  {
          view = new google.picker.DocsUploadView()
          view.setIncludeFolders(true)
        }
        var picker = new google.picker.PickerBuilder()
          .enableFeature(google.picker.Feature.NAV_HIDDEN)
          .enableFeature(google.picker.Feature.MULTISELECT_ENABLED)
          .setAppId(this.config.appId)
          .setOAuthToken(this.oAuthToken)
          .addView(view)
          .addView(new google.picker.DocsUploadView())
          .setLocale('ja')
          .setDeveloperKey(this.config.developerKey)
          .setCallback(this.pickerCallback)
          .build()
        picker.setVisible(true)
      }
    },
    pickerCallback (data) {
      if (data[google.picker.Response.ACTION] == google.picker.Action.PICKED) {
        console.log(data)
      }
    }    
  }
}
</script>

<style>

</style>
