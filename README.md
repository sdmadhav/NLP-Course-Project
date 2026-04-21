# Claim Verification for HINDI

This documents helps to understand how to use the files to reproduces the results again. You can see there are 4 files in this repository. 
1. BERT_NLI_Claim_Verification.ipynb
2. dataset_full.json (Annotated)
3. hindi_claim_verification_v4 (1).ipynb
4. v4.zip


## Dataset Generation
Start with file 3. hindi_claim_verification_v4 (1).ipynb. This File does not requires any input to run. Just connect to GPU enabled device and run all cells. 
We experimented in colab T4 GPU runtime type which is free version to get the results. 

---
## Output Files
```
hindi_cv_v4/
├── knowledge_base.csv   ← entity | key | value | value_type | ner_type | topic
├── evidence_db.csv      ← entity | passage (Family A + IWN key synonym variation)
├── claims_db.csv        ← all TRUE and FALSE claims before final validation
├── dataset_full.csv/json← validated, joined, deduplicated {premise, hypothesis, label} for NLI model
├── nli_train.json       
├── nli_val.json
├── nli_test.json
├── metadata.json        ← full provenance, IWN stats, tool versions
└── quality_dashboard.png
```

Our results are stored in v4.zip file. After that we did human annotation in file dataset_full.json to remove logically ill defined claims which can not be classified as either true or false or no true factual assertions. We found the pattern that when key in the wikipedia infobox 
is numeric type the claim was always meaningless. So we removed all the types of claims which had key as numeric type. The regex used are as follows:
> "key"\s*:\s*"\d+[^"]*"
> 
> "key"\s*:\s*"\d{4}-(present|\d{4})"
> 
> "key"\s*:\s*"\d+"

We used VScode to search and delete the bad claims. VScode have tool to search with regex and then select all the occurances and then delete them. It can be easily done with Python as well. We choose VScode.
After removal of this we randomly sampled the claims among 4 teammates checked the quality. After this we finally got the annotated dataset. Our final annotated dataset is dataset_full.json. 

# Claim Verification
For NLI task we used file 1. BERT_NLI_Claim_Verification.ipynb for which the input is file 2. dataset_full.json (Annotated). Same again runn all the cells in ipynb file
same runtimetype as colab T4 GPU. In 10min you will get the classification  classwise SUPPORTS/REFUTES results and transformationwise. 
