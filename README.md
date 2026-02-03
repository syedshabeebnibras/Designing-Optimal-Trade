# Designing-Optimal-Trade
This project designs a single trade using quantitative reasoning and single-variable calculus. Each decision is framed as an optimization problem, balancing reward, cost, and risk. Trade intuition is converted into objective functions, and derivatives are used to justify optimal parameters step by step.

## **Section 0: Framing the Problem**
Before introducing the mathematics, we define the core principle of this project: every trading parameter is an optimization decision with explicit trade-offs.
Choosing trade size, leverage, or stop-loss levels implicitly solves an optimization problem balancing expected return, cost, and risk. This project makes that process explicit by translating trading intuition into mathematical objective functions.
We analyze a single trade and optimize one variable at a time to build clear intuition for how calculus directly informs rational, systematic trading decisions.

## **Section 1: Modeling Profit as a Function of Trade Size**

### **1.1 Define the Profit Function**
When executing a trade, profit depends on three key components:

Expected return per share (α): The average profit you expect to earn per unit traded.

Market impact (βx²): Larger trades move the market against you, increasing execution cost nonlinearly.

Transaction costs (c·x): Linear costs such as commissions, fees, and bid–ask spread.

We model profit as a function of trade size x:

              𝑃(𝑥) = 𝛼𝑥 − βx² − 𝑐𝑥

Where:

x = number of shares traded

αx = expected profit

βx² = market impact cost

cx = transaction cost

Expected returns increase profit, while market impact and transaction costs reduce it.
This function captures a realistic trading constraint: bigger trades are not always better.

### **1.2 Shape Analysis Before Calculus**
Before computing derivatives, let's build intuition about the profit function's shape.
As trade size x increases from 0, what happens to profit P(x)?

Ans: Profit grows initially, reaches a peak, then decreases
    WHY?
    
The profit function is a downward-opening parabola (negative coefficient on x²). It starts at 0, increases as the linear term dominates, reaches a maximum at the vertex, then decreases as the quadratic market impact term dominates. This is why there's an optimal trade size, too small leaves money on the table, too large gets crushed by market impact.

### **1.3 Finding the Optimal Trade Size**
To find the optimal trade size, we take the derivative and set it to zero:

                              P'(x) = α − 2βx − c = 0

Solving for x:

                              x* = (α − c) / 2β

### **1.4 Verifying Optimality**
We found a critical point, but is it a maximum? The second derivative tells us:

                               P′′(x) = −2β

Example:
Given that β>0, what does P ′′(x)=−2β<0 tell us about the profit function?

Ans: The function is concave (opens downward), so the critical point is a maximum
      WHY?
      
A negative second derivative means the function is concave (curves downward like an upside-down bowl). For concave functions, critical points are maxima. This confirms our x* is indeed the profit-maximizing trade size, not a profit-minimizing one.

What does this concavity (negative second derivative) mean for decision-making under costs?

Ans: Diminishing returns gurantee an optimal interior point where costs start to outweigh benefits
    WHY?

Negative concavity means diminishing marginal returns: each additional unit contributes less than the previous one. Eventually, the marginal cost exceeds the marginal benefit, guaranteeing an interior maximum exists. This is why 'more is not always better in trading, there's a sweet spot where you stop.

### **1.5 Sensitivity Analysis**
How does the optimal trade size change when parameters change?

Recall the formula:

x∗ = (α − c) /  2β 
​
where 

α is expected return per share, 

c is transaction cost per share, and 

β is the market impact coefficient.

If transaction costs c increase, what happens to the optimal trade size x∗ ?

Ans: x* decreases (trade less)
WHY?

From x* = (α - c) / (2β), increasing c decreases the numerator, so x* decreases. Higher costs make trading less attractive, so you should trade smaller sizes. This is intuitive, when trading is expensive, do less of it.

### **1.6 Implementation: Profit Function?**
Now let's implement what we've learned. Write a Python function that computes the profit and finds the optimal trade size.

```python
def profit_analysis(alpha, beta, c):
    """
    Analyze the profit function P(x) = αx - βx² - cx
    """
    import numpy as np

    # Optimal trade size from first-order condition
    optimal_x = (alpha - c) / (2 * beta)

    # Maximum profit at optimal x
    max_profit = alpha * optimal_x - beta * optimal_x**2 - c * optimal_x

    return (optimal_x, max_profit)
```


## **Section 2: Optimal Leverage and Risk Penalty**

### **2.1 Introducing Leverage**
Leverage lets you control more capital than you actually have. If you have $10,000 and use 3x leverage, you control $30,000 worth of assets, but you only put up $10,000 of your own money.

The catch: leverage amplifies both gains AND losses. A 10% gain becomes 30%, but a 10% loss also becomes 30%.

The Liquidation Problem

Here's the critical insight: you can't lose money you borrowed, you can only lose your money. The broker won't let the position go negative on their funds.

With 10x leverage on $10,000, you control $100,000. Your $10,000 is your margin, the buffer protecting the borrowed $90,000. If the position drops 10%, you lose $10,000 which is all your money. You get liquidated, the broker force-closes your position before it can lose their money.

The formula: Liquidation % = 100% / Leverage

At 20x leverage, a mere 5% dip wipes you out. Bitcoin moves 5% on a slow Tuesday.

### **2.2 Risk-Penalized Return Function**
A rational investor doesn't just maximize expected return, they penalize for risk. The risk-adjusted return with leverage ℓ is:

              R(ℓ) = ℓ*μ − 0.5 * ℓ2 * σ2
where:

μ = expected return (e.g., 8% annually)

σ = volatility (e.g., 20% annually)

ℓ = leverage multiplier

The term 
0.5 * ℓ2 * σ2 is the risk penalty, it grows quadratically with leverage.

### **2.3 Finding Optimal Leverage**
Taking the derivative and setting to zero:

                R′(ℓ) = μ − ℓ*σ2 = 0

                ℓ∗ = μ / σ2

This is the famous **Kelly Criterion** (in its simplest form)!

### **2.4 Implementation: Kelly Criterion**
```python
def kelly_analysis(mu, sigma):
  """
  Analyze optimal leverage using the Kelly Criterion.

  The risk-adjusted return is: R(ℓ) = ℓμ - (1/2)ℓ²σ²
  Optimal leverage is: ℓ* = μ / σ²

  Args:
      mu: expected return (e.g., 0.08 for 8%)
      sigma: volatility (e.g., 0.20 for 20%)

  Returns:
      tuple: (optimal_leverage, max_risk_adjusted_return)

  Example:
      >>> kelly_analysis(0.08, 0.20)
      (2.0, 0.08)
  """
  import numpy as np
  #TODO: Calculate optimal leverage ℓ* = μ / σ²
  optimal_leverage = mu / (sigma ** 2)

  #TODO: Calculate max risk-adjusted return at optimal leverage
  max_return = optimal_leverage * mu - 0.5 * (optimal_leverage ** 2) * (sigma ** 2)
  return (optimal_leverage, max_return)
  pass
```
