---
{"dg-publish":true,"permalink":"/understanding-normalize-data-function/","noteIcon":"","updated":"2024-08-13T03:24:50.022+05:30"}
---

In this short article, using a tiny, 5x5 matrix example dataset, I will explain the `NormalizeData()` function and the three `Normalization.method()` choices.

---

The cell expression data is often contained in the form of a $\scriptsize \textbf {Genes x Sample}$ matrix.

- **Rows:** Genes
- **Columns:** Cells/Samples/Replicates
- **Entries:** Number of RNA transcripts detected

## **$\color{orange} \textrm {Example dataset:}$**

The expression data is contained

||Cell1|Cell2|Cell3|Cell4|Cell5|
|---|---|---|---|---|---|
|GeneA|2|10|9|8|9|
|GeneB|2|4|10|8|6|
|GeneC|9|3|4|8|9|
|GeneD|1|5|2|2|8|
|GeneE|5|8|10|7|2|
|Total|19|30|35|33|34|

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/fa75db2a-077a-4de3-a95e-6d39b098184c/9c0ca7b6-4d16-4056-8a61-d5a69f017531/4c8d15a7-d4d9-4afa-8e18-f26fca7eb7ec.png)

As columns in count matrix represent UMI counts for each cells, the sums of UMI counts in a column represent total cellular expression.

---

As mentioned above, there are three `Normalization.method()` choices under `NormalizeData()` function in Seurat.

## $\color {orange} \textrm {Method 1: LogNormalize}$

### **$\color{violet} \textbf {Step1: Scaling}$**

By default, the `NormalizeData()` function of Seurat pipeline uses 10,000 as a `scale.factor`. The UMI counts for each cell is multiplied by the `scale.factor` and divided by the total UMI counts for the cell.

||Cell1|Cell2|Cell3|Cell4|Cell5|
|---|---|---|---|---|---|
|GeneA|(2 * 10000) / 19|(10 * 10000) / 30|(9 * 10000) / 35|(8 * 10000) / 33|(9 * 10000) / 34|
|GeneB|(2 * 10000) / 19|(4 * 10000) / 30|(10 * 10000) / 35|(8 * 10000) / 33|(6 * 10000) / 34|
|GeneC|(9 * 10000) / 19|(3 * 10000) / 30|(4 * 10000) / 35|(8 * 10000) / 33|(9 * 10000) / 34|
|GeneD|(1 * 10000) / 19|(5 * 10000) / 30|(2 * 10000) / 35|(2 * 10000) / 33|(8 * 10000) / 34|
|GeneE|(5 * 10000) / 19|(8 * 10000) / 30|(10 * 10000) / 35|(7 * 10000) / 33|(2 * 10000) / 34|

||Cell1|Cell2|Cell3|Cell4|Cell5|
|---|---|---|---|---|---|
|GeneA|1052.63|3333.33|2571.42|2424.24|2647.05|
|GeneB|1052.63|1333.33|2857.14|2424.24|1764.70|
|GeneC|4736.84|1000|1142.85|2424.24|2647.05|
|GeneD|526.31|1666.66|571.42|606.06|2352.94|
|GeneE|2631.57|2666.67|2857.14|2121.21|588.23|

### **$\color{violet} \textbf {Step2: Log-Normalization}$**

In this step, the scaled UMI counts are natural log transformed using log1p, i.e. ln(x + 1).

||Cell1|Cell2|Cell3|Cell4|Cell5|
|---|---|---|---|---|---|
|GeneA|ln(1052.63 + 1)|ln(3333.33 + 1)|ln(2571.42 + 1)|ln(2424.24 + 1)|ln(2647.05 + 1)|
|GeneB|ln(1052.63 + 1|ln(1333.33 + 1)|ln(2857.14 + 1)|ln(2424.24 + 1)|ln(1764.70 + 1)|
|GeneC|ln(4736.84 + 1)|ln(1000 + 1)|ln(1142.85 + 1)|ln(2424.24 + 1)|ln(2647.05 + 1)|
|GeneD|ln(526.31 + 1)|ln(1666.66 + 1)|ln(571.42 + 1)|ln(606.06 + 1)|ln(2352.94 + 1)|
|GeneE|ln(2631.57 + 1)|ln(2666.67 + 1)|ln(2857.14 + 1)|ln(2121.21 + 1)|ln(588.23 + 1)|

> **Why +1?**
> 
> If any empty cell is detected, i.e. zero UMI count, the log-normalization, ln(0), will result in a ‘mathematical error’. In contrast, ln(0 + 1) will give a value of 0.
> 
> In short,
> 
> - ln(0) = Numerical error
> - ln(0 + 1) = 0

## **$\color{violet} \textrm{Result}$**

||Cell1|Cell2|Cell3|Cell4|Cell5|
|---|---|---|---|---|---|
|GeneA|6.959998122|8.112028038|7.852605701|7.793686767|7.881582131|
|GeneB|6.959998122|7.19618707|7.957927342|7.793686767|7.476305823|
|GeneC|8.463337059|6.908754779|7.042161289|7.793686767|7.881582131|
|GeneD|6.26779959|7.419180723|6.349887962|6.408628631|7.763846299|
|GeneE|7.875719233|7.888959462|7.957927342|7.660214277|6.378825585|
|Total|36.52685213|37.52511007|37.16050964|37.44990321|37.38214197|

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/fa75db2a-077a-4de3-a95e-6d39b098184c/425b4de6-7118-42fe-9bd2-a3f2c37e2335/cbb40b04-e171-4391-90bf-4ad7a1b822bb.png)

---

## $\color{orange} \textrm {Method 2: Centered Log Ratio (CLR)}$

(This section is under development)

---

## $\color{orange} \textrm {Method 3: Relative counts}$

In contrast to $\color{orange} \textrm {Method 1:}$ Log-Normalize, the relative count normalization is a one-step method, only involves the scaling.

### **$\color{violet} \textbf {Scaling}$**

By default, the `NormalizeData()` function of Seurat pipeline uses 10,000 as a `scale.factor`. The UMI counts for each cell is multiplied by the `scale.factor` and divided by the total UMI counts for the cell.

||Cell1|Cell2|Cell3|Cell4|Cell5|
|---|---|---|---|---|---|
|GeneA|(2 * 10000) / 19|(10 * 10000) / 30|(9 * 10000) / 35|(8 * 10000) / 33|(9 * 10000) / 34|
|GeneB|(2 * 10000) / 19|(4 * 10000) / 30|(10 * 10000) / 35|(8 * 10000) / 33|(6 * 10000) / 34|
|GeneC|(9 * 10000) / 19|(3 * 10000) / 30|(4 * 10000) / 35|(8 * 10000) / 33|(9 * 10000) / 34|
|GeneD|(1 * 10000) / 19|(5 * 10000) / 30|(2 * 10000) / 35|(2 * 10000) / 33|(8 * 10000) / 34|
|GeneE|(5 * 10000) / 19|(8 * 10000) / 30|(10 * 10000) / 35|(7 * 10000) / 33|(2 * 10000) / 34|

## $\color{violet} \textrm {Result:}$

||Cell1|Cell2|Cell3|Cell4|Cell5|
|---|---|---|---|---|---|
|GeneA|1052.63|3333.33|2571.42|2424.24|2647.05|
|GeneB|1052.63|1333.33|2857.14|2424.24|1764.70|
|GeneC|4736.84|1000|1142.85|2424.24|2647.05|
|GeneD|526.31|1666.66|571.42|606.06|2352.94|
|GeneE|2631.57|2666.67|2857.14|2121.21|588.23|
|Total|10000|10000|10000|10000|10000|

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/fa75db2a-077a-4de3-a95e-6d39b098184c/69e25ab1-ff66-4f2c-91f3-ba8e65f486eb/022ca8f5-2968-4d93-9d66-19e65d710540.png)

---