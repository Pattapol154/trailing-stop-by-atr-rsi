# trailing-stop-by-atr-rsi

OnInit():
This function is called when the EA is initialized (e.g., when attached to a chart or recompiled).
Purpose: Initializes indicator handles, sets up arrays, and configures the CTrade object.

OnTick():
This function is called on every price tick.
Purpose: Implements the main logic for managing positions (trailing stop adjustments) and opening new trades based on the ATR and RSI indicators.

iATR():
This is an MQL5 built-in function that creates an ATR (Average True Range) indicator handle.
Parameters: The symbol, period, and number of periods (14).
Purpose: Retrieves the handle to the ATR indicator for later use in the EA.

iRSI():
This is an MQL5 built-in function that creates an RSI (Relative Strength Index) indicator handle.
Parameters: The symbol, period, number of periods (14), and price type (PRICE_CLOSE).
Purpose: Retrieves the handle to the RSI indicator for later use in the EA.

CopyBuffer():
This is an MQL5 built-in function that copies indicator data into an array.
Parameters: The indicator handle, buffer index, starting position, and number of values to copy.
Purpose: Fetches the most recent values of ATR and RSI to be used in trading logic.

NormalizeDouble():
This is an MQL5 built-in function that normalizes a floating-point value to a specific number of decimal places.
Parameters: The value to normalize and the number of digits.
Purpose: Ensures that price levels (such as stop-loss and take-profit) are adjusted to the appropriate number of decimal places for the symbol.

SymbolInfoDouble():
This is an MQL5 built-in function that retrieves various information about the symbol.
Parameters: The symbol and the property (e.g., SYMBOL_ASK or SYMBOL_BID).
Purpose: Retrieves the current ask and bid prices.

PositionsTotal():
This is an MQL5 built-in function that returns the total number of open positions.
Purpose: Used to iterate over all open positions to manage stop-loss levels.

PositionGetTicket():
This is an MQL5 built-in function that retrieves the ticket number of a position by index.
Parameters: The index of the position.
Purpose: Used to select specific positions for further analysis and modification.

PositionSelectByTicket():
This is an MQL5 built-in function that selects a position by its ticket number.
Parameters: The ticket number.
Purpose: Retrieves detailed information about the selected position.

PositionGetString():
This is an MQL5 built-in function that retrieves a string property of a selected position (e.g., POSITION_SYMBOL).
Purpose: Used to check the symbol of the position.

PositionGetInteger():
This is an MQL5 built-in function that retrieves an integer property of a selected position (e.g., POSITION_TYPE or POSITION_MAGIC).
Purpose: Used to check the type and magic number of the position.

PositionGetDouble():
This is an MQL5 built-in function that retrieves a double property of a selected position (e.g., POSITION_PRICE_OPEN, POSITION_SL, or POSITION_TP).
Purpose: Used to check the open price, stop-loss, and take-profit levels of the position.

CTrade.PositionModify():
This is a method of the CTrade class that modifies an existing position's stop-loss and/or take-profit levels.
Parameters: The ticket number, new stop-loss, and new take-profit.
Purpose: Adjusts the stop-loss and take-profit levels of open positions based on the calculated trailing stop.

CTrade.Buy():
This is a method of the CTrade class that opens a new buy position.
Parameters: The lot size, symbol (optional), slippage (optional), stop-loss, take-profit, and comment.
Purpose: Executes a buy order when the buy conditions are met (RSI > 30).

CTrade.Sell():
This is a method of the CTrade class that opens a new sell position.
Parameters: The lot size, symbol (optional), slippage (optional), stop-loss, take-profit, and comment.
Purpose: Executes a sell order when the sell conditions are met (RSI > 70).
