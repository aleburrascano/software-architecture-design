---
title: Observability
source: "https://en.wikipedia.org/wiki/Observability"
author:
  - Contributors to Wikimedia projects
published: 2004-08-01
created: 2026-05-06
description: "From Wikipedia, the free encyclopedia"
tags:
  - clippings
  - "vault-scout"
  - "source:tier2"
  - "kind:article"
  - "score:7"
---

							

						From Wikipedia, the free encyclopedia
					

Observability is a measure of how well internal states of a system can be inferred from knowledge of its external outputs.
In control theory, the observability and controllability of a linear system are mathematical duals.
The concept of observability was introduced by the Hungarian-American engineer Rudolf E. Kálmán for linear dynamic systems.[1][2] A dynamical system designed to estimate the state of a system from measurements of the outputs is called a state observer for that system, such as Kalman filters.



Consider a physical system modeled in state-space representation. A system is said to be observable if, for every possible evolution of state and control vectors, the current state can be estimated using only the information from outputs (physically, this generally corresponds to information obtained by sensors). In other words, one can determine the behavior of the entire system from the system's outputs. On the other hand, if the system is not observable, there are state trajectories that are not distinguishable by only measuring the outputs.

Linear time-invariant systems[edit]
For time-invariant linear systems in the state space representation, there are convenient tests to check whether a system is observable. Consider a SISO system with  state variables (see state space for details about MIMO systems) given by



Observability matrix[edit]
If and only if the column rank of the observability matrix, defined as


is equal to , then the system is observable. The rationale for this test is that if  columns are linearly independent, then each of the  state variables is viewable through linear combinations of the output variables . Observability is a sufficient and necessary condition for the design of continuous-time state observers.


Observability index[edit]
The observability index  of a linear time-invariant discrete system is the smallest natural number for which the following is satisfied: , where


Unobservable subspace[edit]
The unobservable subspace  of the linear system is the kernel of the linear map  given by[3]where  is the set of continuous functions from  to .   can also be written as [3]

Since the system is observable if and only if  ,  the system is observable if and only if  is the zero subspace.
The following properties for the unobservable subspace are valid:[3]





A slightly weaker notion than observability is detectability. A system is detectable if all the unobservable states are stable.[4]
Detectability conditions are important in the context of sensor networks.[5][6]

Functional observability[edit]
Functional observability is a property that extends the classical notion of observability for cases in which full-state observability is not possible or required (due to lack of measurement signals and sensor placement). Rather than requiring full-state reconstruction, functional observability establishes the condition under which a linear functional  can still be estimated using solely information from the output signals.[7] Formally, given a (typically low-dimensional)  matrix , where , a system is functionally observable if and only if [8]


Functional observability is an important concept because it determines the sufficient and necessary condition under which a functional observer (also known as a Darouach observer [9]) can be designed to asymptotically estimate . Under certain conditions, functional observability and output controllability are mathematical duals[10], implying that the problems of estimating and controlling a linear functional  (rather than the full state ) are equivalent under a system transformation.

Linear time-varying systems[edit]
Consider the continuous linear time-variant system



Suppose that the matrices ,   and  are given as well as inputs and outputs  and  for all  then it is possible to determine  to within an additive constant vector which lies in the null space of  defined by


where  is the state-transition matrix.
It is possible to determine a unique  if  is nonsingular.  In fact, it is not possible to distinguish the initial state for  from that of  if  is in the null space of .
Note that the matrix  defined as above has the following properties:



 satisfies the equation
[11]
Observability matrix generalization[edit]
The system is observable in  if and only if there exists an interval  in  such that the matrix  is nonsingular.
If  are analytic, then the system is observable in the interval [,] if there exists  and a positive integer k such that[12]


where  and  is defined recursively as



Consider a system varying analytically  in  and matrices Then  , and since this matrix has rank = 3, the system is observable on every nontrivial interval of  .

Given the system , . Where  the state vector,  the input vector and  the output vector.   are to be smooth vector fields.
Define the observation space  to be the space containing all repeated Lie derivatives, then the system is observable in  if and only if , where

[13]
Early criteria for observability in nonlinear dynamic systems were discovered by Griffith and Kumar,[14] Kou, Elliot and Tarn,[15] and Singh.[16]
There also exist an observability criteria for nonlinear time-varying systems.[17]

Static systems and general topological spaces[edit]
Observability may also be characterized for steady state systems (systems typically defined in terms of algebraic equations and inequalities), or more generally, for sets in .[18][19] Just as observability criteria are used to predict the behavior of Kalman filters or other observers in the dynamic system case,  observability criteria for sets in  are used to predict the behavior of data reconciliation and other static estimators. In the nonlinear case,  observability can be characterized for individual variables, and also for local estimator behavior rather than just global behavior.


Controllability
Hautus lemma
Identifiability
State observer
State space (controls)


^ Kalman, R.E. (1960). "On the general theory of control systems". IFAC Proceedings Volumes. 1: 491–502. doi:10.1016/S1474-6670(17)70094-8.

^ Kalman, R. E. (1963). "Mathematical Description of Linear Dynamical Systems". Journal of the Society for Industrial and Applied Mathematics, Series A: Control. 1 (2): 152–192. doi:10.1137/0301010.

^ a b c Sontag, E.D., "Mathematical Control Theory", Texts in Applied Mathematics, 1998

^ "Controllability and Observability" (PDF). Archived from the original (PDF) on 2019-06-10. Retrieved 2024-05-19.

^ Li, W.; Wei, G.; Ho, D. W. C.; Ding, D. (November 2018). "A Weightedly Uniform Detectability for Sensor Networks". IEEE Transactions on Neural Networks and Learning Systems. 29 (11): 5790–5796. Bibcode:2018ITNNL..29.5790L. doi:10.1109/TNNLS.2018.2817244. PMID 29993845. S2CID 51615852.

^ Li, W.; Wang, Z.; Ho, D. W. C.; Wei, G. (2019). "On Boundedness of Error Covariances for Kalman Consensus Filtering Problems". IEEE Transactions on Automatic Control. 65 (6): 2654–2661. doi:10.1109/TAC.2019.2942826. S2CID 204196474.

^ Fernando, T.; Trinh, H. M.; Jennings, L. (2010). "Functional observability and the design of minimum order linear functional observers". IEEE Transactions on Automatic Control. 55 (5): 1268–1273. doi:10.1109/TAC.2010.2042005.

^ Jennings, L.; Fernando, T.; Trinh, H. M. (2011). "Existence conditions for functional observability from an eigenspace perspective". IEEE Transactions on Automatic Control. 56 (12): 2957–2961. doi:10.1109/TAC.2011.2162793.

^ Darouach, M. (2002). "Existence and design of functional observers for linear systems". IEEE Transactions on Automatic Control. 45 (5): 940–943. doi:10.1109/9.855563.

^ Montanari, A. N.; Duan, C.; Motter, A. E. (2025). "Duality between controllability and observability for target control and estimation in networks". IEEE Transactions on Automatic Control. doi:10.1109/TAC.2025.3552001.

^ Brockett, Roger W. (1970). Finite Dimensional Linear Systems. John Wiley & Sons. ISBN 978-0-471-10585-5.

^ Eduardo D. Sontag, Mathematical Control Theory: Deterministic Finite Dimensional Systems.

^ Lecture notes for Nonlinear Systems Theory by prof. dr. D.Jeltsema, prof dr. J.M.A.Scherpen and prof dr. A.J.van der Schaft.

^ Griffith, E. W.; Kumar, K. S. P. (1971). "On the observability of nonlinear systems: I". Journal of Mathematical Analysis and Applications. 35: 135–147. doi:10.1016/0022-247X(71)90241-1.

^ Kou, Shauying R.; Elliott, David L.; Tarn, Tzyh Jong (1973). "Observability of nonlinear systems". Information and Control. 22: 89–99. doi:10.1016/S0019-9958(73)90508-1.

^ Singh, Sahjendra N. (1975). "Observability in non-linear systems with immeasurable inputs". International Journal of Systems Science. 6 (8): 723–732. doi:10.1080/00207727508941856.

^ Martinelli, Agostino (2022). "Extension of the Observability Rank Condition to Time-Varying Nonlinear Systems". IEEE Transactions on Automatic Control. 67 (9): 5002–5008. Bibcode:2022ITAC...67.5002M. doi:10.1109/TAC.2022.3180771. ISSN 0018-9286. S2CID 251957578.

^ Stanley, G. M.; Mah, R. S. H. (1981). "Observability and redundancy in process data estimation" (PDF). Chemical Engineering Science. 36 (2): 259–272. Bibcode:1981ChEnS..36..259S. doi:10.1016/0009-2509(81)85004-X.

^ Stanley, G.M.; Mah, R.S.H. (1981). "Observability and redundancy classification in process networks" (PDF). Chemical Engineering Science. 36 (12): 1941–1954. doi:10.1016/0009-2509(81)80034-6.



"Observability". PlanetMath.
MATLAB function for checking observability of a system Archived 2012-02-19 at the Wayback Machine
Mathematica function for checking observability of a system






