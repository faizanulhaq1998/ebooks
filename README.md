# Worm Digit Bot for Deriv Trading

This repository contains a skeleton implementation of a worm digit bot for the Deriv trading platform (formerly Binary.com). The bot analyzes the last digits of tick prices to identify patterns and make automated trading decisions.

## 📋 Overview

The worm digit bot is designed to:
- Track and analyze the last digits of tick prices
- Identify patterns in digit sequences
- Execute trades based on digit prediction logic
- Implement risk management strategies
- Adjust stake sizes dynamically based on win/loss streaks

## 🚀 Getting Started

### Prerequisites

- A Deriv account (demo account recommended for testing)
- Access to [bot.deriv.com](https://bot.deriv.com)
- Basic understanding of binary options trading

### Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/faizanulhaq1998/ebooks.git
   cd ebooks
   ```

2. Open [bot.deriv.com](https://bot.deriv.com) in your browser

3. Click on "Import" and select the `worm-digit-bot.xml` file

4. The bot will load in the Deriv Bot workspace

## ⚙️ Configuration

Before running the bot, customize these parameters in the XML file:

### Trading Parameters
- `initial_stake`: Starting stake amount (default: 0.35)
- `max_stake`: Maximum allowed stake (default: 10)
- `current_stake`: Dynamic stake that adjusts during trading

### Risk Management
- `stop_loss`: Maximum loss before bot stops (default: -10)
- `take_profit`: Profit target before bot stops (default: 10)

### Analysis Settings
- `digit_history_size`: Number of last digits to track (default: 10)
- `predicted_digit`: The digit to trade based on analysis

### Streak Tracking
- `win_streak`: Consecutive wins counter
- `loss_streak`: Consecutive losses counter
- Stake increases after 3 consecutive losses (Martingale-like strategy)

## 📊 Bot Strategy

### Data Collection
The bot continuously collects the last digit from each tick price and maintains a rolling window of recent digits.

### Pattern Analysis
The skeleton provides a framework for implementing pattern analysis logic. You can extend it with:
- Frequency analysis of digits
- Sequence pattern detection
- Statistical probability calculations
- Custom prediction algorithms

### Trading Logic
Currently implements "Digit Differs" contract type. The bot:
1. Analyzes collected digit data
2. Predicts the next likely digit
3. Places trade with current stake
4. Adjusts stake based on result

### Risk Management
- **Win**: Resets stake to initial amount, increments win streak
- **Loss**: Increments loss streak, increases stake after 3+ losses
- **Limits**: Caps stake at max_stake, stops at stop_loss/take_profit

## 🎯 Usage

1. **Test First**: Always start with a demo account
2. **Configure**: Adjust parameters to match your risk tolerance
3. **Monitor**: Watch the bot's performance and adjust as needed
4. **Stop Conditions**: The bot will stop when hitting stop_loss or take_profit

## 📖 Learning Resources

This repository also contains trading strategy ebooks:
- `ilide.info-the-binary-number-sequence-trading-algorithm-*.pdf` - Binary number sequence strategy guide
- `rise-or-fall-brand-new-strategy_compress(1).pdf` - Rise/fall trading strategy

Study these materials to understand the underlying strategies and improve your bot's logic.

## ⚠️ Disclaimer

**IMPORTANT**: 
- Trading involves significant risk. Never trade with money you cannot afford to lose.
- This bot is provided as-is for educational purposes.
- Past performance does not guarantee future results.
- Always test thoroughly on a demo account before live trading.
- The authors are not responsible for any financial losses incurred using this bot.

## 🔧 Customization

### Extending the Bot

You can customize the bot by:

1. **Adding Custom Pattern Detection**:
   - Implement your own digit analysis logic
   - Add more sophisticated prediction algorithms

2. **Modifying Risk Management**:
   - Adjust the Martingale multiplier (currently 1.5x)
   - Implement anti-Martingale strategies
   - Add time-based trading restrictions

3. **Contract Type Changes**:
   - Switch from DIGITDIFF to DIGITOVER, DIGITUNDER, or DIGITMATCH
   - Implement multiple contract types simultaneously

4. **Advanced Features**:
   - Add time-of-day trading restrictions
   - Implement session-based trading
   - Add notification alerts for specific conditions

## 📝 Bot Structure

The XML bot follows the Deriv Bot blockly structure:

```
- INITIALIZATION: Set up variables and parameters
- SUBMARKET: Define market and trading logic
  - PURCHASE: Define purchase conditions and stake
    - DURING_PURCHASE: Logic while trade is active
    - AFTER_PURCHASE: Post-trade analysis and adjustments
```

## 🤝 Contributing

Contributions are welcome! Feel free to:
- Report bugs
- Suggest enhancements
- Submit pull requests with improvements

## 📄 License

This project is provided for educational purposes. Use at your own risk.

## 🔗 Useful Links

- [Deriv Bot Documentation](https://deriv.com/bot)
- [Deriv API Documentation](https://api.deriv.com)
- [Binary Trading Strategies](https://deriv.com/academy)

## 📞 Support

For questions or issues:
1. Check existing issues in this repository
2. Review the Deriv Bot documentation
3. Test on demo accounts first
4. Start with small stakes when going live

---

**Remember**: Responsible trading is smart trading. Always understand the risks involved.
