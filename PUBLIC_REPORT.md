# **Module 1 Report – Volatility Curve Fitting in the Options Market**

## 1. Introduction

This report summarizes the activities and findings of the **first module** of the project focused on modeling and analyzing volatility dynamics in the options market. The primary goal of this module was to build a solid mathematical foundation for understanding the behavior of implied volatility across different strike prices, as well as to evaluate classical and modern techniques for detecting shifts and distortions in market data.

Throughout this phase, a number of **Key Performance Indicators (KPIs)** were registered to track progress and model accuracy. These KPIs include fit error metrics, model stability indicators, and detection rates of abrupt changes in underlying price behavior.

## 2. The Black-Scholes Model: Theoretical Basis

### 2.1. Theoretical Foundation
The Black-Scholes model is a cornerstone in financial mathematics used to determine the fair price of European-style options. It offers a systematic approach to understand how various factors—such as the underlying asset's price, time until expiration, volatility, and the risk-free rate—influence the option's value. The model is based on the following partial differential equation (PDE):

```math
\frac{\partial V}{\partial t} + \frac{1}{2} \sigma^2 S^2 \frac{\partial^2 V}{\partial S^2} + rS \frac{\partial V}{\partial S} - rV = 0
```

where:
- **\(V\)**: Price of the option
- **\(S\)**: Current price of the underlying asset
- **\(t\)**: Time
- **\(σ)**: Volatility of the underlying asset
- **\(r\)**: Risk-free interest rate

In its original form, the model assumes constant volatility (\(σ\)). However, in real-market conditions, volatility varies with the strike price, creating what is known as the "volatility smile" or "skew." This key limitation motivated the exploration of additional analytical techniques within the project.

### 2.2. Context and Application

Think of the Black-Scholes model as a recipe to price an option—a financial instrument giving you the right to buy or sell an asset. Instead of merely guessing, the model uses measurable inputs to calculate how much an option should logically cost under a set of idealized assumptions. The model helps bridge the gap between theoretical pricing and practical trading by factoring in:

- **Underlying Asset Price (\(S\))**: The current market price of the asset.
- **Time to Expiration (\(t\))**: The remaining time until the option expires.
- **Volatility (\(σ\))**: How drastically the asset’s price tends to fluctuate.
- **Risk-Free Interest Rate (\(r\))**: The theoretical rate of return on a riskless investment.

#### How the Black-Scholes Equation Works
The Black-Scholes PDE breaks the option pricing problem into several interconnected parts:

```math
\textbf{1. Time Sensitivity (\(\frac{\partial V}{\partial t}\))}

\frac{\partial V}{\partial t}
```

This term quantifies how the option’s value changes over time. As the option nears its expiration date, it typically loses "time value" — a phenomenon known as time decay.

```math
\textbf{2. Volatility and Curvature (\(\frac{1}{2} \sigma^2 S^2 \frac{\partial^2 V}{\partial S^2}\))}

\frac{1}{2} \sigma^2 S^2 \frac{\partial^2 V}{\partial S^2}
```

This component captures the effect of the underlying asset's volatility on the curvature (or "gamma") of the option's price function. Higher volatility (σ) results in a more pronounced curvature, thereby greatly influencing the pricing sensitivity to changes in (S).

```math
\textbf{3. Immediate Sensitivity (\(rS \frac{\partial V}{\partial S}\))}

rS \frac{\partial V}{\partial S}
```

Often referred to as "delta," this term measures the immediate responsiveness of the option’s price to small changes in the underlying asset's price.

```math
\textbf{4. Discounting (\(-rV\))}
```
This term adjusts for the time value of money by discounting the option's future payoffs back to their present value, embodying the concept that a dollar today is more valuable than a dollar in the future.

#### Example
Imagine you are evaluating a European call option with these parameters:
- **Underlying stock price (\(S\))**: \$100
- **Strike price**: \$105
- **Time until expiration (\(t\))**: 0.5 years
- **Volatility (\(σ))**: 20% per annum (0.20)
- **Risk-free rate (\(r\))**: 5% per annum (0.05)

**Process:**
1. **Input Data into the Formula**: The model takes these values and calculates the fair price of the option by considering how the aforementioned factors interact.
2. **Consider the Effects**: 
   - **Time decay** reduces value as expiration nears.
   - **Volatility's impact** is computed through the curvature (gamma), suggesting how sensitive the option is to fluctuations.
   - **Delta** provides a direct linkage between changes in the stock price and the resultant option price.
3. **Output**: The computed option price might be, for example, \$7.50, which represents the estimated premium one should pay, given the current market conditions and risk factors.

### 2.3. Limitations and Real-World Considerations
While the Black-Scholes model is extremely useful as a pricing benchmark, it's important to recognize its limitations:
- **Assumption of Constant Volatility**: The model presumes that volatility remains constant over time, which does not hold true in real-world trading, especially across varying strikes.
- **Market Idealizations**: It assumes smooth, continuous trading with no sudden jumps or discontinuities, yet actual market behavior can be affected by liquidity gaps, behavioral factors, and algorithmic trading.

Understanding these concepts not only aids in grasping the theoretical underpinnings of option pricing but also sets the stage for the subsequent modules, where additional data-driven approaches are integrated to manage real-world market complexities effectively.


## 3. Differential Equations

To effectively analyze and model the volatility curve, it is crucial to understand not only the overall trend of the data but also its dynamic behavior—especially when the market experiences abrupt changes. Differential equations, both first and second-order, were employed to capture these rapid transitions. This approach allows us to pinpoint specific areas of the curve where sudden changes occur and to treat these segments differently when predicting future behavior and calibrating model parameters.

### 3.1 Capturing Abrupt Changes with Differential Equations

The use of differential equations provides a robust framework to capture the dynamic features of volatility curves:

- **First-Order Differential Equations:**  
  These equations, which analyze the rate of change of the underlying asset’s price or implied volatility, help detect sudden shifts in the trend. In our application, a first-order model was used to measure the velocity at which the volatility changes:
  
  ```math
  \frac{dS}{dt} = -k(S - S_0)
  ```
  
  Here, the coefficient \( k \) plays a critical role in quantifying how quickly the market reverts to a baseline level \( S_0 \). When a rapid movement is detected, the model flags this as an atypical behavior that might require special treatment or further analysis.

- **Second-Order Differential Equations:**  
  Beyond merely detecting the speed of change, it is important to capture the acceleration or deceleration in the movement of the curve. The second-order differential equation adds an extra layer of nuance by incorporating acceleration:
  
  ```math
  \frac{d^2S}{dt^2} + a \frac{dS}{dt} + b(S - S_0) = 0
  ```
  
  In this model, the term $```\frac{d^2S}{dt^2}\```$ directly measures the curvature, capturing the effects of abrupt changes (or spikes) in volatility. The parameters \( a \) and \( b \) are tuned so that the model can distinguish between typical market fluctuations and more significant, abrupt shifts that may indicate regime changes or the onset of market stress.

### 3.2 Identification and Differential Treatment of Abrupt Changes

By applying these differential equation models to the volatility curves, we can identify specific segments where the behavior deviates sharply from the norm:

- **Detection of Regime Shifts:**  
  When the first- and second-order derivatives show extreme values, it often signals that the curve is experiencing a regime shift—a period where market behavior changes drastically. These insights allow us to isolate segments that do not conform to the gradual dynamics captured by standard curve-fitting techniques.

- **Differential Parameter Estimation:**  
  Once abrupt changes are identified, the modeling process can treat these segments separately. The parameters used for curve fitting in these regions are calibrated differently from those used in more stable periods. This segmentation ensures that the overall model remains robust and does not get biased by these outlier events.

- **Adaptive Prediction and Risk Management:**  
  By incorporating differential equations into the analysis, the predictive models can adapt to the presence of these abrupt changes. This adaptive mechanism improves forecasting performance by allowing the model to adjust predictions based on the detected volatility regimes. Additionally, in risk management, recognizing these patterns early can prompt more dynamic hedging strategies, thereby reducing exposure during turbulent periods.

### 3.3 Integration into the Volatility Modeling Framework

The insights derived from the differential equation analysis seamlessly integrate with the broader volatility modeling framework. Here's how this integration benefits the overall process:

- **Enhanced Data Segmentation:**  
  The differential analysis provides a quantitative basis for segmenting the volatility curve into regions of normal and abnormal behavior. This segmentation is fundamental for targeted model calibration, ensuring that fitting parameters accurately represent the underlying market conditions.

- **Improved Prediction Accuracy:**  
  By treating abrupt changes as distinct events rather than anomalies in the data, the model can more accurately predict future volatility behavior. This targeted approach not only refines the prediction of future prices but also enhances the model's overall stability.

- **Robust Calibration Strategy:**  
  The combination of differential equations and traditional curve-fitting techniques creates a more resilient calibration process. By isolating and addressing the segments with abrupt changes separately, the model maintains high accuracy across the entire volatility surface, even in the presence of market shocks.

In summary, the application of differential equations is a fundamental component in our methodology. It allows for the precise detection and treatment of abrupt changes, ensuring that our volatility model remains both accurate and adaptable in capturing the dynamic behavior of market data.

## 4. Analytical Challenges and KPIs

In the process of modeling volatility dynamics in the options market, several analytical challenges were encountered. These challenges are primarily attributed to the intrinsic complexities of financial time series data, including noisy market signals, non-stationarity, and abrupt shifts in underlying asset behavior. Overcoming these issues is crucial for achieving reliable curve fitting and ensuring robust model performance.

### 4.1 Key Analytical Challenges

- **Parameter Estimation:**  
  Estimating key parameters such as volatility from market data is often complicated by data noise and the varying frequency of observations. This can lead to errors in model calibration if not meticulously managed.

- **Model Sensitivity:**  
  Many analytical methods exhibit high sensitivity to outliers or irregularities in market data. Such fluctuations can skew the analysis, potentially distorting the fitted volatility curve.

- **Market Non-Stationarity:**  
  Financial markets are dynamic, with regimes that can change rapidly. These transitions impact the stability and accuracy of traditional modeling assumptions, which may rely on stationary data.

- **Data Quality and Granularity:**  
  The presence of liquidity gaps, discontinuous trading intervals, or delayed data can further complicate the curve-fitting process, leading to potential misinterpretations of market behavior.

### 4.2 KPIs in the Modeling Process

Tracking Key Performance Indicators (KPIs) is pivotal for assessing and improving the performance of volatility models. KPIs provide a structured way to monitor both the accuracy and reliability of the models and help to identify areas that require refinement. Here are some key reasons why KPIs are indispensable:

- **Performance Benchmarking:**  
  KPIs such as fit error metrics, residual distribution, and detection rates of abrupt market changes serve as quantitative benchmarks. They enable the modeling team to compare the current model performance against historical data or alternative models, ensuring continuous improvement.

- **Model Calibration and Tuning:**  
  Regular evaluation through KPIs helps in fine-tuning model parameters. This ensures that the model adapts well to changing market conditions, thus maintaining relevance and accuracy over time. Calibration using KPIs also aids in identifying periods where the model may need re-adjustments due to market stress or regime shifts.

- **Early Warning for Model Breakdown:**  
  The consistent monitoring of KPIs acts as an early warning system for potential model breakdowns. If key metrics indicate an unexpected increase in errors or instability, it signals the need for immediate attention. This proactive approach minimizes the risk of model underperformance during critical market periods.

- **Risk Management Efficiency:**  
  By quantifying how well the model captures the volatility dynamics, KPIs directly contribute to better risk management. They provide risk managers with confidence in the model’s outputs, facilitating more accurate hedging strategies and better portfolio management.

- **Operational Transparency:**  
  KPIs offer a transparent way of demonstrating model performance to stakeholders, including investors, regulatory bodies, and internal audit teams. This transparency builds confidence in the financial institution’s modeling practices and risk assessment methodologies.

- **Continuous Improvement Feedback Loop:**  
  The feedback derived from KPIs feeds into an iterative process of model refinement. As new market data is incorporated, KPIs help the team to monitor the impact of any changes, ensuring the model remains robust and flexible in dynamic market environments.

In summary, the inclusion and detailed tracking of KPIs are fundamental not only for the validation of the current volatility model but also for the ongoing process of enhancing model precision. KPIs empower the team to diagnose issues promptly, adjust parameters as needed, and ultimately sustain a high level of

## 5. Market Distortions and Realities

While mathematical models and differential analysis provide powerful tools, certain distortions impact their accuracy:

- **Liquidity Gaps**: Discontinuities in order books can produce non-smooth data.  
- **Behavioral Effects**: Fear, speculation, and crowd behavior influence implied volatilities.  
- **Algorithmic Trading**: Automated responses to market signals may exaggerate curve features.

Recognizing these distortions was critical in interpreting volatility surfaces and setting realistic expectations for the modeling process.

## 6. Importance of Accurate Volatility Curve Fitting and Precision in Pricing

Accurately fitting the volatility curve is a linchpin in the realm of financial engineering, underpinning effective options pricing, hedging strategies, and robust risk management. This section delves deeply into why precision in modeling the relationship between volatility points and strike prices is critical and how it impacts various market stakeholders.

### 6.1 Enhancing Option Pricing Models

A primary use of the volatility curve is to supply the implied volatility inputs essential for pricing options accurately. Precision in curve fitting matters because:

- **Alignment with Market Sentiments:**  
  The volatility curve reflects market expectations and risk perceptions. A well-calibrated curve ensures that pricing models, such as Black-Scholes and its extensions, incorporate volatility figures that truly mirror market conditions. Inaccurate estimates can lead to substantial mispricing and suboptimal trading decisions.

- **Correcting the Volatility Smile/Skew:**  
  In real markets, implied volatility varies with strike prices, creating a “smile” or “skew” rather than remaining constant. Capturing this phenomenon through detailed curve fitting is essential for:
  - **Pricing Consistency:** Ensuring theoretical prices align closely with observed market prices.
  - **Model Robustness:** Allowing models to adjust dynamically and better reflect the underlying market behavior.

- **Systematic Error Reduction:**  
  Inaccurate curve fittings can result in systematic pricing errors across different strike levels, which may accumulate risk over time. A precise model minimizes these discrepancies and supports more stable option pricing.

### 6.2 Improving Risk Management Practices

Effective risk management depends heavily on accurately capturing volatility dynamics:

- **Refinement of Option Sensitivities (Greeks):**  
  Critical risk metrics such as Delta, Gamma, and Vega are sensitive to the underlying volatility inputs. An accurately fitted volatility curve provides a solid foundation for calculating these measures, thereby enhancing the efficacy of hedging strategies.

- **Scenario Analysis and Stress Testing:**  
  With a reliable volatility model, risk managers can simulate a variety of market conditions—including sudden spikes or drops in volatility—enabling proactive measures to mitigate potential adverse impacts.

- **Minimizing Basis Risk in Hedging:**  
  Inaccurate volatility curves can introduce basis risk by creating inconsistencies between theoretical and market prices. Precision in curve fitting ensures that hedging instruments closely align with actual risk exposures.

### 6.3 Curtailing Arbitrage Opportunities

Market efficiencies rely on the elimination of pricing anomalies:

- **Detection of Mispricing:**  
  A meticulously fitted volatility surface reduces discrepancies between the theoretical and market values of options. This minimizes arbitrage opportunities that could otherwise be exploited due to misaligned volatility inputs.

- **Uniformity Across Strike Prices:**  
  Ensuring the entire volatility surface is coherently modeled prevents inconsistencies that might be exploited, particularly in high-frequency and algorithmic trading environments where minor misalignments are magnified.

### 6.4 Facilitating Dynamic Model Calibration

Market conditions are in constant flux, necessitating adaptive models:

- **Rapid Recalibration:**  
  A dynamic volatility model can be quickly recalibrated in response to evolving market data. This adaptability is key to maintaining pricing accuracy amid abrupt market shifts or regime changes.

- **Long-Term Stability:**  
  Continuously refined models ensure that transient mis-specifications do not persist, thereby maintaining the long-term reliability and credibility of the pricing framework.

### 6.5 Enhancing Market Interpretation and Strategic Insight

Beyond pricing and risk management, a well-fitted volatility curve provides valuable market insights:

- **Detection of Regime Shifts:**  
  Detailed analysis of the volatility surface can reveal subtle shifts in market behavior, serving as an early warning system for transitions from stable to turbulent conditions.

- **Quantitative Understanding of Behavioral Trends:**  
  The shape of the volatility curve often encapsulates behavioral aspects such as investor sentiment and speculative pressures. A precise model helps quantify these dynamics, informing both short-term trading strategies and long-term investment decisions.

- **Supporting Empirical Research:**  
  Accurate volatility modeling drives academic and applied research, fostering the development of advanced models that capture phenomena like volatility clustering or abrupt market jumps. This continuous improvement cycle benefits both practitioners and theorists in finance.

## 7. Conclusion and Next Steps

A robust model was implemented during this module, showing promising performance based on key performance indicators like fit accuracy, residual analysis, and the detection of abrupt market changes. The integration of precise volatility curve fitting has enhanced the model's ability to capture the nuanced dynamics of implied volatility across different strikes, thereby bridging theoretical assumptions and real market behavior.

The encouraging preliminary results motivate further refinements in subsequent modules. Future steps include:
- Enhancing the predictive accuracy and stability of the volatility model.
- Integrating additional data-driven techniques to better manage market complexities.
- Documenting the methodologies and findings for academic and professional dissemination.

This continuous evolution of the modeling framework will not only solidify its application in pricing and risk management but also contribute to broader research and strategic financial decision-making.



# **Module 2 Report – Volatility Curve Fitting in the Options Market**

## Sprint 1

During the first sprint, the initial planning of the module was carried out, focusing on the organization and completion of the official documentation required by the academic institution. In addition, the algorithm developed in the previous module was refactored to improve code readability and efficiency, aligning it with best development practices.

## Sprint 2

In the second sprint, a thorough analysis of potential journals for article submission was conducted. Academic journals, magazines, and renowned conferences were considered. After evaluating the options, it was decided to submit the article to *IEEE Magazine* due to its visibility and thematic alignment.

## Sprint 3

During the third sprint, the methodology section of the article was written, providing a detailed description of the algorithm’s application and the tests carried out. Additionally, the introduction was initiated, including the contextualization of the research problem and the definition of the study’s objectives.

## Sprint 4

The fourth sprint focused on completing the introduction and writing the results section. Tables and quantitative analyses were included, accompanied by a critical interpretation of the data, strengthening the scientific argumentation of the article.

## Sprint 5

In the fifth and final sprint, the article’s conclusion was developed and the complete manuscript underwent a thorough revision. It was decided to adopt the *IEEE Access* template, considering the possibility of publishing in that journal. Furthermore, feedback from the peer review process was incorporated to enhance the final quality of the work.

# **Module 3 Report – Volatility Curve Fitting in the Options Market**

#### Sprint 1

During the progress of Module 3 of the project, there was a significant change in the supervision structure: the replacement of the responsible advisor. This change required some adjustments in planning and in communications with the parties involved.

Among the main actions resulting from this change, the following stand out:

1. **Contact with the project partner**

    It was necessary to reach out to the project partner to introduce the new advisor, ensuring that both were aligned regarding the objectives, deadlines, and requirements already established.

2. **Inclusion of the new advisor in the confidentiality agreement (NDA)**

    The process of contacting the new advisor to formally include them in the current NDA was initiated, in order to ensure access to restricted materials and enable them to perform their role in full legal compliance.

Despite these changes and the necessary adjustments, the overall impact on the schedule was minimal. There was a small delay in carrying out some steps, but nothing that compromises the project’s final delivery deadline or the quality of the expected results.

#### Sprint 2

At this stage of the project, the main focus was the internal sharing of the article with the faculty advisors. The material was submitted for an initial review and received a first set of structured feedback from one of the advisors.

The feedback received was analyzed and served as the basis for developing an adjustment plan for the article.

In terms of schedule, the progress of Sprint 2 remained as expected, without significant delays. However, approval from the project partner is still pending for submission and publication of the article.

During this sprint 2, the article was internally reviewed and received structured feedback. Based on this evaluation, we initiated the implementation of the suggested improvements. The main adjustments being carried out are:

- Replace “orienter/student” terminology with neutral scientific writing, removing - references to roles not relevant to the academic community.

- Replace “partner’s algorithm” with appropriate terms such as “reference - technique,” “benchmark technique,” or “gold standard technique,” supported by - academic references.

- Add quantitative results to substantiate claims (numerical evidence for - superiority of the proposed method).

- Revise citations: “several studies” must be supported by at least four - references, or wording adjusted accordingly.

- Move company names (e.g., BTG Pactual) to acknowledgments and replace with - academic references when comparing methods.

- Remove irrelevant statements regarding intellectual property and confidentiality - restrictions.

- Revise Table 1:

    - Use single-letter notation for variables (e.g., e, K, V).

    - Define or reference all variables (n, Kᵢ, Vᵢ).

    - Remove trivial formulas (e.g., mean), while clarifying less common terms (e.g., strike price).

- Improve mathematical notation consistency: replace words in equations (e.g., - inlier, interp, pair) with symbols or letters; adjust “Minimization Function” → - “Cost Function.”

- Clarify definition of weights (w) in Eq. 10.

- Restructure Results section: move methodological descriptions to Methods; enrich the discussion of results and consider renaming section to “Results and - Discussion.”

- Revise Table 2:

    - Reduce excessive significant figures.

    - Verify large discrepancies between methods to exclude implementation errors.

    - Replace standard deviation with coefficient of variation for better comparability.

    - Remove redundancy in “Mean MSE.”

- Simplify Table 3: integrate single-value results directly into the text.

- Expand results interpretation with clear comparative insights (e.g., performance gains expressed as percentages).

- Improve figure readability (use two-column width when needed).

- Enhance the Discussion with more depth, separating it clearly from Results.

- Refine the Conclusion: remove unrelated literature review paragraphs; maintain focus on contributions and findings.

- Correct minor text issues (e.g., “volatility volatility” repetition).

#### Sprint 3

In Sprint 3, the team continued working on the implementation of the mapped feedback received for revisions to the article. The partner’s confirmation for publication is still pending, but additional attempts have been made to establish contact and expedite their response.

Furthermore, the preparation of a scientific initiation report was initiated, aiming at approval by the educational institution and the recognition of complementary academic hours.

#### Sprint 4

In this stage, the final implementations of the feedback received from the faculty advisors were completed.

Additionally, during this sprint, the awaited response from the partner was obtained. The partner confirmed receipt of the updated material and initiated their internal review process.

From the perspective of schedule, Sprint 4 maintained consistency with the planning. Next steps are to structure the final presentation for the next, and final module.

#### Sprint 5

During Sprint 5, the focus was on consolidating and finalizing the adjustments based on the complete set of feedback received from the academic advisors. All points raised in the review process were carefully analyzed, and implemented.

Among the most relevant changes were the clarification of definitions, the restructuring of sections to improve coherence and flow, and the enhancement of quantitative results to better support the conclusions. The redundant or unclear elements were removed, and all terminology was standardized for technical precision and academic consistency.

Several specific adjustments also stand out: the article now includes additional references to substantiate the theoretical framework, reformulated equations with explicit parameter definitions, and improved presentation of figures and tables for greater readability. Sections were reorganized to ensure proper logical sequence, with the “Results and Discussion” section now clearly separating methodological content from analysis.

Regarding partner communication, a new response was received during this sprint, confirming that the material is under active review. The feedback loop with the partner has become more dynamic, and progress in the approval and publication process appears to be advancing at a faster pace.

# **Module 4 Report – Volatility Curve Fitting in the Options Market**

During Module 4, the project reached its final stage, focusing on consolidation, validation, and formal dissemination of the research results. After completing all revisions suggested by academic advisors and incorporating the final round of improvements, the scientific article was finalized. The manuscript was then submitted to the Journal of Alternative Investments (JAI).