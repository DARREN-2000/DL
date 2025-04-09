# License Usage Analyzer Documentation

## Overview

The License Usage Analyzer is a comprehensive Python-based toolkit designed to analyze software license usage data from CSV or Excel files. It generates detailed reports, visualizations, and optimization recommendations to help manage software licenses efficiently. This toolkit enables data-driven decisions about license allocation and identifies potential cost savings.

The analyzer is available in three versions:

1. **license_analyser.py** - Standard version with German holiday detection
2. **license_analyser_without.py** - Lightweight version without holiday detection
3. **license_analyser_with_holidays.py** - Enhanced version with custom German and Bavarian holiday detection

## Comparison of Versions

| Feature | license_analyser.py | license_analyser_without.py | license_analyser_with_holidays.py |
|---------|--------------------|-----------------------------|----------------------------------|
| Core Analysis | ✓ | ✓ | ✓ |
| Visualizations | ✓ | ✓ | ✓ |
| Excel Reports | ✓ | ✓ | ✓ |
| PDF Reports | ✓ | ✗ | ✓ |
| Holiday Detection | Standard German | None | Custom German & Bavarian |
| Holiday Library | `holidays` package | Not used | Custom implementation |
| Holiday Specific Analysis | Basic | None | Enhanced |
| File Size | Medium | Small | Large |
| Dependencies | More | Fewer | Most |

## Core Functionality (All Versions)

### Data Import and Processing
- Support for CSV files with both semicolon and comma separators
- Support for Excel files (.xls, .xlsx)
- Intelligent column detection and naming
- Flexible date format parsing
- Support for analyzing multiple files sequentially
- Multi-file combination with various resampling options

### Analysis Features
- Usage statistics by hour of day, day of week, and month
- Weekend filtering (excludes Saturday and Sunday)
- Working hours analysis (configurable with start/end hour parameters)
- Categorization of usage into meaningful groups:
  - High Usage (≥90%)
  - Medium-High Usage (70-90%)
  - Medium Usage (50-70%)
  - Low Usage (<50%)
- Peak usage detection and analysis
- License optimization calculations (95% usage coverage)

### Output and Reporting
- Summary text reports
- Excel reports with multiple sheets and conditional formatting
- Comprehensive visualizations:
  - Usage by hour of day with thresholds
  - Usage category distribution pie charts
  - Day-of-week usage patterns
  - Heatmaps of usage by day and hour
  - Distribution of usage categories by hour
  - Monthly usage comparison (when applicable)

## Version-Specific Details

### 1. license_analyser.py (Standard Version)
This is the standard version that includes basic German holiday detection using the `holidays` Python package.

**Features:**
- Uses the standard `holidays` package for German holiday detection
- Generates PDF reports
- Provides holiday-specific analysis
- Creates holiday vs. non-holiday usage comparisons

**Use this version when:**
- You need holiday detection for Germany without custom holidays
- You want PDF reports generated
- You don't need Bavarian-specific holidays

### 2. license_analyser_without.py (Lightweight Version)
This is a simplified version without any holiday detection functionality.

**Features:**
- No dependencies on the `holidays` package
- Smaller file size
- Fewer dependencies
- All data points marked as non-holidays with `df['IsHoliday'] = False`

**Use this version when:**
- You don't need holiday detection
- You want to minimize dependencies
- Holiday analysis is not relevant to your use case
- You're analyzing license data from regions where holiday information is not available

### 3. license_analyser_with_holidays.py (Enhanced Version)
This is the most comprehensive version with custom German and Bavarian holiday detection.

**Features:**
- Custom implementation of German holidays including state-specific holidays
- Enhanced support for Bavarian holidays
- Includes the `GermanHolidays` class that extends `HolidayBase`
- More detailed holiday information (names, types)
- Generates PDF reports with holiday-specific sections
- More sophisticated holiday vs. non-holiday usage comparisons

**Use this version when:**
- You need detailed holiday detection for Germany, especially Bavaria
- You want the most comprehensive holiday analysis
- You need state-specific holidays
- You're analyzing license data from German/Bavarian locations

## Installation and Dependencies

### Common Dependencies (All Versions)
```
pandas
numpy
matplotlib
seaborn
```

### Additional Dependencies
For **license_analyser.py** and **license_analyser_with_holidays.py**:
```
holidays (standard version only)
reportlab
```

### Installation Commands
```bash
# For license_analyser_without.py (minimal)
pip install pandas numpy matplotlib seaborn

# For license_analyser.py (standard)
pip install pandas numpy matplotlib seaborn holidays reportlab

# For license_analyser_with_holidays.py (enhanced)
pip install pandas numpy matplotlib seaborn reportlab
```

## Usage Instructions

### Basic Usage

All versions support the same command-line arguments:

```bash
python license_analyser.py path/to/your/license_data.csv
# OR
python license_analyser_without.py path/to/your/license_data.csv
# OR
python license_analyser_with_holidays.py path/to/your/license_data.csv
```

### Command Line Arguments

```
--start-hour HOUR     Start hour for analysis (default: 0)
--end-hour HOUR       End hour for analysis (default: 23)
--combine             Combine multiple files
--auto-combine        Combine with automatic interval detection
--resample INTERVAL   Time interval for resampling (e.g., 5min, 1H)
--output FILE         Output file for combined data
```

### Usage Examples

1. **Analyze a single file:**
   ```bash
   python license_analyser.py license_data.csv
   ```

2. **Analyze a single file focusing on business hours (9 AM to 5 PM):**
   ```bash
   python license_analyser.py --start-hour 9 --end-hour 17 license_data.csv
   ```

3. **Combine multiple files and analyze:**
   ```bash
   python license_analyser.py --combine --output combined.csv file1.csv file2.csv
   ```

4. **Automatically detect sampling intervals and combine files:**
   ```bash
   python license_analyser.py --auto-combine --output combined.csv file1.csv file2.csv file3.csv
   ```

5. **Combine files with specific resampling interval:**
   ```bash
   python license_analyser.py --resample 1H --output hourly.csv file1.csv file2.csv
   ```

## Input Data Format

The tool expects CSV or Excel files with license usage data containing three essential columns:

1. A time/date column (e.g., "Time" or "Date")
2. Available license count (e.g., "avail")
3. Used license count (e.g., "used")

Example CSV format with semicolon separator:
```
Time;avail;used
2024-01-01 00:00:00;100;45
2024-01-01 01:00:00;100;50
...
```

The analyzer will attempt to automatically detect column names and formats, but using the format above ensures optimal recognition.

## Output Files and Directory Structure

For each input file analyzed (e.g., "license_data.csv"), the tool creates:

1. A results directory named "{filename}_results" (e.g., "license_data_results")
2. Inside this directory:
   - Excel report: license_data_analysis.xlsx
   - Summary report: license_data_summary_report.txt
   - PDF report (except in license_analyser_without.py): license_data_report.pdf
   - Visualization files:
     - license_data_usage_summary.png
     - license_data_usage_heatmap.png
     - license_data_usage_categories_by_hour.png
     - license_data_monthly_comparison.png (if applicable)
     - license_data_holiday_usage.png (if applicable)

## Report Contents

### Excel Report
The Excel report contains multiple worksheets:
1. **Executive Summary** - Key metrics and usage statistics
2. **Daily Usage By Hour** - Detailed breakdown of usage patterns by hour
3. **Day Of Week Analysis** - Usage patterns for each weekday
4. **Specific Days Analysis** - Individual day analysis with usage categories
5. **Hourly License Usage** - Technical data on hourly usage statistics

### Summary Text Report
The text report includes:
1. Executive summary and key findings
2. License information and data statistics
3. Usage summaries with percentage breakdowns
4. License optimization recommendations
5. Day-of-week and hourly analysis
6. Monthly analysis (when applicable)
7. Holiday-specific analysis (when applicable)

### PDF Report (not in license_analyser_without.py)
The PDF report contains formatted versions of the same information as the text report, with:
1. Executive summary
2. License information
3. Usage statistics tables
4. Peak usage analysis
5. Hourly and daily breakdowns
6. Holiday analysis (when applicable)

### Visualizations
1. **usage_summary.png** - Multi-panel visualization with:
   - Average license usage by hour
   - Usage category distribution pie chart
   - Day-of-week usage patterns
   - Holiday comparison (when applicable)
   
2. **usage_heatmap.png** - Heatmap of usage by day of week and hour

3. **usage_categories_by_hour.png** - Bar chart showing distribution of usage categories by hour

4. **monthly_comparison.png** - Month-to-month comparison (when multiple months present)

5. **holiday_usage.png** - Holiday-specific analysis (when applicable)

## Custom German Holiday Implementation

The **license_analyser_with_holidays.py** version includes a custom `GermanHolidays` class that provides enhanced holiday detection for Germany with state-specific holidays, particularly for Bavaria (BY).

Key features of this implementation:
- Support for all German federal holidays
- State-specific holidays (using state codes like BY for Bavaria)
- Proper handling of Easter-related floating holidays
- German holiday names with English translations
- Special handling of religious holidays in specific states

The class supports state codes:
- BY: Bavaria
- BW: Baden-Württemberg
- BE: Berlin
- BB: Brandenburg
- HB: Bremen
- HH: Hamburg
- HE: Hesse
- MV: Mecklenburg-Western Pomerania
- NI: Lower Saxony
- NW: North Rhine-Westphalia
- RP: Rhineland-Palatinate
- SL: Saarland
- SN: Saxony
- ST: Saxony-Anhalt
- SH: Schleswig-Holstein
- TH: Thuringia

## Understanding the Results

### License Usage Categories

The analysis categorizes license usage into four tiers:

1. **High Usage (≥90%)** - Critical periods where licenses are nearly depleted
   - Risk of license shortage
   - Potential productivity impact
   - Consider adding licenses if occurs frequently

2. **Medium-High Usage (70-90%)** - Significant usage but with buffer
   - Good utilization of purchased licenses
   - Small buffer available
   - Optimal operating range for cost-efficiency

3. **Medium Usage (50-70%)** - Moderate usage with comfortable buffer
   - Adequate license availability
   - No immediate risk of shortage
   - Potential for optimization in some scenarios

4. **Low Usage (<50%)** - Low utilization
   - Excess license capacity
   - Potential for license reduction
   - Cost-saving opportunity

### Optimization Recommendations

The "Licenses needed for 95% coverage" metric shows how many licenses would be sufficient to cover 95% of historical usage scenarios. The potential reduction value indicates how many licenses could potentially be reduced while maintaining this 95% coverage level.

### Holiday Analysis (in supported versions)

The holiday analysis shows:
1. How usage differs between holidays and regular working days
2. Which specific holidays have unusual usage patterns
3. Whether holiday-specific license policies might be beneficial

## Best Practices

1. **Data Quality**: Use continuous monitoring data rather than spot samples for better analysis.

2. **Time Ranges**: When analyzing business hours, set appropriate `--start-hour` and `--end-hour` parameters.

3. **Multi-file Analysis**: When combining files, ensure they cover different time periods to avoid duplicate data.

4. **Resampling**: Use resampling (`--resample`) when combining files with different sampling rates.

5. **Version Selection**: Choose the version appropriate for your region and holiday requirements.

6. **Regular Analysis**: Run the analysis periodically (monthly or quarterly) to track trends over time.

7. **Decision Making**: Use the 95% coverage metric for license optimization decisions rather than peak usage.

## Troubleshooting

### Common Issues

1. **"File not found" error**:
   - Verify the file path is correct and accessible
   - Check for spaces or special characters in the path
   - Use quotes around file paths with spaces

2. **Date parsing errors**:
   - Check that date formats in your file are consistent
   - If dates are in an unusual format, try preprocessing the file
   - Ensure date strings include time components

3. **Column detection issues**:
   - Rename columns to match expected format: "Time", "avail", "used"
   - Check for hidden columns or unexpected characters
   - Use the exact column format shown in the input data format section

4. **Holiday detection not working**:
   - Ensure you're using the version with holiday support
   - Check that the holidays package is installed (`pip install holidays`)
   - Verify your data includes dates that are actual holidays

5. **Memory errors with large files**:
   - Use resampling to reduce data density
   - Process files individually rather than combining
   - Run on a machine with more RAM

### Getting Help

If you encounter issues not covered in this documentation:

1. Check if your input data matches the expected format
2. Verify all dependencies are installed
3. Try with a smaller subset of data to isolate the problem
4. Run with explicit parameters rather than defaults

## Conclusion

The License Usage Analyzer toolkit provides powerful capabilities for analyzing software license usage patterns and identifying optimization opportunities. By selecting the appropriate version (standard, lightweight, or enhanced) and using the available features, organizations can make data-driven decisions about license allocation and potentially reduce costs while ensuring license availability when needed.
