# NASDAQ Session Highlighter

## Overview

The **NASDAQ Session Highlighter** Pine Script indicator is designed for use on TradingView. It highlights different trading sessions on the NASDAQ market and shades the areas from the high to the low for each session.

## Features

- **Session Highlighting**: Automatically shades the high and low for each defined trading session.
- **Historical Data Support**: The shading and vertical lines are displayed on historical data as well as current data.

## Trading Sessions Highlighted

The script highlights the following trading sessions:

1. **Opening Session**: 9:30 AM - 10:00 AM
2. **Morning Session**: 10:00 AM - 12:00 PM
3. **Midday Session**: 12:00 PM - 2:00 PM
4. **Afternoon Session**: 2:00 PM - 3:30 PM
5. **Closing Session**: 3:30 PM - 4:00 PM

## Installation

1. Open TradingView and go to the Pine Script editor.
2. Copy and paste the Pine Script code provided below into the editor.
3. Click "Add to Chart" to apply the indicator to your chart.

```pinescript
//@version=5
indicator("NASDAQ Session Highlighter", overlay=true)

// Function to highlight sessions
f_highlightSession(startHour, startMinute, endHour, endMinute, sessionColor) =>
    var float sessionHigh = na
    var float sessionLow = na
    var box sessionBox = na

    // Check if the current bar is within the session
    inSession = (hour == startHour and minute >= startMinute) or (hour == endHour and minute <= endMinute) or (hour > startHour and hour < endHour)

    // If the session is ongoing or has just started, update high and low
    if inSession
        sessionHigh := na(sessionHigh) ? high : math.max(sessionHigh, high)
        sessionLow := na(sessionLow) ? low : math.min(sessionLow, low)
        
        if na(sessionBox)
            sessionBox := box.new(left=bar_index, top=sessionHigh, right=bar_index, bottom=sessionLow, border_width=0, bgcolor=color.new(sessionColor, 90))
        else
            box.set_right(sessionBox, bar_index)
            box.set_top(sessionBox, sessionHigh)
            box.set_bottom(sessionBox, sessionLow)
    else
        // If the session has ended, draw the box to cover the session duration and reset
        if not na(sessionBox)
            box.set_right(sessionBox, bar_index)
            sessionBox := na
        sessionHigh := na
        sessionLow := na
    sessionBox

// Define the session times (using hour and minute)
openingSessionStartHour = 9
openingSessionStartMinute = 30
openingSessionEndHour = 10
openingSessionEndMinute = 0

morningSessionStartHour = 10
morningSessionStartMinute = 0
morningSessionEndHour = 12
morningSessionEndMinute = 0

middaySessionStartHour = 12
middaySessionStartMinute = 0
middaySessionEndHour = 14
middaySessionEndMinute = 0

afternoonSessionStartHour = 14
afternoonSessionStartMinute = 0
afternoonSessionEndHour = 15
afternoonSessionEndMinute = 30

closingSessionStartHour = 15
closingSessionStartMinute = 30
closingSessionEndHour = 16
closingSessionEndMinute = 0

// Highlight each session
openingSessionBox = f_highlightSession(openingSessionStartHour, openingSessionStartMinute, openingSessionEndHour, openingSessionEndMinute, color.red)
morningSessionBox = f_highlightSession(morningSessionStartHour, morningSessionStartMinute, morningSessionEndHour, morningSessionEndMinute, color.blue)
middaySessionBox = f_highlightSession(middaySessionStartHour, middaySessionStartMinute, middaySessionEndHour, middaySessionEndMinute, color.green)
afternoonSessionBox = f_highlightSession(afternoonSessionStartHour, afternoonSessionStartMinute, afternoonSessionEndHour, afternoonSessionEndMinute, color.orange)
closingSessionBox = f_highlightSession(closingSessionStartHour, closingSessionStartMinute, closingSessionEndHour, closingSessionEndMinute, color.purple)
```

## Customization

You can customize the session times and colors by modifying the `startHour`, `startMinute`, `endHour`, `endMinute`, and `sessionColor` parameters in the `f_highlightSession` function calls.
