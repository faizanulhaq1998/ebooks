# Bot Examples and Extensions

This document shows practical examples of how to extend and customize the worm digit bot.

## Example 1: Simple Frequency Analysis

This example adds frequency analysis to avoid frequently appearing digits.

### Logic:
```
1. Track last 10 digits
2. Count how many times each digit (0-9) appears
3. If a digit appears 4+ times (40%+):
   - Use DIGITDIFF with that digit
4. Otherwise, pick a random digit for DIGITDIFF
```

### Implementation Snippet:
```xml
<!-- In your tick_analysis section -->
<block type="controls_forEach">
  <field name="VAR">last_digits</field>
  <statement name="DO">
    <!-- Count frequency of each digit -->
    <block type="math_change">
      <field name="VAR">frequency_count</field>
      <value name="DELTA">
        <block type="math_number">
          <field name="NUM">1</field>
        </block>
      </value>
    </block>
  </statement>
</block>
```

## Example 2: Streak Detection

Detect when the same digit appears multiple times in a row.

### Logic:
```
1. Check if last 3 digits are the same
2. If yes, use DIGITDIFF with that digit
3. High probability next digit will be different
```

### Pseudo-code:
```javascript
if (last_digits[7] == last_digits[8] && last_digits[8] == last_digits[9]) {
  predicted_digit = last_digits[9];
  contract_type = "DIGITDIFF";
}
```

## Example 3: Odd/Even Strategy

Simple odd/even alternating pattern detection.

### Logic:
```
1. Track if last 3 digits followed odd/even pattern
2. Example: [7(odd), 4(even), 3(odd), 8(even)] → expect odd next
3. Use DIGITODD or DIGITEVEN contracts
```

### Implementation:
```xml
<block type="controls_if">
  <value name="IF0">
    <block type="logic_compare">
      <field name="OP">EQ</field>
      <value name="A">
        <block type="math_modulo">
          <value name="DIVIDEND">
            <block type="lists_getIndex">
              <field name="MODE">GET</field>
              <field name="WHERE">LAST</field>
              <value name="VALUE">
                <block type="variables_get">
                  <field name="VAR">last_digits</field>
                </block>
              </value>
            </block>
          </value>
          <value name="DIVISOR">
            <block type="math_number">
              <field name="NUM">2</field>
            </block>
          </value>
        </block>
      </value>
      <value name="B">
        <block type="math_number">
          <field name="NUM">0</field>
        </block>
      </value>
    </block>
  </value>
  <statement name="DO0">
    <!-- Last digit is even, predict odd -->
    <block type="notify">
      <field name="MESSAGE">Last digit even, predicting ODD</field>
    </block>
  </statement>
</block>
```

## Example 4: Moving Average Strategy

Use statistical mean to predict high/low digits.

### Logic:
```
1. Calculate average of last 10 digits
2. If average < 4: Expect higher digits (use DIGITOVER with 5)
3. If average > 6: Expect lower digits (use DIGITUNDER with 5)
4. If average 4-6: Use DIGITDIFF with most frequent digit
```

### Calculation Example:
```
Last 10 digits: [2, 3, 1, 2, 4, 3, 2, 1, 3, 2]
Average: 2.3
Strategy: DIGITOVER with 4 or 5
```

## Example 5: Hot Digit Avoidance

Actively avoid digits that appeared very recently.

### Logic:
```
1. Look at last 3 digits
2. Never predict any digit from those 3
3. Randomly choose from remaining 7 digits
4. Use DIGITMATCH with chosen digit
```

### Code Structure:
```xml
<block type="lists_create_with">
  <field name="VAR">available_digits</field>
  <mutation items="10"></mutation>
  <!-- Create list [0,1,2,3,4,5,6,7,8,9] -->
</block>

<!-- Remove last 3 digits from available list -->
<block type="lists_setIndex">
  <field name="MODE">REMOVE</field>
  <!-- Remove recent digits -->
</block>

<!-- Pick random from remaining -->
<block type="lists_getIndex">
  <field name="MODE">GET_RANDOM</field>
  <value name="VALUE">
    <block type="variables_get">
      <field name="VAR">available_digits</field>
    </block>
  </value>
</block>
```

## Example 6: Worm Pattern Detection

True "worm" pattern - tracking directional movement.

### Logic:
```
1. Calculate differences between consecutive digits
   Example: [3, 5, 4, 6, 5] → differences: [+2, -1, +2, -1]
2. Identify trend: More positive = upward worm, More negative = downward
3. If upward worm: Use DIGITOVER
4. If downward worm: Use DIGITUNDER
```

### Pattern Recognition:
```
Upward Worm:     [2, 3, 4, 3, 5, 6, 5, 7]
Differences:      +1 +1 -1 +2 +1 -1 +2
Net movement:    +7 (upward trend)
Strategy:        DIGITOVER with (last_digit + 1)

Downward Worm:   [8, 7, 6, 7, 5, 4, 5, 3]
Differences:      -1 -1 +1 -2 -1 +1 -2
Net movement:    -5 (downward trend)
Strategy:        DIGITUNDER with (last_digit - 1)
```

## Example 7: Martingale Variations

Different stake management strategies.

### Conservative (Anti-Martingale):
```
After win: Increase stake by 10%
After loss: Reset to initial stake
```

```xml
<statement name="WIN">
  <block type="variables_set">
    <field name="VAR">current_stake</field>
    <value name="VALUE">
      <block type="math_arithmetic">
        <field name="OP">MULTIPLY</field>
        <value name="A">
          <block type="variables_get">
            <field name="VAR">current_stake</field>
          </block>
        </value>
        <value name="B">
          <block type="math_number">
            <field name="NUM">1.1</field> <!-- Increase by 10% -->
          </block>
        </value>
      </block>
    </value>
  </block>
</statement>

<statement name="LOSS">
  <block type="variables_set">
    <field name="VAR">current_stake</field>
    <value name="VALUE">
      <block type="variables_get">
        <field name="VAR">initial_stake</field>
      </block>
    </value>
  </block>
</statement>
```

### Aggressive (Classic Martingale):
```
After loss: Double the stake immediately
After win: Reset to initial
```

```xml
<statement name="LOSS">
  <block type="variables_set">
    <field name="VAR">current_stake</field>
    <value name="VALUE">
      <block type="math_arithmetic">
        <field name="OP">MULTIPLY</field>
        <value name="A">
          <block type="variables_get">
            <field name="VAR">current_stake</field>
          </block>
        </value>
        <value name="B">
          <block type="math_number">
            <field name="NUM">2</field> <!-- Double stake -->
          </block>
        </value>
      </block>
    </value>
  </block>
</statement>
```

## Example 8: Time-Based Trading

Only trade during specific times.

### Logic:
```
1. Get current hour
2. Only trade between 8 AM - 6 PM
3. Pause outside these hours
```

### Implementation:
```xml
<block type="controls_if">
  <value name="IF0">
    <block type="logic_operation">
      <field name="OP">AND</field>
      <value name="A">
        <block type="logic_compare">
          <field name="OP">GTE</field>
          <value name="A">
            <block type="epoch_time">
              <field name="TYPE">HOUR</field>
            </block>
          </value>
          <value name="B">
            <block type="math_number">
              <field name="NUM">8</field> <!-- 8 AM -->
            </block>
          </value>
        </block>
      </value>
      <value name="B">
        <block type="logic_compare">
          <field name="OP">LTE</field>
          <value name="A">
            <block type="epoch_time">
              <field name="TYPE">HOUR</field>
            </block>
          </value>
          <value name="B">
            <block type="math_number">
              <field name="NUM">18</field> <!-- 6 PM -->
            </block>
          </value>
        </block>
      </value>
    </block>
  </value>
  <statement name="DO0">
    <!-- Execute trade -->
  </statement>
  <statement name="ELSE">
    <!-- Skip trade -->
    <block type="notify">
      <field name="MESSAGE">Outside trading hours</field>
    </block>
  </statement>
</block>
```

## Example 9: Combination Strategy

Combine multiple patterns for higher confidence.

### Logic:
```
1. Check frequency analysis → Score 0-10
2. Check streak detection → Score 0-10
3. Check odd/even pattern → Score 0-10
4. Sum scores
5. If total > 20: High confidence trade
6. If total 15-20: Medium confidence (reduce stake 50%)
7. If total < 15: Low confidence (skip trade)
```

### Confidence-Based Stake:
```xml
<block type="controls_if">
  <value name="IF0">
    <block type="logic_compare">
      <field name="OP">GT</field>
      <value name="A">
        <block type="variables_get">
          <field name="VAR">confidence_score</field>
        </block>
      </value>
      <value name="B">
        <block type="math_number">
          <field name="NUM">20</field>
        </block>
      </value>
    </block>
  </value>
  <statement name="DO0">
    <!-- High confidence: Use full stake -->
    <block type="variables_set">
      <field name="VAR">trade_stake</field>
      <value name="VALUE">
        <block type="variables_get">
          <field name="VAR">current_stake</field>
        </block>
      </value>
    </block>
  </statement>
  <statement name="ELSE">
    <!-- Lower confidence: Use 50% stake -->
    <block type="variables_set">
      <field name="VAR">trade_stake</field>
      <value name="VALUE">
        <block type="math_arithmetic">
          <field name="OP">MULTIPLY</field>
          <value name="A">
            <block type="variables_get">
              <field name="VAR">current_stake</field>
            </block>
          </value>
          <value name="B">
            <block type="math_number">
              <field name="NUM">0.5</field>
            </block>
          </value>
        </block>
      </value>
    </block>
  </statement>
</block>
```

## Example 10: Statistical Validation

Only trade when patterns show statistical significance.

### Logic:
```
1. Track pattern success rate over last 20 trades
2. If pattern wins > 55%: Continue using
3. If pattern wins < 45%: Switch to different pattern
4. If pattern wins 45-55%: Reduce stake or pause
```

### Pattern Performance Tracking:
```xml
<variables>
  <variable id="pattern_wins">pattern_wins</variable>
  <variable id="pattern_trades">pattern_trades</variable>
  <variable id="win_rate">win_rate</variable>
</variables>

<!-- Calculate win rate -->
<block type="variables_set">
  <field name="VAR">win_rate</field>
  <value name="VALUE">
    <block type="math_arithmetic">
      <field name="OP">DIVIDE</field>
      <value name="A">
        <block type="variables_get">
          <field name="VAR">pattern_wins</field>
        </block>
      </value>
      <value name="B">
        <block type="variables_get">
          <field name="VAR">pattern_trades</field>
        </block>
      </value>
    </block>
  </value>
</block>

<!-- Only trade if win rate is good -->
<block type="controls_if">
  <value name="IF0">
    <block type="logic_compare">
      <field name="OP">GT</field>
      <value name="A">
        <block type="variables_get">
          <field name="VAR">win_rate</field>
        </block>
      </value>
      <value name="B">
        <block type="math_number">
          <field name="NUM">0.55</field> <!-- 55% win rate -->
        </block>
      </value>
    </block>
  </value>
  <statement name="DO0">
    <!-- Execute trade -->
  </statement>
  <statement name="ELSE">
    <!-- Pattern not performing, skip -->
    <block type="notify">
      <field name="MESSAGE">Pattern win rate too low, skipping</field>
    </block>
  </statement>
</block>
```

## Tips for Implementing Extensions

1. **Test Each Extension Separately**: Don't combine multiple extensions at once
2. **Use Notifications**: Add notify blocks to see what the bot is thinking
3. **Track Metrics**: Add variables to track performance of each strategy
4. **Start Simple**: Implement basic version first, then enhance
5. **Document Changes**: Comment your code so you remember what you did
6. **Version Control**: Save different versions as you experiment
7. **Demo Test**: Always test on demo for at least 50-100 trades

## Performance Expectations

### Realistic Goals:
- **Win Rate**: 52-58% is excellent
- **Profit Factor**: 1.3-1.8 is good
- **Max Drawdown**: Should be < 20% of balance
- **Recovery Time**: Should recover within 10-15 trades

### Warning Signs:
- Win rate < 45% for extended period
- Drawdown > 30% of balance
- Frequent max stake hits
- Unable to recover from losses

## Next Steps

1. Choose one example that interests you
2. Implement it in your bot
3. Test on demo for 1-2 weeks
4. Measure performance
5. Adjust and refine
6. Move to next example or combine strategies

---

**Remember**: These are examples for learning. Always test thoroughly before using real money. Not all strategies work in all market conditions.
