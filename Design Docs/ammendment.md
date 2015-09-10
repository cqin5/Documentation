# Ammendment
An event to modify the order after it is charged and dispatched.

## How it works.
Driver can issue `price modification request` to server, which will result in amendments to an order. There are two types or `price modification request`: price deduction request and additional charge request.

### Price Deduction Request
This happens when certain product is out of stock. The driver should attempt to contact the customer. If the customer failed to respond, the driver can carry out the default action (in this case, drop the number of out-of-stock items from the order). On delivery, the user can initiate a price deduction request for the driver to confirm. The server, on receiving the request, creates a `PriceDeductionRequest` object at `Pending` state and push notify the driver. The driver can confirm or cancel the request. On confirmation, the server changes the state of the `PriceDeductionRequest` to `Completed` and refund the user. In addition, it results in an `OrderAmendment` being added to the order and updates its content.

### Additional Charge Reuqest
This happens when driver had to travel extra distance to get some out-of-stock product. Before the driver goes the extra mile, he should have communicated with the customer and inform the cusotmer about the extra charge if certain product will be in stock. On discovering stock in the alternative store, the driver should issue an `AdditionalChargeRequest` to the server, which will push notify the customer to confirm. On confirmation, the server will attempt to charge the customer and notify the driver with the go-ahead.
