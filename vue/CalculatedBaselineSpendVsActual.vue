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

    .text-blue-500 {
        color: #4299e1;
    }
</style>

<template>
    <div class="p-2">
        <h3 class="my-3 p-1 text-blue-500 font-medium text-xl">
            Core vs Baseline Waste Spend
        </h3>

        <p class="my-3 text-gray-700 text-sm">
            All commercial & rolloff service charges (and medical, if applicable), excluding temporary containers, excess/extra pickup charges, and Sequoia's fees.
        </p>

        <div class="flex my-3">
            <div id="core-vs-baseline-spend-container">

            </div>
        </div>
    </div>
</template>

<script>
    import Highcharts from 'highcharts';

    Highcharts.setOptions({
        lang: {
          decimalPoint: '.',
          thousandsSep: ','
        }
    });

    export default {
        props: {
            wasteSpend: {
                type: Array,
                required: true
            },

            calculatedBaselineSpend: {
                type: Array,
                required: true
            },

            dateRange: {
                type: Array,
                required: true
            }
        },

        mounted() {
            this.drawChart();
        },

        data() {
            return {

            }
        },

        methods: {
            drawChart() {
                Highcharts.chart('core-vs-baseline-spend-container', {
                    chart: {
                        type: 'line'
                    },

                    title: {
                        text: null
                    },

                    xAxis: {
                        categories: this.dateRange.map(date => this.$dateFns.format(date, 'YYYY-MM'))
                    },

                    yAxis: {
                        title: {
                            text: null
                        },

                        labels: {
                             format: "${value:,.2f}"
                        }
                    },

                    legend: {

                    },

                    tooltip: {
                        pointFormat: '{series.name}: ${point.y:,.2f}'
                    },

                    plotOptions: {
                        series: {
                            label: {
                                connectorAllowed: false
                            }
                        }
                    },

                    series: [
                        this.wasteSpendSeries,
                        this.calculatedBaselineSpendSeries
                    ]
                });
            }
        },

        watch: {
            wasteSpend: function(val) {
                this.drawChart();
            }
        },

        computed: {
            wasteSpendSeries() {
                return {
                    name: 'Actual Client Charges',
                    data: this.wasteSpend.map(record => record.summed_total_amount.amount / 100)
                };
            },

            calculatedBaselineSpendSeries() {
                return {
                    name: 'Cost Without Sequoia',
                    data: this.calculatedBaselineSpend.map(record => record.total_projected_cost.amount / 100)
                }
            }
        }
    }
</script>