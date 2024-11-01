# Bar-Maker

A simple program to create a file containing one or more vertical bars which map values for a certain characteristic (e.g. clonal complexes or resistance to an antibiotic) according to a given order (e.g. the order in which strains appear in a phylogenetic tree). This can be a useful way to visualise the clustering of the chosen characteristic(s) within the given order and/or with each other.

Works on Windows and Linux, not tested on MacOS.


## Dependencies

plotly

kaleido

pandas

numpy

On Windows, depending on Windows and python versions, kaleido may cause a hang up where keyboard interrupt does nothing - this can be solved by installing another kaleido version - `pip install kaleido==0.1.0.post1`

If the above hang up happens, make sure to use Task Manager to manually stop kaleido (under Background Processes) as it will keep going even after closing the python instance or command line!

The same hang up may also happen on Linux and can be fixed with `pip install kaleido==0.1.0` - even with this version, kaleido may still run after the process is complete which can cause hang ups when running the program multiple times - this can be solved by manually ending the kaleido task before running the program again (this issue may be specific to virtual Linux machines on Windows).

If on Linux WSL1 - use pandas v1.24 or older, otherwise the program will hang up, but can still be keyboard interrupted - `pip install pandas==1.24`


## Usage

Download the script and put into the directory you want to run it from.


The input file should be a file in a tab-delimited format (e.g. .tsv or .txt) with 1 row per sample & 1 column per characteristic and can include columns that will not be processed. The input file can easily be created by copying from an excel sheet into a txt file or saving an excel sheet in a tab-delimited file format.

The below file is an example for very few (this program is best used for large datasets) _C. jejuni_ isolates with data for clonal complex, quinolone resistance, tetracycline resistance and resistance to both antibiotic classes (resistance marked as 1). Note the id and cgMLST columns will be ignored as they are not defined in char_dict below.




    cgMLST Order	id	Quinolone Resistant	Tetracycline Resistant	Resistant to Both	CC

    104	        28604	          1	                     0	                0	       CC48

    105	        22228	          1	                     0	                0	       CC48

    106	        43065	          1	                     0	                0	       CC48

    107	        77649	          1	                     0	                0	       CC48

    108	        77812	          0	                     0	                0	       CC48

    109	        25404             0	                     0	                0	       CC21

    110	        47326	          1	                     1	                1	       CC206

    111	        18233	          1	                     1	                1	       CC206



You should edit the script itself before usage in 2 main ways - setting up dictionaries of value:colour pairs and pairing column headers to the appropriate dictionaries with the char_dict dictionary. The below examples are also found in the script itself.

Firstly, set up dictionaries which relate the possible values for a certain characteristic to the RGB values of the desired colours. RGB values should be in [n, n, n] format, where n is a number between 0 and 255. Unassigned values will default to white, so not all values need to be defined if only looking at a subset of them.

    Example dictionary for _C. jejuni_ Clonal Complexes:               Example dictionary for resistance presence/absence (absence not defined as it will be white for a black/white pattern):

    CC_colours = {"CC21":[31, 73, 125],                                Resistance_colours = {"1":[0, 0, 0]}

                  "CC61":[79, 129, 189],
              
                  "CC464":[128, 100, 162],
              
                  "CC353":[155, 187, 89],
              
                  "CC354":[148, 138, 84],
              
                  "CC257":[192, 80, 77],
              
                  "CC45":[247, 150, 70]}

You should then edit the char_dict dictionary to relate the headers of each column to the appropriate dictionary. If a header is defined here but does not appear in the file it will be ignored, so you can build it out without deleting previous header:dictionary pairs. The most important part is that the header should exactly match the header (first row) of the appropriate column. Multiple headers can be matched to the same dictionary.
You can also have unmatched dictionaries (i.e defined as above but not part of char_dict) if you don't want to use them but also don't want to delete them. 

The bars will be produced in the order in which headers are defined in char_dict, NOT the order in which they are input - keep this is mind as the program does not (currently) put headers on the bars.

Below is an example of char_dict for the dictionaries above. CC_colours will only be used for the "CC" column, whereas Resistance_colours will be used for the other 3 characteristics to create 3 black and white (presence/absence) bars for the antibiotic resistances:

    char_dict = {"CC":CC_colours, "Quinolone Resistant":Resistance_colours, 
                "Tetracycline Resistant":Resistance_colours, "Resistant to Both":Resistance_colours}

Now you're all set! Run the program with `python bar_maker.py optional/path/input_file.txt`



## Output

The output will be a pdf file with the resulting image in the same directory as the input file and with the same name with the file extension switched to .pdf. The below example is the output for the above dictionaries and char_dict but with a larger input dataset. From left to right: clonal complex, quinolone resistance, tetracycline resistance and resistance to both as defined in char_dict.



![image](https://github.com/user-attachments/assets/612a074c-8f6c-40b5-b597-6f0549a5b3e5)


## Features To Be Added

Testing around kaleido versions and Linux distributions to solve hang ups when running the program multiple times in succession

`-o` to specify output file name - this will also enable changing of output file format

Headers for characteristics & legends for value:colour pairs

Feel free to request features! (No promises though 😜)
