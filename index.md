# **Strategy Template in C++**

## Features
1. **Order Management** 
    * Orders can be sent either immediately or scheduled for a later time based on config file.
2. **Maintenance Mode**
    * If enabled, failed orders will automatically be retried at a frequency specified in the config file.
3. **Square Off** 
    * Orders will be scheduled after a specified start time, with the scheduling duration defined in the config file.
4. **Stop Loss**
    * Multiple types of stop loss functions can be enabled or disabled using the config file.
    * For more details, refer to the [Stop Loss Types](https://26milesclub.atlassian.net/wiki/spaces/~62b95d70ed036549273c23b4/pages/edit-v2/478642189?draftShareId=6295bb60-6529-4b79-b51d-20b903740eb6) documentation.
5. **Logging**  
    * **PnL Logging** – Logs variation-symbol PnL every minute, though the frequency can be changed through config file  
    * **Order Logging** – Logs all successfully placed orders.
6. **Variable**
    * Following are the variables that are already defined in the template
        * View* _view
        * Universe* _univ
        * Portfolio* _portfolio
        * Logger* _logger
        * EXCHANGE exchange
        * string dateToday
        * vector<string> tradingDays

