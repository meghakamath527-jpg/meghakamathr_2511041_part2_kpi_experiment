# Task 6: Frame Hypotheses

## Hypothesis Set 1: Evaluating the Impact of Order Cancellations on Profit Margins

| Requirement Component               | Details & Operational Application                                                                                                                                                                                                                                |
| ----------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Metric Being Tested**             | Average Profit Margin (profit_margin)                                                                                                                                                                                                                            |
| **Reason for Choosing Metric**      | Data profiling isolated a heavy volume of leakage (146 Cancelled, 69 Failed, and 72 Refunded records). Measuring the margin baseline across these groups proves whether process friction is actively draining corporate returns.                                 |
| **Null Hypothesis (H0)**            | There is no statistical difference between the average profit margin of completed orders and orders that experienced transaction disruptions. <br><br> [Average Profit Margin of Clean Orders] = [Average Profit Margin of Disrupted Orders]                     |
| **Alternate Hypothesis (Ha)**       | The average profit margin of completed orders is significantly higher than that of structurally disrupted transactions. <br><br> [Average Profit Margin of Clean Orders] > [Average Profit Margin of Disrupted Orders]                                           |
| **Test Tail Type**                  | One-tailed test (Directional right-tailed check tracking relative profit erosion).                                                                                                                                                                               |
| **Significance Level**              | Alpha = 0.05 (95% statistical confidence threshold standard for business operations).                                                                                                                                                                            |
| **Connection to Business Decision** | Infrastructure Capital Approval: If the null hypothesis is rejected, it proves transaction failures severely depress company margins. This gives you the empirical justification needed to back an executive decision to upgrade the e-commerce payment gateway. |

### Interpretation Logic

* If p-value < 0.05: Reject the Null Hypothesis. Conclude that transaction leakage significantly degrades profitability, validating immediate infrastructure investment.
* If p-value >= 0.05: Fail to Reject the Null Hypothesis. Conclude margin variance is due to random noise, meaning capital should be deployed elsewhere.

---

## Hypothesis Set 2: The Operational Cost of Shipping Delays

| Requirement Component               | Details & Operational Application                                                                                                                                                                                                                                                   |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Metric Being Tested**             | Average Shipping Delay in Days (shipping_delay_days)                                                                                                                                                                                                                                |
| **Reason for Choosing Metric**      | The data cleanup log flagged a severe sequence anomaly (94 records with backward timelines where the ship date preceded the order date). Analyzing standard delays across different fulfillment channels isolates supply chain logjams.                                             |
| **Null Hypothesis (H0)**            | The true mean shipping delay across all shipping carrier modes is identical. <br><br> Plain Text: [Mean Delay of Standard Class] = [Mean Delay of Second Class] = [Mean Delay of First Class]                                                                                       |
| **Alternate Hypothesis (Ha)**       | At least one shipping carrier mode exhibits a mean shipping delay that is structurally different from the others.                                                                                                                                                                   |
| **Test Tail Type**                  | Two-tailed test (Evaluated via an ANOVA F-test structure to catch variation anywhere across multiple group means).                                                                                                                                                                  |
| **Significance Level**              | Alpha = 0.05                                                                                                                                                                                                                                                                        |
| **Connection to Business Decision** | Logistics Vendor Management: If the test indicates significant variance, it flags exactly which carrier tiers are breaching corporate delivery Service Level Agreements (SLAs). Management can use this to terminate underperforming vendor contracts or renegotiate rate matrices. |

### Interpretation Logic

* If p-value < 0.05: Reject the Null Hypothesis. Conclude delivery performance varies significantly by mode, revealing a clear opportunity to trim logistics overhead.
* If p-value >= 0.05: Fail to Reject the Null Hypothesis. Conclude distribution timing is statistically uniform across the fulfillment network.

---

# Task 7: Perform Hypothesis or A/B Test Analysis

## 1. Summary of Test Inputs

### Metric Tracked: Converted to Paid Rate (%)

| Metric Summary  | Control | Treatment |
| --------------- | ------- | --------- |
| Conversions     | 22      | 50        |
| Total Sample    | 690     | 710       |
| Conversion Rate | 3.19%   | 7.04%     |

* Significance Level (α): 0.05

---

## 2. Test Output & Evidence

* Pooled Proportion: 5.14%
* Standard Error: 0.0118
* Z-Statistic: +3.265 (The Treatment group sits 3.265 standard deviations above the Control baseline)
* P-Value (One-Tailed): 0.00055 (Our core mathematical evidence)

---

## 3. Decision Rule

* Rule: If One-Tailed P-value < Alpha (0.05) then Reject the Null Hypothesis
* Result: 0.00055 < 0.05
* Verdict: **REJECT THE NULL HYPOTHESIS.** The difference between the two groups is highly statistically significant.

---

## 4. Business Interpretation

* The Results: The new Treatment strategy successfully grew the paid conversion rate from 3.19% to 7.04%. This is a +3.85% absolute increase, which represents a massive +120.7% relative improvement in conversion performance.
* The Risk: The probability that this conversion growth happened due to random luck or data noise is less than 0.06%.
* Final Recommendation: The test is a clear success. Roll out the Treatment workflow to 100% of production traffic immediately to optimize the user onboarding journey and maximize recurring revenue.
