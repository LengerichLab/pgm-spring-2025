---

lecturers:2025-02-05 Lecture
  - name: Ben Lengerich
    url: "https://lengerichlab.github.io/"

authors:

  - name: Justina Jing
    url: "https://github.com/JustinaJing/JustinaJing.github.io.git"
  - name: Lois Liu
    url: ""





## Logistics Review
**No class on 2/11**

**HW2 deadline pushed to 2/11 11:59pm**

**Quiz in-class on 2/13**

**3 HW problems, 2 new problems. All of the questions are multiple choice. Time: Whole lecture.**
  
## Undirected Graphic Models
**Definition**: UGMs provide a visual representation of the structure of joint probability distribution from which conditional independence relationships can be inferred.
- Unlike a directed graphic mode, UGMs **do not imply a causal direction** between the variables. UGMs give correlations between variables. (Figure 1)
-  key features of UGMs:
    - Pairwise relationship 
    - no explicit way to generate samples
    - Contingency constraints on node configurations. 
![Figure 1: Undirected Graphic Model](image.png)
figure 1
- UGM Example: Lattice 
    - An UGM lattice used for spatial inference, each pixel in the image is represented as a node, each node is connected to its neighboring pixels (Figure 2)
        - To infer the yellow pixel, the model classifies the yellow selected pixel based on its surrounding pixels instead of its absolute location. 
Since the most of the adjacent pixels belong to "water", even it's on the top of right, lattice UGM classifies it as **water**
![Figure 2: Lattice example](image-1.png)
figure 2
  

## Representing Undirected Graphic Model
- An undirected graphical model represents a distribution $P(x)$ defined by an undirected graph $H$ and a set of positive potential functions $\psi$ associated with the cliques of $H$ such that  $P(X_1,...,X_n) = \frac{1}{Z} \prod_c \psi_c (X_c)$ 

    - $Z$ is the **partition function** (normalization):  $Z = \sum_X \prod_c \psi_c (X_c) $
- The potential function $\psi_c(X_c)$ can be understood as a "score" of the joint configuration. 
- Unlike probability densities, which must integrate (or sum) to 1, the potential function only provides a score of how favorable a particular configuration is compared to others. 
- The ${P(X)}$ is a proper probability density because the partition function is finite, which means that ${P(X)}$ is properly normalized so that sum of all probabilities equals 1. Also, all potential functions are positive. 


## Clique
**Definition**: A clique is a fully connected subset of nodes in an undirected graph model.
for $G = \{V,E\}$, a clique is a subgraph such that the nodes in $V'$ are fully connected.
![figure 3: Clique](image-2.png) Figure 3\
Maximal Clique is a clique that cannot be extended by adding more nodes without breaking the full connectivity condition.
  - $\{x_1,x_4\}$ and $\{x_1,x_2,x_3\}$ are **maximal clique**.
  - $\{x_1,x_2,x_3\}$ clique are fully connected since each node has edges to the other two. Adding $x_4$ would break the clique condition because $x_4$ is not connected to both $x_1$ and $x_2$.
  - $\{x_1,x_2\}$, $\{x_2,x_3\}$, $\{x_1,x_3\}$, $\{x_1\}$,$\{x_2\}$, $\{x_3\}$$\{x_4\}$ are **sub-cliques** because their nodes are directly connected.

\
**Interpretation of Clique Potentials**
![figure 4](image-3.png)<figcaption> Figure 4 
- The figure 4 model implies $ x_1 \perp x_2 \mid x_3$, so joint must factorize accordingly as: 
 $P(X_1,X_2,X_3) = P(X_2) P(X1\mid X2) P(X_3\mid X_2)$

 - In UGMs, we do not use probabilities, we define the distribution in terms of clique potentials $\psi$, which measure\textbf{ "compatibility" }rather than probability. Therefore, we could write the above equation as  
 $P(X_1, X_2, X_3) = P(X_1, X_2) P(X_3 \mid X_2)$ or $P(X_3, X_2) P(X_1 \mid X_2)$
    - **Cannot have all potential be marginals**
    - **Cannot have all potential be conditions**
  
 ![alt text](image-4.png)
 figure 5
  - Maximal Cliques: Based on Figure 5, the probability distribution of the entire system is written as 
  $P(A.B,C,D) = \frac{1}{Z}\psi_{ABC}(A, B, C) \psi_{BCD}(B, C, D)$
    - To make $P(A,B,C,D)$ a valid probability distribution, we normalize it by using the partition function $Z$.
   Therefore,  $Z = \sum_{ABCD}\psi_{ABC}(A, B, C) \psi_{BCD} (B, C, D) $ This ensures the sum of all possible configurations equals 1.


## Markov Independencies
**Global Markov Independencies**
!![figure 6](image-6.png)
Figure 6
- The undirected graph $H$ in Graph 6 demonstrates that the nodes in $B$ separate $A$ and $C$, meaning that all paths from any node in $A$ to any node in $C$ must pass through $B$. This is written as $(sep_H(A;C\mid B))$ 
- If you know the values of $B$, then $A$ provides no additional information about $C$ because all paths are blacked by $B$.
- Global Markov Property: A probability distribution satisfies it if for any disjoint$\ A,B,C $\ such that $B$ separates $A$ and $C$, $A$ is independent of $C$ given $B$. This is written as
$I(H) = \{A\perp C \mid B:sep_H(A;C \mid B)\}$ 

**Local Markov Independencies**
![graph 7](image-7.png)
Figure 7
- In the Graph 7, we make blue node be $X_i$. We say that there is a unique Markov blanket of $X_i$, denoted $M B_{x_i}$, which is the set of neighbors of $X_i$ in the graph 6 (red nodes)
- The local Markov independencies in $H$ are:  $I_l(H) =\{X_i\perp V-{X_i}-M B_{x_i}\mid MB_{X_i}:\forall_i\}$
- In other words, $X_i$ is independent of the rest of the nodes in the graph given its immediate neighbors, and this is satisfied for all nodes $i$ in the graph. Once you know the Markov blanket, you do not need any other nodes to determine $X_i$


**Pairwise Markov Independencies**
![figure 8: pairwise](image-9.png)
figure 8
- The pairwise Markov property associated with H are:
 $I_p(H) = \{X\perp Y \mid V- \{X,Y\} : \{X,Y\} \notin E\}$
- In figure 8, we can see that nodes $X_1$ and $X_5$ are not directly connected. By the pairwise Markov independence rule, we can say: $X_1\perp X_5 \mid \{X_2,X_3,X_4\}$ 
- This means that once we know $X_2$,$X_3$,and $X_4$, knowing $X_1$ tells us nothing more about $X_5$, because the intermediate nodes block all paths between $X_1$ and $X_5$, ensuring their independence.

## I-Maps of UG
### Definition of I-Map
An undirected graph \( H \) is an **I-Map (Independence Map)** for a probability distribution \( P \) if:

$$
I(H) \subseteq I(P)
$$

where:
<d-footnote>- \( I(H) \) is the set of independence relationships implied by the graph \( H \).
<d-footnote>- \( I(P) \) is the set of independence relationships present in \( P \).

## Gibbs Distribution
A probability distribution \( P \) is a **Gibbs Distribution** on a graph \( H \) if:

$$
P(X) = \frac{1}{Z} \prod_{c \in C} \psi_c(X_c)
$$

- This means that a **Gibbs distribution** can be decomposed into a product of factors over the cliques of the graph.

### **Theorem:**
If \( P \) is a Gibbs distribution over the graph \( H \), then \( H \) is an **I-Map** of \( P \).

---

## Perfect Maps
<figure id="perfect-map" class="l-body-outset">
  <img src="assets/img/perfect_map.png" />
  <figcaption>
    <strong>Figure 1:</strong> Not all structures have a perfect map.
  </figcaption>
</figure>

### Definition of Perfect Map
An undirected graph \( H \) is a **Perfect Map** for a probability distribution \( P \) if:

$$
\text{sep}_H(X; Z \mid Y) \iff X \perp Z \mid Y
$$

### **Key Point:**
- Not all distributions have a **perfect map** as a UG.
- **Example: V-Structure \( X \to Y \leftarrow Z \)**:
  - \( X \perp Z \)
  - But \( X \) and \( Z \) are **dependent given \( Y \)**:

$$
\neg (X \perp Z \mid Y)
$$

Since an **undirected graph cannot represent directionality**, it cannot capture this dependency.

---

## Graphical Models and Overlapping Distributions
<figure id="gm-overlap" class="l-body-outset">
  <img src="assets/img/overlapping_dists.png" />
  <figcaption>
    <strong>Figure 2:</strong> Overlapping sets of distributions in Directed and Undirected Graphical Models.
  </figcaption>
</figure>

<d-footnote>- **\( P \)**: The largest set, containing all possible probability distributions.
<d-footnote>- **\( D \)**: The set of probability distributions that can be represented by **Directed Graphical Models (DGMs)**, such as Bayesian Networks.
<d-footnote>- **\( U \)**: The set of probability distributions that can be represented by **Undirected Graphical Models (UGMs)**, such as Markov Random Fields.
- The **overlap** represents distributions that can be represented by **both** DGMs and UGMs.

---

## Exponential Families in UGMs
### **Energy Representation**
Clique potentials \( \psi_c(X_c) \) can be rewritten using an **energy function**:

$$
\psi_c(X_c) = \exp(-\phi_c(X_c))
$$

This leads to the **Boltzmann Distribution**:

$$
P(X) = \frac{1}{Z} \exp\left(-\sum_{c \in C} \phi_c(X_c)\right)
$$

- In **physics**, this is called the **Boltzmann distribution**.
- In **statistics**, this is known as a **log-linear model**.

---

## MAP Inference as Free Energy Minimization
### **Boltzmann Distribution**
The **Boltzmann distribution** is defined as:

$$
P_{\text{Boltzmann}}(X) = \arg\min_H F(P(X; H))
$$

where:
- \( P(X;H) \) is the probability distribution over configurations \( X \).
- \( F(P) \) is the **free energy**.

### **Free Energy Decomposition**
$$
F(P) = E[H(X)] - TS(P(X))
$$

where:
- \( E[H(X)] \) is the **expected energy**.
- \( TS(P(X)) \) represents the entropy term.

### **MAP Inference and Free Energy**
In probabilistic inference, **MAP estimation** minimizes the free energy:

$$
Q^*(\theta) = \arg\min_{Q} \left[ E_Q(-\log P(X \mid \theta)) - TS(Q) \right]
$$

- The first term represents the **negative log-likelihood**.
- The second term **regularizes** the distribution.

---

## **Conclusion**
- **I-Maps** and **Gibbs distributions** define the relationship between UGMs and probability distributions.
- **Perfect Maps** do not always exist due to **directionality limitations** in UGMs.
- **Boltzmann distributions** provide an energy-based formulation of UGMs.

---

## **Next Steps**
- Further explore **Variational Inference**.
- Applications of UGMs in **machine learning and physics**.