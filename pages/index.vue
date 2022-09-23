<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card>
        <v-card-title class="headline">
          Pedometer
        </v-card-title>
        <v-card-text>
          Pedometer는 Github README.md 방문자 수를 보여주고 통계를 제공하는 서비스입니다.
        </v-card-text>
        <v-text-field
          label="URL을 입력해주세요."
          class="mx-4"
          v-model="url"
        ></v-text-field>
        <v-card-text>
          README.md에 아래의 텍스트를 삽입하세요.
        </v-card-text>
        <v-card-text>
          {{ pedometerUrl }}
        </v-card-text>
        <v-card-actions>
            <v-btn
              nuxt
              @click="copy"
            >
              텍스트 복사
            </v-btn>
            <v-btn
              nuxt
              :to="statisticPageURL"
            >
              통계 보기
            </v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      url: ''
    }
  },
  computed: {
    pedometerUrl() {
      if (this.url == '')
        return '![Pedometer]()'
      return `![Pedometer](https://f9039c3zbh.execute-api.us-east-1.amazonaws.com/dev/count/${Buffer.from(this.url).toString('base64')}/image.svg)`;
    },
    statisticPageURL() {
      return '/statistic/' + Buffer.from(this.url).toString('base64');
    }
  },
  methods: {
    copy() {
      if (navigator.clipboard && window.isSecureContext)
        navigator.clipboard.writeText(this.pedometerUrl());
      else
        alert('이 브라우저는 복사를 지원하지 않습니다.')
    }
  }
}
</script>
