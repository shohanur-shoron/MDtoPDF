![image](https://github.com/user-attachments/assets/00c66b35-1d17-41c1-b51b-792bef53e4d1)

Let's assume the standard sigmoid activation function `f(z) = 1 / (1 + exp(-z))` for all neurons.
For the hidden layer neurons (3, 4, 5), the net input will be `net_j = Σ(x_i * W_ij)`, and the output will be `out_j = f(net_j - θ_j)`.
For the output layer neurons (6, 7), the problem does not specify a threshold, so we'll assume their threshold is 0, meaning `out_k = f(net_k)`.

Given:
Feature vector X = [x₁, x₂] = [1, 0]
Desired output Y = [y₆_desired, y₇_desired] = [0, 1]
Thresholds for hidden layer: θ₃ = θ₄ = θ₅ = 0.3
Weights:
W₁,₃ = 0.2, W₁,₄ = 0.3, W₁,₅ = 0.4
W₂,₃ = 0.5, W₂,₄ = 0.5, W₂,₅ = 0.6
W₃,₆ = 0.3, W₄,₆ = 0.7, W₅,₆ = 0.3
W₃,₇ = 0.4, W₄,₇ = 0.7, W₅,₇ = 0.3

**i) Determine the predicted outputs of the hidden layer (neuron 3, 4 and 5)**

*   **Neuron 3:**
    Net input net₃ = (x₁ * W₁,₃) + (x₂ * W₂,₃) = (1 * 0.2) + (0 * 0.5) = 0.2
    Output out₃ = f(net₃ - θ₃) = f(0.2 - 0.3) = f(-0.1) = 1 / (1 + exp(0.1))
    exp(0.1) ≈ 1.10517
    out₃ = 1 / (1 + 1.10517) = 1 / 2.10517 ≈ 0.47502

*   **Neuron 4:**
    Net input net₄ = (x₁ * W₁,₄) + (x₂ * W₂,₄) = (1 * 0.3) + (0 * 0.5) = 0.3
    Output out₄ = f(net₄ - θ₄) = f(0.3 - 0.3) = f(0) = 1 / (1 + exp(0)) = 1 / (1 + 1) = 1 / 2 = 0.50000

*   **Neuron 5:**
    Net input net₅ = (x₁ * W₁,₅) + (x₂ * W₂,₅) = (1 * 0.4) + (0 * 0.6) = 0.4
    Output out₅ = f(net₅ - θ₅) = f(0.4 - 0.3) = f(0.1) = 1 / (1 + exp(-0.1))
    exp(-0.1) ≈ 0.90484
    out₅ = 1 / (1 + 0.90484) = 1 / 1.90484 ≈ 0.52498

Predicted outputs of the hidden layer:
out₃ ≈ 0.47502
out₄ = 0.50000
out₅ ≈ 0.52498
(Note: The handwritten numbers near neurons 3,4,5 in the image, `0.475`, `0.500`, `0.525`, seem to align with these calculations rounded to 3 decimal places.)

**ii) Determine the predicted outputs of the output layer (neuron 6, 7)**
The inputs to this layer are out₃, out₄, out₅.

*   **Neuron 6:**
    Net input net₆ = (out₃ * W₃,₆) + (out₄ * W₄,₆) + (out₅ * W₅,₆)
    net₆ = (0.47502 * 0.3) + (0.50000 * 0.7) + (0.52498 * 0.3)
    net₆ = 0.142506 + 0.350000 + 0.157494 = 0.65000
    Output out₆ = f(net₆) = f(0.65) = 1 / (1 + exp(-0.65))
    exp(-0.65) ≈ 0.52205
    out₆ = 1 / (1 + 0.52205) = 1 / 1.52205 ≈ 0.65698

*   **Neuron 7:**
    Net input net₇ = (out₃ * W₃,₇) + (out₄ * W₄,₇) + (out₅ * W₅,₇)
    net₇ = (0.47502 * 0.4) + (0.50000 * 0.7) + (0.52498 * 0.3)
    net₇ = 0.190008 + 0.350000 + 0.157494 = 0.697502
    Output out₇ = f(net₇) = f(0.697502) = 1 / (1 + exp(-0.697502))
    exp(-0.697502) ≈ 0.49783
    out₇ = 1 / (1 + 0.49783) = 1 / 1.49783 ≈ 0.66763

Predicted outputs of the output layer:
out₆ ≈ 0.65698
out₇ ≈ 0.66763
(Note: The handwritten numbers `.586` and `.703` on the image for outputs 6 and 7 differ from these calculations. This discrepancy could be due to different rounding, a different activation function, or errors in the handwritten notes. The calculations above follow standard BPNN procedure with sigmoid activation.)

**iii) Estimate the loss (error) at the output layer (neuron 6, 7)**
The desired output vector is Y_desired = [0, 1].
The predicted output vector is Y_predicted = [out₆, out₇] ≈ [0.65698, 0.66763].
We will use the common loss function component for each neuron: L_k = 1/2 * (desired_k - predicted_k)².

*   **Loss for neuron 6:**
    Desired output y₆_desired = 0
    Predicted output out₆ ≈ 0.65698
    Error_6 = y₆_desired - out₆ = 0 - 0.65698 = -0.65698
    Loss_6 = 1/2 * (Error_6)² = 1/2 * (-0.65698)² = 1/2 * 0.4316227 ≈ 0.21581

*   **Loss for neuron 7:**
    Desired output y₇_desired = 1
    Predicted output out₇ ≈ 0.66763
    Error_7 = y₇_desired - out₇ = 1 - 0.66763 = 0.33237
    Loss_7 = 1/2 * (Error_7)² = 1/2 * (0.33237)² = 1/2 * 0.1104696 ≈ 0.05523

Loss at the output layer:
For neuron 6, L₆ ≈ 0.21581
For neuron 7, L₇ ≈ 0.05523

Summary of results (using 5 decimal places for precision):
i) Hidden layer outputs:
   out₃ = 0.47502
   out₄ = 0.50000
   out₅ = 0.52498

ii) Output layer outputs:
    out₆ = 0.65698
    out₇ = 0.66763

iii) Loss (error) at the output layer:
     Loss for neuron 6 (L₆) = 0.21581
     Loss for neuron 7 (L₇) = 0.05523
