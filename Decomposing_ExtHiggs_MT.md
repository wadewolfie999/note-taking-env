
| Component | Responsibility | Information it depends on |
|---|---|---|
| **1. Model identification** | Establish which imported model is being scanned and which particles supply the required mass features. | The actual `VG_Doublet/work/models/` files and the physical meaning of each particle entry. |
| **2. Scan definition** | Define the parameter space from which sample points are drawn. | Independent parameters, ranges, units, constraints, and sample count. |
| **3. Sampling** | Produce reproducible candidate parameter points. | The scan definition, sampling method, and random seed. |
| **4. Mass-spectrum construction** | Obtain the required masses for each sampled point using the model’s definitions and relations. | The sampled parameters, mass symbols, and expressions in the model files. |
| **5. Validity and coverage checks** | Identify invalid points and determine whether the generated dataset covers the intended region. | Explicit validity rules and a definition of the desired coverage. |
| **6. Dataset formatting** | Arrange the generated quantities into the exact inputs expected by your PySR expression. | Training feature names, order, units, and preprocessing. |
| **7. Classification** | Evaluate the expression and convert its output into predicted excluded/allowed labels. | The exact expression, decision threshold, and label convention. |
