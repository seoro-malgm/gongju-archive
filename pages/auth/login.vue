<template>
  <div class="min-vh-100">
    <b-container
      fluid
      class="py-5 my-5 w-100 d-flex flex-column align-items-center justify-content-center"
    >
      <b-row align-h="center" class="w-100">
        <b-col cols="12" md="7" xl="5">
          <b-card class="w-100">
            <b-card-header
              header-bg-variant="white"
              header-border-variant="white"
              header-tag="header"
            >
              <b-card-title title-tag="h1" class="text-center">
                로그인
              </b-card-title>
            </b-card-header>
            <b-card-body>
              <b-btn
                variant="naver"
                class="btn-naver w-100 py-2"
                @click="loginWithNaver"
                :disabled="disabled"
              >
                <img :src="require('@/assets/naver.svg')" alt="네이버 심볼" />
                <span class="ml-2">Naver로 로그인</span>
              </b-btn>
              <div class="mt-4 text-center">
                <small>이미 가입된 아이디가 있다면?</small>
                <b-btn
                  variant="outline-light ml-2"
                  :to="{ name: 'auth-signup' }"
                  >회원가입</b-btn
                >
              </div>
            </b-card-body>
          </b-card>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
// import firebase from '~/plugins/firebase'

export default {
  layout: "default",
  name: "auth-login",
  head() {
    return {
      title: `신물결 | 로그인`,
      script: [
        {
          src: "https://static.nid.naver.com/js/naveridlogin_js_sdk_2.0.2-nopolyfill.js",
        },
      ],
    };
  },
  props: {
    auth: {
      type: Object,
      default: null,
    },
  },
  data() {
    return {
      disabled: false,
    };
  },
  computed: {
    validate() {
      return false;
    },
    redirect() {
      return this.$route.query.redirect;
    },
  },
  mounted() {
    if (this.auth) {
      this.$router.push("/");
    }
  },
  methods: {
    // async login(info) {
    //   const { login } = this.$firebase();
    //   try {
    //     const { data, token } = await login(info);
    //     console.log("data, token:", data, token);
    //     if (data) {
    //       // 세션스토리지에 저장
    //       sessionStorage.setItem(process.env.TOKEN_NAME, token.accessToken);
    //       // store에 저장
    //       this.$store.dispatch("setUser", data);
    //       this.$router.push("/");
    //     }
    //   } catch (error) {
    //     console.error("error:", error);
    //   }
    // },
    loginWithNaver() {
      const naverLogin = new naver.LoginWithNaverId({
        clientId: process.env.NAVER_CLIENT_ID,
        callbackUrl: `${window.location.origin}/auth/callback/naver${
          this.redirect ? `?redirect=${this.redirect}` : ""
        }`,
        callbackHandle: true,
      });
      naverLogin.init();
      // 로그인 진행
      naverLogin.reprompt();
      // naverLogin.getLoginStatus(async (status) => {
      //   if (!status) {
      //     window.toast("죄송합니다. 현재 오류로 인해 로그인이 불가합니다.");
      //     this.disabled = true;
      //     return;
      //   }
      //   if (status) {
      //     this.login(naverLogin.user);
      //   }
      //   // }
      // });
    },
  },
};
</script>

<style lang="scss" scoped>
.btn-naver {
  display: flex;
  align-items: center;
  justify-content: center;
  img {
    width: 24px;
    height: 24px;
  }
  span {
    font-size: 22px;
  }
}
</style>
