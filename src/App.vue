<template>
  <div id="app">
    <amplify-authenticator>
      <div class="welcome">
        <h1>Hey, {{user.username}}!</h1>
        <amplify-sign-out></amplify-sign-out>
      </div>
      <div class="form-body">
        <form v-on:submit.prevent>
          <button @click='getWines' class='button'>GET /wines</button>
          <button @click='getWine' :disabled='lastWineId==""' class='button'>GET /wines/:id</button>
          <button @click='addWine' class='button'>POST /wines</button>
          <button @click='updateWine' :disabled='lastWineId==""' class='button'>PUT /wines</button>
          <button @click='deleteWine' :disabled='lastWineId==""' class='button'>DELETE /wines/:id</button>
          <div>Current Wine: {{ lastWineId || "Not set. Add a new wine" }}</div>
        </form>
      </div>
    </amplify-authenticator>
  </div>
</template>

<script>
import { AuthState, onAuthUIStateChange } from '@aws-amplify/ui-components';
import { API } from 'aws-amplify';
export default {
  name: 'app',
  data() {
    return {
      user: { },
      lastWineId: ""
    }
  },
  created() {
    // authentication state managament
    onAuthUIStateChange((state, user) => {
      // set current user and load data after login
      if (state === AuthState.SignedIn) {
        this.user = user;
        console.log(user);
      }
    })
  },
  methods: {
    getWines: function() {
      console.log("getWines");
      API.get('winesapi', `/wines`, {}).then((result) => {
        console.log(result);
      }).catch(err => {
        console.log(err);
      })
    },
    getWine: function() {
      const id = this.lastWineId;
      if (!id) return;
      console.log(`getWine-${id}`);
      API.get('winesapi', `/wines/${id}`, {}).then((result) => {
        console.log(result);
      }).catch(err => {
        console.log(err);
      })
    },    
    addWine: function() {
      console.log(`addWine`);
      API.post('winesapi', `/wines`, { 
        body: {
          text: "wine1"
        }
      }).then(result => {
        console.log(result);
        this.lastWineId = JSON.parse(result.body).id;
      }).catch(err => {
        console.log(err);
      })
    },    
    updateWine: function() {
      const id = this.lastWineId;
      if (!id) return;
      console.log(`updateWine-${id}`);
      API.put('winesapi', `/wines`, { 
        body: {
          id: id,
          text: "wine2",
          complete: true
        }
      }).then(result => {
        console.log(result);
      }).catch(err => {
        console.log(err);
      })      
    },    
    deleteWine: function() {
      const id = this.lastWineId;
      if (!id) return;
      console.log(`deleteWine-${id}`);
      API.del('winesapi', `/wines/${id}`, {}).then(result => {
        console.log(result);
        this.lastWineId = "";
      }).catch(err => {
        console.log(err);
      })
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

:root {
  --amazonOrange: #FF9900;
  --lightAmazonOrange: #FFAC31;
  --darkAmazonOrange: #E88B01;
  --squidInk: #232F3E;
  --lightSquidInk: #31465F;
  --deepSquidInk: #152939;
  --grey: #828282;
  --lightGrey: #C4C4C4;
  --silver: #E1E4EA;
  --darkBlue: #31465F;
  --red: #DD3F5B;
  --white: #FFFFFF;
  --light-blue: #00a1c9;
  --button-color: var(--white);
  --button-background-color: var(--amazonOrange);
  --button-click: var(--darkAmazonOrange);
  --link-color: var(--amazonOrange);
  --form-color: var(--white);
  --input-color: var(--deepSquidInk);
  --input-background-color: var(--white);
  --font-family: "Amazon Ember","Helvetica Neue Light","Helvetica Neue","Helvetica" ,"Arial","sans-serif";
  --body-background: #F8F4F4;
  --component-width-desktop: 460px;
  --component-width-mobile: 100%;
  --color-primary: #FF9900;
  --color-primary-accent: #232F3E;
  --color-primary-highlight: #FFC46D;
  --color-background: #232F3E;
  --color-secondary: #152939;
  --color-secondary-accent: #31465F;
  --color-danger: #DD3F5B;
  --color-error: #D0021B;
  --color-accent-brown: #828282;
  --color-accent-blue: #E1E4EA;
  --gradient-blaze: linear-gradient(270deg, #FFC300 0%, #FF9000 100%);
  --color-blue: #007EB9;
  --color-purple: #527FFF;
  --color-gray: #828282;
  --color-white: #FFFFFF;
  --input-border: 1px solid #C4C4C4;
  --input-padding: 0.5em 0.5em 0.3em 1em;
  --box-shadow: 1px 1px 4px 0 rgba(0,0,0,0.15);
  --button-height: 42px;
  --interactions-conversation-height: 250px;
  --ion-color-primary: #FF9900;
  --ion-color-primary-rgb: 255,153,0;
  --ion-color-primary-contrast: #fff;
  --ion-color-primary-contrast-rgb: 255,255,255;
  --ion-color-primary-shade: #232F3E;
  --ion-color-primary-tint: #FFC46D;
}
html,
body {
  font-family: "Amazon Ember", "Helvetica", "sans-serif";
  margin: 0;
}
a {
  color: #ff9900;
}
h1 {
  font-weight: 300;
}
.app {
  width: 100%;
}
.app-header {
  color: white;
  text-align: center;
  background: linear-gradient(30deg, #f90 55%, #ffc300);
  width: 100%;
  margin: 0 0 1em 0;
  padding: 3em 0 3em 0;
  box-shadow: 1px 2px 4px rgba(0, 0, 0, 0.3);
}
.app-body {
  width: 60%;
  margin: 0 auto;
  text-align: center;
  min-height: 500px;
}
.form-body {
  display: flex;
  justify-content: center;
  align-items: center;
  display: -webkit-flex;
  -webkit-justify-content: center;
  -webkit-align-items: center;
  flex-direction: row;
  flex-wrap: wrap; 
}
.form-body button {
  background-color: #ff9900;
  font-size: 14px;
  color: white;
  text-transform: uppercase;
  padding: 1em;
  border: none;
  cursor: pointer;
  margin: 10px;
}
button:hover {
  opacity: 0.8;
}
/* remove blue highlight */
textarea:hover, 
input:hover:not([type="checkbox"]), 
textarea:active, 
input:active:not([type="checkbox"]), 
textarea:focus, 
input:focus:not([type="checkbox"]),
button:focus,
button:active,
button:hover,
label:focus,
.btn:active,
.btn.active,
select
{
  outline:0px !important;
  -webkit-appearance:none;
  box-shadow: none !important;
}
.welcome {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: row;
}
.welcome h1 {
  margin-right: 40px;
}
button:disabled,
button[disabled]{
  border: 1px solid #999999;
  background-color: #cccccc;
  color: #666666;
}
</style>
