<template>
  <div>
    <b-navbar toggleable="lg" type="dark" variant="info">
      <b-navbar-brand href="#">Torre</b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
          <b-nav-item href="#">User</b-nav-item>
        </b-navbar-nav>

        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
          <b-nav-form>
            <b-form-input size="sm" class="mr-sm-2" placeholder="Search">
              <b-iconstack font-scale="5">
                <b-icon stacked icon="circle-fill" variant="info"></b-icon>
                <b-icon stacked icon="search-fill" scale="0.5" variant="white"></b-icon>
                <b-icon stacked icon="circle" variant="danger"></b-icon>
              </b-iconstack>
            </b-form-input>
          </b-nav-form>
          <b-button size="sm" class="my-2 my-sm-0" type="submit">Login</b-button>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
    <User />

  </div>
</template>

<script>
import { initializeApp } from "firebase/app";
export default {
  created() {
    this.Default()
  },
  methods: {
    Default() {
      const firebaseConfig = {
        apiKey: "AIzaSyARR5qT1pXJyLnhGP8_7V7bt2by5TgTd7U",
        authDomain: "torre-23ba1.firebaseapp.com",
        projectId: "torre-23ba1",
        storageBucket: "torre-23ba1.appspot.com",
        messagingSenderId: "721745215397",
        appId: "1:721745215397:web:ea295c759dc987ee9df24c"
      };

      const app = initializeApp(firebaseConfig);
    }
  }
}
</script>
