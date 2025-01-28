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
* `vector<PositionInfo*> instIdToPositionInfo`: `: Instrument-wise position information, where the vector index represents the instrument ID.
* `enum OrderType`: `type` of order 
* `enum QuantityType`: `type` of quantity

### Variation Dependent
* `vector<multimap<string, map<string, vector<string>>>>  histData`: Stores historical data.
* `vector<multimap<string, map<string, vector<string>>>> dayData`: Stores data generated during the day.
* `map<int, vector<vector<array<int, 3>>>> varNoToInstIdToOrderTypeToPosition`: Stores all positions corresponding to a particular variation in this map, where the variation number is the key.
* `map<int, vector<PositionInfo*>> varNoToInstIdToPositionInfo`: Stores PnL and price information of a particular variation in this map, with the variation number as the key.
* `map<int, int> varNoToIndexMap`: Maps the variation number to the index corresponding to that variation in all variables of type `vector`.
* `map<int, set<int>> varNoToActiveInstruments`: Contains the set of all instruments traded under a particular variation.
* `map<pair<int, int>, Timestamp> varInstWiseStoplossCoolDownEndTime`: Stores the time after which a position can be taken for a specific (variation, instId) pair after hitting a stop loss.
#### For all these variable, an index is fixed for a particular variation 
* `vector<int> varNos`: All variation numbers running in this strategy.
* `vector<bool> varRun`: Indicates whether the variation should run or not.
* `vector<int> varLogicType`: The logic type of the variation.
* `vector<string> varStartTime`: Start time of the variation.
* `vector<string> varEndTime`: End time of the variation.
* `vector<int> varFrequencySeconds`: Frequency (in seconds) at which a timer should run for the variation.
* `vector<string> varEntryStartTime`: Time at which entry will start for the variation.
* `vector<string> varEntryEndTime`: Time at which entry will end for the variation.
* `vector<bool> varOrderSchedulingEnabled`: Indicates whether order scheduling is required for the variation.
* `vector<bool> varTakeTrade`: Indicates whether the variation should take trades or not.
* `vector<string> varMaintenanceStartTime`: Time at which maintenance will start for the variation.
* `vector<string> varMaintenanceEndTime`: Time at which maintenance will end for the variation.
* `vector<bool> varIsMaintenanceRequired`: Indicates whether maintenance is required for the variation.
* `vector<int> varOrderSchedulingDuration`: Duration (in seconds) for which orders will be scheduled for the variation.
* `vector<bool> varIsStoplossType1`: Indicates whether stoploss of type 1 is required for the variation.
* `vector<double> varStoplossType1Parameter`: Parameter value for stoploss of type 1.
* `vector<bool> varIsStoplossType2`: Indicates whether stoploss of type 2 is required for the variation.
* `vector<double> varStoplossType2Parameter`: Parameter value for stoploss of type 2.
* `vector<bool> varIsStoplossType3`: Indicates whether stoploss of type 3 is required for the variation.
* `vector<double> varStoplossType3Parameter`: Parameter value for stoploss of type 3.
* `vector<bool> varIsStoplossType4`: Indicates whether stoploss of type 4 is required for the variation.
* `vector<double> varStoplossType4Parameter`: Parameter value for stoploss of type 4.
* `vector<bool> varIsStoplossType5`: Indicates whether stoploss of type 5 is required for the variation.
* `vector<double> varStoplossType5Parameter`: Parameter value for stoploss of type 5.
* `vector<bool> varIsStoplossType6`: Indicates whether stoploss of type 6 is required for the variation.
* `vector<double> varStoplossType6Parameter`: Parameter value for stoploss of type 6.
* `vector<int> varStoplossCoolDownSec`: Duration (in seconds) for which the variation must stop trading after a stoploss is hit.
* `vector<bool> varIsSquareoffRequired`: Indicates whether template-based square-off is required for the variation.
* `vector<string> varSquareoffStartTime`: Square-off start time for the variation.
* `vector<int> varSquareoffDuration`: Duration (in seconds) of the square-off for the variation.
* `vector<bool> varSendStoplossOrdersNow`: Indicates whether stoploss orders should be sent immediately.

## Function Description
* `void init(Config* config, View* view, string& name)`:  This is the initialization function for the strategy, where all the variables and timers that are common across the strategy system are defined and initialized.
* `void logic_init(Config* config)`: This function loads all the variables and data structures specific to the strategy logic.
* `void setup_timers(vector<int> varNos)`: Initializes timers for all variations that are scheduled to run today. This function is called within `logic_init(Config* config)`.
* `void load_trading_days(Config* config)`: Initializes the `tradingDays` variable.
* `void load_historical_data(Config* config)`: Calls the `load_trading_days(Config* config)` function to initialize the `tradingDays` variable and loads historical data into the `histData` variable. This function is also called within `logic_init(Config* config)`.
* `void crash_handler(Config* config)`: Re-initializes the data structures `varNoToInstIdToOrderTypeToPosition`, `varNoToInstIdToPositionInfo`, and `dayData` after a crash.
* `void handle_trade_events(Event* e)`: Handles trade-related events.
* `void handle_touch_events(Event* e)`: Handles touch-related events.
* `void handle_index_events(Event* e)`: Handles index-related events.
* `void handle_predictor_events(Event* e)`: Handles predictor-related events.
* `void handle_timer_events(Event* e)`: Handles timer-related events. All timers created within the strategy trigger this function when executed.
* `void create_signal_and_place_order(int varNo)`: Called for each variation at a specific frequency. Within this function, the `create_entry` and `create_exit` functions are invoked.
* `void create_entry(int varNo)` and `void create_exit(int varNo)`: Functions for generating entry and exit signals, respectively. Users must implement their specific logic within these functions.
* `void write_theoretical_signal(int varNo, string symbol, int localId, string signal, double entryPrice, int quantity, int type)`: Logs the executed signal into a file.
* `void csv_2_vec(const string& str, vector<string>& results)`: Converts a comma-separated string into a vector.
* `int get_trading_day_index(string tradingDay, vector<string> tradingDays)`: Returns the index of a given date within the `tradingDays` vector.
* `bool is_valid_inst(int instId)`: Checks the validity of an `instId`, ensuring it is suitable for placing an order. Users must call this function before generating a signal for an `instId`.
