<template>
<div>
    <LineChart
        :chart-data="dataCollection"
        :options="options"
    ></LineChart>
</div>
</template>

<script>
import LineChart from './LineChart.js'

export default {
    components: {
        LineChart
    },
    data: () => ({
        dataCollection: null,
        loaded: false,
        dataIndonesiaTime: [],
        time: null,
        death: null,
        recovered: null,
        options: {
            responsive: true,
            scales: {
            yAxes: [{
                ticks: {
                    stepSize: 150
                }
            }]
    }
        }
    }),
    mounted(){
        this.fillData()
    },
    created(){
        // Indonesia Data by Province
        this.axios.get('https://coronavirus-monitor.p.rapidapi.com/coronavirus/cases_by_particular_country.php', 
        {
            headers: {
                "content-type":"application/octet-stream",
                "x-rapidapi-host":process.env.VUE_APP_RAPIDAPI_HOST,
                "x-rapidapi-key":process.env.VUE_APP_RAPIDAPI_KEY
        }, params: {
                "country":"indonesia"
            }
        })
        .then((response) => {
            
            this.dataIndonesiaTime = response.data.stat_by_country;

            let count = this.dataIndonesiaTime.length;        
            var formatData = [];

            for (let i = 0; i < count-1; i++) {
                if ( this.dataIndonesiaTime[i].record_date.split(' ')[0] != this.dataIndonesiaTime[i + 1].record_date.split(' ')[0] ) {
                    formatData.push(this.dataIndonesiaTime[i])
                } 
            }

            let countTmp = formatData.length;

            if (formatData[countTmp-1].record_date.split(' ')[0] != this.dataIndonesiaTime[count-1]) {
                formatData.push(this.dataIndonesiaTime[count-1])
            }

            let formatDateChart = [];
            let formatDeathChart = [];
            let formatRecoveredChart = [];
            let formatCaseChart = [];
            formatData.forEach((val) => {
                let date = new Date(val.record_date.split(' ')[0]);
                let day = new Intl.DateTimeFormat('en', {day: '2-digit'}).format(date);
                let month = new Intl.DateTimeFormat('en', {month: 'short'}).format(date);
                let formatDate = day + ' ' + month;
                formatDateChart.push(formatDate);
                formatDeathChart.push(val.total_deaths);
                formatRecoveredChart.push(val.total_recovered);
                formatCaseChart.push(val.total_cases.replace(',', ''));
            });

            this.time = formatDateChart;
            this.death = formatDeathChart;
            this.recovered = formatRecoveredChart;
            this.case = formatCaseChart;
            this.fillData()

        })
        .catch((error) => {
            console.log(error);
        });
    },
    methods: {
        fillData(){ 
            this.dataCollection = {
                labels: this.time,
                datasets: [
                    {
                        label: 'Meninggal',
                        backgroundColor: 'rgba(255,0,0,0.2)',
                        borderColor: 'red',
                        pointBackgroundColor: 'red',
                        borderWidth: 1,
                        pointBorderColor: 'white',
                        data: this.death,
                    },
                    {
                        label: 'Sembuh',
                        backgroundColor: 'rgba(0, 255, 0, 0.2)',
                        borderColor: 'green',
                        pointBackgroundColor: 'green',
                        borderWidth: 1,
                        pointBorderColor: 'white',
                        data: this.recovered,
                    },
                    {
                        label: 'Kasus',
                        backgroundColor: 'rgba(255, 222, 103, 0.61)',
                        borderColor: 'orange',
                        pointBackgroundColor: 'orange',
                        borderWidth: 1,
                        pointBorderColor: 'white',
                        data: this.case,
                    },
                ]
            }
        }
    },
    computed: {
        dataForChart: function() {
            let count = this.dataIndonesiaTime.length;        
            let tmpData = [];

            for (let i = 0; i < count-1; i++) {
                if ( this.dataIndonesiaTime[i].record_date.split(' ')[0] != this.dataIndonesiaTime[i + 1].record_date.split(' ')[0] ) {
                    tmpData.push(this.dataIndonesiaTime[i])
                } 
            }

            // let countTmp = tmpData.length;

            // if (tmpData[countTmp-1].record_date.split(' ')[0] != this.dataIndonesiaTime[count-1]) {
            //     tmpData.push(this.dataIndonesiaTime[count-1])
            // }

            return tmpData;
        },
        dateForChart: function() {
            let tmpData = [];

            this.dataForChart.forEach((val) => {
                let date = new Date(val.record_date.split(' ')[0]);
                let day = new Intl.DateTimeFormat('en', {day: '2-digit'}).format(date);
                let month = new Intl.DateTimeFormat('en', {month: 'short'}).format(date);
                let formatDate = day + ' ' + month;
                tmpData.push(formatDate);
            });

            return tmpData;
        }
    }
}
</script>