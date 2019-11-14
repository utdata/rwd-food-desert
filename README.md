# Food desert data

Some work to help some students on a project. We are using a python-based package called [csvkit](https://csvkit.readthedocs.io/en/latest/) to manage a large Excel file without having Excel.

## Resources

- [csvkit documentation](https://csvkit.readthedocs.io/en/latest/).
- Another [tutorial on using using csvkit](https://github.com/utdata/csvkit-nicar2018).

## Getting the data

Data file _Food Access Research Atlas Data Download 2015_ downloaded from [United States Department of Agriculture](https://www.ers.usda.gov/data-products/food-access-research-atlas/download-the-data/). Filename is `DataDownload2015.xlsx` in data-raw. This has three tabs. I saved the tab **Variables Lookup** as it's own file `data-dictionary.xlsx` for reference in a smaller file.

FWIW, I also used Excel to SaveAs a csv as `data-processed/DataDownload2015.csv` for the data. Directions below convert to csv from the original using csvkit. Interesting that is a much bigger file, probably because of quoting of text.

Most of this that follows is done in a Terminal.

## Install Miniconda if you don't have it

- [Go here](https://docs.conda.io/en/latest/miniconda.html) to download the Python 3.7 version of Miniconda. The **pkg** version is probably easiest ... just double-click on it to install.
- If using the **bash** version: [installation instructions](https://conda.io/projects/conda/en/latest/user-guide/install/macos.html).

## Create a conda environment

- Create a new environment or "env" that includes the csvkit package:

`conda create -n data csvkit`

The creates an env called **data**. You only have to do this once on your computer. (After that, you just use `conda activate data` to load the env.)

## Preparing the project

- Create a folder for the project.
- Create two folders inside called **data-raw** and **data-processed**. This is so we won't overwrite the original data. Just good practice.
- Put your downloaded Excel file into the **data-raw** folder.

There is some benefit to doing the rest of this from inside VS Code with its integrated Terminal. That way you can see the files created and inspect them.

- Open the project folder.
- Use `cd` to go inside the **data-processed** folder
- Do `conda activate data` to launch the python env.

## Using csvkit to convert, filter, select

- We need to learn what the sheet names are:

`in2csv -n ../data-raw/DataDownload2015.xlsx`

The answer looks like this:

```
Read Me
Variable Lookup
Food Access Research Atlas
```

- Now that we know the sheet name we want **Food Access Research Atlas** we can convert the excel spreadsheet into a csv file:

`in2csv ../data-raw/DataDownload2015.xlsx --sheet "Food Access Research Atlas" -y 200 > food_access.csv`

- We should also convert the record layout for reference purposes:

`in2csv ../data-raw/DataDownload2015.xlsx --sheet "Variable Lookup" > record_layout.csv`

- Next is to get a list of columns names in the **food_access.csv** file so we know what we are looking at:

`csvcut -n food_access.csv`

The answer looks like this:

```
csvcut -n food_access.csv
  1: CensusTract
  2: State
  3: County
  4: Urban
  5: POP2010
  6: OHU2010
  7: GroupQuartersFlag
  8: NUMGQTRS
  9: PCTGQTRS
 10: LILATracts_1And10
 11: LILATracts_halfAnd10
 12: LILATracts_1And20
 13: LILATracts_Vehicle
 14: HUNVFlag
 15: LowIncomeTracts
 16: PovertyRate
 17: MedianFamilyIncome
 18: LA1and10
 19: LAhalfand10
 20: LA1and20
 21: LATracts_half
 22: LATracts1
 23: LATracts10
 24: LATracts20
 25: LATractsVehicle_20
 26: LAPOP1_10
 27: LAPOP05_10
 28: LAPOP1_20
 29: LALOWI1_10
 30: LALOWI05_10
 31: LALOWI1_20
 32: lapophalf
 33: lapophalfshare
 34: lalowihalf
 35: lalowihalfshare
 36: lakidshalf
 37: lakidshalfshare
 38: laseniorshalf
 39: laseniorshalfshare
 40: lawhitehalf
 41: lawhitehalfshare
 42: lablackhalf
 43: lablackhalfshare
 44: laasianhalf
 45: laasianhalfshare
 46: lanhopihalf
 47: lanhopihalfshare
 48: laaianhalf
 49: laaianhalfshare
 50: laomultirhalf
 51: laomultirhalfshare
 52: lahisphalf
 53: lahisphalfshare
 54: lahunvhalf
 55: lahunvhalfshare
 56: lasnaphalf
 57: lasnaphalfshare
 58: lapop1
 59: lapop1share
 60: lalowi1
 61: lalowi1share
 62: lakids1
 63: lakids1share
 64: laseniors1
 65: laseniors1share
 66: lawhite1
 67: lawhite1share
 68: lablack1
 69: lablack1share
 70: laasian1
 71: laasian1share
 72: lanhopi1
 73: lanhopi1share
 74: laaian1
 75: laaian1share
 76: laomultir1
 77: laomultir1share
 78: lahisp1
 79: lahisp1share
 80: lahunv1
 81: lahunv1share
 82: lasnap1
 83: lasnap1share
 84: lapop10
 85: lapop10share
 86: lalowi10
 87: lalowi10share
 88: lakids10
 89: lakids10share
 90: laseniors10
 91: laseniors10share
 92: lawhite10
 93: lawhite10share
 94: lablack10
 95: lablack10share
 96: laasian10
 97: laasian10share
 98: lanhopi10
 99: lanhopi10share
100: laaian10
101: laaian10share
102: laomultir10
103: laomultir10share
104: lahisp10
105: lahisp10share
106: lahunv10
107: lahunv10share
108: lasnap10
109: lasnap10share
110: lapop20
111: lapop20share
112: lalowi20
113: lalowi20share
114: lakids20
115: lakids20share
116: laseniors20
117: laseniors20share
118: lawhite20
119: lawhite20share
120: lablack20
121: lablack20share
122: laasian20
123: laasian20share
124: lanhopi20
125: lanhopi20share
126: laaian20
127: laaian20share
128: laomultir20
129: laomultir20share
130: lahisp20
131: lahisp20share
132: lahunv20
133: lahunv20share
134: lasnap20
135: lasnap20share
136: TractLOWI
137: TractKids
138: TractSeniors
139: TractWhite
140: TractBlack
141: TractAsian
142: TractNHOPI
143: TractAIAN
144: TractOMultir
145: TractHispanic
146: TractHUNV
147: TractSNAP
```

- Now lets filter the data for "State = Texas" and "County = Travis" and save as a new file:

`csvgrep -c State -m "Texas" -c County -m "Travis" food_access.csv > travis.csv`

- Now, we use the column list above to select columns based on their number:

`csvcut -c 1-6,10-13,15-24 travis.csv > travis_select.csv`

We might change those numbers later if we find we want different columns. Note there are no spaces between those commas and numbers.

This creates the final "travis_select.csv" file you can use in Workbench or Tableau. FWIW, you might do the pivots in Workbench using [pandas melt](https://github.com/utdata/rwd-workbench/blob/master/reshape-melt.md).
