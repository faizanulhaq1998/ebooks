# Pattern Analysis Guide for Worm Digit Bot

This guide explains various digit analysis patterns and strategies you can implement in your bot.

## Understanding Last Digit Analysis

Each tick price on Deriv has a last digit (0-9). For example:
- Price: 1234.567 → Last digit: 7
- Price: 9876.543 → Last digit: 3

The bot tracks these last digits to identify patterns and make predictions.

## Common Pattern Strategies

### 1. Frequency Analysis

**Concept**: Track which digits appear most/least frequently.

**Theory**: 
- Over time, each digit should appear ~10% of the time
- Short-term deviations create trading opportunities

**Implementation Strategy**:
```
- Track last 10-20 digits
- Count frequency of each digit (0-9)
- If a digit appears >30% → Bet it WON'T appear (DIGITDIFF)
- If a digit appears <5% → Bet it WILL appear (DIGITMATCH)
```

**Example**:
```
Last 10 digits: [7, 3, 7, 2, 7, 5, 7, 1, 7, 4]
Digit 7 appears 5 times (50%)
Strategy: Use DIGITDIFF with prediction = 7
```

### 2. Hot and Cold Digits

**Concept**: Identify "hot" (frequently appearing) and "cold" (rarely appearing) digits.

**Hot Digit Strategy**:
- Digit appears 3+ times in last 10 ticks
- Two approaches:
  - Bet it continues (momentum trading)
  - Bet against it (mean reversion)

**Cold Digit Strategy**:
- Digit hasn't appeared in last 10+ ticks
- Probability suggests it's "due" to appear
- Use DIGITMATCH with cold digit

**Example**:
```
Last 15 digits: [2, 3, 4, 5, 6, 7, 8, 9, 0, 1, 2, 3, 4, 5, 6]
Digit 7 appeared once
Digit 8 appeared once
Digit 9 appeared once
These are "cold" - consider betting on them
```

### 3. Sequence Pattern Detection

**Concept**: Look for repeating sequences or patterns.

**Pattern Types**:

a) **Repeating Digit**:
```
[5, 5, 5, ...] → Next likely: NOT 5
Strategy: DIGITDIFF with 5
```

b) **Alternating Pattern**:
```
[3, 7, 3, 7, 3, ...] → Next likely: 7
Strategy: DIGITMATCH with 7
```

c) **Ascending/Descending**:
```
[1, 2, 3, 4, 5, ...] → Next likely: 6
Strategy: DIGITMATCH with 6
```

d) **Gap Pattern**:
```
[2, 4, 6, 8, ...] → Next likely: 0
Strategy: DIGITMATCH with 0
```

### 4. Streak Breaking

**Concept**: After consecutive appearances, a digit is less likely to appear again.

**Implementation**:
```
- Track consecutive appearances of same digit
- After 2-3 consecutive: Bet DIGITDIFF
- After 4+ consecutive: Strong DIGITDIFF signal
```

**Example**:
```
[..., 3, 3, 3, 3]
Strong signal: Next digit ≠ 3
Strategy: DIGITDIFF with 3
```

### 5. Statistical Mean Reversion

**Concept**: Prices tend to revert to statistical mean.

**Implementation**:
```
- Calculate average of last N digits
- If current digit >> average: Bet lower (DIGITUNDER)
- If current digit << average: Bet higher (DIGITOVER)
```

**Example**:
```
Last 10 digits: [2, 3, 2, 4, 3, 5, 4, 3, 8, 9]
Average: 4.3
Last digit: 9 (much higher than average)
Strategy: DIGITUNDER with 5 or 6
```

### 6. Odd/Even Patterns

**Concept**: Track odd vs even digit patterns.

**Implementation**:
```
Last 10 digits: [O, E, O, E, O, E, O, E, O, ?]
Clear alternating pattern
Strategy: DIGITEVEN
```

**Distribution Analysis**:
```
Last 20 digits: 15 odd, 5 even
Strategy: DIGITEVEN (mean reversion)
```

### 7. Range Analysis

**Concept**: Classify digits into ranges.

**Ranges**:
- Low: 0-3
- Medium: 4-6
- High: 7-9

**Strategy**:
```
If last 5 digits all "High" → Next likely "Low" or "Medium"
Use DIGITUNDER with 6
```

### 8. Worm Strategy (Specific)

**Concept**: Named strategy that tracks digit "movement".

**Implementation**:
```
- Track if digits are generally increasing or decreasing
- Classify as "upward worm" or "downward worm"
- Bet in direction of the trend
```

**Example - Upward Worm**:
```
[2, 3, 4, 3, 4, 5, 6, 5, 6, 7]
General upward trend
Strategy: DIGITOVER with last digit
```

**Example - Downward Worm**:
```
[8, 7, 6, 7, 6, 5, 4, 5, 4, 3]
General downward trend
Strategy: DIGITUNDER with last digit
```

## Advanced Techniques

### 1. Weighted Frequency

Give more weight to recent digits:
```
Last 10 digits with weights:
Position 1 (oldest): weight 1
Position 5: weight 5
Position 10 (newest): weight 10

Calculate weighted frequency for better prediction
```

### 2. Multiple Timeframes

Analyze different history sizes:
```
- Short-term: Last 5 digits (immediate patterns)
- Medium-term: Last 10 digits (current trend)
- Long-term: Last 20 digits (overall bias)

Trade when all timeframes align
```

### 3. Probability Scoring

Assign probability scores to each digit:
```
For each digit 0-9:
- Base probability: 10%
- Add/subtract based on frequency
- Add/subtract based on recent appearance
- Add/subtract based on patterns

Choose digit with highest/lowest score
```

### 4. Combination Strategy

Combine multiple patterns:
```
Pattern 1: Digit 7 is "hot" (appeared 3 times in last 10)
Pattern 2: Last 2 digits were 7
Pattern 3: Frequency > 30%

All patterns suggest: Next ≠ 7
Strong confidence: DIGITDIFF with 7
```

## Practical Implementation

### Decision Tree Example

```
1. Check for consecutive digits (3+ same)
   YES → DIGITDIFF
   NO → Continue

2. Check frequency distribution
   Any digit >30%? → DIGITDIFF with that digit
   Any digit <3%? → DIGITMATCH with that digit
   NO → Continue

3. Check for sequences
   Pattern detected? → Follow pattern logic
   NO → Continue

4. Check odd/even distribution
   Heavily skewed? → Bet on minority
   NO → Continue

5. Default: Use most recent pattern or random
```

## Performance Metrics

### Evaluating Pattern Effectiveness

Track these metrics for each pattern:
- **Win Rate**: % of winning trades
- **Sample Size**: Number of trades tested
- **Profit Factor**: Total wins / Total losses
- **Max Drawdown**: Largest losing streak
- **Recovery Time**: Time to recover from drawdown

### Pattern Reliability Score

```
Score = (Win Rate × 100) - 50
Examples:
- 60% win rate = Score: 10 (Good)
- 55% win rate = Score: 5 (Acceptable)
- 50% win rate = Score: 0 (Break-even)
- 45% win rate = Score: -5 (Bad)
```

Target: Patterns with Score > 5

## Testing Your Patterns

### Step-by-Step Testing

1. **Historical Data Analysis**:
   - Review past tick data
   - Test pattern on historical data
   - Calculate theoretical win rate

2. **Demo Account Testing**:
   - Run bot with pattern on demo
   - Minimum 50-100 trades
   - Track all metrics

3. **Optimization**:
   - Adjust parameters based on results
   - Test variations of the pattern
   - Find optimal settings

4. **Live Testing**:
   - Start with minimum stakes
   - Monitor for 1-2 weeks
   - Compare to demo results

### Warning Signs

Stop using a pattern if:
- Win rate consistently <45%
- Max drawdown exceeds comfort level
- Pattern stops working after market changes
- Better patterns are discovered

## Combining with Risk Management

### Pattern Confidence Levels

Adjust stake based on pattern confidence:

```
High Confidence (multiple patterns align):
- Use 100% of current_stake

Medium Confidence (one strong pattern):
- Use 75% of current_stake

Low Confidence (weak or no pattern):
- Use 50% of current_stake
- Or skip the trade
```

### Pattern-Based Stop Loss

```
After 5 consecutive losses using same pattern:
- Pause and review pattern
- Switch to different pattern
- Take a break
```

## Common Pitfalls

### ❌ Avoid These Mistakes:

1. **Over-fitting**: Pattern works on historical data but fails live
2. **Confirmation Bias**: Seeing patterns that don't exist
3. **Ignoring Sample Size**: Drawing conclusions from too few trades
4. **Pattern Mixing**: Using conflicting patterns simultaneously
5. **Static Patterns**: Not adapting to market changes

### ✅ Best Practices:

1. Test patterns thoroughly before live use
2. Use multiple metrics to evaluate patterns
3. Keep patterns simple and logical
4. Adapt patterns to changing conditions
5. Document what works and what doesn't

## Example Pattern Implementation

### Simple Moving Average Pattern

```
1. Collect last 10 digits
2. Calculate average (e.g., 5.2)
3. Round average to nearest integer (5)
4. If current digit > average + 2:
   → DIGITUNDER with (average + 1)
5. If current digit < average - 2:
   → DIGITOVER with (average - 1)
6. Else:
   → DIGITDIFF with most frequent digit
```

## Resources and Further Learning

- Study the provided strategy PDFs in this repository
- Keep a trading journal with pattern notes
- Review Deriv community strategies
- Test and refine continuously

---

**Remember**: No pattern guarantees profits. The goal is to find patterns with >50% win rate combined with proper risk management for long-term profitability.
