#include <Trade/Trade.mqh>
CTrade obj_Trade;

// Inputs
input double riskPercentage = 1.0; // Risk percentage per trade
input double lotSize = 0.01; // Lot size
input long InpMagicNumber = 120100; // Magic Number
input double riskRewardRatio = 3.0; // Risk/Reward ratio

int handleATR = INVALID_HANDLE;
int handleRSI = INVALID_HANDLE;
double dataATR[];
double dataRSI[];

int OnInit() {
    handleATR = iATR(_Symbol, _Period, 14);
    if (handleATR == INVALID_HANDLE) {
        Print("INVALID IND ATR HANDLE. REVERTING NOW");
        return (INIT_FAILED);
    }

    handleRSI = iRSI(_Symbol, _Period, 14, PRICE_CLOSE);
    if (handleRSI == INVALID_HANDLE) {
        Print("INVALID IND RSI HANDLE. REVERTING NOW");
        return (INIT_FAILED);
    }

    ArraySetAsSeries(dataATR, true);
    ArraySetAsSeries(dataRSI, true);

    obj_Trade.SetExpertMagicNumber(InpMagicNumber);

    return (INIT_SUCCEEDED);
}

void OnTick() {
    if (CopyBuffer(handleATR, 0, 0, 3, dataATR) < 3 || CopyBuffer(handleRSI, 0, 0, 3, dataRSI) < 3) {
        Print("NOT ENOUGH DATA FOR FURTHER CALC'S. REVERTING");
        return;
    }

    if (dataATR[0] > 0 && dataRSI[0] > 0) {
        double Ask = NormalizeDouble(SymbolInfoDouble(_Symbol, SYMBOL_ASK), _Digits);
        double Bid = NormalizeDouble(SymbolInfoDouble(_Symbol, SYMBOL_BID), _Digits);
        
        double buy_trail = Bid - NormalizeDouble(dataATR[0], _Digits);
        double sell_trail = Ask + NormalizeDouble(dataATR[0], _Digits);

        for (int i = PositionsTotal() - 1; i >= 0; i--) {
            ulong ticket = PositionGetTicket(i);
            if (ticket > 0) {
                if (PositionSelectByTicket(ticket)) {
                    string symb = PositionGetString(POSITION_SYMBOL);
                    long type = PositionGetInteger(POSITION_TYPE);
                    ulong magic = PositionGetInteger(POSITION_MAGIC);
                    double open_p = PositionGetDouble(POSITION_PRICE_OPEN);
                    double sl_current = PositionGetDouble(POSITION_SL);
                    double tp_current = PositionGetDouble(POSITION_TP);
                    if (symb == _Symbol && magic == InpMagicNumber) {
                        if (type == POSITION_TYPE_BUY) {
                            if (buy_trail > open_p && (sl_current == 0 || buy_trail > sl_current)) {
                                obj_Trade.PositionModify(ticket, buy_trail, tp_current);
                            }
                        } else if (type == POSITION_TYPE_SELL) {
                            if (sell_trail < open_p && (sl_current == 0 || sell_trail < sl_current)) {
                                obj_Trade.PositionModify(ticket, sell_trail, tp_current);
                            }
                        }
                    }
                }
            }
        }

        // RSI based entry logic
        double sl = NormalizeDouble(dataATR[0], _Digits);
        double tp = sl * riskRewardRatio;

        if (dataRSI[0] > 30) { // Buy signal
            obj_Trade.Buy(lotSize, NULL, 0, Bid - sl, Bid + tp, "RSI Buy");
        } else if (dataRSI[0] > 70) { // Sell signal
            obj_Trade.Sell(lotSize, NULL, 0, Ask + sl, Ask - tp, "RSI Sell");
        }
    }
}
