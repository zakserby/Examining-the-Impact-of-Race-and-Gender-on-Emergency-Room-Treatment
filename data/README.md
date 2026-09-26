# Data

The raw data are not stored in this repository. They are public and can be downloaded from the
U.S. Consumer Product Safety Commission's National Electronic Injury Surveillance System (NEISS):
https://www.cpsc.gov/Research--Statistics/NEISS-Injury-Data

## Extract used in this project

Using the NEISS Estimates Query Builder:

- Years: 2013–2022 (one file per year)
- Age: 16–25
- Diagnoses: serious injuries (fractures, hemorrhage, lacerations, concussions, etc.), excluding dental injuries
- All body parts, races and sexes

## Expected layout

```
data/
├── NEISS_FMT.XLSX        # NEISS format key (code → label lookup)
└── raw/
    ├── NEISS_2013.XLSX
    ├── ...
    └── NEISS_2022.XLSX
```

The analysis reads every `.XLSX` file in `data/raw/` and takes the year from the file name.

## Codes used

| Variable | Codes |
|---|---|
| Sex | 1 = Male, 2 = Female |
| Race | 0 = Not stated, 1 = White (reference), 2 = Black, 3 = Other, 4 = Asian, 5 = American Indian/Alaska Native, 6 = Native Hawaiian/Pacific Islander |
| Disposition | 1 = Treated/examined and released, 2 = Treated and transferred, 4 = Treated and admitted, 5 = Held for observation, 6 = Left without being seen, 8 = Died in the ER |
