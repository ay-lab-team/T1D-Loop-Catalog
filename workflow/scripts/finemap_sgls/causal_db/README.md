## To Run:
The working directory for this readme is considered: `workflow/qscripts/finemap/causal_db`

In some cases you have to run scripts from the project directory: `/mnt/BioHome/jreyna/jreyna-temp/projects/t1d-loop-catalog`

### Setup the data 
The Loop Catalog has attempted to classify the organ of samples. All "Immune-associated" will be used. To extract these samples simple
go to: (https://loopcatalog.lji.org/loops/?genome=hg38)[https://loopcatalog.lji.org/loops/?genome=hg38]. In the search bar look for "Immune-associated" and click CSV to download a samplesheet. From there run the following script to make a samplesheet:

```
run this
```

Symlink the LJI-LCSD for Access to Loops
```
ln -s /mnt/BioAdHoc/Groups/vd-ay/hichip-db-loop-calling/results/lji_lcsd_hub/release-0.1/hub/hg38/loops results/hg38/
```


### Process the CAUSALdb fine-mapping studies

1) Convert the Fine-mapped SNPs to hg38 with:
    ```
        lc.finemap_snps.hg38_conversion.ipynb (Jupyter)
    ```

2) Summarize the fine-mapping results with:
    ```
        lc.finemap_snps.summary.ipynb (Jupyter)
    ```

### Intersect and determine the SGLs
1) Download the Fine-mapped SNPs metadata and create the necessary samplesheet with:
    ```
        bash workflow/qscripts/finemap/causal_db/lc.sgls_finemap_with_hichip.create.samplesheet.sh
    ```

    The samplesheet will be stored over at: `./lc.sgls_finemap_with_hichip.samplesheet.txt`

2) Intersect loops, SNPs and genes using SGE jobs with:
    ```
        squeue workflow/qscripts/finemap/causal_db/lc.sgls_finemap_with_hichip.run.sh # SQUEUE
    ```
    This script will call `./lc.sgls_finemap_with_hichip.run.sh` for each line in 
    the samplesheet `./lc.sgls_finemap_with_hichip.samplesheet`

3) Summarize the fine-mapping SGL results with:
    ```
        lc.sgls_finemap_with_hichip.summary.ipynb (Jupyter)
    ```
