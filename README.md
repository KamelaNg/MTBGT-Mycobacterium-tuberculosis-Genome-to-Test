# TBGT.py README

The TBGT tool can be run on any python-enabled operating system with no additional prerequisites. The tool is comprised of 3 scripts – TBGT, TBGT_IO, and TBGT_Tests, where TBGT.py is the main scipt to be run in the command line, and scripts ‘IO’ and ‘Tests’ are imported into this main script as tbgtio and tbgtt respectively.

TBGT.py parses the inputs, reads in the map file, then creates a dictionary of codons to genome positions. It then sets-up the type of input file (VCF or tab) to be read, keeping the SNP that is present at each of the positions in the genome, and creates a dictionary where the core filename is the key and a dictionary of all genome positions (keys) and their SNPs (if any) (values) is returned. It then sets-up the tests to be run and tables to be outputed as specified by the user.

It is executed as: python3 TBGT.py --folder <folderName> --map <codonMapFile> (optional) --out <outputFilename> (optional) --tables y/n. (optional).

The script accepts two types of input file – VCF or MTBseq tab. The folder containing the input files to be read and processed as well as the file type are required inputs from the user, whereas the map, prefix of output files, and summary table are optional. The user has to specify the path or location of the input files by typing in the argument --folder followed by the path or location of the directory, and the specifc format of the input file by typing in the argument --type followed by either VCF or tab, in which upper or lower case are both acceptable. By default, the module will run all the RDTs and Sanger sequencing on the VCF and tab files. The user has the option to select which individual tests to run. 

The optional inputs include a map file that enumerates codons where mutations are known to be detected by the RDTs, validated in a previous study (Ng et al. 2018) and indicates their relationship with genome positions of the reference standard. If the map file is not supplied, the reference genome is assumed to be H37Rv NC000962.3, and the file ‘default_mapfile.txt’ integrated with the script is used. An example of this tab-separated file is in https://github.com/KamelaNg/TBGT/blob/master/default_mapfile.txt. All codons in this mapfile are used by the Sanger module, whereas Xpert Classic and Ultra and LPAs Hain and Nipro exclude codons 170 and 491 situated outside the rifampicin resistance determining region (RRDR) and are undetected by the commercial RDTs. The Sanger module displays any change in amino acid at the default codon positions. If more codon positions are to be included, the end-user must supply the additional codons to the codon_nuc dictionary in lines 6 to 21 of the TBGT_Tests.py module, including the wildtype nucloetides for the 3 codon positions.

The filename prefix for the output is also an optional input. The default prefix used is genomeToProbe, for example, genomeToProbe_XpertUltra.txt. If to be supplied, the test(s) to run are indicated as one-letter inputs. If the user wants all tests to be run on their vcf or tab file(s), the option --tests is not necessary. To specifiy individual tests, the following code in upper or lower case should be indicated: X for Xpert Classic, U for Xpert Ultra, H for LPA-Hain, N for LPA-Nipro, and S for rpoB Sanger sequencing. Thus, using --tests xu will run classic Xpert Classic and Xpert Ultra only. The letters can be supplied in any order, i.e., --tests xhn and --tests HnX are the same. The final optional input deals with a table summarizing the results generated by the script. If an overview table of the results from each requested test is to be made the --summary y options should be used. This is on by default.

TBGT_IO (tbgtio) is a series of functions to read in and analyze VCF or tab input files, and create tabulated results of RDTs and/or rpoB Sanger sequencing to be run on the inputs, supplemented with RDT probe and/or RR mutation counts and proportions.

TBGT_Tests (tbgtt) is comprised of a dictionary of wildtype codon positions following the Mtb numbering system as the key and the list of codon nucelotides as the value; dictionary of all codon nucelotides to 3 letter amino acid codes; and dictionary of codon positions with the mutated codons and the associated probe change in the RDTs; as well as several functions to create the RDT outputs.

The tbgtt script examines each VCF or tab file in the folder to check for genome positions that are associated with any change in a codon base. For each VCF or tab file, the module will generate a copy of the wild-type (WT) codon patterns, then it will check if the genome position from the VCF or tab file is in a codon of interest. Next, it will determine if the codon is in the mutation list. If so, it will modify the WT codon to be the new mutated codon and will compare the WT and the sample codon lists. In the positions where the WT and sample codon lists differ, the module will tag the corresponding RDT probe with a 0 or 1, in which 0 denotes a capturing or absent WT probe whereas 1 indicates a developing LPA-Hain or LPA-Nipro mutant probe that captures one of the high confidence RR-conferring mutations namely D435V, H445D, H445Y, and S450L. The module will return RIF resistance DETECTED and reflect this in the output file if the sample being tested differs from the WT profile. If the output filename is not supplied, it will be indicated as genomeToProbe_<nameofRDT>.txt.

The rpoB Sanger module extracts all RR-associated SNPs from the whole genome sequences and outputs it as 3-letter WT amino acid followed by the codon number (position) and the 3-letter mutant amino acid.

The generated output file will inform the end-user of the following details for each input file processed: the Sample name, RIF resistance or susceptibility indicated as RIF Resistance DETECTED or NOT DETECTED respectively; mutant codon position, and mutant codon nucleotides, and sample output list which is a series of 0’s or 1’s indicating absence or presence of the capturing probe for the RDTs, and the RR-conferring mutation for rpoB Sanger sequencing. Capturing probe and sample results profile will be shown for classic Xpert and Ultra; absent WT and developing mutant probes, and sample results profile for LPA-Hain and LPA-Nipro; and RR-conferring mutation for rpoB Sanger sequencing. Additionally, a summary table with frequencies and proportions of RS-TB cases detected and RDT probes and RR-conferring mutations will be generated.

If the summary option is selected, a tab delimited file with the frequency and total count of each probe (Xpert, Ultra, Haina nd Nipro) or mutation (Sanger) along with the rifampicin sensitive counts is created. Tests are in the order Xpert, Ultra, Hain, Nipro, Sanger (or a subset as requested).
