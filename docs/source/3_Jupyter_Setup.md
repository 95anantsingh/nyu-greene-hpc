# Jupyter Notebooks

## Setup

```bash
mkdir /scratch/<NetID>/singularity
cd /scratch/<NetID>/singularity
```

```bash
cp -rp /scratch/work/public/overlay-fs-ext3/overlay-10GB-400K.ext3.gz .
gunzip overlay-10GB-400K.ext3.gz
mv gunzip overlay-10GB-400K.ext3 partition.ext3
```


```bash
singularity exec --overlay /scratch/as14229/singularity/partition.ext3:rw /scratch/work/public/singularity/cuda11.2.2-cudnn8-devel-ubuntu20.04.sif /bin/bash
```

```bash
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
sh Miniconda3-latest-Linux-x86_64.sh -b -p /ext3/miniconda3
```

```bash
rm Miniconda3-latest-Linux-x86_64.sh
```

```bash
touch /ext3/env.sh
echo '#!/bin/bash' >> /ext3/env.sh
echo 'source /ext3/miniconda3/etc/profile.d/conda.sh' >> /ext3/env.sh
echo 'export PATH=/ext3/miniconda3/bin:$PATH'         >> /ext3/env.sh
echo 'export PYTHONPATH=/ext3/miniconda3/bin:$PATH'   >> /ext3/env.sh
```

```bash
source /ext3/env.sh
```

```bash
conda update -n base conda -y
conda clean --all -y
conda install pip -y
conda install ipykernel -y
```

```bash
which conda
# output: /ext3/miniconda3/bin/conda
```

```bash
which python
# output: /ext3/miniconda3/bin/python
```


```bash
find /ext3 | wc -l
# output: should be something like 45445
```

```bash
du -sh  /ext3        
# output should be something like 4.9G    /ext3
```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```


## Installing Kernels