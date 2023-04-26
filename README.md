# Minimal Reproducible Example - Steps:
```Bash
# ---- Clone the repo
MRE_REPO="https://github.com/MaelLefeuvre/tkgwv2-mre.git"
git glone --recursive $MRE_REPO

# ---- Enter the repo and setup tkgwv2
#WORKDIR="${pwd}/$(basename ${MRE_REPO%.git})" mkdir -p $WORKDIR && cd $WORKDIR
find tkgwv2/scripts/ -exec chmod 755 {} \;

# ---- Download reference Genome
REFERENCE_URL="http://ftp.ensembl.org/pub/grch37/current/fasta/homo_sapiens/dna/Homo_sapiens.GRCh37.dna_sm.primary_assembly.fa.gz"
wget -O- $REFERENCE_URL | gzip -dc > GRCh37.fa


# ---- Setup testing directory
mkdir test && cd test && ln -s ../GRCh37.fa && cp ../dummy-data/* .

# ---- Run tkgwv2
python3 ../tkgwv2/TKGWV2.py bam2plink -r GRCh37.fa -L gwvList.bed -p gwvPlink -e ".sam" plink2tkrelated -f freqFile-numeric.frq -d dyads.txt
```
