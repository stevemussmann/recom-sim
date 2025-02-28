# `recom-sim`: A hybrid simulation tool

## Notes 02-09-2023
This repository was forked from salanova-elliott/recom-sim. Changes are as follows:
* I fixed some casting of variables that were causing errors. 
* I implemented the bug fix here: https://github.com/salanova-elliott/recom-sim/issues/1
* I modified the assumptions in genepop input format to be more in line with the 'traditional' format. In other words, the program now assumes that sample names and genotype data will be separated by `<space><comma><space><space>` rather than `<comma><space><space>`. Likewise, genepop outputs underwent similar minor alterations.
* The biggest functional change is that an introgression level of 2 now simulates F2 hybrids. Previously this class was only simulated at introgression level 3 despite being shown in the figure below for level 2.

All other program features remain the same.

## Uses

`recom-sim` is a tool based on Nielsen et al.'s `HYBRIDLAB` for simulating hybrids from the genetic data of two reference populations using allele frequencies. `recom-sim` allows for faster simulations, the inclusion of significantly more markers, and the creation of common introgression classes (F2, B2, B3) in one step. While mainly geared towards SNP data, any biallelic data in `GENEPOP` format can be used.

### Example use

```python recom-sim.py input_file.txt 1 --num-offs 1000 --out output_file.txt```

## Input file

Similar to `HYBRIDLAB`, `recom-sim` uses the `GENEPOP` format as input. Any variation of comma or newline separated loci, two or three digit alleles, and tab or space separated data is accepted. Ensure that only two populations are present in the file.

## Options
```
Usage: recom-sim [OPTIONS]

Positional
  input_file          file path to GENEPOP reference
  introgression       choice of {1,2,3}

Optional
  --num-offs INT      number of offspring to simulate (def = 100)
  --p1name   TEXT     name for pop1, used to differentiate backcrosses (def = 'POP1')
  --p2name   TEXT     name for pop2, used to differentiate backcrosses (def = 'POP2')
  --exclude           excludes reference populations from output file
  --out      TEXT     output file name (def = 'out')
  ```

### Introgression level

The positional argument 'introgression' signifies the generational level to simulate.

1. Only **F1** hybrids
2. **F1**, **F2**, **B2POP1**, **B2POP2** (backcrosses to both reference populations: F1 x POP1, F1 x POP2)
3. **F1**, **F2**, **B2POP1**, **B2POP2**, **B3POP1**, **B3POP2** (B2POP1 x POP1, B2POP2 x POP2)

```
        R E F P O P 1   R E F P O P 2
         |    |    \    /    |    |
         |    |     \  /     |    |
Gen1     |    |     F1HYB    |    |
         |    |    / | | \   |    |
         |    |   /  | |  \  |    |
Gen2     |  B2POP1  F2HYB  B2POP2 |
         |   /                \   |
         |  /                  \  |
Gen3    B3POP1                B3POP2
```

Other introgression classes can be simulated by editing the output and feeding it back into the program (e.g. inputing two 'populations' of F2 with an introgression level of 1 to get F3)

## References
Nielsen, E. E., L. A. Bach and P. Kotlicki (2006). "HYBRIDLAB (version 1.0): a program for generating simulated hybrids from population samples." Molecular Ecology Notes. 6(4): 971-973
