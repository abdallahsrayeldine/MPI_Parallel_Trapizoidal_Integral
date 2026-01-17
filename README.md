# MPI Parallel Trapezoidal Integral 📈

**Compute definite integrals faster** by dividing the workload across MPI processes using the **trapezoidal rule**. This project parallelizes numerical integration using the **Message Passing Interface (MPI)** in C.

The trapezoidal rule approximates:
[
\int_a^b f(x),dx \approx h \left[\frac{f(a)+f(b)}{2} + \sum_{k=1}^{n-1} f(a + k h)\right]
]
where (h = (b-a)/n). Parallelism divides this interval across multiple MPI ranks, computes partial sums locally, and reduces them into the final result. ([BrainKart][1])

---

## 🚀 Features

* Parallel integral approximation using MPI.
* Static partitioning of the interval across processes.
* Each rank computes partial sums and participates in a **reduction** to aggregate results.
* Minimal dependencies — just MPI and standard C.

---

## 📌 How It Works

1. **Input:** Bounds `a`, `b` and number of trapezoids `n` passed to the executable (via arguments or constants).
2. **MPI Initialization:** All processes call `MPI_Init()`.
3. **Work Partitioning:**

   * The interval ([a,b]) is split among ranks.
   * Each process computes its local sum over its sub-interval.
4. **Communication & Reduction:**

   * Use `MPI_Reduce()` to sum all partial integrals at rank 0.
5. **Final Output:**

   * Rank 0 prints the estimated integral.

MPI enables simultaneous computation on multiple processes and then aggregates results efficiently. ([cseweb.ucsd.edu][2])

---

## ⚙️ Build & Run

### Requirements

* C Compiler (e.g., `gcc` with MPI support)
* MPI implementation (OpenMPI, MPICH, etc.)

---

### Compile

```bash
mpicc trapIntegral.c -o trapIntegral
```

---

### Run

```bash
mpirun -np 4 ./trapIntegral <lower> <upper> <num_traps>
```

Example:

```bash
mpirun -np 4 ./trapIntegral 0 3 1000000
```

This runs the integral approximation from `0` to `3` using 1,000,000 trapezoids across 4 processes.

---

## 🧪 Expected Behavior

* Each process computes a portion of the integral.
* Results are **aggregated via MPI reduction**.
* With more processes, work is split finer, leading to **faster execution**, especially for large `n`. ([BrainKart][1])

---

## 📝 Notes & Tips

* **MPI must be installed** and in your PATH (`mpicc`, `mpirun`).
* Running with `np=1` gives the serial baseline; increasing `np` shows speed-up.
* This implementation assumes balanced work and simple function definitions.

---

## 🛠️ Potential Enhancements

* Support dynamic load balancing for `n` not divisible by `np`.
* Allow function input from user or file.
* Add timing and performance output for benchmarking.
* Support more MPI collectives (`MPI_Allreduce`, scatter/gather).

---

## 📖 References

* Trapezoidal numerical integration and simple MPI distribution pattern. ([learning.rc.virginia.edu][3])

---

If you want, I can also draft a **Makefile + usage example file** or convert this into a **badge-ready GitHub README** with build/test sections.

[1]: https://www.brainkart.com/article/The-Trapezoidal-Rule-in-MPI_9332/ "The Trapezoidal Rule in MPI"
[2]: https://cseweb.ucsd.edu/classes/wi13/cse160-a/Lectures/Lec19.pdf "CSE 160 
Lecture 19 
Programming with MPI"
[3]: https://learning.rc.virginia.edu/courses/parallel-computing-introduction/distributed_mpi_project_set1 "MPI Project Set 1 | RC Learning Portal"
