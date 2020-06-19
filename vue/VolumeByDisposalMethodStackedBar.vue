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

    .flex {
        display: flex;
    }
</style>

<template>
    <div class="p-2">
        <h3 class="my-3 p-1 text-blue-500 font-medium text-xl">Volume by Disposal Method</h3>

        <div class="flex">
            <div id="volume-by-disposal-method-container">

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
            series: {
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

        methods: {
            drawChart() {
                Highcharts.chart('volume-by-disposal-method-container', {
                    chart: {
                        type: 'column'
                    },

                    title: {
                        text: null
                    },

                    xAxis: {
                        categories: this.dateRange.map(date => this.$dateFns.format(date, 'YYYY-MM'))
                    },

                    yAxis: {
                        min: 0,
                        title: {
                            text: null
                        },
                        stackLabels: {
                            enabled: true,
                            style: {
                                fontWeight: 'bold',
                                color: (Highcharts.theme && Highcharts.theme.textColor) || 'gray'
                            },
                            format: "{total:,.2f} lbs."
                        }
                    },

                    legend: {
                        backgroundColor: (Highcharts.theme && Highcharts.theme.background2) || 'white',
                        borderColor: '#CCC',
                        borderWidth: 1,
                        shadow: false
                    },

                    tooltip: {
                        pointFormatter() {
                            let y = this.y.toLocaleString('en-US', {maximumFractionDigits: 2, minimumFractionDigits: 2});
                            let stackTotal = this.stackTotal.toLocaleString('en-US', {maximumFractionDigits: 2, minimumFractionDigits: 2});
                            let percent = parseFloat((this.y / this.stackTotal) * 100).toFixed(2) + '%';

                            return `${this.series.name}: ${y} (${percent})<br/>Total: ${stackTotal}`;
                        }
                    },

                    plotOptions: {
                        column: {
                            stacking: 'normal'
                        }
                    },

                    series: this.series
                });
            }
        },

        watch: {
            series: function(val) {
                this.drawChart();
            }
        }
    }
</script>