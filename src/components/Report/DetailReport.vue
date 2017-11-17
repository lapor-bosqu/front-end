<template>
<v-container>
  <v-layout row wrap>
    <v-flex xs12 sm10 md10 offset-sm1 offset-md1>
        <v-card>
        <h2 class="headline text-xs-center pt-4">Report Detail</h2>
        <h4 class="title text-xs-center pt-4" v-if="report">{{ report.title}}</h4>
        <v-card-title>
          <v-btn primary dark>Convert to Jira Issue</v-btn>

          <v-spacer></v-spacer>
        </v-card-title>
    
      </v-card>
    </v-flex>
  </v-layout>
</v-container>
</template>
<script>
  export default {
    data () {
      return {
        report: {},
      }
    },
    mounted() {
      this.fetchDetailReport()
    },
    methods: {
      fetchDetailReport() {
        this.axios({
          method: 'get',
          url: this.$store.getters.apiUrl + '/report/' + this.$route.params.id
        }).then((report) => {
          this.report = report.data

        }).catch(err => {
          console.log('error when get detail report : ', err);
        })
      }
    }
  }
</script>