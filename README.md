# Tutorial for running PoPS on Esrum
PoPS is a gene prioritization method that can be applied to GWAS summary statistics. You can find the official Github [here](https://github.com/FinucaneLab/pops), and the publication [here](https://doi.org/10.1038/s41588-023-01443-6).

PoPS has been made availble as an environment module in the `loos_group-AUDIT` folder on Esrum. This tutorial describes how you can load PoPS and associated tools, and run the program from start to finish.

> Please note that you need to be added to the `loos_group-AUDIT` folder on [Esrum](https://cbmr-data.github.io/esrum/) to be able to access and run this tool as demonstrated here. If you have not already been added to this folder, then [contact me](#contact-for-help) for help. 

## Loading modules


```
module purge
module use --prepend /projects/loos_group-AUDIT/apps/modules/modulefiles/
module load PoPS/v0.2
module load MAGMA/v1.10 
```

# Contact for help
Siddhi Jain
siddhi.jain@sund.ku.dk