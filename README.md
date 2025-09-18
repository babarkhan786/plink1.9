How to start plink (Windows)
1.	https://www.cog-genomics.org/plink2/ 
 
2.	Download plink according to your MacOS or Windows
3.	Extract downloaded file and save extracted contents into a folder plink1.9
4.	Type cmd (command prompt) in search and open it
5.	Change directory to desktop and plink1.9 folder
 
6.	Type plink, different options will display
 
7.	Place .ped and .map file into plink1.9 folder
8.	Type dir in cmd, it will show all directories present in that folder
 
9.	plink --file 886geno --make-bed --out dataset
 
10.	your plink is working perfectly, play around

Summary statistics: allele frequencies
Next we perform a similar analysis, except requesting allele frequencies instead of genotyping rates. The following command generates a file called freq_stat.frq which contains the minor allele frequency and allele codes for each SNP.
plink --bfile dataset --freq --out freq_stat
 
 

Association (GWAS)
plink --bfile dataset --linear --pheno 886pheno.txt --pheno-name PXO61 --allow-no-sex --out result_PXO61

✅ Step 1. Install packages in R
Run this once:
install.packages("qqman")
________________________________________
✅ Step 2. Load your PLINK results
Save this as gwas_plot.R and run in R:
# Load qqman
library(qqman)

# Read PLINK output
gwas <- read.table("result_PXO61.assoc.linear", header=TRUE)

# Keep only additive model (ignore DOMDEV/GENO tests)
gwas_add <- subset(gwas, TEST == "ADD")

# Manhattan plot
manhattan(gwas_add, chr="CHR", bp="BP", snp="SNP", p="P",
          genomewideline = -log10(0.05 / nrow(gwas_add)), # Bonferroni threshold
          suggestiveline = -log10(1e-5), 
          main="GWAS Manhattan Plot for PXO61")

# QQ plot
qq(gwas_add$P, main="QQ Plot for PXO61")
________________________________________
Results can be saved from R session as export pdf
plink --bfile dataset --linear --pheno 886pheno.txt --pheno-name PXO61 \ --covar pcs.txt --covar-name PC1,PC2,PC3 --allow-no-sex --out result_PXO61-new

generate PCA
plink --bfile dataset --pca 10 --out dataset_pca
rename dataset_pca.eigenvec pcs.txt
add column names
plink --bfile dataset --linear --pheno 886pheno.txt --pheno-name PXO61 --covar pcs.txt --covar-name PC1,PC2,PC3 --allow-no-sex --out result_PXO61-new

