# Binomial Option Pricing Model

A compact Python project that demonstrates option pricing with binomial trees. The notebooks cover American option valuation, backward induction, and a runtime comparison between a simple loop-based implementation and a faster vectorized NumPy version.

## Project Structure

| File | Description |
| --- | --- |
| `american_option.ipynb` | Prices an American option using a binomial tree and checks early exercise value at every node. |
| `slow_vs_fast.ipynb` | Compares a nested-loop implementation with a vectorized NumPy implementation and visualizes runtime differences. |

## Binomial Tree Idea

The binomial model assumes that over each small time step, the stock price can move either up or down:

$$
S_{i,j} = S_0 u^j d^{i-j}
$$

where:

- $S_0$ is the initial stock price
- $u$ is the up factor
- $d$ is the down factor
- $i$ is the time step
- $j$ is the number of upward moves

The length of each time step is:

$$
\Delta t = \frac{T}{N}
$$

## Risk-Neutral Valuation

The risk-neutral probability of an upward move is:

$$
p = \frac{e^{r\Delta t} - d}{u - d}
$$

The discount factor for one step is:

$$
e^{-r\Delta t}
$$

For a European-style backward induction step, the option value is:

$$
C_{i,j} = e^{-r\Delta t}\left(pC_{i+1,j+1} + (1-p)C_{i+1,j}\right)
$$

## Payoff Functions

For a call option:

$$
C_T = \max(S_T - K, 0)
$$

For a put option:

$$
P_T = \max(K - S_T, 0)
$$

where $K$ is the strike price and $S_T$ is the stock price at maturity.

## American Option Early Exercise

American options can be exercised before expiration. At each node, the model compares the continuation value with the immediate exercise value.

For an American call:

$$
C_{i,j} = \max\left(S_{i,j} - K,\ e^{-r\Delta t}\left(pC_{i+1,j+1} + (1-p)C_{i+1,j}\right)\right)
$$

For an American put:

$$
P_{i,j} = \max\left(K - S_{i,j},\ e^{-r\Delta t}\left(pP_{i+1,j+1} + (1-p)P_{i+1,j}\right)\right)
$$

## Implementation Highlights

- Uses NumPy for efficient array calculations.
- Demonstrates backward induction from terminal payoffs to the present value.
- Shows how American options differ from European options through early exercise.
- Compares slow nested loops with a vectorized implementation.
- Includes runtime plots on both linear and logarithmic scales.

## Requirements

The notebooks use:

- Python 3
- NumPy
- Matplotlib
- Jupyter Notebook or JupyterLab

Install the dependencies with:

```bash
pip install numpy matplotlib notebook
```

## How to Run

Start Jupyter from this folder:

```bash
jupyter notebook
```

Then open either notebook and run the cells from top to bottom.

## Example Parameters

The notebooks use a simple starting setup:

| Parameter | Meaning | Example |
| --- | --- | --- |
| `S0` | Initial stock price | `100` |
| `K` | Strike price | `100` |
| `T` | Time to maturity in years | `1` |
| `r` | Annual risk-free rate | `0.06` |
| `N` | Number of time steps | `3` |
| `u` | Up factor | `1.1` |
| `d` | Down factor | `1/u` |
| `opttype` | Option type | `C` or `P` |

## Learning Outcome

This project is useful for understanding how option prices can be computed numerically when closed-form formulas are not flexible enough, especially for American options where early exercise must be considered.
