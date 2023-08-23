---
layout: post
title:  "Example Looker To pandas Pipeline"
date:   2023-08-23 16:00:00 +0800
categories: looker pandas
---

This brief example shows you how to pull ChainArgos data out of Looker queries and into a pandas
DataFrame for analysis.
If you can work with pandas you surely have enough technical knowledge to get this working.

## Looker
First set up your queries [here](https://chainargosbi.cloud.looker.com). Do not worry if the results are
truncated in the UI -- that won't be a limitation here.

## Google Sheets
Now we are going to pull those query results into Google Sheets.
Google gives simple instructions [here](https://cloud.google.com/looker/docs/connected-sheets).

Set up whatever pivot tables you want from your looker queries.
[Here](https://docs.google.com/spreadsheets/d/10_Urfe_U-LuvydpxWozPtkEdh7tUNFFtMlmYitNapV0/) is an example
sheet.

Note the document ID -- the long random-looking string of characters near the end of the URL -- and the
names of the sheets you want to extract.

## Pandas

Here is a single-sheet example:
```python
import pandas as pd

# the document ID
DOC_ID = '10_Urfe_U-LuvydpxWozPtkEdh7tUNFFtMlmYitNapV0'
# turn this in to a URL that dumps a csv file
SHEET_URI = 'https://docs.google.com/spreadsheets/d/' + DOC_ID + '/gviz/tq?tqx=out:csv&sheet=some_sheet'

# thats it. pandas does the rest.
from_df = pd.read_csv(SHEET_URI)

```

If you want to work with data from several sheets that is not much more work:
```python
import pandas as pd

# the document ID
DOC_ID = '10_Urfe_U-LuvydpxWozPtkEdh7tUNFFtMlmYitNapV0'
# turn this in to a URL that dumps a csv file
SHEET_URI = 'https://docs.google.com/spreadsheets/d/' + DOC_ID + '/gviz/tq?tqx=out:csv&sheet='

# one uri per sheet
FROM_URI = SHEET_URI + 'from_table'
TO_URI = SHEET_URI + 'to_table'

# grab both results
from_df = pd.read_csv(FROM_URI)
to_df = pd.read_csv(TO_URI)

# merge the two separate pivot tables
merged_df = pd.merge(from_df, to_df, left_on='From Address', right_on='To Address')
```

## Periodic Updates

You can configure the Google doc to automatically update the query results by following Google's
simple instructions [here](https://cloud.google.com/looker/docs/connected-sheets#scheduling_regular_refresh_times).

That's it. It'd be no-code except you *wanted* to use pandas.
