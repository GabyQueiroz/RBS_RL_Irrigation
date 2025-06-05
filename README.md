# This code was published in:
# G. de Queiroz Pereira, D. Paulo Bertrand Renaux, and A. E. Lazzaretti,
# "Comparative Analysis of Reinforcement Learning and Rule-Based System Approaches for Irrigation in Horticulture,"
# IEEE Access, vol. 13, pp. 90418–90432, 2025. DOI: 10.1109/ACCESS.2025.3572288

# Project Structure

## **1. File: `RBS.py`**

This file contains the code for the Rule-Based System (RBS), which calculates the required irrigation based on the provided input data.

- **Functionality**: Irrigation decisions are made based on predefined rules.
- **Customization**: Input data must be updated for each season to ensure the system adapts to specific seasonal conditions.

---

## **2. File: `RL.py`**

This file implements the Reinforcement Learning (RL) system to determine the optimal irrigation amount based on input data and learning from the AquaCrop model.

- **Functionality**: 
  - The system learns to make irrigation decisions by interacting with AquaCrop, adjusting its actions to maximize efficiency.

- **Steps**:
  1. **Execution and Learning**: The file must be executed for the model to learn the best irrigation strategy.
  2. **Testing with Seasonal Data**: After training, test files containing data for each season should be used to evaluate and apply the learned model.

- **Data Update**: Ensure to input suitable test files for each season to guarantee proper system operation under different climate conditions.

---

## **3. File: `irrigation_soil.py`**

This file is part of the AquaCrop model and implements specific logic to determine irrigation values based on soil moisture rules.

- **Functionality**: 
  - Adjustments must be made to the AquaCrop `irrigation.py` file in the `elif IrrMngt_IrrMethod == 1` section to include new irrigation decision logic.

- **Required Changes in AquaCrop**:
  - Add the following logic to the `irrigation.py` file:

    ```python
    index = int(NewCond_GrowthStage) - 1  # Determine the phase based on growth stage
    # Adjusted irrigation decision
    threshold = IrrMngt_SMT[index] / 100  # Threshold converted to a fraction

    if Dr > threshold:
        # Calculate the required irrigation amount
        # Dr: Relative soil water deficit
        # Indicates how dry the soil is relative to TAW
        deficit = Dr - threshold  # Relative soil deficit
        IrrReq = deficit * 1.5  # Convert deficit to mm of water (using a 1.5 mm factor)
        
        # Limit to the maximum irrigation depth
        Irr = IrrReq
    else:
        # No irrigation needed
        Irr = 0
    ```

- **Logic Details**:
  - The growth phase is determined by the current stage (`NewCond_GrowthStage`).
  - The irrigation threshold is calculated as a fraction of the configured value (`IrrMngt_SMT`).
  - If the relative soil water deficit (`Dr`) exceeds the threshold, the required irrigation amount is calculated based on the adjusted deficit with a factor of 1.5 mm.
  - Otherwise, no irrigation is applied.

---

## **4. File: `CompareProductivity.py`**

This file executes the comparison between the Rule-Based System (RBS) and Reinforcement Learning (RL) methods, evaluating simulated productivity using the AquaCrop model.

- **Functionality**:
  - Simulates productivity based on the irrigation values provided by the *RBS* and *RL* methods.
  - Inputs the daily irrigation values into the `ITN` matrix for analysis.

- **Steps**:
  1. **Data Input**: Insert the daily irrigation values provided by the *RBS* and *RL* methods into the `ITN` matrix.
  2. **Simulation in AquaCrop**: The file runs Python code to simulate the impact of irrigation strategies on productivity.

- **Requirements**:
  - Ensure the irrigation values are in the correct format and match the daily inputs required by the AquaCrop model.
  - Update the `ITN` matrix with the predicted values before starting the simulation.

- **Output**:
  - Detailed results on the productivity and water efficiency of both methods.
