# Bot Configuration Guide

This guide explains how to configure and customize the Worm Digit Bot for optimal performance.

## Basic Configuration

### 1. Trading Parameters

Located in the `INITIALIZATION` section of the XML:

```xml
<variable id="initial_stake">0.35</variable>
```

**Recommended Settings:**
- **Conservative**: 0.35 - 1.00
- **Moderate**: 1.00 - 5.00  
- **Aggressive**: 5.00 - 10.00

⚠️ **Warning**: Higher stakes mean higher risk!

### 2. Risk Management Settings

#### Stop Loss
```xml
<variable id="stop_loss">-10</variable>
```
- Set as a negative number (e.g., -10 means stop after losing $10)
- Calculate based on your total trading capital
- Recommended: 5-10% of your total balance

#### Take Profit
```xml
<variable id="take_profit">10</variable>
```
- Set as a positive number (e.g., 10 means stop after gaining $10)
- Should match your daily/session profit goals
- Consider setting lower than stop_loss for better risk/reward ratio

#### Maximum Stake
```xml
<variable id="max_stake">10</variable>
```
- Caps the maximum stake during losing streaks
- Prevents excessive losses from Martingale progression
- Recommended: 10-20x your initial stake

### 3. Digit Analysis Parameters

#### History Size
```xml
<variable id="digit_history_size">10</variable>
```
- Number of previous digits to track and analyze
- Larger values provide more data but slower pattern detection
- Recommended: 8-15 for optimal balance

## Advanced Configuration

### Martingale Settings

The bot uses a modified Martingale strategy:

```xml
<!-- Triggers after 3 consecutive losses -->
<block type="logic_compare">
  <field name="OP">GTE</field>
  <value name="B">
    <block type="math_number">
      <field name="NUM">3</field>  <!-- Adjust this value -->
    </block>
  </value>
</block>

<!-- Multiplies stake by 1.5 -->
<block type="math_arithmetic">
  <field name="OP">MULTIPLY</field>
  <value name="B">
    <block type="math_number">
      <field name="NUM">1.5</field>  <!-- Adjust multiplier -->
    </block>
  </value>
</block>
```

**Adjustment Options:**
- **Loss Threshold**: Change `3` to trigger stake increase earlier/later
- **Multiplier**: Change `1.5` for more/less aggressive recovery
  - Conservative: 1.2 - 1.3
  - Moderate: 1.4 - 1.6
  - Aggressive: 1.7 - 2.0

⚠️ **Warning**: Higher multipliers increase risk exponentially!

## Contract Type Selection

The default contract type is `DIGITDIFF` (Digit Differs):

```xml
<field name="PURCHASE_LIST">DIGITDIFF</field>
```

### Available Contract Types:

1. **DIGITDIFF** - Predicts the last digit will differ from your prediction
2. **DIGITMATCH** - Predicts the last digit will match your prediction
3. **DIGITOVER** - Predicts the last digit will be higher than your prediction
4. **DIGITUNDER** - Predicts the last digit will be lower than your prediction
5. **DIGITODD** - Predicts the last digit will be odd
6. **DIGITEVEN** - Predicts the last digit will be even

**How to Change:**
Simply replace `DIGITDIFF` with one of the options above.

## Pattern Analysis Customization

### Implementing Custom Logic

The bot provides a framework in the `tick_analysis` block where you can add custom pattern detection:

```xml
<block type="tick_analysis">
  <statement name="STATEMENT">
    <!-- Add your custom analysis here -->
    <!-- Example: Frequency analysis, sequence detection, etc. -->
  </statement>
</block>
```

### Example Pattern Strategies:

1. **Frequency Analysis**:
   - Track which digits appear most often
   - Bet against frequently appearing digits (DIGITDIFF)

2. **Sequence Detection**:
   - Look for repeating patterns (e.g., 1-2-3-1-2-3)
   - Predict next digit in sequence

3. **Hot/Cold Digits**:
   - Identify "hot" digits (appearing frequently recently)
   - Identify "cold" digits (haven't appeared in a while)

4. **Statistical Probability**:
   - Each digit (0-9) has ~10% probability
   - Look for deviations from expected distribution

## Performance Tuning

### For Different Market Conditions

#### High Volatility Markets
```
initial_stake: Lower (0.35 - 1.00)
digit_history_size: Smaller (5-8)
loss_threshold: Lower (2-3 losses)
multiplier: Conservative (1.2-1.3)
```

#### Low Volatility Markets
```
initial_stake: Moderate (1.00 - 3.00)
digit_history_size: Larger (12-15)
loss_threshold: Higher (4-5 losses)
multiplier: Moderate (1.4-1.6)
```

#### Trending Markets
```
Contract: DIGITOVER or DIGITUNDER
History: Larger sample (15-20)
Pattern: Look for directional bias
```

## Testing Recommendations

### Phase 1: Demo Testing (1-2 weeks)
- Use minimum stakes
- Test different configurations
- Record win/loss ratios
- Monitor drawdowns

### Phase 2: Small Live Stakes (1-2 weeks)
- Use 10-20% of intended stake size
- Validate demo performance
- Adjust based on real conditions

### Phase 3: Full Implementation
- Gradually increase to target stake
- Continue monitoring and adjusting
- Set strict daily/weekly limits

## Safety Features

### Built-in Protections

1. **Stake Cap**: Prevents excessive losses
   ```xml
   <variable id="max_stake">10</variable>
   ```

2. **Stop Loss**: Auto-stops on loss threshold
   ```xml
   <variable id="stop_loss">-10</variable>
   ```

3. **Take Profit**: Auto-stops on profit target
   ```xml
   <variable id="take_profit">10</variable>
   ```

### Recommended Additional Protections

1. **Time Limits**: Add session time restrictions
2. **Daily Trade Limit**: Cap number of trades per day
3. **Cool-down Periods**: Pause after significant wins/losses
4. **Manual Override**: Always be ready to stop the bot manually

## Common Configuration Mistakes

### ❌ Don't:
- Set initial_stake too high relative to balance
- Use aggressive Martingale without adequate capital
- Ignore stop_loss settings
- Run without monitoring for extended periods
- Skip demo testing phase

### ✅ Do:
- Start conservative and scale up gradually
- Test thoroughly on demo accounts
- Keep detailed performance records
- Adjust based on actual results
- Have a clear exit strategy

## Configuration Templates

### Template 1: Conservative Trader
```
initial_stake: 0.35
max_stake: 5
stop_loss: -5
take_profit: 3
loss_threshold: 4
multiplier: 1.2
```

### Template 2: Moderate Trader  
```
initial_stake: 1.00
max_stake: 10
stop_loss: -10
take_profit: 10
loss_threshold: 3
multiplier: 1.5
```

### Template 3: Aggressive Trader
```
initial_stake: 5.00
max_stake: 50
stop_loss: -25
take_profit: 25
loss_threshold: 2
multiplier: 1.8
```

⚠️ **Remember**: Always trade responsibly and within your means!

## Monitoring and Adjustment

### Key Metrics to Track

1. **Win Rate**: Target >50% for profitability
2. **Average Win/Loss**: Should favor wins
3. **Max Drawdown**: Monitor maximum consecutive losses
4. **Recovery Time**: How quickly bot recovers from losses
5. **Profit Factor**: Total wins / Total losses (target >1.5)

### When to Adjust

- **Low Win Rate (<45%)**: Review contract type and pattern logic
- **Large Drawdowns**: Reduce stake or multiplier
- **Slow Profit**: Consider higher initial stake (cautiously)
- **Frequent Max Stake Hits**: Increase max_stake or reduce multiplier

## Support and Resources

- Test configurations on demo accounts first
- Join Deriv community forums for strategies
- Document your configuration changes
- Review performance regularly

---

**Final Note**: No configuration guarantees profits. Always trade within your risk tolerance and never invest more than you can afford to lose.
