
# Hotel Booking & Revenue Intelligence
## Research Log & Analytical Validation

**Project:** Hotel Booking & Revenue Intelligence  
**Tools:** Power BI, Power Query, DAX  
**Dataset:** Hotel Booking Demand (António et al., 2019)  
**Records:** 119,390 hotel bookings  
**Arrival period:** July 2015 – August 2017

---

## 1. Research Objective

Investigate cancellation behaviour across hotel types,
market segments, lead times and deposit categories.

The analysis aims to distinguish high cancellation rates
from high aggregate booking-value exposure and identify
areas warranting further commercial investigation.

Findings are descriptive rather than causal.

## 2. Baseline Performance

| Metric | Value |
|---|---:|
| Total bookings | 119,390 |
| Cancelled bookings | 44,224 |
| Cancellation rate | 37.04% |
| City Hotel bookings | 79,330 |
| Resort Hotel bookings | 40,060 |
| Retained booking-value proxy | €26.00M |
| Cancellation-value exposure proxy | €16.73M |

Booking value is estimated as:

`ADR × Total Booked Room Nights`

This represents booking value, not verified revenue
or financial loss. Euro formatting is an assumption.

## 3. Investigation: Lead Time and Cancellation Risk

| Booking lead time | Cancellation rate |
|---|---:|
| 0–7 days | 9.6% |
| 8–30 days | 27.9% |
| 31–90 days | 37.7% |
| 91–180 days | 44.7% |
| 181+ days | 57.0% |

### Observation

Cancellation rates increase across the displayed
lead-time bands.

### Interpretation

Longer booking lead times are associated with higher
observed cancellation rates. This does not establish
that lead time independently causes cancellations.

### Further validation

- Compare within hotel types and market segments.
- Examine booking counts in each category.
- Investigate deposit-category composition.
- Consider arrival-year effects.

---

## 4. Investigation: Market Segment Risk

| Market segment | Cancellation rate |
|---|---:|
| Groups | 61.1% |
| Online TA | 36.7% |
| Offline TA/TO | 34.3% |
| Corporate | 18.7% |
| Direct | 15.3% |

### Initial finding

Groups had the highest cancellation rate among the
major market segments.

Groups accounted for:

- 19,811 total bookings
- 12,097 cancellations
- 61.06% cancellation rate

However, this aggregate comparison does not account
for differences in deposit categories.

---

## 5. Investigation: Deposit Category

| Deposit category | Bookings | Cancelled | Cancellation rate |
|---|---:|---:|---:|
| No Deposit | 104,641 | 29,694 | 28.38% |
| Non Refund | 14,587 | 14,494 | 99.36% |
| Refundable | 162 | 36 | 22.22% |

### Observation

The Non Refund category exhibits an unusually high
cancellation rate of 99.36%.

Although Non Refund represents approximately 12.2%
of all bookings, it accounts for approximately 32.8%
of cancellations.

### Interpretation

Deposit-category composition is strongly associated
with observed cancellation behaviour.

The Non Refund result requires investigation of the
source data, deposit definitions and operational
recording practices.

A cancelled non-refundable reservation does not
necessarily imply that the hotel lost its revenue.

---

## 6. Investigation: Groups After Deposit Filtering

| Metric | All deposits | No Deposit only |
|---|---:|---:|
| Group bookings | 19,811 | 10,509 |
| Group cancellations | 12,097 | 2,974 |
| Cancellation rate | 61.06% | 28.30% |

### Finding

Groups have a 61.06% cancellation rate overall,
compared with 28.30% when considering only bookings
classified as No Deposit.

These are different subsets of observations,
not a before-and-after intervention.

Online TA records approximately 36.6% cancellations
within the No Deposit subset.

### Interpretation

The aggregate Groups cancellation rate should not
be used in isolation to prioritise interventions.

Deposit composition substantially changes the
descriptive comparison between segments.

---

## 7. Investigation: No Deposit Segment Risk by Lead Time

| Lead time | Groups | Online TA |
|---|---:|---:|
| 0–7 days | 12.81% | 9.22% |
| 8–30 days | 24.44% | 31.99% |
| 31–90 days | 24.93% | 39.63% |
| 91–180 days | 25.71% | 44.50% |
| 181+ days | 35.53% | 53.46% |

### Supporting booking counts

| Lead time | Groups | Online TA |
|---|---:|---:|
| 0–7 days | 640 | 8,315 |
| 8–30 days | 1,305 | 10,923 |
| 31–90 days | 2,507 | 16,454 |
| 91–180 days | 2,077 | 13,233 |
| 181+ days | 3,980 | 7,478 |

### Finding

Within the No Deposit subset, Online TA has a higher
observed cancellation rate than Groups in four of
five lead-time bands.

This supports a deposit-related aggregation and
confounding concern.

It is not sufficient to establish a formal
Simpson's paradox or a causal relationship.

---

## 8. Investigation: Cancellation-Value Exposure

| Market segment | Approximate exposure |
|---|---:|
| Online TA | €10.2M |
| Groups | €2.8M |
| Offline TA/TO | €2.5M |
| Direct | €1.0M |

### Finding

Online TA accounts for approximately 61% of total
cancellation-related booking-value exposure.

Its cancellation rate is lower than the overall
Groups rate, but its aggregate exposure is higher.

### Commercial implication

Prioritisation should consider both cancellation
frequency and the value associated with cancelled
reservations.

### Further validation

- Decompose exposure into booking volume and value.
- Examine average value per cancelled booking.
- Compare exposure by hotel type and year.
- Investigate sensitivity to data-quality exclusions.

---

## 9. Investigation: Long-Lead City Hotel Groups

**Segment:** Groups  
**Hotel:** City Hotel  
**Lead time:** 181+ days

| Metric | Value |
|---|---:|
| Total bookings | 6,493 |
| Cancelled bookings | 5,372 |
| Cancellation rate | 82.74% |
| Non Refund cancellations | 4,397 |

### Finding

Of 6,493 long-lead City Hotel group bookings,
5,372 were cancelled.

Of these cancellations, 4,397 were classified
as Non Refund.

### Interpretation

The high cancellation rate is closely associated
with the unusual Non Refund category.

### Recommendation

Audit deposit classifications and reservation
records before recommending changes to group
booking policies.

---

## 10. Cancellation Reduction Simulator

The dashboard includes an interactive What-if
parameter ranging from 0% to 50%.

The simulator calculates:

**Potential Value Retained**

`Cancellation Value Exposure × Selected Reduction`

**Scenario Cancellation Exposure**

`Current Exposure − Potential Value Retained`

### Example: 20% hypothetical reduction

| Metric | Approximate value |
|---|---:|
| Current exposure | €16.73M |
| Potential reduction | €3.35M |
| Remaining exposure | €13.38M |

### Limitations

This is an illustrative scenario, not a forecast.

The model does not account for:

- Refunds or cancellation fees
- Replacement bookings
- Changes in booking demand
- Operational costs
- Actual revenue collection

---

## 11. Analytical Limitations

1. The dataset contains historical records from two
   Portuguese hotels and may not generalise to other
   properties or markets.

2. Booking-value exposure is not equivalent to
   realised revenue loss.

3. The source currency has not been independently
   verified.

4. Some bookings contain non-positive ADR values
   or zero-night stays.

5. Deposit-category definitions and the unusual
   Non Refund cancellation rate require validation.

6. The dataset does not contain the room inventory
   necessary for reliable occupancy or RevPAR.

7. Results are observational and do not establish
   causal relationships.

8. Small categories such as Undefined require
   sample-size warnings.

---

## 12. Final Business Recommendations

### Recommendation 1: Investigate Deposit Classification

Validate the Non Refund category and determine
whether its cancellation behaviour reflects booking
policies, recording practices or other factors.

### Recommendation 2: Prioritise High-Exposure Segments

Investigate Online TA bookings because they account
for the largest aggregate cancellation-value exposure.

### Recommendation 3: Examine Long-Lead Group Bookings

Review the booking conditions and data quality
associated with long-lead City Hotel group reservations.

### Recommendation 4: Test Before Implementing

Use controlled experiments or further statistical
analysis before claiming that policy changes will
reduce cancellations or increase realised revenue.

---

## 13. Research Status

The findings above were developed through exploratory
Power BI analysis, DAX measures, conditional filtering,
supporting matrices and interactive visualisations.

Reported figures reflect the project analysis and
rounded dashboard outputs. They have not all been
independently recomputed from the source CSV.

Further work could include sensitivity analysis,
uncertainty intervals, multivariable modelling
and validation against operational booking records.
