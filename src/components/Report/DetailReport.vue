<template>
<v-container>
  <v-layout row wrap>
    <v-flex xs12 sm12 md10 offset-sm1 offset-md1>
        <v-card>
        <h2 class="headline text-xs-center pt-4">Report Detail</h2>
        <h4 class="title text-xs-center pt-4" v-if="report">{{ report.title}}</h4>
        <v-card-title>
            <v-spacer></v-spacer>
        <v-btn primary v-if="isConvertedToJira(report)" @click="convertToJira(props.item.id)">
            Conver to Jira
        </v-btn>
        <v-btn outline v-else @click="goToJiraIssue(report.jiraIssue.key)">
            go to Jira Issue
        </v-btn>
        </v-card-title>
        <v-layout row>
            <v-flex xs4>
                <div class="pa-3">
                    <img :src="'https://udin.us/lapor/images/' + report.screenshot" alt="">
                </div>
            </v-flex>
            <v-flex xs4>

                <v-layout row class="pa-4">
                    <v-flex xs6>
                        <p class="subheading">Squad </p>
                        <p class="subheading">Status </p>
                        <p class="subheading">Saverity </p>
                        <p class="subheading">Reported At </p>
                        <p class="subheading">Reporter </p>
     
                    </v-flex>
                    <v-flex xs6>
                        <p class="subheading">{{report.screenReferer}} </p>
                        <p class="subheading">{{report.status}} </p>
                        <p class="subheading">{{report.saverity}} </p>
                        <p class="subheading">{{report.createdAt | moment("from", "now") }} </p>
                        <p class="subheading">{{report.reporter}} </p>
                        
                    </v-flex>
                </v-layout>
                <div class="pa-3">
                    <h2 class="title">Repro</h2>
                    <p>Lorem ipsum dolor, sit amet consectetur adipisicing elit. Possimus accusamus aliquid dolorem sunt dolores! Ipsum ipsam officiis, fugit minus itaque accusamus neque doloribus hic tenetur, enim qui recusandae illum repellat?</p>
                </div>
            </v-flex>
            <v-flex xs4>
                <v-layout row class="pa-4">
                    <v-flex xs6>
                        <p class="subheading">Platform </p>
                        <p class="subheading">OS Version </p>
                        <p class="subheading">App Version </p>
                        <p class="subheading">Device Manufacture </p>
                        <p class="subheading">Device Type </p>
                        <p class="subheading">Device ID </p>
                        <p class="subheading">User ID </p>
                        <p class="subheading">Browser Version </p>
                        <p class="subheading">User Agent </p>
                    </v-flex>
                    <v-flex xs6>
                        <p class="subheading">{{report.platform}} </p>
                        <p class="subheading">{{report.osVersion}} </p>
                        <p class="subheading">{{report.appVersion}} </p>
                        <p class="subheading">{{report.deviceManufacturer}} </p>
                        <p class="subheading">{{report.deviceType}} </p>
                        <p class="subheading">{{report.deviceId}} </p>
                        <p class="subheading">{{report.userId}} </p>
                        <p class="subheading">{{report.browserVersion}} </p>
                        <p class="subheading">{{report.userAgent}} </p>
 
                    </v-flex>
                </v-layout>
            </v-flex>

        </v-layout>
      </v-card>
    </v-flex>
  </v-layout>
</v-container>
</template>
<script>
const jiraURLBase = 'https://lapor-bosqu.atlassian.net'

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
      },
        isConvertedToJira(value) {
            return this.report.jiraIssue == undefined ? true : false;
        },
        goToJiraIssue(issueKey) {
            console.log('goto jira issue')
            window.open(jiraURLBase + '/browse/' + issueKey, '_blank');
        },
    }
  }
</script>