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
          {{ pedometerUrl }}
        </v-card-text>
        <v-card-actions>
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
        return 'URL을 입력해주세요.'
      return `https://f9039c3zbh.execute-api.us-east-1.amazonaws.com/dev/count/${Buffer.from(this.url).toString('base64')}/image.svg`;
    },
    statisticPageURL() {
      return '/statistic/' + Buffer.from(this.url).toString('base64');
    }
  }
}
</script>
