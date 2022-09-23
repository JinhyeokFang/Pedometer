<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card>
        <v-card-title class="headline">
          {{ decodedURL }}
        </v-card-title>
        <canvas id="myChart" style="width:100%;max-width:600px"></canvas>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import dayjs from 'dayjs'
import Chart from 'chart.js/auto'

export default {
  name: 'StatisticPage',
  data() {
    return {
      visitData: []
    }
  },
  computed: {
    decodedURL() {
      return Buffer.from(this.$route.params.url, 'base64').toString();
    }
  },
  async mounted() {
    const result = await this.$axios.get(`https://f9039c3zbh.execute-api.us-east-1.amazonaws.com/dev/count/${this.$route.params.url}/all`);
    const lists = JSON.parse(Buffer.from(result.data, 'base64').toString());
    for (let rawData of lists) {
      const date = dayjs(rawData.createdAt);
      const dataIndex = this.visitData.findIndex(item => item.name == `${date.year()}-${date.month() + 1}-${date.date()}`);
      if (dataIndex == -1) {
        this.visitData.push({
          name: `${date.year()}-${date.month() + 1}-${date.date()}`,
          value: 1
        });
        continue;
      }
      this.visitData[dataIndex].value += 1;
    }

    const xValues = [];
    const yValues = [];

    for (let data of this.visitData) {
      xValues.push(data.name);
      yValues.push(data.value);
    }


    new Chart("myChart", {
      type: "bar",
      data: {
        labels: xValues,
        datasets: [{
          label: 'visit',
          backgroundColor: '#ffffff',
          data: yValues
        }]
      }
    });
  }
}
</script>
