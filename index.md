# **Strategy Template in C++**

## Features
1. **Config**
    * Config file parameters are read and loaded into variables.
2. **Historical Data**
    * The specified data file from the config is loaded into a data structure
3. **Order Management** 
    * Orders can be sent either immediately or scheduled for a later time based on config file.
4. **Maintenance Mode**
    * If enabled, failed orders will automatically be retried at a frequency specified in the config file.
5. **Square Off** 
    * Orders will be scheduled after a specified start time, with the scheduling duration defined in the config file.
6. **Stop Loss**
    * Multiple types of stop loss functions can be enabled or disabled using the config file.
    * For more details, refer to the [Stop Loss Types](https://26milesclub.atlassian.net/wiki/spaces/~62b95d70ed036549273c23b4/pages/edit-v2/478642189?draftShareId=6295bb60-6529-4b79-b51d-20b903740eb6) documentation.
7. **Crash Handling**
    * The required data structure is re-initialized after the crash with theoretical and executed positions.
    * The current day's data is loaded into the specified data structure as well.
8. **Logging**  
    * **PnL Logging** – Logs variation-symbol PnL every minute, though the frequency can be changed through config file  
    * **Order Logging** – Logs all successfully placed orders.
9. **Variables**
    * Following are the variables that are already defined in the template:
        * `View* _view`
        * `Universe* _univ`
        * `Portfolio* _portfolio`
        * `Logger* _logger`
        * `EXCHANGE exchange`
        * `string dateToday`
        * `vector<string> tradingDays`

