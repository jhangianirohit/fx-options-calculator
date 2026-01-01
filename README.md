# FX Options P&L Calculator

A professional-grade, browser-based FX options P&L calculator with delta-hedged volatility analysis. Built as a standalone HTML application with a modern dark terminal theme.

## Overview

This tool enables traders and analysts to:
- Calculate P&L for FX options across multiple strikes
- Compare realized volatility against implied volatility
- Analyze delta-hedging performance
- Evaluate cross-pair correlations
- Generate formatted email reports for distribution

## Features

### Single Pair Analysis
- **Black-Scholes Pricing**: Option valuation using Black-Scholes model (r=0, q=0)
- **Multi-Strike Analysis**: Simultaneously analyze 5 standard strikes (10Δ Put, 25Δ Put, ATM, 25Δ Call, 10Δ Call)
- **Custom Strikes**: Add up to 5 custom strikes for specific analysis
- **Variable Hedging Frequencies**: Choose from 10, 30, 60, or 120-minute hedging intervals
- **Test Data Generation**: Generate synthetic spot data for testing and validation

### Batch Analysis
- **Excel Upload**: Import spot data and implied vols from Excel files
- **Multi-Currency Processing**: Analyze multiple currency pairs simultaneously
- **Cross-Pair Derivation**: Automatically derive cross rates from USD pairs (e.g., EURGBP from EURUSD and GBPUSD)
- **Correlation Analysis**: Compare realized vs implied correlations across currency groups
- **Analysis Window Selection**: Filter data to specific time ranges
- **G10 Currency Support**: Quick selection for G10 currency pairs

### Reporting
- **Email Report Generation**: One-click copy of formatted analysis for email distribution
- **Top 5 / Bottom 5 Rankings**: Identify best and worst performing pairs by P&L
- **Currency Block Grouping**: Results organized by base currency

## Quick Start

1. **Open the Calculator**
   ```
   Open fx_option_pnl_calculator.html in any modern web browser
   ```

2. **Single Pair Analysis**
   - Navigate to the "Single Pair Analysis" tab
   - Enter initial spot, volatility, and configuration
   - Paste spot prices or generate test data
   - Click "Calculate All Strikes"

3. **Batch Analysis**
   - Navigate to the "Batch Analysis" tab
   - Upload an Excel file with required format (see below)
   - Select currency pairs to analyze
   - Click "Calculate All Selected Pairs"

## Excel File Format

### Sheet 1: "Spot Data"
| Timestamp | EURUSD | GBPUSD | USDJPY | ... |
|-----------|--------|--------|--------|-----|
| 2025-01-01 09:00 | 1.1000 | 1.2500 | 150.00 | ... |
| 2025-01-01 09:10 | 1.1005 | 1.2503 | 150.05 | ... |

- Column A: Timestamps (10-minute intervals recommended)
- Column B onwards: USD pairs only (e.g., EURUSD, GBPUSD, USDJPY)

### Sheet 2: "Implied Vols"
| Pair | ImpliedVol |
|------|------------|
| EURUSD | 0.08 |
| GBPUSD | 0.10 |
| EURGBP | 0.06 |

- Volatility expressed as decimals (0.08 = 8%)
- Include cross pairs here; they will be derived from USD pairs

## Cross Pair Calculations

The calculator automatically derives cross rates from USD pairs:

| Cross Pair | Formula |
|------------|---------|
| EURGBP | EURUSD / GBPUSD |
| EURJPY | EURUSD × USDJPY |
| GBPJPY | GBPUSD × USDJPY |
| CADCHF | USDCHF / USDCAD |

## Technical Details

### Pricing Model
- **Model**: Black-Scholes
- **Interest Rates**: r = 0 (domestic), q = 0 (foreign)
- **Notional**: 100M USD per strike (settlement currency)

### Hedging Simulation
- Delta-neutral hedging at specified frequency
- Mark-to-market P&L calculation
- Realized volatility computed from hedge intervals

### Dependencies
- **SheetJS (xlsx)**: Excel file parsing (loaded from CDN)
- **Google Fonts**: Inter and JetBrains Mono typefaces
- No other external dependencies - fully self-contained HTML

## Browser Compatibility

Tested and supported on:
- Google Chrome (recommended)
- Mozilla Firefox
- Microsoft Edge
- Safari

## File Structure

```
fx-options-calculator/
├── fx_option_pnl_calculator.html   # Main application
├── README.md                        # This file
├── TESTING_GUIDE.md                 # Detailed testing instructions
├── test_calculator.html             # Test harness
├── test_calc_logic.js               # Core calculation tests
├── test_custom_strikes.js           # Custom strike tests
├── test_hedge_pnl_mtm_fix.js        # Hedge P&L MTM tests
├── test_realized_vol.js             # Realized volatility tests
├── test_variable_frequencies.js     # Frequency handling tests
└── test_variable_length.js          # Variable length tests
```

## Usage Tips

1. **Correlation Analysis**: Enable "Include Correlation Analysis" to compare realized vs implied correlations
2. **Analysis Window**: Use custom time windows to focus on specific market events
3. **G10 Filter**: Use "G10 Only" button to quickly select major currency pairs
4. **Email Reports**: Click "Copy Email Report" to copy formatted results to clipboard

## Troubleshooting

**File won't upload:**
- Verify sheet names are exactly "Spot Data" and "Implied Vols"
- Ensure timestamps are valid date formats
- Check that all spot prices are positive numbers

**Cross pairs not appearing:**
- Both constituent USD pairs must be in Spot Data
- Cross pair names must be exactly 6 characters (e.g., "EURGBP")
- Check browser console (F12) for warnings

**Calculation fails:**
- Ensure all selected pairs have implied vols defined
- Verify timestamp format consistency
- Check for minimum data points (at least 2 spot prices)

## Development

To run tests, open `test_calculator.html` in a browser and check the console for results.

For detailed testing procedures, see `TESTING_GUIDE.md`.

## License

This project is proprietary software. All rights reserved.
