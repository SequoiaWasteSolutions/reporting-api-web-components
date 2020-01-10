<style>
    .my-3 {
        margin-top: 0.75rem;
        margin-bottom: 0.75rem;
    }

    .p-1 {
        padding: 0.25rem;
    }

    .p-2 {
        padding: 0.5rem;
    }

    .font-medium {
        font-weight: 500;
    }

    .text-xl {
        font-size: 1.25rem;
    }
</style>

<template>
    <div class="p-2">
        <h3 class="my-3 p-1 text-blue-500 font-medium text-xl">Number of Invoices Audited</h3>

        <div id="audited-invoices-container"></div>
    </div>
</template>

<script>
    import Highcharts from 'highcharts';

    export default {
        props: {
            numberOfInvoicesWithErrors: {
                type: Number,
                required: true
            },

            numberOfInvoicesWithoutErrors: {
                type: Number,
                required: true
            }
        },

        mounted() {
            this.drawChart();
        },

        methods: {
            drawChart() {
                // Create the chart
                Highcharts.chart('audited-invoices-container', {
                    chart: {
                        type: 'pie'
                    },

                    title: {
                        text: null
                    },

                    yAxis: {
                        title: {
                            text: 'Auditing Error Rates'
                        }
                    },

                    plotOptions: {
                        pie: {
                            shadow: false,
                        }
                    },

                    tooltip: {
                       formatter: function() {
                           return `<strong>${this.point.name}</strong>: ${this.y}`;
                       }
                    },

                    series: [{
                        name: 'Invoice Audits',
                        data: this.seriesData,
                        size: '60%',
                        innerSize: '20%',
                        showInLegend:true,
                        dataLabels: {
                            enabled: false
                        }
                    }],
                });
            }
        },

        watch: {
            numberOfInvoicesWithErrors: function(val) {
                this.drawChart();
            },

            numberOfInvoicesWithoutErrors: function(val) {
                this.drawChart();
            },
        },

        computed: {
            seriesData() {
                return [
                    ['Errors', this.numberOfInvoicesWithErrors],
                    ['Correct', this.numberOfInvoicesWithoutErrors]
                ];
            }
        }
    }
</script>