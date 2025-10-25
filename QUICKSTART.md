# Quick Start Guide

Get your Worm Digit Bot running in 5 minutes!

## ⚡ Super Quick Start

1. **Download the bot**:
   - Click on `worm-digit-bot.xml` in this repository
   - Click "Raw" button
   - Right-click → "Save As" (or Ctrl+S / Cmd+S)

2. **Open Deriv Bot**:
   - Go to [bot.deriv.com](https://bot.deriv.com)
   - Login with your Deriv account (use demo account!)

3. **Import the bot**:
   - Click "Import" button in Deriv Bot
   - Select the downloaded `worm-digit-bot.xml` file
   - Bot will load in the workspace

4. **Configure (Optional)**:
   - The bot comes with safe default settings
   - See [CONFIGURATION.md](CONFIGURATION.md) to customize

5. **Run the bot**:
   - Ensure you're on a DEMO account!
   - Click "Run" button
   - Monitor the bot's performance

## 🎯 First Run Checklist

Before running the bot, verify:

- [ ] You're on a **DEMO account** (not real money!)
- [ ] You understand the [risk disclaimer](README.md#-disclaimer)
- [ ] The bot settings match your risk tolerance
- [ ] You have time to monitor the first session

## 📊 Default Settings

The bot comes configured with conservative settings:

```
Initial Stake:        $0.35
Max Stake:           $10.00
Stop Loss:          -$10.00
Take Profit:         $10.00
Contract Type:       Digit Differs
```

These settings are designed for:
- Low risk
- Learning and testing
- Demo account practice

## 🔧 Basic Customization

Want to adjust the bot? Edit these key values in the XML:

### Change Stake Amount
Find this line (~line 55):
```xml
<field name="NUM">0.35</field>
```
Change `0.35` to your desired stake (e.g., `1.00`)

### Change Stop Loss
Find this line (~line 69):
```xml
<field name="NUM">-10</field>
```
Change `-10` to your stop loss (e.g., `-20`)

### Change Take Profit
Find this line (~line 78):
```xml
<field name="NUM">10</field>
```
Change `10` to your profit target (e.g., `15`)

## 📚 Learning Path

### Day 1-3: Learn the Basics
- Run bot on demo with default settings
- Watch how it analyzes digits
- Read [PATTERNS.md](PATTERNS.md) to understand strategies
- Monitor win/loss patterns

### Week 1: Understand Configuration
- Read [CONFIGURATION.md](CONFIGURATION.md)
- Try different stake amounts on demo
- Test different stop loss/take profit values
- Find your comfort zone

### Week 2: Pattern Analysis
- Study [PATTERNS.md](PATTERNS.md) in detail
- Observe which patterns work best
- Consider customizing the bot's logic
- Document your findings

### Week 3+: Optimization
- Fine-tune parameters based on results
- Test on different market conditions
- Consider small live stakes (only if confident!)
- Always trade responsibly

## 🆘 Common Issues

### Bot doesn't start
- **Check**: Are you logged into Deriv?
- **Check**: Is bot.deriv.com loaded properly?
- **Fix**: Refresh page and re-import bot

### Bot stops immediately
- **Check**: Do you have sufficient balance?
- **Check**: Is your stop loss too tight?
- **Fix**: Increase balance or adjust stop loss

### Bot keeps losing
- **Check**: Are you in a high volatility period?
- **Check**: Is your pattern logic sound?
- **Fix**: Review [PATTERNS.md](PATTERNS.md) and adjust

### Stake gets too high
- **Check**: Is max_stake set properly?
- **Check**: Are you hitting loss streaks?
- **Fix**: Lower the Martingale multiplier or increase loss threshold

## 💡 Pro Tips

1. **Always Demo First**: Test ANY changes on demo account first
2. **Start Small**: Use minimum stakes when going live
3. **Set Limits**: Use stop loss and take profit religiously
4. **Take Breaks**: Don't run bot 24/7, take regular breaks
5. **Keep Records**: Track your bot's performance
6. **Stay Informed**: Market conditions change, adapt accordingly

## 🎓 Understanding the Bot

### How It Works

1. **Collect Data**: Bot tracks last digit of each tick
2. **Analyze Pattern**: Looks for patterns in digit sequence
3. **Make Decision**: Predicts next digit behavior
4. **Place Trade**: Executes trade with current stake
5. **Manage Risk**: Adjusts stake based on win/loss
6. **Repeat**: Continues until stop conditions met

### Win/Loss Management

- **After Win**: Stake resets to initial amount
- **After 1-2 Losses**: Stake stays the same
- **After 3+ Losses**: Stake increases by 1.5x
- **At Max Stake**: Stake won't increase further
- **At Stop Loss/Profit**: Bot stops automatically

## 🔐 Safety First

### Demo Account Benefits
- ✅ Risk-free testing
- ✅ Unlimited "money" to experiment
- ✅ Real market conditions
- ✅ No emotional pressure
- ✅ Perfect for learning

### When to Go Live
Only consider live trading when:
- ✅ You've tested on demo for 2+ weeks
- ✅ Bot shows consistent profit on demo
- ✅ You understand all settings
- ✅ You can afford to lose the stake
- ✅ You're comfortable with the risk

## 📞 Need Help?

1. **Read the docs**: 
   - [README.md](README.md) - Overview
   - [CONFIGURATION.md](CONFIGURATION.md) - Settings
   - [PATTERNS.md](PATTERNS.md) - Strategies

2. **Check the PDFs**: Study the trading strategy books in this repo

3. **Community**: Join Deriv community forums for support

4. **Practice**: Most issues resolve with more practice!

## 🚀 Ready to Start?

1. Make sure you're on a **DEMO account**
2. Download `worm-digit-bot.xml`
3. Import to bot.deriv.com
4. Click Run and watch it work!
5. Learn, adjust, improve!

---

**Remember**: Trading involves risk. This bot is for educational purposes. Never trade more than you can afford to lose. Start with demo accounts and only move to live trading when you're ready and understand the risks.

Happy Trading! 🎯
