<template>
<v-container fluid>

  <v-toolbar flat>
    <v-toolbar-title class="title mx-auto">REALTIME DATA GLOBAL</v-toolbar-title>
    <!-- <v-spacer></v-spacer>
    <div>
      <small>Terakhir diupdate: {{ worldStatistic.statistic_taken_at }}</small>
    </div> -->
  </v-toolbar>

  <v-row>
    <v-col cols="12" md="4">
      <v-card color="blue-grey lighten-1" dark>
        <div class="d-flex flex-no-wrap justify-space-between">
          <div>
            <v-card-title>
              {{ worldStatistic.total_cases }} Orang
            </v-card-title>
            <v-card-subtitle>
              Jumlah Kasus
            </v-card-subtitle>
          </div>
          <v-icon class="pa-3" large dark>mdi-alarm-light-outline</v-icon>
        </div>
      </v-card>
    </v-col>
    <v-col cols="12" md="4">
      <v-card color="red lighten-1" dark>
        <div class="d-flex flex-no-wrap justify-space-between">
          <div>
            <v-card-title>
              {{ worldStatistic.total_deaths }} Orang
            </v-card-title>
            <v-card-subtitle>
              Jumlah Meninggal
            </v-card-subtitle>
          </div>
          <v-icon class="pa-3" large dark>mdi-bed-outline</v-icon>
        </div>
      </v-card>
    </v-col>
    <v-col cols="12" md="4">
      <v-card color="teal lighten-1" dark>
        <div class="d-flex flex-no-wrap justify-space-between">
          <div>
            <v-card-title>
              {{ worldStatistic.total_recovered }} Orang
            </v-card-title>
            <v-card-subtitle>
              Total Pulih
            </v-card-subtitle>
          </div>
          <v-icon class="pa-3" large dark>mdi-human</v-icon>
        </div>
      </v-card>
    </v-col>
    <small class="pl-4">Terakhir diupdate: {{ worldStatistic.statistic_taken_at }}</small>
  </v-row>

  <v-row>
    <v-col cols="12">
      <v-card>
        <v-card-title>
            <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="Search"
            single-line
            hide-details
            ></v-text-field>
        </v-card-title>
        <v-data-table
            :headers="headers"
            :items="items"
            :search="search"
        ></v-data-table>
      </v-card>
    </v-col>
    <!-- <v-col cols="7" xs="12" sm="7">
      
    </v-col> -->
  </v-row>

  <v-toolbar flat>
    <v-toolbar-title class="title mx-auto">REALTIME DATA INDONESIA</v-toolbar-title>
    <!-- <v-spacer></v-spacer>
    <div>
      <small>Terakhir diupdate: {{ worldStatistic.statistic_taken_at }}</small>
    </div> -->
  </v-toolbar>

  <v-row>
    <v-col cols="12" md="4">
      <v-card color="blue-grey lighten-1" dark>
        <div class="d-flex flex-no-wrap justify-space-between">
          <div>
            <v-card-title>
              {{ indonesiaStatistic.positif }} Orang
            </v-card-title>
            <v-card-subtitle>
              Positif
            </v-card-subtitle>
          </div>
          <v-icon class="pa-3" large dark>mdi-alarm-light-outline</v-icon>
        </div>
      </v-card>
    </v-col>
    <v-col cols="12" md="4">
      <v-card color="red lighten-1" dark>
        <div class="d-flex flex-no-wrap justify-space-between">
          <div>
            <v-card-title>
              {{ indonesiaStatistic.meninggal }} Orang
            </v-card-title>
            <v-card-subtitle>
              Meninggal
            </v-card-subtitle>
          </div>
          <v-icon class="pa-3" large dark>mdi-bed-outline</v-icon>
        </div>
      </v-card>
    </v-col>
    <v-col cols="12" md="4">
      <v-card color="teal lighten-1" dark>
        <div class="d-flex flex-no-wrap justify-space-between">
          <div>
            <v-card-title>
              {{ indonesiaStatistic.sembuh }} Orang
            </v-card-title>
            <v-card-subtitle>
              Sembuh
            </v-card-subtitle>
          </div>
          <v-icon class="pa-3" large dark>mdi-human</v-icon>
        </div>
      </v-card>
    </v-col>
  </v-row>

  <v-row>
    <v-col cols="12" md="6">
      <v-card>
        <v-card-title>
            <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="Search"
            single-line
            hide-details
            ></v-text-field>
        </v-card-title>
        <v-data-table
            :headers="headersProvince"
            :items="provinceIndonesiaData"
            :search="search"
        ></v-data-table>
      </v-card>
    </v-col>
    <v-col cols="12" md="6">
      <Chart></Chart>
    </v-col>
  </v-row>

</v-container>
</template>

<script>

import Chart from '@/components/Chart.vue'

export default {
  name: 'Home',
  components: {
    Chart
  },
  data: () => ({
    search: '',
    headers: [
      {
        text: 'Negara',
        align: 'start',
        value: 'country_name',
      },
      { text: 'Kasus', value: 'cases' },
      { text: 'Meninggal', value: 'deaths' },
      { text: 'Pulih', value: 'total_recovered' },
      { text: 'Kematian Baru', value: 'new_deaths' },
      { text: 'Kasus Baru', value: 'new_cases' },
      { text: 'Kritis', value: 'serious_critical' },
      { text: 'Kasus Aktif', value: 'active_cases' },
    ],
    headersProvince: [
      {
        text: 'Provinsi',
        align: 'start',
        value: 'attributes.Provinsi',
      },
      { text: 'Positif', value: 'attributes.Kasus_Posi' },
      { text: 'Sembuh', value: 'attributes.Kasus_Semb' },
      { text: 'Meninggal', value: 'attributes.Kasus_Meni' },
    ],
    items: [],
    worldStatistic: [],
    indonesiaStatistic: [],
    provinceIndonesiaData: [],
  }),
  created(){
    // World Statistic
    this.axios.get('https://coronavirus-monitor.p.rapidapi.com/coronavirus/worldstat.php', 
    {
    headers: {"content-type":"application/octet-stream",
    "x-rapidapi-host":process.env.VUE_APP_RAPIDAPI_HOST,
    "x-rapidapi-key":process.env.VUE_APP_RAPIDAPI_KEY}
    })
    .then((response) => {
      this.worldStatistic = response.data;
    })
    .catch((error) => {
      console.log(error);
    });

    // Case by Country use for data table
    this.axios.get('https://coronavirus-monitor.p.rapidapi.com/coronavirus/cases_by_country.php', 
    {
    headers: {"content-type":"application/octet-stream",
    "x-rapidapi-host":process.env.VUE_APP_RAPIDAPI_HOST,
    "x-rapidapi-key":process.env.VUE_APP_RAPIDAPI_KEY
    }
    })
    .then((response) => {
      this.items = response.data.countries_stat;
    })
    .catch((error) => {
      console.log(error);
    });

    // Indonesia Statistic
    this.axios.get('https://covid19.finlabs.dev/finlabs-proxy.php?method=GET&url=https://api.kawalcorona.com/indonesia')
    .then((response) => {
      this.indonesiaStatistic = response.data[0];
    })
    .catch((error) => {
      console.log(error);
    });

    // Indonesia Data by Province
    this.axios.get('https://covid19.finlabs.dev/finlabs-proxy.php?method=GET&url=https://api.kawalcorona.com/indonesia/provinsi')
    .then((response) => {
      this.provinceIndonesiaData = response.data
    })
    .catch((error) => {
      console.log(error);
    });
  },
  computed: {
    // cardCase: function() {
    //   let data = this.worldStatistic.total_cases + ' Orang'
    //   return data;
    // }
  },
}
</script>
