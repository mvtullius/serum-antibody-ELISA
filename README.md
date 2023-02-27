# **serum-antibody-ELISA**
This python notebook processes raw data from 96-well plate serum antibody ELISAs to visualize the data and calculate antibody titers.
<br>

# Edit this cell to incude your input directory and input file, which contains all of the parameters for the experiment.
<br>

### The input file (Excel Spreadsheet) must have the following sheets:
<br>

* The 'Directories' sheet contains the names of Directories and Files.
* The 'Conditions' sheet contains all the necessary information regarding the content of the wells of all 96-well plates used in this experiment.
* The 'Plates' sheet contains the file names for the raw data for the 96-well plates.
* The 'Dilutions' sheet contains the fold-dilution values.
<br><br>

```python
class ExperimentName(Enum):
    EXAMPLE = auto()
    Bp_m01 = auto()
    Bp_m02 = auto()

class Experiment:
    
    def __init__(self, name, input_directory, input_file_name):
        
        self.name = name
        self.input_directory = input_directory
        self.input_file_name = input_file_name
        
experiments = {}
 
experiments[ExperimentName.EXAMPLE] = Experiment(ExperimentName.EXAMPLE,
                                                 './example',
                                                 'example input.xlsx')

experiments[ExperimentName.Bp_m01] = Experiment(ExperimentName.Bp_m01,
                                                r'C:\Users\mitullius\Notebook\2023\Bp vaccine paper\Re-analysis of Serum Ab\Bp_m01',
                                                'Bp m01 serum antibody - plate maps 2021 05-05.xlsx')

experiments[ExperimentName.Bp_m02] = Experiment(ExperimentName.Bp_m02,
                                                r'C:\Users\mitullius\Notebook\2023\Bp vaccine paper\Re-analysis of Serum Ab\Bp_m02',
                                                'Bp m02 serum antibody - plate maps 2021 03-22.xlsx')

```
<br>

### ***Directories Sheet from example input file:***
<br>

| Key                | Value                                                                 |
|--------------------|-----------------------------------------------------------------------|
| Raw Data Directory | ./raw data                                                            |
| Output Directory   | ./output {DATE}                                                       |
| Output File        | EXAMPLE serum antibody - Data Formatted for GraphPad Prism {DATE}.xlsx |
| Plates File        | EXAMPLE serum antibody - All Plates {DATE}.xlsx                        |
<br>

### ***Conditions Sheet from example input file:***
<br>

| Group | # | Mouse | Plate | Dilution_1 | Dilution_2 | Dilution_3 | Dilution_4 | Dilution_5 | Dilution_6 |
|-------|---|-------|-------|------------|------------|------------|------------|------------|------------|
| A     | 1 | A1    | AB1   | A1         | A2         | A3         | A4         | A5         | A6         |
| A     | 2 | A2    | AB1   | B1         | B2         | B3         | B4         | B5         | B6         |
| A     | 3 | A3    | AB1   | C1         | C2         | C3         | C4         | C5         | C6         |
| A     | 4 | A4    | AB1   | D1         | D2         | D3         | D4         | D5         | D6         |
| A     | 5 | A5    | AB1   | E1         | E2         | E3         | E4         | E5         | E6         |
| A     | 6 | A6    | AB1   | F1         | F2         | F3         | F4         | F5         | F6         |
| A     | 7 | A7    | AB1   | G1         | G2         | G3         | G4         | G5         | G6         |
| A     | 8 | A8    | AB1   | H1         | H2         | H3         | H4         | H5         | H6         |
| B     | 1 | B1    | AB1   | H12        | H11        | H10        | H9         | H8         | H7         |
| B     | 2 | B2    | AB1   | G12        | G11        | G10        | G9         | G8         | G7         |
| B     | 3 | B3    | AB1   | F12        | F11        | F10        | F9         | F8         | F7         |
| B     | 4 | B4    | AB1   | E12        | E11        | E10        | E9         | E8         | E7         |
| B     | 5 | B5    | AB1   | D12        | D11        | D10        | D9         | D8         | D7         |
| B     | 6 | B6    | AB1   | C12        | C11        | C10        | C9         | C8         | C7         |
| B     | 7 | B7    | AB1   | B12        | B11        | B10        | B9         | B8         | B7         |
| B     | 8 | B8    | AB1   | A12        | A11        | A10        | A9         | A8         | A7         |
| A     | 1 | A1    | AB2   | A1         | A2         | A3         | A4         | A5         | A6         |
| A     | 2 | A2    | AB2   | B1         | B2         | B3         | B4         | B5         | B6         |
| A     | 3 | A3    | AB2   | C1         | C2         | C3         | C4         | C5         | C6         |
| A     | 4 | A4    | AB2   | D1         | D2         | D3         | D4         | D5         | D6         |
| A     | 5 | A5    | AB2   | E1         | E2         | E3         | E4         | E5         | E6         |
| A     | 6 | A6    | AB2   | F1         | F2         | F3         | F4         | F5         | F6         |
| A     | 7 | A7    | AB2   | G1         | G2         | G3         | G4         | G5         | G6         |
| A     | 8 | A8    | AB2   | H1         | H2         | H3         | H4         | H5         | H6         |
| B     | 1 | B1    | AB2   | H12        | H11        | H10        | H9         | H8         | H7         |
| B     | 2 | B2    | AB2   | G12        | G11        | G10        | G9         | G8         | G7         |
| B     | 3 | B3    | AB2   | F12        | F11        | F10        | F9         | F8         | F7         |
| B     | 4 | B4    | AB2   | E12        | E11        | E10        | E9         | E8         | E7         |
| B     | 5 | B5    | AB2   | D12        | D11        | D10        | D9         | D8         | D7         |
| B     | 6 | B6    | AB2   | C12        | C11        | C10        | C9         | C8         | C7         |
| B     | 7 | B7    | AB2   | B12        | B11        | B10        | B9         | B8         | B7         |
| B     | 8 | B8    | AB2   | A12        | A11        | A10        | A9         | A8         | A7         |
| C     | 1 | C1    | CD1   | A1         | A2         | A3         | A4         | A5         | A6         |
| C     | 2 | C2    | CD1   | B1         | B2         | B3         | B4         | B5         | B6         |
| C     | 3 | C3    | CD1   | C1         | C2         | C3         | C4         | C5         | C6         |
| C     | 4 | C4    | CD1   | D1         | D2         | D3         | D4         | D5         | D6         |
| C     | 5 | C5    | CD1   | E1         | E2         | E3         | E4         | E5         | E6         |
| C     | 6 | C6    | CD1   | F1         | F2         | F3         | F4         | F5         | F6         |
| C     | 7 | C7    | CD1   | G1         | G2         | G3         | G4         | G5         | G6         |
| C     | 8 | C8    | CD1   | H1         | H2         | H3         | H4         | H5         | H6         |
| D     | 1 | D1    | CD1   | H12        | H11        | H10        | H9         | H8         | H7         |
| D     | 2 | D2    | CD1   | G12        | G11        | G10        | G9         | G8         | G7         |
| D     | 3 | D3    | CD1   | F12        | F11        | F10        | F9         | F8         | F7         |
| D     | 4 | D4    | CD1   | E12        | E11        | E10        | E9         | E8         | E7         |
| D     | 5 | D5    | CD1   | D12        | D11        | D10        | D9         | D8         | D7         |
| D     | 6 | D6    | CD1   | C12        | C11        | C10        | C9         | C8         | C7         |
| D     | 7 | D7    | CD1   | B12        | B11        | B10        | B9         | B8         | B7         |
| D     | 8 | D8    | CD1   | A12        | A11        | A10        | A9         | A8         | A7         |
| C     | 1 | C1    | CD2   | A1         | A2         | A3         | A4         | A5         | A6         |
| C     | 2 | C2    | CD2   | B1         | B2         | B3         | B4         | B5         | B6         |
| C     | 3 | C3    | CD2   | C1         | C2         | C3         | C4         | C5         | C6         |
| C     | 4 | C4    | CD2   | D1         | D2         | D3         | D4         | D5         | D6         |
| C     | 5 | C5    | CD2   | E1         | E2         | E3         | E4         | E5         | E6         |
| C     | 6 | C6    | CD2   | F1         | F2         | F3         | F4         | F5         | F6         |
| C     | 7 | C7    | CD2   | G1         | G2         | G3         | G4         | G5         | G6         |
| C     | 8 | C8    | CD2   | H1         | H2         | H3         | H4         | H5         | H6         |
| D     | 1 | D1    | CD2   | H12        | H11        | H10        | H9         | H8         | H7         |
| D     | 2 | D2    | CD2   | G12        | G11        | G10        | G9         | G8         | G7         |
| D     | 3 | D3    | CD2   | F12        | F11        | F10        | F9         | F8         | F7         |
| D     | 4 | D4    | CD2   | E12        | E11        | E10        | E9         | E8         | E7         |
| D     | 5 | D5    | CD2   | D12        | D11        | D10        | D9         | D8         | D7         |
| D     | 6 | D6    | CD2   | C12        | C11        | C10        | C9         | C8         | C7         |
| D     | 7 | D7    | CD2   | B12        | B11        | B10        | B9         | B8         | B7         |
| D     | 8 | D8    | CD2   | A12        | A11        | A10        | A9         | A8         | A7         |
| E     | 1 | E1    | EF1   | A1         | A2         | A3         | A4         | A5         | A6         |
| E     | 2 | E2    | EF1   | B1         | B2         | B3         | B4         | B5         | B6         |
| E     | 3 | E3    | EF1   | C1         | C2         | C3         | C4         | C5         | C6         |
| E     | 4 | E4    | EF1   | D1         | D2         | D3         | D4         | D5         | D6         |
| E     | 5 | E5    | EF1   | E1         | E2         | E3         | E4         | E5         | E6         |
| E     | 6 | E6    | EF1   | F1         | F2         | F3         | F4         | F5         | F6         |
| E     | 7 | E7    | EF1   | G1         | G2         | G3         | G4         | G5         | G6         |
| E     | 8 | E8    | EF1   | H1         | H2         | H3         | H4         | H5         | H6         |
| F     | 1 | F1    | EF1   | H12        | H11        | H10        | H9         | H8         | H7         |
| F     | 2 | F2    | EF1   | G12        | G11        | G10        | G9         | G8         | G7         |
| F     | 3 | F3    | EF1   | F12        | F11        | F10        | F9         | F8         | F7         |
| F     | 4 | F4    | EF1   | E12        | E11        | E10        | E9         | E8         | E7         |
| F     | 5 | F5    | EF1   | D12        | D11        | D10        | D9         | D8         | D7         |
| F     | 6 | F6    | EF1   | C12        | C11        | C10        | C9         | C8         | C7         |
| F     | 7 | F7    | EF1   | B12        | B11        | B10        | B9         | B8         | B7         |
| F     | 8 | F8    | EF1   | A12        | A11        | A10        | A9         | A8         | A7         |
| E     | 1 | E1    | EF2   | A1         | A2         | A3         | A4         | A5         | A6         |
| E     | 2 | E2    | EF2   | B1         | B2         | B3         | B4         | B5         | B6         |
| E     | 3 | E3    | EF2   | C1         | C2         | C3         | C4         | C5         | C6         |
| E     | 4 | E4    | EF2   | D1         | D2         | D3         | D4         | D5         | D6         |
| E     | 5 | E5    | EF2   | E1         | E2         | E3         | E4         | E5         | E6         |
| E     | 6 | E6    | EF2   | F1         | F2         | F3         | F4         | F5         | F6         |
| E     | 7 | E7    | EF2   | G1         | G2         | G3         | G4         | G5         | G6         |
| E     | 8 | E8    | EF2   | H1         | H2         | H3         | H4         | H5         | H6         |
| F     | 1 | F1    | EF2   | H12        | H11        | H10        | H9         | H8         | H7         |
| F     | 2 | F2    | EF2   | G12        | G11        | G10        | G9         | G8         | G7         |
| F     | 3 | F3    | EF2   | F12        | F11        | F10        | F9         | F8         | F7         |
| F     | 4 | F4    | EF2   | E12        | E11        | E10        | E9         | E8         | E7         |
| F     | 5 | F5    | EF2   | D12        | D11        | D10        | D9         | D8         | D7         |
| F     | 6 | F6    | EF2   | C12        | C11        | C10        | C9         | C8         | C7         |
| F     | 7 | F7    | EF2   | B12        | B11        | B10        | B9         | B8         | B7         |
| F     | 8 | F8    | EF2   | A12        | A11        | A10        | A9         | A8         | A7         |
| G     | 1 | G1    | GH1   | A1         | A2         | A3         | A4         | A5         | A6         |
| G     | 2 | G2    | GH1   | B1         | B2         | B3         | B4         | B5         | B6         |
| G     | 3 | G3    | GH1   | C1         | C2         | C3         | C4         | C5         | C6         |
| G     | 4 | G4    | GH1   | D1         | D2         | D3         | D4         | D5         | D6         |
| G     | 5 | G5    | GH1   | E1         | E2         | E3         | E4         | E5         | E6         |
| G     | 6 | G6    | GH1   | F1         | F2         | F3         | F4         | F5         | F6         |
| G     | 7 | G7    | GH1   | G1         | G2         | G3         | G4         | G5         | G6         |
| G     | 8 | G8    | GH1   | H1         | H2         | H3         | H4         | H5         | H6         |
| H     | 1 | H1    | GH1   | H12        | H11        | H10        | H9         | H8         | H7         |
| H     | 2 | H2    | GH1   | G12        | G11        | G10        | G9         | G8         | G7         |
| H     | 3 | H3    | GH1   | F12        | F11        | F10        | F9         | F8         | F7         |
| H     | 4 | H4    | GH1   | E12        | E11        | E10        | E9         | E8         | E7         |
| H     | 5 | H5    | GH1   | D12        | D11        | D10        | D9         | D8         | D7         |
| H     | 6 | H6    | GH1   | C12        | C11        | C10        | C9         | C8         | C7         |
| H     | 7 | H7    | GH1   | B12        | B11        | B10        | B9         | B8         | B7         |
| H     | 8 | H8    | GH1   | A12        | A11        | A10        | A9         | A8         | A7         |
| G     | 1 | G1    | GH2   | A1         | A2         | A3         | A4         | A5         | A6         |
| G     | 2 | G2    | GH2   | B1         | B2         | B3         | B4         | B5         | B6         |
| G     | 3 | G3    | GH2   | C1         | C2         | C3         | C4         | C5         | C6         |
| G     | 4 | G4    | GH2   | D1         | D2         | D3         | D4         | D5         | D6         |
| G     | 5 | G5    | GH2   | E1         | E2         | E3         | E4         | E5         | E6         |
| G     | 6 | G6    | GH2   | F1         | F2         | F3         | F4         | F5         | F6         |
| G     | 7 | G7    | GH2   | G1         | G2         | G3         | G4         | G5         | G6         |
| G     | 8 | G8    | GH2   | H1         | H2         | H3         | H4         | H5         | H6         |
| H     | 1 | H1    | GH2   | H12        | H11        | H10        | H9         | H8         | H7         |
| H     | 2 | H2    | GH2   | G12        | G11        | G10        | G9         | G8         | G7         |
| H     | 3 | H3    | GH2   | F12        | F11        | F10        | F9         | F8         | F7         |
| H     | 4 | H4    | GH2   | E12        | E11        | E10        | E9         | E8         | E7         |
| H     | 5 | H5    | GH2   | D12        | D11        | D10        | D9         | D8         | D7         |
| H     | 6 | H6    | GH2   | C12        | C11        | C10        | C9         | C8         | C7         |
| H     | 7 | H7    | GH2   | B12        | B11        | B10        | B9         | B8         | B7         |
| H     | 8 | H8    | GH2   | A12        | A11        | A10        | A9         | A8         | A7         |
<br>


### ***Plates Sheet from example input file:***
<br>

| Plate | File                                | Group   | Antigen   | 1st Ab | 2nd Ab | Condition                                                     |
|-------|-------------------------------------|---------|-----------|--------|--------|---------------------------------------------------------------|
| AB1   | Plate AB1 (Serum Ab) 2021 05-20.xls | A and B | Antigen 1 | serum  | IgG    | Group A and B, Antigen: Antigen 1, 1st Ab: serum, 2nd Ab: IgG |
| AB2   | Plate AB2 (Serum Ab) 2021 05-20.xls | A and B | Antigen 2 | serum  | IgG    | Group A and B, Antigen: Antigen 2, 1st Ab: serum, 2nd Ab: IgG |
| CD1   | Plate CD1 (Serum Ab) 2021 05-20.xls | C and D | Antigen 1 | serum  | IgG    | Group C and D, Antigen: Antigen 1, 1st Ab: serum, 2nd Ab: IgG |
| CD2   | Plate CD2 (Serum Ab) 2021 05-20.xls | C and D | Antigen 2 | serum  | IgG    | Group C and D, Antigen: Antigen 2, 1st Ab: serum, 2nd Ab: IgG |
| EF1   | Plate EF1 (Serum Ab) 2021 05-20.xls | E and F | Antigen 1 | serum  | IgG    | Group E and F, Antigen: Antigen 1, 1st Ab: serum, 2nd Ab: IgG |
| EF2   | Plate EF2 (Serum Ab) 2021 05-20.xls | E and F | Antigen 2 | serum  | IgG    | Group E and F, Antigen: Antigen 2, 1st Ab: serum, 2nd Ab: IgG |
| GH1   | Plate GH1 (Serum Ab) 2021 05-20.xls | G and H | Antigen 1 | serum  | IgG    | Group G and H, Antigen: Antigen 1, 1st Ab: serum, 2nd Ab: IgG |
| GH2   | Plate GH2 (Serum Ab) 2021 05-20.xls | G and H | Antigen 2 | serum  | IgG    | Group G and H, Antigen: Antigen 2, 1st Ab: serum, 2nd Ab: IgG |

<br>


### ***Dilutions Sheet from example input file:***
<br>

| Dilution_Name | Dilution |
|---------------|----------|
| Dilution_1    | 200      |
| Dilution_2    | 800      |
| Dilution_3    | 3,200    |
| Dilution_4    | 12,800   |
| Dilution_5    | 51,200   |
| Dilution_6    | 204,800  |

<br>

### ***Example Output for a particular Antigen and Group with 8 mice:***
<br>

![Example Output](https://github.com/mvtullius/serum-antibody-ELISA/blob/main/example/output%202023-02-27/Antigen%201%20-%20(Group%20C)%20%5BA415-A750%5D%202023-02-27.png)


### ***Example Output (html file) for all Antigens and Groups:***
<br>

[Example Figure](https://github.com/mvtullius/serum-antibody-ELISA/blob/main/example/output%202023-02-27/EXAMPLE%20groups%20x%20antigens%20(individual%20mice)%20Log%20A415_minus_A750%202023-02-27.html)

### ***Example Spreadsheet with calculated antibody titers:***
<br>

[Example Spreadsheet](https://github.com/mvtullius/serum-antibody-ELISA/blob/main/example/output%202023-02-27/EXAMPLE%20serum%20antibody%20-%20Data%20Formatted%20for%20GraphPad%20Prism%202023-02-27.xlsx)