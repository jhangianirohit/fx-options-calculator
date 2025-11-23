# Testing Guide for FX Options Calculator

## Quick Start

1. **Open the HTML file in a web browser:**
   - Double-click `fx_option_pnl_calculator.html`
   - OR right-click → "Open with" → Choose your browser (Chrome, Firefox, Safari, Edge)
   - The calculator should load in the browser

2. **Navigate to Batch Analysis tab:**
   - Click on the "Batch Analysis" tab at the top

3. **Create a test Excel file** (see below for format)

4. **Upload and test**

---

## Test Excel File Format

### Sheet 1: "Spot Data"
- Column A: Timestamps (e.g., `2025-01-01 09:00`, `2025-01-01 09:10`, ...)
- Column B onwards: **USD pairs ONLY** (e.g., EURUSD, GBPUSD, USDJPY, USDCAD)

**Example:**
```
Timestamp          | EURUSD | GBPUSD | USDJPY | USDCAD | USDCHF
2025-01-01 09:00   | 1.1000 | 1.2500 | 150.00 | 1.3500 | 0.9500
2025-01-01 09:10   | 1.1005 | 1.2503 | 150.05 | 1.3502 | 0.9502
2025-01-01 09:20   | 1.1010 | 1.2506 | 150.10 | 1.3504 | 0.9504
... (at least 10 rows)
```

### Sheet 2: "Implied Vols"
- Column A: Pair names
- Column B: Volatility (as decimals, e.g., 0.08 = 8%)

**Example:**
```
Pair      | ImpliedVol
EURUSD    | 0.08
GBPUSD    | 0.10
USDJPY    | 0.09
USDCAD    | 0.06
USDCHF    | 0.07
EURGBP    | 0.06    ← Cross pair
EURJPY    | 0.09    ← Cross pair
GBPJPY    | 0.11    ← Cross pair
CADCHF    | 0.05    ← Cross pair
```

---

## Step-by-Step Testing

### Test 1: Basic USD Pairs Only
1. Create Excel with only USD pairs in Spot Data (e.g., EURUSD, GBPUSD, USDJPY)
2. Upload file
3. ✅ Verify: System detects USD pairs correctly
4. ✅ Verify: Only USD pairs appear in selection UI
5. ✅ Verify: Calculation works for USD pairs

### Test 2: Cross Pair Detection
1. Create Excel with:
   - Spot Data: EURUSD, GBPUSD, USDJPY, USDCAD, USDCHF
   - Implied Vols: Include EURGBP, EURJPY, CADCHF
2. Upload file
3. ✅ Verify: System shows "Available currencies: EUR, GBP, JPY, CAD, CHF, USD"
4. ✅ Verify: Cross pairs appear in currency blocks (EUR block shows EURGBP, EURJPY)
5. ✅ Verify: Warnings appear if a cross pair is missing required USD pair

### Test 3: Cross Pair Calculation
1. Use test data with EURUSD=1.1000, GBPUSD=1.2500
2. Include EURGBP in Implied Vols
3. Select EURGBP for calculation
4. ✅ Verify: System derives EURGBP = 1.1000 / 1.2500 = 0.8800
5. ✅ Verify: Calculation completes successfully
6. ✅ Verify: Results show EURGBP in both EUR and GBP currency blocks

### Test 4: Mixed Format (XXX/USD and USD/XXX)
1. Create Excel with:
   - Spot Data: EURUSD (XXX/USD), USDJPY (USD/XXX), USDCAD (USD/XXX)
2. Include cross pair EURJPY in Implied Vols
3. ✅ Verify: System correctly handles both formats
4. ✅ Verify: EURJPY = EURUSD * USDJPY (correct calculation)

### Test 5: Missing USD Pair Warning
1. Create Excel with:
   - Spot Data: EURUSD, GBPUSD (no USDJPY)
   - Implied Vols: Include EURJPY
2. Upload file
3. ✅ Verify: Warning appears: "Cannot calculate EURJPY: Missing USD pair for JPY"
4. ✅ Verify: EURJPY does NOT appear in available crosses

### Test 6: Currency Blocks
1. Upload file with multiple crosses (EURGBP, EURJPY, GBPJPY, CADCHF)
2. Calculate all pairs
3. ✅ Verify: EUR block shows EURUSD, EURGBP, EURJPY
4. ✅ Verify: GBP block shows GBPUSD, EURGBP, GBPJPY
5. ✅ Verify: EURGBP appears in BOTH EUR and GBP blocks (correct!)

### Test 7: Selection Helpers
1. Upload file with USD pairs and crosses
2. ✅ Verify: "Select All" selects everything
3. ✅ Verify: "Deselect All" deselects everything
4. ✅ Verify: "USD Pairs Only" selects only USD pairs
5. ✅ Verify: "G10 Only" selects only G10 currency pairs

---

## Quick Test Data Generator

You can create a simple test Excel file with this structure:

**Spot Data Sheet:**
- 20-30 rows of timestamps (10-minute intervals)
- EURUSD: Start at 1.1000, add small random variations
- GBPUSD: Start at 1.2500, add small random variations
- USDJPY: Start at 150.00, add small random variations
- USDCAD: Start at 1.3500, add small random variations

**Implied Vols Sheet:**
- EURUSD: 0.08
- GBPUSD: 0.10
- USDJPY: 0.09
- USDCAD: 0.06
- EURGBP: 0.06
- EURJPY: 0.09
- GBPCAD: 0.08

---

## Common Issues to Check

1. **Browser Console Errors:**
   - Open browser DevTools (F12)
   - Check Console tab for JavaScript errors
   - Look for warnings about missing USD pairs

2. **Upload Validation:**
   - If you include cross pairs in Spot Data, you should get an error
   - Only USD pairs are allowed in Spot Data

3. **Cross Calculation:**
   - Verify cross rates match manual calculations
   - EURGBP = EURUSD / GBPUSD
   - EURJPY = EURUSD * USDJPY
   - CADCHF = (1/USDCAD) / (1/USDCHF) = USDCHF / USDCAD

4. **Currency Blocks:**
   - Each currency block should show ALL pairs containing that currency
   - Pairs can appear in multiple blocks (this is correct!)

---

## Expected Console Output

When you upload a file, check the browser console (F12) for:
```
Detected 5 USD pairs: ['EURUSD', 'GBPUSD', 'USDJPY', 'USDCAD', 'USDCHF']
Available currencies: ['CAD', 'CHF', 'EUR', 'GBP', 'JPY', 'USD']
✓ Derived EURGBP
✓ Derived EURJPY
Analyzing 5 USD pairs and 2 cross pairs
```

---

## Troubleshooting

**File won't upload:**
- Check that sheet names are exactly "Spot Data" and "Implied Vols"
- Check that timestamps are valid dates
- Check that all spot prices are positive numbers

**Cross pairs not appearing:**
- Verify both currencies have USD pairs in Spot Data
- Check that cross pair name is exactly 6 characters (e.g., "EURGBP")
- Check browser console for warnings

**Calculation fails:**
- Check that all selected pairs have implied vols
- Verify timestamps are in correct format
- Check browser console for error messages

---

## Next Steps After Testing

Once basic functionality is verified:
1. Test with real market data
2. Test with larger datasets (100+ rows)
3. Test with EM currencies (USDZAR, USDMXN, etc.)
4. Verify P&L calculations match expected results
5. Test analysis window filtering with cross pairs

