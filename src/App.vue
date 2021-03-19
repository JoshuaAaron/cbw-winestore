<template>
  <div id="app">
  </div>
</template>

<script>
import { API } from 'aws-amplify';

// client request: fetching wines
API.get('winesapi', '/wines', {}).then(result => {
 this.wines = JSON.parse(result.body);
}).catch(err => {
 console.log(err);
})
// client request: fetching a wine by id
API.get('winesapi', `/wines/${id}`, {}).then((result) => {
  this.wine = JSON.parse(result.body);
}).catch(err => {
  console.log(err);
})
// client request: creating a new wine
API.post('winesapi', '/wines', {
  body: {
    name: "name-1",
    type: "Merlot",
    price: 19.99,
    featured: true,
    image: ""
  }
}).then(result => {
  this.wine = JSON.parse(result.body);
}).catch(err => {
  console.log(err);
})
// client request: updating a wine
API.put('winesapi', `/wines`, { 
  body: {
    id: id,
    name: "wine-2",
    type: "Chardonnay",
    price: 9.99,
    featured: true,
    image: ""
  }
}).then(result => {
  this.wine = JSON.parse(result.body);
}).catch(err => {
  console.log(err);
})
// client request: deleting a wine by id
API.del('winesapi', `/wines/${id}`, {}).then(result => {
  console.log(result);
}).catch(err => {
  console.log(err);
})
export default {

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
</style>
