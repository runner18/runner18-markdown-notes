## Algebra, Exponents, Logarithms
### Multiplying Numbers w Exponents
Add the exponents
$3x^2(4x^3)$
$2+3=5$
$12x^5$

### Dividing Numbers w Exponents
Here you subtract the exponents:
$\dfrac{e^a}{e^b}=e^{a-b}$

## When Do you Multiply Exponents?
$(x^a)^b = x^{a*b}$

Example:
$(3x^2)^3$
$27x^6$
### Difference of Squares
$x^2-9 = (x+3)(x-3)$

$(x+3)(x-3)$
$x^2+3x-3x+9$
$x^2-9$

Example:
$\dfrac{x^2-9}{x-3}$

$\dfrac{(x+3)(x-3)}{x-3}$

$(x+3)$
$x\neq3$ because bottom of fraction can't be zero

### Make the bottom of fractions the same

### Solve Quadratic Equation
Example:
$x^2-5x+6=0$

Find two numbers that
- Multiply to $6$
- Add to $-5$

$-2*-3=6$
$-2+-3=-5$
Answer is $-2$ and $-3$.

$x^2-5x+6=0$ becomes
$(x-2)(x-3)=0$ meaning
$x=2$ or $x=3$

### Log and Natural Log

**Log:**
$log_b(n) = x$
$b^x=n$

**Common Log:**
$log N = x$
$N = 10^x$

**Natural Log:**
$lnN=x$
$N=e^x$

Example:
$ln(y)=3x+2$
$y=e^{3x+2}$

Example:
$ln(y)=2x+ln(4)$
$e^{ln(y)}=e^{2x+ln(4)}$
$y=e^{2x}*e^{ln(4)}$
$y=e^{2x}*4$
$y=4e^{2x}$

**Log Product**
$ln(x)+ln(2)=ln(2x)$

## Trigonometry
If you have a right angled triangle whose long side is a length of one
That triangle can represent a vector of length one poking out from (0,0)

From here you can use trigonometry to translate between (x,y) and (angle, length) 


![](Pasted%20image%2020260910154510.png)


$a^2+b^2=c^2$
$sin^2(θ)+cos^2(θ)=1$
$sin(θ)=y$ if the hyp is only 1, so
$hyp*sin(θ)=y$
$hyp*cos(θ) = x$


## Complex Numbers

## Derivatives
### Power Rule
$\frac{d}{dx}x^n = nx^{n-1}$
x HAS to be the base, this rule won't work for $2^n$

### Constant Rule
$\dfrac{d}{dx}c=0$

$f(x)= 17$
$f'(x)=0$

$f(x)=x^3+42$
$f'(x)=3x^2$

### Sum/Difference Rule
When adding or subtracting, differentiate each term independently:
$\dfrac{d}{dx}[f(x)+g(x)]=f'(x)+g'(x)$

Example:
$f(x)=x^4+3x^2-7x+12$
$f'(x)=4x^3+6x-7$

### Product Rule
$(fg)'=f'g+fg'$

### Quotient Rule
$\dfrac{d}{dx}(\dfrac{f}{g})=\dfrac{f'g-fg'}{g^2}$

### Chain Rule
$f(x)=(3x^2+1)^5$

Power rule doesn't work on its own because the "base" isn't x, it's another function.

$\dfrac{d}{dx}=f'(g(x)) * g'(x)$

Example:
$(3x^2+1)^5$
$(u)^5$
$5(u)^4$
$5(u)^4 * \dfrac{d}{dx}(u)$
$5(3x^2+1)^4 * \dfrac{d}{dx}(3x^2+1)$
$5(3x^2+1)^4 * 6x$
$30x * 5(3x^2+1)^4$

Don't expand this function lol
**"Expanding it would be actively stupid in most calculus situations"**

### e and Exponential Functions
$\dfrac{d}{dx}e^x=e^x$

**e Chain Rule**
$\dfrac{d}{dx}(e^u)=e^u * \dfrac{du}{dx}$

$\dfrac{d}{dx}(a^x) = a^x * ln(a)$

If "a" is "e":
$\dfrac{d}{dx}(e^x)=e^x*ln(e)=e^x*1=e^x$

$\dfrac{d}{dx}ln(x)=\dfrac{1}{x}$

### Trig
![347](Pasted%20image%2020260911145342.png)


## Integration
A lot of this is just the same rules as deriving but working backwards.

Like the derivative of 7x is 7, and the integral of 7 is 7x + C wooooah

Same idea for the sum/difference

### Power rule

$\int x^{n}dx=\dfrac{1}{n+1}*x^{n+1}+C,n\neq-1$

Example:
$\int x^3dx=\frac{1}{4}*x^4+C$

### Power Rule Exception!!!
$\int\dfrac{1}{x}dx=\ln|x|+C$
Power rule doesn't work for $x^{-1}$

### Natural Exponential
$\int e^{ax}dc=\frac1a e^{ax}+C$

### Other Bases
$\int a^x dx=\dfrac{a^x}{\ln a}+C$
$\qquad(a>0,\ a\neq1)$

### Trig
![199](Pasted%20image%2020260911145441.png)

### u-substitution
This function must become entirely u-based!

Example:

$\int2x(x^2+1)^5dx$
				$u=x^2+1$
$\int2x(u)^5dx$
				$\dfrac{du}{dx}=2x$
				$du=2x*dx$
				$dx=\dfrac{du}{2x}$
$\int2x(u)^5\dfrac{du}{2x}$
$\int(u)^5du$
$\frac{1}{6}(u)^6+C$
$\frac{1}{6}(x^2+1)^6+C$

Basically
1. Replace the inner function with u
2. Derive u=inner_function to get du/dx=derived_inner_function
3. Now you can make equation all u-based by swapping dx for du!
4. Integrate more simple u-based equation
5. Finally, subtitute u back in with inner_function
## Multivariable Calculus

## Linear Algebra
test
