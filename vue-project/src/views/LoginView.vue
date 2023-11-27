
<template>
  <div class="body">
    <fieldset class="fieldset">
      <h2 class="heading2"><b>Log in account</b></h2>

      <div class="inputbox">
        <i class="fa-solid fa-user icon"></i>
        <input v-model="username" type="text" required="required">
        <label for="username">Username</label>
      </div>

      <div class="inputbox">
        <i class="fa-solid fa-key icon"></i>
        <input v-model="password" type="password" required="required">
        <label for="password">Password</label>
      </div>

      <div class="inputbox">
        <i class="fa-solid fa-hard-drive icon"></i>
        <input v-model="deviceId" type="text" required="required">
        <label for="deviceId">Device ID</label>
      </div>

      <div class="button">
        <button @click="login">Log in</button>
      </div>

    </fieldset>
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: "",
      password: "",
      deviceId: "",
    };
  },
  methods: {
    login() {
      // Check if the provided credentials match the predefined ones
      if (
        this.username === "mycowatch_user" &&
        this.password === "mycowatch_pass" &&
        this.deviceId === "1"
      ) {
        // Redirect to /home using Vue Router
        this.$router.push({ path: "/home" });
      } else {
        // Display an error message or handle failed login
        alert("Invalid credentials. Please try again.");
      }
    },
  },
};
</script>

<style>
  html, body, div, span, applet, object, iframe, h1, h2, h3, h4, h5, h6, p, blockquote, pre, a, abbr, acronym, address, big, cite, code, del, dfn, em, img, ins, kbd, q, s, samp, small, strike, strong, sub, sup, tt, var, u, i, center, dl, dt, dd, ol, ul, li, fieldset, form, label, legend, table, caption, tbody, tfoot, thead, tr, th, td, article, aside, canvas, details, embed,  figure, figcaption, footer, header, hgroup, menu, nav, output, ruby, section, summary, time, mark, audio, video {
	margin: 0; padding: 0; border: 0; font-size: 100%; font: inherit; vertical-align: baseline; font-family: 'Roboto', sans-serif;;
}

.body{
  height: 100vh;
  width: 100vw;
 
  /* background-color: gray; */
  display: flex;
  flex-direction: column;
  justify-content: center;

}
.fieldset{
  width: 18rem;
  height: 70vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 2rem 0 2rem 0;
  gap: 2rem; }

.heading2{
  font-size: 2rem;
 }

.inputbox{
  position: relative;
  }

.inputbox input{
  width: 18rem;
  height: 2.5rem;
  border-radius: 5px;
  background-color: #f3f5f7;
  border: none;
  padding: 0 0 0 40px
}

.inputbox input:valid ~ label, .inputbox input:focus ~ label
{
  color:gray;
  transform: translateX(10px) translateY(-10px);
  font-size: 10px;
}

.inputbox input:focus{
  outline: none
}

.inputbox label{
  position: absolute;
  left: 0;
  padding: 10px 0 10px 30px;
  pointer-events: none;
  transition: .4s;
}

.inputbox i{
  position: absolute;
  left: 0;
  padding: 10px 0 10px 10px;
 
}

.linkcontainer{
  margin:  0 2rem 0 2rem;
}

.inputbox .icon, .link{
  color:#01bf63;
}

.button{
  margin: 10px;
}
.button button{
  width: 18rem;
  height: 2.5rem;
  border-radius: 2rem;
  border: none;
  color: white;
  background-color: #01bf63;
}

.button button:hover{
  background-color: #06a357;
}

  @media screen and (max-width: 360px) {
    .nav{
          /* background-color: red; */
      }
  }
</style>
