---
title: Cart
---

Cart loop displays cart information.      
`{loop type="cart" name="the-loop-name" [argument="value"], [...]}`

## Arguments {#pse-arguments}

| Argument | Description | Default | Example |
| ------------- |:-------------| :-------------: | :-------------|
| order       | A list of values <br/> Expected values : <br/> - reverse : reverse chronological item add order | reverse | order="reverse" |

Plus the [global arguments](./global_arguments)

## Outputs

| Variable                     | Value                                                                 |
| :--------------------------- | :-------------------------------------------------------------------- |
| $IS_PROMO	                   | if the product sale elements is in promo or not                       |
| $ITEM_ID	                   | the cart item id                                                      |
| $PRICE	                   | the product sale elements price (unit price)                          |
| $PRODUCT_ID	               | the product id                                                        |
| $PRODUCT_SALE_ELEMENTS_ID	   | the product sale elements id                                          |
| $PRODUCT_URL	               | the product url                                                       |
| $PROMO_PRICE	               | the product sale elements in promo price (unit price)                 | 
| $PROMO_TAXED_PRICE	       | the product sale elements in promo price including taxes (unit price) |
| $QUANTITY	                   | the cart item quantity                                                |
| $REF	                       | the product ref                                                       |
| $STOCK	                   | the product sale elements available stock                             |
| $TAXED_PRICE	               | the product sale elements price including taxes (unit price)          |
| $TITLE	                   | the product title                                                     |
| $TOTAL_PRICE	               | the product sale elements price (total price)                         |
| $TOTAL_PROMO_PRICE	       | the product sale elements in promo price (total price)                |
| $TOTAL_PROMO_TAXED_PRICE	   | the product sale elements in promo price including taxes (total price)|
| $TOTAL_TAXED_PRICE	       | the product sale elements price including taxes (total price)         |

Plus the [global outputs](./global_outputs)
