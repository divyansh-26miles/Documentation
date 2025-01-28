# **Strategy Template in C++**

## Features
1. **Config**
    * Config file parameters are read and loaded into variables.
2. **Historical Data**
    * The specified data file from the config is loaded into a data structure.
3. **Order Management** 
    * Orders can be sent either immediately or scheduled for a later time based on config file.
4. **Maintenance Mode**
    * If enabled, failed orders will automatically be retried at a frequency specified in the config file.
5. **Square Off** 
    * Orders will be scheduled after a specified start time, with the scheduling duration defined in the config file.
6. **Stop Loss**
    * Multiple types of stop loss functions can be enabled or disabled using the config file.
    * For more details, refer to the [Stop Loss Types](https://26milesclub.atlassian.net/wiki/x/IoCJH) documentation.
7. **Crash Handling**
    * The required data structure is re-initialized after the crash with theoretical and executed positions.
    * The current day's data is loaded into the specified data structure as well.
8. **Logging**  
    * **PnL Logging** – Logging of variation-symbol wise PnL every minute, though the frequency can be changed through config file  .
    * **Order Logging** – All successfully placed orders are logged in a file.

## Risk Checks
1. **For User purpose**
    * A function `is_valid_inst()` is provided, which takes an instrument ID as input and checks whether the instrument is suitable for placing an order.
2. **Before placing an order**
    * The existence of the instrument pointer is validated.
    * The validity of the quote is verified.
    * The existence of the required order worker is checked.

## Global Variables
### Variation Dependent
* `vector<multimap<string, map<string, vector<string>>>>  histData`: Stores the historical data 
* 

### Variation Independent
* `View* _view`: Pointer to the master view, which manages all other data and pointers.
* `Universe* _univ`: Pointer for fetching information about the universe.
* `Portfolio* _portfolio`: Pointer for fetching portfolio data.
* `Logger* _logger`: Pointer for logging data into files.
* `EXCHANGE exchange`: Exchange on which trading will be executed.
* `string dateToday`: Today's date.
* `string stratType`: The `type` of strategy.
* `string startTime`: A dummy variable required by the API.
* `string endTime`: A dummy variable required by the API.
* `string maintenanceFreq`: Frequency at which maintenance will run.
* `bool shouldRun`: Indicates whether the current logic needs to run.
* `bool hasCrashed`: Indicates whether the strategy has crashed.
* `int maxLotsAtOnceOpt`: Maximum number of option lots that can be sent at once.
* `int maxLotsAtOnceFut`: Maximum number of future lots that can be sent at once.
* `vector<string> tradingDays`: Contains all trading days.
* `vector<string> events`: Contains all events that need to be subscribed.
* `vector<string> orderworkerRequired`: Instrument types for which an order worker is required.
* `vector<string> orderworkerNames`: Names of the required order workers.
* `map<string, OrderWorkerPtr> orderWorkers`: Contains pointers to all the required order workers.
* `string maintenanceOrderSchedulingTime`: Duration for which maintenance orders need to be scheduled.
* `bool varIsCheckStoploss`: Indicates whether stoploss checking is required.
* `int maintenanceVarOffset`: Offset for setting up the maintenance variation.
* `string maintenanceStartTime`: Time from which maintenance should start.
* `string maintenanceEndTime`: Time at which maintenance should end.
* `string stoplossCheckStartTime`: Time at which stoploss checking will start.
* `string stoplossCheckEndTime`: Time at which stoploss checking will end.
* `string stoplossCheckFreq`: Frequency at which stoploss will be checked.
* `int _log_var_sym_pnl`: File pointer sequence for PnL logging.
* `int _log_theoretical_signal`: File pointer sequence for executed signal logging.
* `vector<vector<array<int, 3>>> instIdToOrderTypeToPosition`: Mapping of instrument ID to order type and position.
* `vector<PositionInfo*> instIdToPositionInfo;`: Instrument-wise position information, where the vector index represents the instrument ID.
* `enum OrderType`: `type` of order 
* `enum QuantityType`: `type` of quantity

## Function Description
1. **init()**: This is the init function of the strategy where all the variable and timers that are common accross the strategy system is defined and initialised.
2. **logic_init()**: All the variables and data structures specific to the logic are loaded in this function.
3. **setup_timers()**: This function initialises timers for all the variation that are supposed to be running today. This is called inside `logic_init()`.
4. **load_historical_data()**: This function calls the function `load_trading_days` that initialises the `tradingDays` variable and loads the historical data in the variable `histData`. This function is called inside `logic_init()` as well.
5. **crash_handler()**:
