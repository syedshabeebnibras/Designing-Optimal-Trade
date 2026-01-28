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

𝑃(𝑥) = 𝛼𝑥 − 𝛽𝑥**2 − 𝑐𝑥

Where:
x = number of shares traded
αx = expected profit
βx**2 = market impact cost
cx = transaction cost
Expected returns increase profit, while market impact and transaction costs reduce it.
This function captures a realistic trading constraint: bigger trades are not always better.
