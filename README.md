MicRooN
=======

A two-step machine based classifier model – miRSEQ and miRINT, which will identify a miR to be associated with cancer and also classify its association, i.e., either with an oncogene or a tumour suppressor.

PREREQUISITE:
1. LibSVM 3.7
2. Python 2.6 or later
3. RNAHybrid


* Linux Based Installation
To use miRSEQ and miRINT, the user needs the following file:

1. Test dataset – Prepared from the features extracted from the sequence and from
hybridized structures for miRSEQ and miRINT respectively.

2. All input file should be in libsvm format only.

3. General syntax to run the model
    a. svm-predict [option] test_file model model_file output_file


Note:
1. For feature extraction follow the methods described in the article.
2. Convert csv file to libsvm using csvlibsvm.py
3. Output file has information about the prediction made. It may be analysed as follows.
      For miRSEQ: The miRSEQ will directly give the miR sequences associated with cancer.
      For miRINT:
        If 1 – miR associated with oncogenes
        If 2 – miR associated with tumour suppressors
                                                  


                                                    *****Happy Computing*****
