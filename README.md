# reporting-api-web-components
This repository contains a handful of ready to use JS components (Vue, React, and Web) that plug directly into the data returned from the Discovery API.

### WasteSpendByCategoryStackedBar

A stacked bar chart for waste spend by category.
It can be plugged into the following reporting endpoints:
* **GET** client-companies/{id}/reports/client-invoice-waste-spend-by-category
* **GET** client-companies/{id}/reports/vendor-invoice-waste-spend-by-category

### VolumeByDisposalMethodStackedBar

A stacked by chart for volume (in pounds) by disposal method.
It can be plugged into the following reporting endpoints:
* **GET** client-companies/{id}/reports/volume-by-disposal-method

### InvoiceSavingsTable

A table component with metrics (hauler error rate, savings from audits, # of bills processed) across haulers servicing the selected locations.
It can be plugged into the following reporting endpoints:
* **GET** client-companies/{id}/reports/savings-from-audits


### AuditedInvoicesDonut
Donut chart showing the number of invoices that were audited.
It can be plugged into the following reporting endpoints:
* **GET** client-companies/{id}/reports/number-of-invoices-audited

### CaculatedBaselineSpendVsActual
Line chart showing the difference between the projected waste spend vs the actual (client or vendor invoice) waste spend

The calculated baseline spend prop must come from the following endpoint:
* **GET** client-companies/{id}/reports/calculated-baseline-waste-spend

The waste spend prop can come from either of the following endpoints:
* **GET** client-companies/{id}/reports/vendor-invoice-waste-spend
* **GET** client-companies/{id}/reports/client-invoice-waste-spend

## Notes for the Vue.js components
The [date-fns library](https://date-fns.org/) is used to format dates in the components.

`this.$dateFns.format(date, 'YYYY-MM')`
