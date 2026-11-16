# Problem 7: Polynomial and Big-Integer Multiplication

**Algorithms & Data Structures: Final Examination, Group 2**

This project implements five ways to multiply polynomials (V1 to V5), shows how each one improves on the last, and tests and benchmarks them against each other. Everything lives in one Jupyter notebook: `Group2_Problem7_Visual_Final_Ready.ipynb`.

Coefficient arrays are in **ascending power order**, so `[1, 5, 2, 3]` represents `1 + 5x + 2x² + 3x³`.

## The five versions

| Version | Idea | Time (length n) | Arithmetic | Trade-off |
|---|---|---|---|---|
| **V1: Schoolbook** | Multiply every pair of coefficients | Θ(n²) | Exact integer | Simplest and the reference for correctness, but slow at scale |
| **V2: Four-way divide and conquer** | Split each polynomial in half, make four recursive products | Θ(n²) | Exact integer | Nicer structure, but no asymptotic speedup |
| **V3: Karatsuba** | Rebuild the cross terms from three products instead of four | Θ(n^1.585) | Exact integer | Faster growth, with some recursion overhead |
| **V4: Recursive FFT** | Transform, multiply point by point, transform back | O(n log n) | Floating point | Fast for large n, but rounding error is possible |
| **V5: Iterative FFT** | Same maths as V4, using in-place butterflies and bit reversal | O(n log n) | Floating point | Less recursion overhead, more complex to implement |

Big-O doesn't guarantee the fastest real runtime at every size, so the notebook includes measured benchmarks to compare against the theory.

The notebook also covers **big-integer multiplication**: an integer is turned into a digit array, multiplied with Karatsuba (V3), and the carries are propagated back into a normal number (for example `3251 × 2143 = 6966893`).

## What's in this folder

| File | Purpose |
|---|---|
| `Group2_Problem7_Visual_Final_Ready.ipynb` | The full project: all five algorithms, tests, benchmarks, precision stress test, and visualisations |
| `tara_runtime.csv` | Recorded benchmark results (runtime and memory), used for the saved-result charts |
| `tara_precision_stress.csv` | Recorded results of the FFT precision stress test |

Keep both CSV files in the **same folder as the notebook**, otherwise the chart cells will raise a `FileNotFoundError`.

## How to run it

1. Download or clone this repository and open a terminal (Mac/Linux) or PowerShell (Windows) in the folder with the notebook.
2. Optional, create a virtual environment:
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate        # Windows: .venv\Scripts\activate
   ```
3. Install the packages:
   ```bash
   python -m pip install notebook numpy matplotlib pandas
   ```
4. Start Jupyter and open the notebook:
   ```bash
   python -m notebook
   ```
5. Choose the Python 3 kernel, then **Kernel → Restart & Run All** (or **Run → Run All Cells**, depending on your Jupyter version).

## What you should see

- **Worked example:** all five versions print `[3, 19, 27, 24, 24, 7, 6]` for `[1,5,2,3] × [3,4,1,2]`.
- **Automated tests:** the final line reads `All cross-algorithm correctness comparisons passed` with no `AssertionError`. V2 to V5 are checked against V1 on random inputs of size 8, 64, 512 and 2048.
- **Big-integer carry propagation:** `Result: 6966893`, matching `3251 × 2143`.
- **Benchmark:** runtime and peak memory for sizes 16, 64, 256, 1024 and 2048 across all five algorithms. Timings depend on your machine, so don't expect identical numbers. This cell can be slow, especially for V1 and V2 because of `tracemalloc`.
- **Precision stress test:** compares the FFT versions against exact schoolbook results at n = 4096 with coefficient sizes up to 10⁶, 10⁷ and 10⁸. Non-zero mismatches are possible, and they are a finding about floating-point FFT, not necessarily a bug.
- **Visualisations:** coefficient bar charts, the 4×4 schoolbook product grid, the four-way split diagram, theoretical growth curves, the N=8 bit-reversal plot, recorded runtime and memory charts, and a precision results table.

## Things worth knowing

- **Memory numbers** come from Python's `tracemalloc`, so they measure Python-traced allocations, not total system RAM.
- **FFT accuracy:** V4 and V5 use floating-point arithmetic and round at the end. With large coefficients and large n, the rounding error can exceed 0.5 and give wrong results. That is what the precision stress test shows.
- **Runtime too long?** Reduce `SIZES` or `REPETITIONS` in the benchmark cell, and note the change. The recorded CSV files can be used for the charts instead.
- **Troubleshooting:**
  - `ModuleNotFoundError`: install the missing package into the same Python environment as the notebook kernel.
  - `NameError`: restart the kernel and run the cells in order, since later cells depend on earlier ones.

## Team

| Member | Contribution |
|---|---|
| Devesh | V1 (Schoolbook) and V2 (Four-way divide and conquer) |
| Maryam | V3 (Karatsuba) and big-integer multiplication with carry propagation |
| Fatema | V4 (Recursive FFT) and V5 (Iterative FFT) |
| Tara | Correctness tests, runtime and memory benchmarks, precision stress test |
