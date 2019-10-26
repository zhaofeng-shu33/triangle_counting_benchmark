[![Build Status](https://api.travis-ci.com/zhaofeng-shu33/triangle_counting_benchmark.svg?branch=master)](https://travis-ci.com/zhaofeng-shu33/triangle_counting_benchmark/)
[![Gitlab](https://gitlab.com/zhaofeng-shu33/triangle_counting_self-hosted_runner/badges/master/pipelines.svg)](https://gitlab.com/zhaofeng-shu33/triangle_counting_self-hosted_runner)

## Baseline

| dataset               | N(V)       | N(E)        | N(T)           |
| --------------------- | ---------- | ----------- | -------------- |
| soc-LiveJournal1.bin  | 4,847,571  | 68,993,773  | 285,730,264    |
| s24.kron.edgelist.bin | 16,777,216 | 268,435,456 | 10,286,638,314 |

The homepage of LiveJournal dataset is hosted at [stanford](https://snap.stanford.edu/data/soc-LiveJournal1.html), both datasets are temporarily accessible at [LiveJournal1](http://datafountain.int-yt.com/BDCI2019/FeiMa/soc-LiveJournal1.bin) and [s24-kron](http://datafountain.int-yt.com/BDCI2019/FeiMa/s24.kron.edgelist.bin). These temporary data download links may be invalid. Notice that the record may have self looped edges and some edge occurs twice in the form (u, v) and (v, u) but some occurs only once. These details need some preprocessing task. Also the label of the node may not be continuous from 0 to |V| -1. Some mapping conversion is necessary.

These results are obtained on a Ubuntu server with 32 CPU cores of Intel(R) Xeon(R) CPU E5-2620 v4 @ 2.10GHz.

| dataset | method                     | times(s) |
| ------- | -------------------------- | -------- |
| journal | single threaded node_first | 63       |
| journal | single threaded edge_first | 610      |
| journal | 32 threads node_first      | 15       |
| journal | 32 threads edge_first      | 102      |
| kron    | single threaded node_first | 16567    |

## Consideration

Travis provides relative high-grade configured virtual machine compared to other CI service. It can be used to run some machine learning or graph computing experiment using CPU. As mentioned in the [documentation](https://docs.travis-ci.com/user/reference/overview/#virtualisation-environment-vs-operating-system), the virtual machine has 2 CPU cores, about 7.5GB memory and 16GB disk space. For medium-sized scientific computing task, it is enough.  

Therefore, we run the graph computing benchmark experiment on Travis using `soc-LiveJournal1.bin` dataset. It should be noticed that the dataset itself is comparatively large. After compression it is about 192MB. As we know, there is no free hosting of dataset with both large storage limit and bandwidth download speed. For GitHub LFS (Large File Storage), the data storage limitation is 1GB.  Therefore, researchers can not host their own dataset without fee. For public dataset, researchers can use Travis to download the dataset but the dataset cannot be too large. Otherwise the download speed will be a bottleneck and the caching size limit will not be enough. We conclude that Travis is suitable for medium-sized experiment which uses CPU only. 
