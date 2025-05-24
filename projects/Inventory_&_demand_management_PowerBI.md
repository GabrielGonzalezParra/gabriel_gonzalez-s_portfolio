# Inventory & Demand Management Dashboard

This project is a Power BI interactive report designed to help a retail food company monitor its current and future stock levels, understand supplier dynamics, and anticipate potential stockouts based on demand forecasting.

## Project Overview

The dashboard is built using a dataset of 77 food products, including information such as:

- Product name, category, and supplier
- Unit price
- Current stock
- Orders placed
- Estimated demand
- Calculated future stock
- Stock coverage ratio and risk classification

The goal is to provide useful insights for inventory decision-making and supply planning.

---

## Key Features

### 1. **Supplier Overview Dashboard**
- Number of products per supplier
- Distribution of product categories per supplier (interactive donut chart)
- Total expenditure per supplier


![Power BI Dashboard](https://drive.google.com/uc?id=1hcMlDsNN0jdX8ieaARc5eo7-TE4EM1U-)


#### Interactive filtering by SupplierID

The user has selected a specific supplier using the button slicer. The dashboard updates all related visuals to show only the data linked to that supplier.


![Interactive filtering by supplierID](https://drive.google.com/uc?id=1PUdCihefzTo4cwxkyvdezaBx1MDYx46e)


#### Interactive filtering by category

This screenshot demonstrates how selecting a product category within the donut chart filters the dashboard to highlight only suppliers that offer products in that specific category.


![Interactive filtering by category](https://drive.google.com/uc?id=1sp1AkpY_E7WL8OgYZ24GHJTcliphA62A)


#### Interactive filtering expense by supplier

Here, the user has clicked on a bar in the supplier expenditure chart. This interaction filters the rest of the dashboard, enabling detailed analysis of which categories and products are contributing to that supplier's total cost.


![Power BI Dashboard 2](https://drive.google.com/uc?id=1V5IYEmMk6lL5LC2baJIzQYJS9Ky5cV3O)

---

### 2. **Inventory Monitoring Dashboard**
- Bar chart of future stock by product
- Conditional bar chart showing stock coverage ratio with custom risk levels
- Visualization of current stock and unit price
- Matrix table with key figures: current stock, orders, demand, future stock


![Power BI Dashboard 2](https://drive.google.com/uc?id=12D6B7FxghbON8a4NNRbJ8to7i6g7L-aI)


#### Interactive filtering - product selected


This view highlights a product that is at high risk of stockout. Once selected, the dashboard displays detailed figures on current stock, demand, and the low future stock level. The bar in the coverage ratio chart is shown in red, indicating insufficient stock coverage for the forecasted demand.


![Interactive filtering - product selected](https://drive.google.com/uc?id=1yHovG3aELjtvFzblFXBXc9RdY0bLCFI7)

---


