## INTRODUCTION
Malaysia's Ministry of Health provides Covid-19 related data on [their Github repository](https://github.com/MoH-Malaysia/covid19-public/) but missing from the data set is the compiled daily reports published on [KKM's blog](https://kpkesihatan.com/) showing the deceased and their associated co-morbidities.  [Individual tables](https://raw.githubusercontent.com/aimrun/covidMY/main/exampletable.jpg) from each publication day of KKM's blog were manually scraped and compiled for the period of 8th October 2020 (first daily report) to 13th July 2021 (last daily report).

## METHODOLOGY
### Data Cleansing & Omission
As the daily report was [provided in Bahasa Malaysia](https://github.com/aimrun/covidMY/blob/main/BM.csv) and had typos (darah tinggi / darah tiggi), unique strings were extracted using Google Sheets UNIQUE function and classified according to WHO ICD-11 Codes.  A VLOOKUP function was used to replace unique strings with the [corresponding ICD-11 code](https://github.com/aimrun/covidMY/blob/main/ICD11.csv).

For the purposes of co-morbidity analysis, the following data/column were omitted:
1.  Hospital of Admission & State
2.  Nationality
3.  Date of Death, Gender, Age
4.  Mortality Number

905 out of 6,240 mortalities recorded had no medical history or were missing a medical history, thus were omitted to enable co-morbidity analysis.

## Constructing the Co-Occurrence Matrix
For each row of data (representing one patient), A Python script (ITERTOOLS toolbox) was used to construct pair combinations of diseases.  An example output of a patient with 3 diseases (Diabetes, Hypertension, Cancer) would be:

| Pair 1    | Pair 2        |
|-----------|---------------|
| Diabetes  | Hypertension  |
| Diabetes  | Cancer        |
| Cancer    | Hypertension  |

Combinations, instead of permutations, were used in constructing the co-occurence matrix as there were no directional data on whether any disease caused another disease.  The result of the combinations for each patient were written to a CSV file.

## Visualisation
A network graph visualisation tool was deemed the best way to visualise probabilities of one disease co-occurring with other diseases.  The result contained within the CSV file above were fed into GEPHI and annotated to provide description of the ICD-11 code description.

![alt text](https://github.com/aimrun/covidMY/blob/main/diseasecooccurrencegraph.png)
