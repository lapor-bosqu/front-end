<template>
<v-container>
  <v-layout row wrap>
    <v-flex xs12 sm10 md10 offset-sm1 offset-md1>
      <v-card>
        <h2 class="headline text-xs-center pt-4">Daftar Laporan</h2>
        <v-card-title>
          <v-btn primary dark>Tambah Laporan</v-btn>

          <v-spacer></v-spacer>
          <v-text-field append-icon="search" label="Search" single-line hide-details v-model="search"></v-text-field>
        </v-card-title>
        <v-data-table v-bind:headers="headers" v-bind:items="items" v-bind:search="search" v-bind:pagination.sync="pagination">
          <template slot="items" scope="props">

              <td>{{ props.item.title }}</td>
              <td>{{ props.item.platform }}</td>              
              <td>{{ props.item.saverity }}</td>
              <td>{{ props.item.reporter }}</td>
              <td>{{ props.item.status }}</td>
              <td>{{ props.item.createdAt | | moment("from", "now") }}</td>
              <td style="display: inline-flex">
                <v-btn icon class="green--text" :to="{ name: 'DetailReport', params: { id:props.item.id }}">
                  <v-icon>visibility</v-icon>
                </v-btn>
                <v-btn primary v-if="isConvertedToJira(props.item)" @click="convertToJira(props.item.id)">
                  Conver to Jira
                </v-btn>
                <v-btn outline v-else @click="goToJiraIssue(props.item.jiraIssue.key)">
                  go to Jira Issue
                </v-btn>

                <v-btn icon class="pink--text" @click.native.stop="confirmDelete(props.item)">
                  <v-icon>delete</v-icon>
                </v-btn>
              </td>
            </template>
          <template slot="pageText" scope="{ pageStart, pageStop }">
              From {{ pageStart }} to {{ pageStop }}
            </template>
        </v-data-table>
      </v-card>
    </v-flex>
  </v-layout>
</v-container>
</template>

<script>
const apiHost = 'https://udin.us/lapor'
const jiraURLBase = 'https://lapor-bosqu.atlassian.net'

export default {
  data () {
    return {
      max25chars: (v) => v.length <= 25 || 'Input too long!',
      tmp: '',
      search: '',
      pagination: {
          sortBy: 'reportDate',
          descending: true
        },
      headers: [{
          text: 'Title',
          align: 'left',
          sortable: false,
          value: 'title'
        },
        {
          text: 'Platform',
          value: 'platform'
        },
        {
          text: 'Saverity',
          value: 'saverity'
        },
        {
          text: 'Reporter',
          value: 'reporter'
        },
        {
          text: 'Status',
          value: 'status'
        },
        {
          text: 'When',
          value: 'createdAt'
        },
        {
          text: 'Action',
          value: ''
        }
      ],
      items: []
    }
  },
  created() {
    console.log('jalan')
    this.fetchReports()
  },
  methods: {
    fetchReports() {
      this.axios({
        url: apiHost + '/reports',
        method: 'get',
      }).then(reports => {
        this.items = reports.data
      }).catch(err => {
        console.log('err when fetch reports : ', err)
      })
    },
    isConvertedToJira(value) {
      return value.jiraIssue == undefined ? true : false;
    },
    goToJiraIssue(issueKey) {
      console.log('goto jira issue')
      window.open(jiraURLBase + '/browse/' + issueKey, '_blank');
    },
    convertToJira(id) {
      console.log('fired up')
      this.axios({
        url: apiHost + '/report/' + id + '/convert-to-jira',
        method: 'get'
      }).then(response => {
        if(response.data.success){
          this.fetchReports()
        } else {
          console.log('error when trying to convert into Jira issue : ', response.data.message)
        }
      }).catch(err => {
        console.log('error when conver to jira : ', err)
      })
    }
  },
  computed: {
    reports() {
      return this.$store.getters.loadedReports
    },
  }
}
</script>
