MicRooN
=======

A two-step machine based classifier model – miRSEQ and miRINT, which will identify a miR to be associated with cancer and also classify its association, i.e., either with an oncogene or a tumour suppressor.


PREREQUISITE:
1. LibSVM 3.7
2. Python 2.6 or later
3. RNAHybrid


* Linux Based Installation
To use miRSEQ and miRINT, the user needs the following file:


FOR MIRSEQ:
1. Create input file for Libsvm
Let us consider a miR sequence with a Length (L) = 22 , and each let each nucleotide position be named as P1, P2, .... ,PL.  A sliding window size of 2W is considered as the best feature for the sequence. Then the final Training instance will be :

Class   P1  P2  P3  P4  P5  P6  P7  P8  P9  P10 P11 P12 P13 P14 P15 P16 P17 P18 P19 P20 P21 P22 AA  UU  GG  CC
1/2     A   U   A   U   G   G   A   A   U   U   G   G   C   C   U   U   A   A   U   C   A   G   2   2   1   1
1/2     1   2   1   2   3   3   1   1   2   2   3   3   4   4   2   2   1   1   2   4   1   3   2   2   1   1

Where first column indicates the class 1 (Positive)or 2 (negative), followed by the miR and its 2w information.Before the actual training process all nucleotide where converted to number since strings can't be used for training. So we converted A as 1, U as 2, G as 3 and C as 4 and then followed by the 2w size. The last row indicates the actual training set used in LibSVM. Note: For LibSVM the corresponding csv file will be converted to libsvm ,(refer: csv2libsvm.py inlibSVM package). 

syntax:svm-predict [options] test_file model_file output_file

Prediction from model file :    1 means "Associated with cancer"
                                2 means "Not associated with cancer"

FOR MIRINT:

1. miR mature sequence in FASTA format
2. RNAHYBRID Result which is the input for PairFinder script.
3. Calculate 36 features for miRINT- save as csv
4. Convert csv to libsvm - csvlibsvm.py (found in LIBSVM package)

Prediction from model file:     1 means "Oncogenic"
                                2 means "Tumour suppressor genes"
                                
Features to calculate for miRINT:
Base Pair Composition - Sequence Features													
%AG,%AU,%CA,%CC,%CG,%CU,%GA,%GC,%GG,%GU,%UA,%UC,%UG,%UU

Base Pair Composition Per Length of the miRNA - structural Features			
|A-U|/L,|G-C|/L,|G-U|/L	
*- L is set to be 22 nt for  the classifier( In some cases it varies till 24 so mirna length is considered here for the parameter(Considering only actual length)		

Avg_Bp_Stem - Analyzing average base pairs in the stem - Gives idea what base pairing predominates		* These features were calculated both for seed and out seed regions	

%(A-U)/stems,%(G-C)/stems,%(G-U)/stems
of buldges miRNA:mRNA hybridized structure forms loops or buldges frequently, analyzing how often they form buldges is a unique feature		

UP bases - Unpaired bases

Thermodynamic Features: Calculated only from the hybridized structures since we are interested in classifying the mirna based on their hybridzation profile			
MFEI1	dG/%(C+G)		
MFEI2	dG/n_Stems		
MFEI3	dG/n_Loops		
MFEI4	MFE/Total Bases		
fe/gcc	MFE/GC content		

Normalized Parameter for classification		
dG	MFE/L	Removes the bias that long sequences have low MFE	
dP	Tot_bases/L	Normalized Base Pairing Propensity  target sequence.	
dQ	-(P-value.log2(P-value))/L	Base Pairing Probability also denoted as Pij. Usually calculated as matrix from RNApfold algorithm. Here we used hybridized structure with known	
dD	(P-value - (P-value)^2)/L	Normalized base pairing distance	

ZG	Normalized Data since random sequence has been generated and its relationship with the stacked base pairs which is very important in the calculation of MFE	Has to be calculated generating 1000 sequence in Random	
ZP,ZQ,ZD,
Z- score	
formula = mfe - (mean- mfe )/(std deviation)	Binding free energy (Prob Score)	A score to compare the two dataset.
