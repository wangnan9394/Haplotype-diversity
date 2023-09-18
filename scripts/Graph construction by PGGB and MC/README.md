Wrokflows in Minigraph-Cactus
## Installation follow from:
'''virtualenv --python /usr/local/webserver/python3.6/bin/python3.6 cactus_env
echo "export PATH=$(pwd)/bin:\$PATH" >> cactus_env/bin/activate
echo "export PYTHONPATH=$(pwd)/lib:\$PYTHONPATH" >> cactus_env/bin/activate
source cactus_env/bin/activate
python3 -m pip install -U setuptools pip
python3 -m pip install -U .
python3 -m pip install -U -r ./toil-requirement.txt'''

## Build PPGv1.0 using the command:
/$PTAH/cactus-pangenome js genome.pos.txt --outDir Output --outName PPGv1.0 --reference DM --vcf full clip filter --giraffe full clip filter --gfa full clip filter --gbz full clip filter --workDir $PWD

### Check paths
vg paths -x PPGv1.0.gfa -L

### From GFA to vcf (if need)
vg deconstruct -P ${path_name} -e -a -t 16 PPGv1.0.gfa

### Pangenome growth ratio (base)
panacus histgrowth -t 10 -c bp PPGv1.0.gfa -l 1,1,1,1,1 -q 0,0,0.1,0.5,0.9 -S -s cultivated_samples.txt >BP-8-28-cul.node.txt
panacus histgrowth -t 10 -c bp PPGv1.0.gfa -l 1,1,1,1,1 -q 0,0,0.1,0.5,0.9 -S -s wild_samples.txt >BP-8-28-wild.node.txt
panacus histgrowth -t 10 -c bp PPGv1.0.gfa -l 1,1,1,1,1 -q 0,0,0.1,0.5,0.9 -S -s all_samples.txt >BP-8-28-all.node.txt

### Pangenome growth ratio (node, default)
panacus histgrowth -c bp PPGv1.0.gfa -l 1,1,1,1,1 -q 0,0,0.1,0.5,0.9 -S -s cultivated_samples.txt >BP-8-28-cul.node.txt
panacus histgrowth -c bp PPGv1.0.gfa -l 1,1,1,1,1 -q 0,0,0.1,0.5,0.9 -S -s wild_samples.txt >BP-8-28-wild.node.txt
panacus histgrowth -c bp PPGv1.0.gfa -l 1,1,1,1,1 -q 0,0,0.1,0.5,0.9 -S -s all_samples.txt >BP-8-28-all.node.txt

### Capture sub-graph
gfabase load -o PPGv1.0.gfab PPGv1.0.gfa
gfabase sub PPGv1.0.gfab DM#chr8:2700000-2850000 --range --view --no-sequences --guess-ranges > DM_chr8_2700000_2850000.gfa
