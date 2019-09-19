<template>
  <v-container>
    <v-layout row wrap align-center>
      <v-flex xs12>
        <v-img
          :src="require('../assets/logo.svg')"
          class="my-3"
          contain
          height="200"
        ></v-img>
      </v-flex>

      <v-flex xs12>
        <div align="center">
          <h1 class="display-2 font-weight-bold mb-3">电子病历</h1>
        </div>
      </v-flex>

      <v-flex xs12>
        <div align="center">
          <h2 class="headline font-weight-bold mb-3">登录</h2>
        </div>
        <v-layout justify-center>
          <v-form v-model="valid">
            <v-container>
              <v-layout>
                <v-flex xs12 md4>
                  <v-text-field
                    v-model="username"
                    :rules="nameRules"
                    :counter="4"
                    label="用户编码"
                    required
                  ></v-text-field>
                </v-flex>

                <v-flex xs12 md4>
                  <v-text-field
                    v-model="passwd"
                    :rules="passRules"
                    label="密码"
                    required
                    type="password"
                  ></v-text-field>
                </v-flex>

                <v-flex xs12 md4>
                  <v-btn color="success" :disabled="!valid" @click="loginSubmit"
                    >登录</v-btn
                  >
                </v-flex>
              </v-layout>
            </v-container>
          </v-form>
        </v-layout>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    valid: false,
    username: "8888",
    passwd: "eastwill",
    loginmsg: "",
    nameRules: [
      v => !!v || "用户编码不能为空",
      v => v.length >= 3 || "用户编码长度必须超过3个字符"
    ],
    passRules: [
      v => !!v || "用户密码不能为空",
      v => v.length >= 6 || "用户密码长度必须超过6个字符"
    ]
  }),
  created() {
    localStorage.removeItem("user");
  },
  methods: {
    loginSubmit() {
      // window.alert("username="+this.username+" passwd="+this.passwd+" |"+process.env.VUE_APP_LOGIN_URL);
      let tinstr =
        '{"opid":"' +
        this.username +
        '","opname":"","oppass":"' +
        this.passwd +
        '","opstatus":""}';
      let sel = this;
      fetch(process.env.VUE_APP_LOGIN_URL, {
        method: "post",
        //credentials: 'include',
        body: tinstr,
        headers: {
          Accept: "text/html",
          "Content-Type": "application/json"
        }
      })
        .then(function(response) {
          if (response.ok) {
            //window.alert('ok');
          } else {
            window.alert("登录失败error");
            sel.loginmsg = "登录失败" + response.err;
          }
          return response.json();
        })
        .then(function(data) {
          //window.alert("data="+JSON.stringify(data)); // this will be a string
          let topstatus = data.opstatus;
          if (topstatus === "200") {
            localStorage.setItem("user", JSON.stringify(data));
            // this.loginmsg = "";
            //sel.$parent.$router.push({ path: "/" });
            sel.$parent.$router.push({ path: "/" });
          } else {
            //登录失败
            window.alert("登录失败!\n");
            sel.loginmsg = "登录失败";
            localStorage.removeItem("user");
          }
        })
        .catch(function(err) {
          window.alert("error=" + err);
        });
    }
  }
};
</script>

<style></style>
