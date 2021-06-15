
# Interest Rates Models

## Short rate models

__Def__: A process for __unobservable__ continuously compounding interest rate,

$$dr = \alpha(r,t)dt + \beta(r,t)dW_t$$

Implied dynamics for money market account:

$$B(t) = \rm exp(\int_0^t r(s)ds)$$

__Cons:__ 

- Dynamics aren't compatible with standard market prices from Black's formula, which makes the calibration and fitting $\alpha, \beta$ difficult. 
- At time t, attainable yield curves is one dimensional, which is too limiting for most exotic interest rate derivatives. For products which derive their value from changing shape of the yield curve aren't priced well.

__Pros:__

Lattice method implementation is fast, and ideal for early exercise products, giving short rate models an advantage over Monte Carlo implementation of a market model.

## Market Models

__Def__: A process for __observable__ quantities (forward rates, swap rates). In forward LIBOR market model, dynamics for forward rates are:

$$\frac{df_i}{f_j} = \mu_j dt + \sigma_t dW_t^{(j)}$$

Vol $\sigma_j$ price the caplet on $j$-th forward rate correctly. So calibration to the current yield curve and at-the-money caplet market is automatic as model is consistent with Black's formula.

The set of attainable yield curves is high dimensional, depending on the chosen dimension of Brownian motion $W_t$, which makes market models more suited to exotic products. Due to high dimensionality, market model must be implemented using Monte Carlo.

## Markov Functional Models

__Def__: To incorporate some good features of both short rate models and market models. We have market rates and instruments as a function of a low-dimensional Markov process $x$ where

$$dx(t) = \lambda(t) dW_t$$

We only need to keep track of underlying Markov process, meaning they can be priced using a lattice. They also allow for easy calibration to market prices as there is freedom to choose functional form of the model.

Similarly for short rate models, the assumption that space of yield curve attainable at a time is expressed by 1 or 2 random factors can be too simplistic to givr good prices for exotic instruments.

## Under what condition is LIBOR market model Markovian?

Volatility function is __separable__, if we ignore drafts. Volatility function for a LIBOR rate is:

$$\sigma_i(t) = v_i \times \nu(t)$$

where $v_i$ is a LIBOR specific scalar, $\nu(t)$ is a deterministic function of time, which is common to all LIBOR rates.

LIBOR market model is never Markovian in BM since it contains state-dependent drifts. But, these can be approximated using schemes like predictor-corrector to make rates functions of underlying increments only across a single step. 

# Option Pricing

## Longstaff-Schwartz algorithm 

### Monte Carlo and Binomial Tree

Binomial tree is easy to do early exercise, its rate of convergence for continuous payoffs including American options is $1/n$, where $n$ is steps number. Since number of computations increases as $n^2$, so the rate of convergence is $t^{-1/2}$ where $t$ is computational time. For higher dimensional trees, convergence will be slower. Path-dependence is not natural in trees but can be dealt with by using an auxiliary variable which increases the dimension by 1.

Monte Carlo pros:

- Convergence is order $t^{-1/2}$ in all dimensions.
- Path-dependency is easy.

Monte Carlo cons:

- Early exercise is hard. 
- Slowly converge in low dimensions, although not slower than binomial tree, there are other faster lattice methods in low dimensions.

__Rule of Thumb__: Use binomial trees for low-dimensional problems involving early exercise, and Monte Carlo for high-dimensional problems involving path-dependence.

### Longstaff-Schwartz for pricing an early exercisable option with Monte Carlo

It's difficult to use Monte Carlo for early exercise (American options). Longstaff-Schwartz method can handle early exercise with Monte Carlo.

When pricing an early exercisable product, we need to know whether it will be worth more by exercising now or by holding onto it. The value of holding onto the option is the continuous value, is what Longstaff-Schwartz method estimates via regression. It's commonly used for Bermudan swaptions.

__Longstaff-Schwartz algorithm__:

- Run 1000 Monte Carlo paths and store the exercise value divided by the numeraire at each possible exercise date for each path. We also need to store the value of basis functions at the same points.
- With all data we now look at the second last exercise date. We perform a regression (least-squares) of the value at the last exercise date against basis functions. This gives coefficients for basis functions.
- Using coefficients, we can calculate continuation value at the second last exercise time for each path
- We replace the deflated exercise value stored initially with continuation value, if the continuation value is greater.
- Continue back through the exercise dates until reaching time 0. The biased value of product will be average of time 0 values multiplied by initial numeraire value.

## Incomplete Markets

### Implied volatility and volatility smile

__Implied Volatility__: Volatility implied from market price of an option. It's calculated taking the option's price and finding the volatility in the Black-Scholes formula that returns the same price.

*Ex:* At-the-money European call option with 1-year to expiry, spot value 100, risk free rate 5%, option price $6.8, then implied volatility = 15%.

__Volatility smile__: If calculate implied volatility with the same expiry date but different strike prices and plots the volatilities, there is a smile.

Reasons:

- Market is likely to move up/down by a large amount than is assumed within Black-Scholes model. So the smile reflects the market's view of the imperfections in Black-Scholes model.

__Volatility skew__ is a downward sloping compared to symmetric smile. 

## FX Exotic option pricing consistent with market smiles

### Jump diffusion

__Jump diffusion__: have FX rate moving as geometric Brownian motion with an added random jump, as a Poisson process. $S_t$ = exchange rate:

$$\frac{dS_t}{S_t} = \mu dt \sigma dW_t +(J-1)dN_t$$

\where $J$ is a random jump size, could be log-normal distribution, $N_t$ = Poisson process  

Jump diffusion produce a floating smile, the smile moves when spot moves, so that the bottom of the smil will remain at-the-money. As a floating smile is a feature of FX markets, it's an advantage of jump diffusion models.

__Cons:__ Smiles produced flatten with maturity quickly, which is not true in most markets.

### Stochastic Volatility

Take both FX rate and volatility process to be stochastic. 

Heston model:

$$\frac{dS_t}{S_t} = \mu dt +  \sqrt{V_t} dW_t^{(1)}$$

$$dV_t = \kappa(\theta - V_t) dt + \sigma_V \sqrt{V_t} dW_t^{(2)}$$

where $\theta$ is long term average volatility, $\kappa$ is the rate of mean-reversion of the process, $\sigma_V$ is vol of vol, $W_t^{(1)}$ and $W_t^{(2)}$ are BMs with correlation $\rho$.

Stochastic volatility models produce a floating smile and their smile shape can be changed by tweaking parameters. Ex: skew can be introduced by having $\rho \neq 0$, the flattening out of the smile can be adjusted by mean reversion parameter $\kappa$.

__Cons of flexibility of changing smile shape__: All parameters need to be fitted in a stable and consistent way with market, which is not straightforward.

### Variance Gamma

Variance Gamma takes FX rate and time as stochastic process. 

__Def__: FX rates move when market information arrives and this information arrival is a stochastic process. If a large amount of information is arriving, then we expect high volatility, but on a day with minimal information arrival, we can expect low volatility. $b(t; \theta,\sigma)$ is a BM,

$$db_t = \theta dt + \sigma dW_t$$

Let

$$X(t;\sigma, \nu,\theta) = b(T_t;\theta,\sigma)$$

where $T_t$ is time $t$ value of a Gamma process with variance rate $\nu$.

Variance Gamma smiles are similar to jump diffusion smiles, the smiles tend to flatten out over time as the model becomes more similar to a pure diffusive model.

### Local Volatility

Local volatility derives volatility surface from quoted market prices, and use this surface to price more exotic instruments. Under risk-neutrality, there's a unique diffusion process consistent with market prices. The unique volatility function $\sigma(S_t, t)$ is local volatility function.

Once local volatility surface is found, exotic instruments can be priced consistently with market price. The problem is that the volatility surface is market's current view of volatility, and this will change in future, so exotic options will no longer be consistent with market prices.

## Stochastic Volatility Model and Smile

Stochastic Volatility model takes volatility to be driven by stochastic process. It's possible that volatility will become large causing large movements in underlying asset price. These large moves give the distribution of underlying asset fatter tails than in Black-Scholes model. So, the options away from money will be priced more expensively than in Black-Scholes leading to a volatility smile.

Also, look at second derivative of option price w.r.t. volatility. In Black-Scholes world, Vega sensitivity (how good our volatility hedge is) = 0. But a stochastic volatility model does not necessarily have a non-zero derivative of Vega, so there is __Vega convexity__ which is related to volatility smile.

# C++

## Difference: `inline` and `macro`

`inline`

Place in front of a function declaration = want to replace each call to the function with the code inside the function. This eliminates the overhead of making a call to the function.

__Notes:__

- Code may be longer, because the definition of function is repeated many times
- Code must be included in the header, so as to be available to clients, so changing it will cause a lot to recompile, and any compiler-time dependencies it has, will be dependencies for clients
- Compiler can use extra optimizations
- Adding/ Removing `inline` never affects anything other than the efficiency of program.

`macro`

Has nothing to do with compiler. It's an instruction to C-preprocessor or Cpp. Cpp reorganizes a file before the compiler starts work by including all files specified, and by expanding macros. A macro is a direct text to text substitution which takes place before the compiler does its job. So it's a error prone tool and should be avoided.

__Similarities:__ They can be used without the run-time overhead of a function call, and without duplicating code.

## Is it possible to throw in a constructor in C++?

There are varying opinions on it. If you throw in a constructor, then the destructor for the object is not called. And it will never be called since the object won't exist after a throw during its construction. But the destructor for each fully-constructed data member is called. If you're in the main part of the constructor, that's within the curly brackets, then all the data members are fully constructed and destroyed.

Why not? Because destructor won't be called, and it won't do the clean-up that is usually done when the object is destroyed. Any memory allocated in the constructor won't be released.

Why OK? Throw in a constructor provided you ensure everything is properly destroyed and deallocated. The easiest way to do is to use smart pointers as data members instead of raw pointers. These will be destroyed on exit and will automatically carry out the required deallocations.

## Copy polymorphic object of unknown type

Use virtual constructor. We can't declare a standard constructor virtual, but we can achieve the same effects by making a class method virtual.

Define in base class:

```cpp
virtual Base* clone() const =0;
```

And in each inherited class:

```cpp
virtual Inherited* clone() const
{
    return new Inherited(*this);
}
```

Inherited class knows its own type. and so can copy itself without any problems. The return type is a pointer to an object of the class allocated on the heap, since the type is unknown to the calling code.

The return type is a pointer to the inherited class, not the base class; this changing of return types is allowed when overriding in an inherited class.

## If base-class has uder-defined assignment operator, is one needed for an inherited class?

No. If I have a class X inherited from Y, and I code

```cpp
X a;
X b;
a = b;
```
 
without defining  an assignment operator for X, then the compiler uses the compiler-generated one. This calls the assignment operator for each base and member of X so whether Y has a user-defined operator or an implicit one doesn't matter. 

## Declare function pointer

Declare a pointer `MyPointer` to a function returning an object of type A and taking in an object of type B:

```cpp
A (*MyPointer)(B);
```

If we want to assign pointer to a function `MyFunction`, then:

```cpp
A MyFunction(B);

void f()
{
    A (*MyPointer)(B);
    MyPointer = &MyFunction;
}
```

## Virtual Function

Virtual function allows us to defer the implementation of a function declared in a base class to an inherited class. If we have vanilla option class, we can make PayOff method virtual. We then inherit calls and puts from option class.

The PayOff method is defined differently from calls and puts. User of an option object may not know which sort of option is, and doesn't need to since the virtual function for the right sort of option is called automatically.

## `strcmp` less than

```cpp
// function signature

int strcmp(const char *str1, const char *str2);

int mystrcmp(const char *str1, const char *str2)
{
    while (*str1 != '\0' && *str2 != '\0' && *str1 == *str2)
    {
        ++str1;
        ++str2;
    }
    if (*str1 < *str2)
        return -1;
    if (*str1 > *str2)
        return 1;
    return 0;
}
```

Although pointers point to const, they're not const themselves. 

## Virtual destructor

If one has an object of inherited class that has been dynamically allocated (created using `new`), then when it's deleted via a pointer to a base class object, one has the issue that the correct destructor needs to be called. If destructor of base class is virtual, then the inherited class one is called directly and no problem.

If it's not virtual, then the base class destructor is called and the object may not be properly deleted. This causes memory leak or even worse. 

So we should always declare destructors virtual if any method is pure abstract, i.e. not defined in the base class. 

## Class B derived from A and contains an object of class C, which order is called in constructor/destructor for B?

The more fundamental one is, the sooner it's constructed and later destructed. A is constructed then C then B, for destruction first B then C then A.

## What does every class equipped with?

Compiler provides a copy constructor, an assignment operator and a destructor. Copy constructor calls the copy constructor of each data member to make a *shallow copy*, the assignment operator calls the assignment operator of each data member. It's OK to use shallow copy if every data member has a copy constructor that does the right thing. It's not the case if data members are pointers, since both old and new objects will point to the same data. This can result in crashes when the point-to object is deleted twice. The same issue applys with assignment operators.

A non-trivial destructor is needed iff class needs to free resources when object dies. This is deleteing allocated memory. A trivial destructor should be declared virtual if object is likely to be deleted via a pointer to a base class.

The best way to get around declaring non-trivial copy constructors, destructors and assignment operators is to use smart pointers that do all correct things when the default behavior is used.

## Convert object from type A to type B

If a class has single argument construtor, then the compiler will treat it as a conversion operator.

```cpp
B( const A& myObject);

// or

B(myObject);
```

Another method: if we convert into objects of type B when class declaration isn't accessible. Suppose B is type `double`: we can't write a new constructor for `doubles`. We can define an operator in class A whose name is B so we include the line

```cpp
operator B() const;
```

in the class declaration for A. When we define operator, we code as

```cpp
B::operator A() const
{
    // create object of type B and return it
}
```

## `const`

Places to be used:

- on a variable declaration
- on an argument of a function or method
- on a method of a class
- on a class data member
- in the declaration of a pointer

Arguments are often passed by reference rather than by value to avoid overhead of making a copy. If passed by reference, then its value can be changed within routine, and the changes propagate outside. Putting a `const` stops it changing.

`const` on a method of a class indicates the class won't change the class's internal state. Inside the method the class's data members can't be changed easily.

`const` on class data member = data member must be initialized in contructor's initialization list, and can't be changed then. Side effect is that the class can't have an assignment operator, since such an operator would change the data member's value.

Pointers can be `const` in 2 ways:
- the object pointed to can be `const`, and 
- the location of pointer can be `const`.

This is distinct from references, which can only be `const` in one way. 

`const_cast` can remove constness from pointers. But if an object was originally declared `const`, an attempt to modify it will result in undefined behavior. 

## `static`

`static` is initialized the first time it's encountered, but doesn't go out of scope when function exits, instead the value persists until the next time function is called and it's then reused. This has advantage of mimicking class-like behavior in C but less useful in C++. It can be used in C++ to reduce time spent on creating objects. 

But, this use is incompatible with multi-threaded programming, since the function could be entered by 2 different threads both wanting to access the same data. It's used in singleton pattern to allow one instance of a class ot exist which is a `static` variable member inside a `static` member function.

So, we have `static` member functions, such a function is a method of a class which isn't associated with an object of class. 

```cpp
MyClass::MyMethod();
```

rather than

```cpp
MyObject.MyMethod();
```

The third usage of `static` is to specify a function which has no linkage outside the translation unit. If we declare a function `static`, we can't call it from another file. This usage is archaic, since it has been replaced by the concept of an anonymous namespace. 

The 4-th use of `static` is to denote class data members for which there's only one copy for entire class rather than 1 for each object in the class. There're issues with thread safety.

## Polymorphism


