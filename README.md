from qiskit import QuantumCircuit, Aer, execute
from qiskit.visualization import plot_histogram

def create_bit_flip_code():
    qc = QuantumCircuit(3, 1)  # 3 qubits, 1 classical bit
    
    # Encoding: Repetition Code
    qc.h(0)  # Create superposition
    qc.cx(0, 1)  # Copy state to second qubit
    qc.cx(0, 2)  # Copy state to third qubit
    
    # Introduce an error (X gate on one qubit to simulate bit-flip error)
    qc.x(1)
    
    # Error Correction (Majority Vote)
    qc.cx(1, 0)
    qc.cx(2, 0)
    qc.ccx(0, 1, 2)  # Controlled-controlled-X
    
    # Measurement
    qc.measure(2, 0)
    
    return qc

# Create the quantum circuit for error correction
qc = create_bit_flip_code()

# Execute on the simulator
simulator = Aer.get_backend('qasm_simulator')
result = execute(qc, simulator, shots=1024).result()
counts = result.get_counts()

# Output results
print("Measurement Results:", counts)
qc.draw('mpl')
