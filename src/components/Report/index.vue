<template>
<v-container>
  <v-layout row wrap>
    <v-flex xs12 sm10 md8 offset-sm1 offset-md2>
      <v-card>
        <h2 class="headline text-xs-center pt-4">Daftar Laporan</h2>
        <v-card-title>
          <v-btn primary dark>Tambah Laporan</v-btn>

          <v-spacer></v-spacer>
          <v-text-field append-icon="search" label="Search" single-line hide-details v-model="search"></v-text-field>
        </v-card-title>
        <v-data-table v-bind:headers="headers" v-bind:items="reports" v-bind:search="search">
          <template slot="items" scope="props">

              <td>{{ props.item.title }}</td>
              <td>{{ props.item.severity }}</td>
              <td>{{ props.item.reporter }}</td>
              <td>{{ props.item.reportDate | | moment("from", "now") }}</td>
              <td style="display: inline-flex">
                <v-btn icon class="green--text" :to="{ name: 'DetailReport', params: { id:props.item.id }}">
                  <v-icon>visibility</v-icon>
                </v-btn>
                <v-btn icon class="blue--text">
                  <v-icon>border_color</v-icon> Validasi
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
export default {
  data () {
    return {
      max25chars: (v) => v.length <= 25 || 'Input too long!',
      tmp: '',
      search: '',
      pagination: {},
      headers: [{
          text: 'Title',
          align: 'left',
          sortable: false,
          value: 'title'
        },
        {
          text: 'Saverity',
          value: 'severity'
        },
        {
          text: 'Reporter',
          value: 'reporter'
        },
        {
          text: 'When',
          value: 'reportDate'
        }
      ],
      items: []
    }
  },
  created() {
    console.log('jalan')
  },
  computed: {
    reports() {
      return this.$store.getters.loadedReports
    }
  }
}
</script>
