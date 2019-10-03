DATA_DIR = ../data
TMP_DIR = ./tmp

rmarkdown = R --quiet -e "rmarkdown::render('$(1)', knit_root_dir='`pwd`')"

#PredictorsList.txt:
# Description: Tab delimeted file with all the features used for prediction
#	echo "TODO"
#	cp ../data/PredictorsList.txt .

AllSubjectsWithBasicInfo.txt:
# Description: Tab delimeted file with the basic demographics of all TADPOLE subjects
	echo "TODO"
	cp ../data_dump/AllSubjectsWithBasicInfo.txt .


CompleMRIObs.txt:
# CompleMRIObs.txt
# Description: Tab delimeted file with the basic longitudinal information of MRI structural information of D1-D2 datasets
#                The file was generated in Microsoft Excel
#                                - All volumes were transformed by the cubic root
#                                - All surface areas were transformed by the square root
#                                - Derived columns with the thickness coeficient of variation: StandardDeviation/Thickeness
#                                - Derived columns with the strucutral compactness: (square root of Surface Area)/(cubic root of Volume)
#                                - Derived columns containing the mean of left and rigth features
#                                - Derived columns containing the absolute difference between left and rigth features
#                                - Derived columns containing mean thickness and standard deviations of thickness
#                                - Derived columns containing mean surface area and standard deviations of transformed areas
#                                - Derived columns containing mean volumens and standard deviations of transformed volumens
#                                - Derived columns containing the maximum absolute differences between left and right
	echo "TODO CompleMRIObs"
	cp ../data_dump/CompleMRIObs.txt .


D3Clean.txt:
# D3Clean.txt
# Description: Tab delimeted file with the basic longitudinal information of MRI structural information of D3 dataset
#                The file was generated in Microsoft Excel
#                                - All volumes were transformed by the cubic root
#                                - All surface areas were transformed by the square root
#                                - Derived columns with the thickness coeficient of variation: StandardDeviation/Thickeness
#                                - Derived columns with the strucutral compactness: (square root of Surface Area)/(cubic root of Volume)
#                                - Derived columns containing the mean of left and rigth features
#                                - Derived columns containing the absolute difference between left and rigth features
#                                - Derived columns containing mean thickness and standard deviations of thickness
#                                - Derived columns containing mean surface area and standard deviations of transformed areas
#                                - Derived columns containing mean volumens and standard deviations of transformed volumens
#                                - Derived columns containing the maximum absolute differences between left and right
	echo "TODO: D3Clean"
	cp ../data_dump/D3Clean.txt .



# Data conditioning
# Script: TADPOLE_Data_Conditioning.Rmd
# It will read the CompleMRIObs.txt and will adjust all features for AGE and ICV
# it will read the D3Clean.txt and it will impute all missing values
# the adjsted files then are normalized by the rank inverse transform.
DATA_CONDITION_INPUT = CompleMRIObs.txt D3Clean.txt
DATA_CONDITION_OUTPUT = TEST_TADPOLEMRI.RDATA D3Imputed.csv D3Imputed.RDATA TRAIN_TADPOLEMRI.RDATA TEST_TADPOLEMRImputed.csv trainTadploe.norm.RDATA testTadploe.norm.RDATA D3.norm.RDATA
# alias data_condition
data_condition: $(DATA_CONDITION_OUTPUT)
$(DATA_CONDITION_OUTPUT): $(DATA_CONDITION_INPUT)
	$(call rmarkdown,src/TADPOLE_Data_Conditioning.Rmd)


# Script: TADPOLE_MCI_to_AD_v4.Rmd
#                Script: TADPOLE_MCI_to_NL_v1.Rmd
#                Script: TADPOLE_NL_to_MCI_v4.Rmd
#                Script: TADPOLE_NL_to_AD_v4.Rmd
# *csv files with conversion prediction
CONVERSION_PREDICTION_INPUT = PredictorsList.txt D3Imputed.RDATA TEST_TADPOLEMRI.RDATA TRAIN_TADPOLEMRI.RDATA trainTadploe.norm.RDATA testTadploe.norm.RDATA D3.norm.RDATA
conversion_prediction: $(CONVERSION_PREDICTION_INPUT)
	$(call rmarkdown,src/TADPOLE_MCI_to_AD_v4.Rmd)
	$(call rmarkdown,src/TADPOLE_MCI_to_NL_v1.Rmd)
	$(call rmarkdown,src/TADPOLE_NL_to_MCI_v4.Rmd)
	$(call rmarkdown,src/TADPOLE_NL_to_AD_v4.Rmd)

probability_conversion: PredictorsList.txt D3Imputed.RDATA TEST_TADPOLEMRI.RDATA TRAIN_TADPOLEMRI.RDATA trainTadploe.norm.RDATA testTadploe.norm.RDATA D3.norm.RDATA
#  Predicting Time to Event conversion:
#                 Script: TADPOLE_Time_to_AD_V2.Rmd
#                Script: TADPOLE_Time_to_MCI_V2.Rmd
	$(call rmarkdown,src/TADPOLE_Time_to_AD_V2.Rmd)
	$(call rmarkdown,src/TADPOLE_Time_to_MCI_V2.Rmd)

ensembling:
# 4) Ensembling the predictions:
# Excel file loaded all the prediction of conversion and the estimated time to conversion
# There the predicted conversion was the multiplication of the predicted conversion probabiliyt by the estimated time of conversion
	echo "TODO"

adas_csvs: PredictorsList.txt D3Imputed.RDATA TEST_TADPOLEMRI.RDATA TRAIN_TADPOLEMRI.RDATA trainTadploe.norm.RDATA testTadploe.norm.RDATA D3.norm.RDATA
#  Predicting ADAS conversion:
#                Script: TADPOLE_Time_ADAS13_.Rmd
#                Script: TADPOLE_Time_ADAS13__MCI.Rmd
#                Script: TADPOLE_Time_ADAS13__NL.Rmd
	echo "TODO"

ventricles_csvs: PredictorsList.txt D3Imputed.RDATA TEST_TADPOLEMRI.RDATA TRAIN_TADPOLEMRI.RDATA trainTadploe.norm.RDATA testTadploe.norm.RDATA D3.norm.RDATA
# Predicting Ventricle volume:
#                 Script: TADPOLE_Time_Ventricle_ICV.Rmd
#                Script: TADPOLE_Time_Ventricle_ICV_MCI.Rmd
#                Script: TADPOLE_Time_Ventricle_ICV_NL.Rmd


clean:
	rm -f CompleMRIObs.txt \
		D3Clean.txt \
		TEST_TADPOLEMRI.RDATA\
		D3Imputed.csv\
		D3Imputed.RDATA\
		TRAIN_TADPOLEMRI.RDATA\
		TEST_TADPOLEMRImputed.csv\
		trainTadploe.norm.RDATA\
		testTadploe.norm.RDATA\
		D3.norm.RDATA