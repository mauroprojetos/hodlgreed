# HODL & Greed
# Introduction
HODL &amp; Greed Strategy for Gekko Trading Bot

# Description
This strategy is designed for wave / sideways markets but will work in 'staircase' markets too with the right settings. The general idea is to catch the bottom of a wave pattern, sell at the top, repeat!

This is not the kind of script that will perform well if you blindly pump various data sets for it. This method depends on market recovery (if you get caught HODL'ing without stoploss) and "hints" provded by a human either at startup time or via IRC while running.

It's important to note that though this strategy has an IRC interface, commands aren't exactly real time. You can issue buy/sell orders via IRC, and you can change settings at anytime. However, actual trades will not be placed until Gekko asks the strategy for advice. This happens once per candle.

So if you are using 5-minute candles and you issue a buy order, you might have to wait for up to 5 minutes or so before an actual trade. Also keep in mind that Gekko has to then submit the trade to the exchange's order book then it has to be filled. 

Standard disclaimer: THIS IS EXPERIMENTAL SOFTWARE. It is subject to change without notice AND it definitely has bugs. USE AT YOUR OWN RISK!

# Features
- Maybe the first Gekko strategy to feature an IRC interface for human interaction from within the strategy itself. (Not to be confused with Gekko's IRC plugin which only makes announcements)
- Sell or BUY for the first trade
- SMA tracking and dynamic adjusting of buy price
- Stoploss support
- Greed!

# Settings
# buy_at_or_below (floating point)
Give buy advice price reaches this level or lower. If you use the dynamically adjusting feature you can set this to a very small number such as 0.0000001 and the script will automatically adjust to mean price minus your percentage for the first buy.

# max_nobuy_candles (integer)
If this many candles go by without a buy, adjust buy_at_or_below price.  This setting must be equal to or greater than your SMA interval. Set this to zero to disable dynamic adjusting.

# adjust (percentage)
Adjust buy_at_or_below price by this percentage below SMA (mean) price.

# greed (percentage)
How greedy are you? :) The strategy will give sell advice if the current candle close price is this percentage above your buy price.

# stoploss (percentage)
Standard stoploss protection. Once this strategy encounters a stoploss, trading is suspended. You'll have to manually enable trading via IRC.

# last_buy_price (floating point)
Normally the script will only offer buy advice for the first trade. This presumes that your exchange account has a currency balance and no asset balance. However, if you also have an asset balance and you want the script to sell first, this is how you let it know what you paid for the asset in order to calculate necessary greed :)

This is useful if you had to restart your gekko process while you were HODL'ing an asset.

Note: Use of this feature will disable buy advice until the first sell has been advised.

# IRC
This strategy uses NodeJS irc library. The full list of settings are here https://github.com/martynsmith/node-irc/blob/master/lib/irc.js

You may want to run multiple bots simultaneously. If you join all of your bots to the same channel, commands you issue in that channel will affect all bots. You can also have each bot join multiple rooms, so you could have a unique channel per bot and a common reporting channel for all bots.

The bot will also accept your commands via private message.

IRC support is VERY basic. The bot will only accept commands from the nickname matching the owner setting. This isn't exactly the most secure authorization mechanism so you should only run this bot on your private IRC server.

# IRC Commands

# ;;status
- Shows various statistics

# ;;buy
- Force buy advice at next candle

# ;;sell
- Force sell advice at next candle

# ;;cancel
- Cancel forced trade advice if issued before next candle

# ;;candle
- Shows various statistics about the last known candle

# ;;set
- Allows you to change greed, buy_at_or_below price, adjust, stoploss variables.
- You can also suspend trading with: ;;set hodl on or resume trading with: ;;set hodl off