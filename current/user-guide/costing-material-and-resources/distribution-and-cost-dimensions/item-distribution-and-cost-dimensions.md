---
sidebar_position: 2
---

# Item - Distribution and Cost Dimensions

## Introduction

Currently Distribution Rules and Cost Centers can only be added to Items and added on the following ProcessForce master data forms and documents:

- Bill of Materials
- Manufacturing Orders
- Pick Receipt
- Pick Order
- Roll Back correction

## Master Data

Distribution Rules can be allocated as follows, and once set at this level will be copied to the related forms, as per the list above:

- Bill of Materials Header
- Item, CoProduct and Scrap Lines

### Bill of Materials

- Select the Distribution rules for the appropriate dimensions

    ![Screenshot](./media/item-distribution-and-cost-dimensions/bill-of-materials-select-distribution-rule.png)

- Using the Form Setting for the form select the dimensions to be displayed

    ![Screenshot](./media/item-distribution-and-cost-dimensions/bill-of-materials-form-settings.png)

- For each line select the distribution rule

    ![Screenshot](./media/item-distribution-and-cost-dimensions/bill-of-materials-list-of-distribution-rules.png)

### Manufacturing Orders

- Based on the Bill of Materials, the dimensions are copied into the Manufacturing Order

    ![Screenshot](./media/item-distribution-and-cost-dimensions/bill-of-materials.png)

### Pick Issues

- Based on the Manufacturing Order, the dimensions are copied into the Pick Issue Document

    ![Screenshot](./media/item-distribution-and-cost-dimensions/pick-issues.png)

### Goods Issues

- Based on the Pick Issue, the dimensions are copied into the SAP Business One Goods Issue Document

    ![Screenshot](./media/item-distribution-and-cost-dimensions/goods-issues.png)

### Pick Receipt

- Based on the Manufacturing Order header, the dimensions are copied into the Pick Receipt

    ![Screenshot](./media/item-distribution-and-cost-dimensions/pick-receipt.png)

### Goods Receipt

- Based on the Pick Receipt, the dimensions are copied into the SAP Business One Goods Receipt

    ![Screenshot](./media/item-distribution-and-cost-dimensions/goods-receipt.png)
