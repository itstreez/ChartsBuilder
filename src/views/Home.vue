/* eslint-disable */
<template>
  <html>
    <head>
      <meta name="viewport" content="width=device-width , initial-scale=1.0" />
    </head>
    <body>
      <div class="banner">
        <div class="navbar">
          <img src="../assets/logo.png" class="logo" />
          <ul>
            <li>
              <router-link id="home" to="/">Home</router-link>
            </li>
            <li>
              <router-link id="templates" to="/templates">Templates</router-link>
            </li>
          </ul>
        </div>
        <div class="content">
          <h1>CREATE POWERFUL CHARTS</h1>
          <p>Customize your own charts , choose your color , type , size ..</p>
        <div>
          <button> <span> </span><router-link to="/templates">TAKE ME THERE</router-link>
</button>
        </div>
        </div>
      </div>
    </body>
  </html>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'Home'
}
</script>
<style scoped>
.logo {
  width: 120px;
  cursor: pointer;
  position: relative;
  bottom: 100px;
}
body{
  height:500px;

}
* {
  margin: 0;
  padding: 0;
  font-family: sans-serif;
}
button{
  width: 200px;
  padding: 15px 0;
  text-align: center;
  margin: 20px 10px;
  border-radius: 25px;
  font-weight: bold;
  border: 2px solid #b0c4de;
  background: transparent;
  color: white;
  cursor:  pointer;
  position: relative;
}
.navbar {
  width: 85%;
  margin: auto;
  display: flex;
  padding: 120px 0;
  align-items: center;
  justify-content: space-between;
}
.navbar ul li {
  list-style: none;
  display: inline-block;
  margin: 0 20px 220px ;
  position: relative;
}
.navbar ul li a {
  font-family :Georgia, 'Times New Roman', Times, serif;
  text-decoration: none;
  color: #fff;
  text-transform: uppercase;
}
.navbar ul li::after {
  content: "";
  height: 3px;
  width: 0;
  background: #b0c4de;
  position: absolute;
  left: 0;
  bottom: -10px;
  transition: 0.5s;
}
.navbar ul li:hover::after {
  width: 100%;
}
.banner {
  position: relative;
  width: 100%;
  height:100vh ;
  background-image: url(../assets/y.jpeg.jpg);
  background-size: cover;
  background-position: center;
}
.content {
  width: 100%;
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  text-align: center;
  color: #fff;
}
.content h1 {
  font-size: 70px;
  margin-top: 80px;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}
.content p {
  font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
  margin: 20px auto;
  font-weight: 100;
  line-height: 25px;
  font-size: 25px;
}
span{
  background: #b0c4de;
  height: 100%;
  width: 0;
  border-radius: 25px;
  position: absolute;
  left: 0;
  bottom: 0;
  z-index: -1;
  transition: 0.5s;
}
button:hover span{
width: 100%;
}
button:hover {
  border: none;
}
</style>
