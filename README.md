# google-picker-sample

Vue.js + Vuetify.js + [Picker API Developer's Guide](https://developers.google.com/picker/docs/)

Before run serve, you should change configration.
```
config: {
        developerKey: 'XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX', // CHANGE TO YOUR KEY
        clientId: 'XXXXXXXXXXXX-XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX.apps.googleusercontent.com', // CHANGE YOUR CLIENT ID
        scope: 'https://www.googleapis.com/auth/drive.file',
        appId: 'XXXXXXXXXXXX' // CHANGE YOUR PROJECT NUMBER
      },
```
## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Run your tests
```
npm run test
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
