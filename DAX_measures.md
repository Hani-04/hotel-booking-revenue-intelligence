# DAX Measures

This document contains the DAX measures used in the
Hotel Booking & Revenue Intelligence Power BI project.

## 1. Booking Metrics

### Total Bookings

Counts all bookings within the current filter context.

```dax
Total Bookings =
COUNTROWS ( FactBookings )
```

### Cancelled Bookings

Counts bookings marked as cancelled.

```dax
Cancelled Bookings =
CALCULATE (
    [Total Bookings],
    FactBookings[is_canceled] = 1
)
```

### Cancellation Rate

Calculates the proportion of bookings cancelled.

```dax
Cancellation Rate =
DIVIDE (
    [Cancelled Bookings],
    [Total Bookings],
    0
)
```

### Booked Room Nights

Sums the total nights associated with bookings.

```dax
Booked Room Nights =
SUM ( FactBookings[TotalNights] )
```

## 2. Booking Value Metrics

### Booked Value Proxy

Sums estimated booking value calculated from
average daily rate multiplied by booked room nights.

```dax
Booked Value Proxy =
SUM ( FactBookings[BookingValueProxy] )
```

### Retained Booking Value

Calculates booking-value proxy for non-cancelled bookings.

```dax
Retained Booking Value =
CALCULATE (
    [Booked Value Proxy],
    FactBookings[is_canceled] = 0
)
```

### Cancellation Value Exposure

Calculates booking-value proxy associated with
cancelled reservations.

```dax
Cancellation Value Exposure =
CALCULATE (
    [Booked Value Proxy],
    FactBookings[is_canceled] = 1
)
```

## 3. Cancellation Reduction Simulator

### Selected Cancellation Reduction

Reads the selected What-if parameter value.
Defaults to 10% if no single value is selected.

```dax
Selected Cancellation Reduction =
SELECTEDVALUE (
    'Cancellation Reduction'[Cancellation Reduction],
    0.10
)
```

### Potential Value Retained

Estimates the hypothetical reduction in
cancellation-related booking-value exposure.

```dax
Potential Value Retained =
[Cancellation Value Exposure]
    * [Selected Cancellation Reduction]
```

### Scenario Cancellation Exposure

Calculates the remaining exposure after applying
the hypothetical reduction.

```dax
Scenario Cancellation Exposure =
[Cancellation Value Exposure]
    - [Potential Value Retained]
```

## 4. Scenario Comparison

### Scenario Comparison Value

Switches between current exposure and
scenario-adjusted exposure using a disconnected
scenario table.

```dax
Scenario Comparison Value =
SWITCH (
    SELECTEDVALUE (
        'Scenario Comparison'[Scenario]
    ),
    "Current", [Cancellation Value Exposure],
    "After Reduction", [Scenario Cancellation Exposure]
)
```

## 5. Methodology Notes

- Measures respond to the current Power BI filter context.
- Booking value is calculated as ADR × booked room nights.
- Cancellation exposure is not equivalent to realised revenue loss.
- The simulator is illustrative rather than predictive.
- No refunds, replacement bookings or operational costs
  are modelled.
- The displayed euro currency is a presentation assumption
  and has not been independently verified.

The definitions reflect the measures recorded during
development. The Power BI file remains the authoritative
source for any subsequent changes.
