<template>
   <div>
      <chart :options="chartOptionsBase" style=" height:430px;"/>
   </div>
</template>

<script>

   export default {
      name: 'visitors-pages',
      data() {
         return {
            loading: false,
            chartOptionsBase: {
               chart: {
                  type: 'column',
                  backgroundColor: null,
               },
               title: {
                  text: '',
               },
               xAxis: {
                  categories: [
                     'Ene',
                     'Feb',
                     'Mar',
                     'Abr',
                     'May',
                     'Jun',
                     'Jul',
                     'Ago',
                     'Sep',
                  ], //Answers
                  title: {
                     text: null,
                     style: {
                        color: '#f96353'
                     }
                  },
                  labels: {
                     style: {
                        fontSize: '12px',
                        color: '#333333',
                     }
                  },
                  gridLineWidth: 0,
                  crosshair: true
               },
               yAxis: {
                  gridLineWidth: 1,
                  min: 0,
                  title: {
                     text: ''
                  },
                  labels: {
                     overflow: 'justify'
                  }
               },
               credits: {
                  enabled: false
               },
               colors: ['#f96353'],
               series: []

            }
         }
      },
      mounted() {
         this.$nextTick(function () {
            this.init()
         })
      },
      methods: {
         async init() {
            this.getVisitorsAndPages()
         },
         getVisitorsAndPages() {
            return new Promise((resolve, reject) => {
               this.loading = true

               //Params
               let params = {
                  params: {
                     filter: {
                        visitorsAndPages: true,
                     }
                  }
               }
               this.$crud.index('apiRoutes.qanalytics.google', params).then(response => {
                  let data = response.data.visitors
                  this.chartOptionsBase.xAxis.dates = data.dates
                  this.chartOptions.series = [
                     {
                        name: 'Visitantes',
                        data: data.visitors
                     },
                     {
                        name:'pages',
                        data:data.pageViews
                     }
                  ]
                  resolve(true)//Resolve
               }).catch(error => {
                  this.$alert.error({message: this.$tr('ui.message.errorRequest'), pos: 'bottom'})
                  this.loading = false
                  reject(error)//Resolve
               })
            })
         },
      }
   }
</script>
