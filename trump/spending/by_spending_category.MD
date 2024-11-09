# DONALD J. TRUMP FOR PRESIDENT, 2024 

## Spending by Category

**Note**: *The spending categories were created to simplify the analysis of disbursements in the Trump campaign’s spending data. The original disbursement_description field contained highly specific and varied descriptions that lacked uniformity. To address this, I grouped similar spending types under standardized categories, making it easier to identify spending patterns in key areas*.

If you’re interested in seeing a similar comparison of spending categories for the Trump campaign, look [here](https://github.com/gaiaus/2024-us-presidential-general-election/blob/main/harris/spending/by_spending_category.MD).

*The methodology for estimating total possible disbursements across spending categories varies between the Trump and Harris campaigns, reflecting differences in the specificity of disbursement descriptions and the potential informational value of recipient names. This variation impacts the interpretability and granularity of expenditure classification across categories for each campaign*.

Source Data: [FEC](https://www.fec.gov/data/disbursements/?committee_id=C00828541&committee_id=C00867275&two_year_transaction_period=2024&data_type=processed)

Coverage dates: 11/15/2022 to 10/16/2024

| Spending Category          | Total Possible Amount Disbursed | Number of Qualifying Disbursements |
|----------------------------|------------------------|--------------------------|
| Media & Advertising        | $266,797,588.50       | 234                      |
| Travel & Transportation     | $39,046,085.68        | 20,499                   |
| Consulting & Strategy      | $21,417,716.61        | 2,016                    |
| Other                      | $20,043,221.33        | 984                      |
| Personnel                  | $9,005,131.57         | 1,842                    |
| Donations & “Transfers”    | $4,569,852.59         | 62                       |
| Event & Facility Rentals   | $3,391,394.68         | 1,365                    |
| Office & Supplies          | $1,425,118.90         | 3,413                    |
| Technology & Software      | $443,171.34           | 632                      |
| Miscellaneous Fees         | $412,751.40           | 305                      |
| Reimbursements             | $54,607.80            | 16                       |


```python
# Based on the most frequent words, define categories that capture common disbursement types
# These categories are derived from the analysis of frequently occurring words

# Updated categories and their associated keywords
category_keywords_updated = {
    "Travel": ["TRAVEL", "AIR", "LODGING", "CAR RENTAL", "GROUND TRANSPORTATION", "FOOD", "RAIL", "TOLLS", "FUEL", "PARKING"],
    "Office Supplies & Fees": ["OFFICE", "SUPPLIES", "MERCHANT FEES", "TRANSACTION FEE", "BANK FEES", "DELIVERY"],
    "Reimbursement": ["REIMBURSEMENT"],
    "Event Services": ["EVENT", "STAGING", "CATERING", "MEETING", "FACILITY", "RENTAL"],
    "Media": ["MEDIA", "ADVERTISING", "DIGITAL", "PRINT", "PROMOTIONAL"],
    "Consulting/Strategy": ["STRATEGY", "CONSULTING", "FUNDRAISING"],
    "Postage & Mailing": ["POSTAGE", "MAIL", "SHIPPING", "DELIVERY"],
    "Technology & Software": ["SOFTWARE", "SUBSCRIPTIONS", "TECHNOLOGY", "WEBSITE", "ONLINE"]
}

# Reapply categorization using the updated category keywords
def categorize_disbursement_updated(description):
    for category, keywords in category_keywords_updated.items():
        if any(keyword in description.upper() for keyword in keywords):
            return category
    return "Other"  # Assign "Other" if no keywords match

# Apply the updated categorization function
corrected_trump_data['updated_category'] = corrected_trump_data['disbursement_description'].apply(categorize_disbursement_updated)

# Aggregate data by the updated category column to calculate total spending and counts
updated_category_totals = corrected_trump_data.groupby('updated_category').agg(
    total_amount_disbursed=('disbursement_amount', 'sum'),
    number_of_disbursements=('disbursement_amount', 'size')
).reset_index()

# Sort the results by total amount disbursed in descending order
updated_category_totals = updated_category_totals.sort_values(by='total_amount_disbursed', ascending=False)


