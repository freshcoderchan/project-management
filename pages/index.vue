<template>
  <div class="home">
    <nav class="navbar">
      <div class="container-fluid">
        <ul class="nav navbar-nav navbar-right">
          <li><NuxtLink to="/">PROJECTS</NuxtLink></li>
          <li><NuxtLink to="/login" @click.native="logout">LOGOUT</NuxtLink></li>
        </ul>
      </div>
    </nav>
    <div class="container projects">
      <div class="project" v-for="project in projects" :key="project.id">
        <div class="item logo" v-if="project.logo_url">
          <img src="https://wallazee.global.ssl.fastly.net/images/variant/20190320-f52dd4bb9bc480dd6714d14d35f0771777f8826be1d49530fa3db29-200.png">
        </div>
        <div class="item logo" v-else>
          <img src="~/assets/no-logo-available.gif">
        </div>
        <div class="item name">
          <NuxtLink to="/">{{ project.name }}</NuxtLink>
        </div>
        <div class="item active">
          <p v-if="project.active">Active</p>
          <p v-else>Inactive</p>
        </div>
        <div class="item time">
          <p>this time week <span>{{ project.spent_time_week }}</span></p>
          <p>this month <span>{{ project.spent_time_month }}</span></p>
          <p>total <span>{{ project.spent_time_all }}</span></p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
const cookie = require('js-cookie');

export default {
  async asyncData ({ store }) {
    let { data } = await axios({
        method: 'get',
        url: 'https://api.quwi.com/v2/projects-manage/index',
        headers: {
          'Authorization': `Bearer ${store.state.token}`
          // 'Authorization': 'Bearer 0931991d7c6308a242ffd8722850eaa6'
        },
        validateStatus: (status) => {
          return true;
        },
    })
    return { projects: data.projects }
  },
  fetch ({ store, redirect }) {
    if (!store.state.authUser) {
      return redirect('/')
    }
  },
  methods: {
    async logout() {
      try {
        // cookie.remove('token')
        await this.$store.dispatch('logout')
      } catch (e) {
        this.formError = e.message
      }
    }
  }
}
</script>

<style>
.container {
  font-family: sans-serif;
  width: 100%;
  padding: 25px 50px;
  margin-right: auto;
  margin-left: auto;
  margin-top: 100px;
}
@media (min-width: 1200px) {
  .container {
    max-width: 1140px;
  }
}
@media (min-width: 992px) {
  .container {
    max-width: 960px;
  }
}
@media (min-width: 768px) {
  .container {
    max-width: 720px;
  }
}
@media (min-width: 576px) {
  .container {
    max-width: 540px;
  }
}
.navbar {
  border-radius: 4px;
  position: relative;
  min-height: 50px;
  margin-bottom: 20px;
  border-bottom: 2px solid #959595;
  background-color: #fafafa;
}
.container-fluid {
  height: 50px;
  padding-right: 15px;
  padding-left: 15px;
  margin-right: auto;
  margin-left: auto;
}
.nav {
  list-style: none;
}
.navbar-right {
  float: right!important;
  margin-right: -15px;
}
.navbar-nav {
  margin: 0;
}
.navbar-nav>li {
  float: left;
}
.navbar-nav>li>a {
  padding-top: 15px;
  padding-bottom: 15px;
}
.nav>li>a {
  position: relative;
  display: block;
}
a {
  margin-right: 20px;
  font-size: 14px;
  color: #999;
  text-decoration: none;
  text-transform: uppercase;
  padding-top: 2px;
  padding-bottom: 2px;
  border-top: 1px solid transparent;
  border-bottom: 1px solid transparent;
  transition: color .25s;
  font-weight: 400;
  line-height: normal;
}
a:hover {
  color: #333;
}
.projects {
  margin-top: 20px !important;
}
.project {
  padding: 15px;
  background-color: #ffffff;
  border: 1px solid #f4f4f4;
  display: grid;
  grid-template-columns: 15% 30% 25% 30%;
}
.project .item {
  padding: 10px;
}
.project .logo img {
  width: 50px;
  height: 50px;
}
.project .name p {
  font-size: 12px;
}
.project .time {
  display: grid;
  grid-template-rows: 33% 33% 33%;
}
.project .time p {
  font-size: 10px;
  margin: 0;
}
.project .time p span {
  float: right;
}
</style>