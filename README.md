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

Negative concavity means diminishing marginal returns: each additional unit contributes less than the previous one. Eventually, the marginal cost exceeds the marginal benefit, guaranteeing an interior maximum exists. This is why 'more is not always better' in trading, there's a sweet spot where you stop.
​
 

