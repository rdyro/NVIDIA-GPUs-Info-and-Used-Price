# NVIDIA GPUs Info and Used Price


A simple dataframe (and how to collect the info) combining Nvidia GPU information from
- [TechPowerUp](https://www.techpowerup.com/gpu-specs/)
- [GeekBench](https://browser.geekbench.com/opencl-benchmarks)
- [eBay](https://www.ebay.com/)

---

## Repo overview

- `data/nvidia_gpu_info.csv` contains the final data, also available [here](https://docs.google.com/spreadsheets/d/1FOn0uD-R37ulkxs9LG1-vbjoeUGfK_yJWFziDx5wjxc/edit?usp=sharing)

- `main.ipynb` contains the main notebook used to obtain the data
- `analysis.ipynb` shows how to query the GPU info dataframe with pandas
- `data` contains other partial csv tables

---

## Example query

We're interested in a GPU with at least 12 GB of memory, a compute score of at
least half of RTX 3090, costing less than $1000.00 used.

```python
compute_score = (df.loc[df["Name"] == "RTX 3090"]["Performance Score"].values[0]) / 2.0
mask = (
    (df["Performance Score"] > compute_score)
    & (df["Memory Size (GB)"] > 12)
    & (df["Used Price (eBay US) 2023-03-15"] < 1000)
)
df.loc[mask].sort_values("Used Price (eBay US) 2023-03-15")
```

giving 

|    |   Unnamed: 0 | Name            |   Memory Size (GB) |   Performance Score |   Used Price (eBay US) 2023-03-15 |   Score Per Price | Manufacturer   | GPU Chip   | Released       | Bus          | GPU clock   | Memory clock   | Memory Type   | Memory Bus Width   |   Shaders |   TMUs |   ROPs |
|---:|-------------:|:----------------|-------------------:|--------------------:|----------------------------------:|------------------:|:---------------|:-----------|:---------------|:-------------|:------------|:---------------|:--------------|:-------------------|----------:|-------:|-------:|
| 17 |           17 | RTX A4000       |                 16 |              126692 |                            489.99 |           258.56  | NVIDIA         | GA104      | Apr 12th, 2021 | PCIe 4.0 x16 | 735 MHz     | 1750 MHz       | GDDR6         | 256 bit            |      6144 |    192 |     96 |
| 13 |           13 | TITAN RTX       |                 24 |              138072 |                            740    |           186.584 | NVIDIA         | TU102      | Dec 18th, 2018 | PCIe 3.0 x16 | 1350 MHz    | 1750 MHz       | GDDR6         | 384 bit            |      4608 |    288 |     96 |
|  5 |            5 | RTX 3090        |                 24 |              204921 |                            782.5  |           261.88  | NVIDIA         | GA102      | Sep 1st, 2020  | PCIe 4.0 x16 | 1395 MHz    | 1219 MHz       | GDDR6X        | 384 bit            |     10496 |    328 |    112 |
| 10 |           10 | RTX A4500       |                 20 |              144819 |                            799.99 |           181.026 | NVIDIA         | GA102      | Nov 23rd, 2021 | PCIe 4.0 x16 | 1050 MHz    | 2000 MHz       | GDDR6         | 320 bit            |      7168 |    224 |     96 |
| 22 |           22 | Quadro RTX 5000 |                 16 |              108130 |                            899.99 |           120.146 | NVIDIA         | TU104      | Aug 13th, 2018 | PCIe 3.0 x16 | 1620 MHz    | 1750 MHz       | GDDR6         | 256 bit            |      3072 |    192 |     64 |