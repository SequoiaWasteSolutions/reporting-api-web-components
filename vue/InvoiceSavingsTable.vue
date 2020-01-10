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

    .font-semibold {
        font-weight: 600;
    }

    .text-xl {
        font-size: 1.25rem;
    }

    .text-blue-500 {
        color: #4299e1;
    }

    .text-right {
        text-align: right;
    }

    .text-green-500 {
        color: #48bb78;
    }

    .text-green-700 {
        color: #2f855a;
    }

    .text-red-500 {
        color: #f56565;
    }

    .text-red-700 {
        color: #c53030;
    }

    .text-gray-900 {
        color: #1a202c;
    }

    .table {
        width: 100%;
        margin-bottom: 0.25rem;
        background-color: transparent;
        border-collapse: collapse;
        text-align: left;
        border-color: #cbd5e0;
        border-spacing: 0;
    }

    .table > thead > tr > th {
        vertical-align: bottom;
        border-bottom-width: 2px;
        border-top-width: 0;
        border-color: #cbd5e0;
    }

    .table tr td {
        border-color: #cbd5e0;
    }

    .table td, .table th {
        padding: 0.75rem;
        vertical-align: middle;
        border-top-width: 1px;
    }

    .table-striped tr:nth-of-type(even) {
        background-color: #f7fafc;
    }
</style>

<template>
    <div class="p-2">
        <h3 class="my-3 p-1 text-blue-500 font-medium text-xl">Savings from Audits by Vendor</h3>

        <table class="table table-striped">
            <thead>
                <tr class="mb-2">
                    <th class="font-semibold w-1/4">Vendor</th>
                    <th class="font-semibold text-right"># Processed</th>
                    <th class="font-semibold text-right"># Incorrect</th>
                    <th class="font-semibold text-right">Error Rate</th>
                    <th class="font-semibold text-right">Savings</th>
                </tr>
            </thead>

            <tbody>
                <tr v-for="record in invoiceSavingsByVendor" class="">
                    <td>{{ record.vendor_name }}</td>
                    <td class="text-right">{{ record.total_count }}</td>
                    <td class="text-right">{{ record.incorrect_count }}</td>
                    <td class="text-red-500 font-semibold text-right">{{ record.error_rate | format_percentage }}</td>
                    <td class="text-green-500 font-semibold text-right">
                        {{ record.savings.amount / 100 | format_money }} <span class="text-gray-900">({{ record.savings_percentage | format_percentage }})</span>
                    </td>
                </tr>
                <tr>
                    <td class="font-bold text-lg">Total</td>
                    <td class="font-bold text-lg text-right">{{ numberOfInvoicesProcessed }}</td>
                    <td class="font-bold text-lg text-right">{{ numberOfInvoicesWithSavings }}</td>
                    <td class="font-bold text-lg text-right" :class="totalErrorRate > 0 ? 'text-red-700' : 'text-gray-900'">{{ totalErrorRate | format_percentage }}</td>
                    <td class="font-bold text-lg text-right" :class="totalSavingsFromAudits.amount > 0 ? 'text-green-700' : 'text-gray-900'">
                        {{ totalSavingsFromAudits.amount / 100 | format_money }}
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    export default {
        props: {
            totalSavingsFromAudits: {
                type: Object,
                required: true
            },

            totalErrorRate: {
                type: Number,
                required: true
            },

            numberOfInvoicesProcessed: {
                type: Number,
                required: true
            },

            numberOfInvoicesWithSavings: {
                type: Number,
                required: true
            },

            invoiceSavingsByVendor: {
                type: Array,
                required: true
            },
        },
    }
</script>