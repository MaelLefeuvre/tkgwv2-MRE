# Minimal Reproducible Example - Steps:
```Bash
# ---- Clone the repo
MRE_REPO="https://github.com/MaelLefeuvre/tkgwv2-mre.git"
git clone --recursive $MRE_REPO

# ---- Enter the repo and setup tkgwv2
WORKDIR="`pwd`/$(basename ${MRE_REPO%.git})" && mkdir -p $WORKDIR && cd $WORKDIR
find tkgwv2/scripts/ -exec chmod 755 {} \;

# ---- Download reference genome 
REFERENCE_URL="http://ftp.ensembl.org/pub/grch37/current/fasta/homo_sapiens/dna/Homo_sapiens.GRCh37.dna_sm.primary_assembly.fa.gz"
wget -O- $REFERENCE_URL | gzip -dc > GRCh37.fa


# ---- Setup testing directory
mkdir -p test && cd test && ln -s ../GRCh37.fa && cp ../dummy-data/* .

# ---- Run tkgwv2
python3 ../tkgwv2/TKGWV2.py bam2plink -r GRCh37.fa -L gwvList.bed -p gwvPlink -e ".sam" plink2tkrelated -f freqFile-numeric.frq -d dyads.txt
```

# Observed results
```Text
Sample1                Sample2                Used_SNPs  HRC     counts0 counts4 Relationship
dummy-numeric-1        dummy-numeric-2        3          0.0633  1       2       2nd degree
dummy-lexicographic-1  dummy-lexicographic-2  1          NA      0       0       NA
```

# Expected results
```Text
Sample1                Sample2                Used_SNPs  HRC     counts0 counts4 Relationship
dummy-numeric-1        dummy-numeric-2        3          0.0633  1       2       2nd degree
dummy-lexicographic-1  dummy-lexicographic-2  3          0.0633  1       2       2nd degree
