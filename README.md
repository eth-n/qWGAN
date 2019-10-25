# qWGAN
The repository contains codes for quantum Wasserstein GAN framework and its applications proposed in the following reference: 

_Shouvanik Chakrabarti, Yiming Huang, Tongyang Li, Soheil Feizi, and Xiaodi Wu, Quantum Wasserstein Generative Adversarial Networks, NeurIPS 2019._ 

###Content
We listed 4 applications which using quantum Wasserstein GAN framework as follows

1. [Learning pure state](./pure_state/README.MD)
    Learn the pure state ![](http://latex.codecogs.com/gif.latex?\\{\rho_p}) generated by applying parameterized Pauli gates on each qubit. 
2. [Learning mixed state](./mixed_state/README.MD)
    Learn the mixed state ![](http://latex.codecogs.com/gif.latex?\\{{\rho_m}=\sum\limits_i{{P_i}}\left|{{\varphi_i}}\right\rangle\left\langle{{\varphi_i}}\right|}), in which each pure state ![](http://latex.codecogs.com/gif.latex?\\{\left|{{\varphi_i}}\right\rangle}) is generated by the same method mentioned in 1. ![](http://latex.codecogs.com/gif.latex?\\{P_i}) is a random number between 0 and 1, and ![](http://latex.codecogs.com/gif.latex?\\{\sum\limits_i{{p_i}}=1}). 
3. [Learning pure state with noise](./noise_qwgan/README.MD)
    Add gaussian noise to the gradient of each parameters. and the generator circuit is constructed by the same ansatz used in training hybrid algorithm on iron-trap quantum computer('[Training of Quantum Circuits on a Hybrid Quantum Computer](https://arxiv.org/abs/1812.08862v1)').
4. [Hamiltonian Simulation](./hamiltonian_simulation/README.MD)
    Learn the pure state generated by applying the unitary operations which restricted by the Hamiltonian dynamics on maximually entangled state. Here we focus on the Hamiltonian of one-dimensional nearest-neighbor Heisenberg model with a random magnetic field in the z direction.

### Getting Started

#### Constructing quantum circuit
1. Create an instance of Quantum Circuit
    Such as we construct a quantum circuit as the generator:
    ```python
    from base_notation import *
    qcircuit = Quantum_Circuit(system_size, "generator") # create an instance of Quantum Circuit
    ```
2. Adding quantum gates to quantum circuit, such as:
	```python
    qcircuit.add(Quantum_Gate("X", i, angle=0.5000 * np.pi)) # add parameterized X Rotation gate with 0.5*pi on ith wire
    ```

#### Create an instance of Generator
1. Create an instance of Generator
    Such as creating the Generator of 2 qubits system
    ```python
    system_size = 2
    gen = Generator(system_size) # create an instance of generator of 2 qubits system
    ```
2. Create the quantum circuit and set it as the variational quantum circuit of the generator.
    ```python
    qc = construct_qcircuit(qc,size) # create an instance of system
    gen.set_qcircuit(qc) # set qc as the circuit of generator
    ```

#### Create an instance of Discriminator
1. Define the Pauli matrices set.
    As the discriminator is represented as linear combination of tensor products of simple Pauli matrices, we first define the Pauli matrices set
    ```python
    herm = [I, X, Y, Z]
    ```
2. Create an instance of Discriminator
    ```python
    dis = Discriminator(herm, system size)
    ```

#### Update the parameters of generator and discriminator
Update the parameters of generator and discriminator alternativly until the fidelity between the generated states and target state approximate to 1.
```python
dis.update_dis(gen,real_state) #update the parameters of the discriminator
gen.update_gen(dis,real_state) #update the parameters of the generator
```

### Dependencies
All the codes have dependencies on following python library:
1. numpy 1.14.5
2. scipy 1.1.0
3. matplotlib 3.0.2

### License
Distributed under the MIT License. See LICENSE for more information.




   