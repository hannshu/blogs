---
categories: 空间转录组
---

The quality function is $$ Q = \sum_{ij} \left(A_{ij} - \gamma \frac{k_i k_j}{2m} \right)\delta(\sigma_i, \sigma_j) $$  

where $$ A $$ is the adjacency matrix, $$ k_i $$ is the degree of node $$ i $$, $$ m $$ is the total number of edges, $$ \sigma_i $$ denotes the community of node $$ i $$, $$ \delta(\sigma_i, \sigma_j) = 1 $$ if $$ \sigma_i = \sigma_j $$ and 0 otherwise, and, finally $$ \gamma $$ is a resolution parameter.  
This can alternatively be formulated as a sum over communities: $$ Q = \sum_{c} \left(m_c - \gamma\frac{K_c^2}{4m} \right) $$  
where $$ m_c $$ is the number of internal edges of community $$ c $$ and $$ K_c = \sum_{i \mid \sigma_i = c} k_i $$ is the total degree of nodes in community $$ c $$.

Note that this is the same as `class: ModularityVertexPartition` for $$ \gamma=1 $$ and using the normalisation by $$ 2m $$.
