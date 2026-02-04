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


## **Section 3: Stop-Loss Placement as an Optimization Problem**

### **3.1 Why Stops Are a Tradeoff**
A stop-loss automatically exits your position if the price drops to a certain level. But there's a fundamental tension:

-Tight stops (close to entry): Limit losses but trigger frequently on normal volatility

-Loose stops (far from entry): Rarely trigger but allow large losses when they do

### **3.2 Expected Value Model**
We can model expected profit using expected value: multiply each outcome by its probability and sum them up. With a stop-loss, there are two outcomes: you either win (hit your profit target) or lose (get stopped out).

             E(s) = p(s)⋅G − (1−p(s))⋅s

where:

p(s) = probability of winning (reaching profit target before stop)

G = gain if you win (fixed target)

s = loss if stopped out

The first term is your expected gain (probability of winning × gain), and the second term is your expected loss (probability of losing × loss amount).

Key insight: 
p(s) increases with s (looser stops are less likely to trigger).


## **Section 4: Execution Speed vs Slippage**
### **4.1 Time as a Decision Variable**
When you want to buy 10,000 shares, you don't have to buy them all at once. Execution speed is how quickly you complete your entire order, you could execute in seconds (aggressive) or spread it over hours (passive).

Why does this matter? Because of slippage, the difference between the price you expected and the price you actually got. When you buy aggressively, you consume all available shares at the best price, then the next-best price, then worse prices. Your average fill price ends up higher than where the market started. That's slippage.

When executing a trade, you face a tradeoff:

-Fast execution: Get your order filled quickly, but pay more slippage (your buying pressure moves the price against you)

-Slow execution: Minimize slippage by letting the market replenish between your orders, but risk the price drifting away while you wait (opportunity cost)

### **4.2 Cost Function in Time**
Let's model the total cost of executing over time t:

           C(t)= k/t + λt

Why this formula?

Slippage cost = k/t: If you execute faster (smaller t), you hit the market harder, causing more slippage. The constant kcaptures how "impactful" your total order is. Spreading it over more time reduces the cost, hence k divided by t.

Opportunity cost =λt: The longer you wait, the more the market can drift away from your target price. If the stock trends up while you're slowly buying, you miss the lower prices. This cost grows linearly with time, hence λ times t.

The parameter λ represents how much the market typically moves per unit time (related to volatility and drift).

4.3 Optimal Execution Time
Taking the derivative:

         C′(t) = − k/t**2 + λ = 0

Solving:

         t∗ =  $\sqrt{k/λ}$


​
 
​
