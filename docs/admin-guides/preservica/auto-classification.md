# Auto Classification for DC Metadata

ICAEW has a strict taxonomy that is held in Symphony. We use this resource to auto classify the Dublin Core subject metadata. The script provides subject classification to a percentage, the decision is to include only subjects with a classification threshold of 90% plus. The second rule is to include only the top 4 subjects classified by Symphony.

Script:

    java -jar Semaphore-CLSClient-5.6.3.jar --cloud-api-key=q+XmveQ3IDm3UXArcKDQLg== --url=https://cloud.smartlogic.com/svc/138b5bab-8ac4-45e0-b36f-815008f9921d/ input --threshold=90 --csv-output-file output.csv


### Things to note:

* threshold=90 is the 90% classification threshold.
* the output must be a csv.

### Process:

* Enter the output csv, apply auto filter.
* Column C, choose exclusively Generic_UPWARD.
* From there, copy the top 4 90 + subjects into the dublin core csv SIP creation tool described below. 