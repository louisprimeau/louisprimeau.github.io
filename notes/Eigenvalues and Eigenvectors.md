**Derivative of an eigenvalue dependent on a parameter**

In numerical applications it is often the case that we have to differentiate a eigenvalue which is a function of some parameter. Let $H(\alpha)$ be a non-singular matrix dependent on a real parameter $\alpha$. Let the eigenvalues be $\lambda_i(\alpha)$ and let the right and left eigenvectors be $v_{i}(\alpha)$ and $u_{i}(\alpha)$, respectively. Let us further assume that in expressions the eigenvectors are already normalized. Now suppose we want to calculate $\frac{\partial \lambda_i(\alpha)}{\partial \alpha}$. The naive way to do this is via finite differences:
$$
\frac{\partial\lambda_i(\alpha)}{\partial \alpha} \approx \frac{\lambda_i(\alpha + \epsilon) - \lambda_i(\alpha)}{\epsilon}
$$
but this requires doing two eigendecompositions. Instead, we can appeal to the definition of the $i$th eigenvalue:
$$
u^{T}_{i} H v_i = \lambda_i
$$
Differentiate both sides w.r.t $\alpha$:
$$
\frac{\partial}{\partial \alpha} \left( u^{T}_{i} H v_i \right) = \frac{\partial u^{T}_{i}}{\partial \alpha}   H v_i + u^T_i \frac{\partial H}{\partial \alpha} v_i + u^T_i H \frac{\partial v_i}{\partial \alpha} = \lambda_i (\frac{\partial u^{T}_{i}}{\partial \alpha} v_i + u^T_i \frac{\partial v_i}{\partial \alpha}) + u^T_i \frac{\partial H}{\partial \alpha} v_i
$$
Now using the orthogonality relations between the right and left eigenvectors, i.e. $u^T_i v_j = \delta_{ij}$ we can see that $\frac{\partial u^T_i}{\partial \alpha} v_i =u^T_i  \frac{\partial v_i}{\partial\alpha} = 0$ because the derivatives are orthogonal to the vectors by normalization (the eigenvectors live on a $d$-dimensional unit hypersphere, where $d$ is the dimension of $H$, and we can just throw away any changes that would bring us off the sphere). So the formula is
$$
\frac{\partial \lambda_i}{\partial \alpha} = \frac{\partial}{\partial \alpha} \left( u^{T}_{i} H v_i \right) = u^T_i \frac{\partial H}{\partial \alpha} v_i
$$
If nothing else you can get the derivative of $H$ by automatic differentiation.

**Degenerate Eigenvalues**

What if $\lambda_i$ is degenerate with another eigenvalue $\lambda_j$ for some range of the parameter $\alpha$ ? Then we are no longer guaranteed the orthogonality from before since we have a two dimensional subspace in which we are free rotate our eigenvectors. Suppose we reorthogonalize $u_i$ and $u_j$ inside the degenerate subspace. Reviewing the steps, we find that the derivative $\frac{\partial u^{T}_{i}}{\partial \alpha}$ may rotate inside the degenerate subspace, acquiring components in the direction $u_j$. Then we get
$$
\frac{\partial}{\partial \alpha} \left( u^{T}_{i} H v_i \right) = \lambda_i (\frac{\partial u^{T}_{i}}{\partial \alpha} v_i + u^T_i \frac{\partial v_i}{\partial \alpha}) + u^T_i \frac{\partial H}{\partial \alpha} v_i = \lambda_i (g(\alpha) u^{T}_{j} v_i + g'(\alpha) u^T_i v_j) + u^T_i \frac{\partial H}{\partial \alpha} v_i
$$
where $g(\alpha)$ and $g'(\alpha)$ are functions that measure the magnitude of the derivative in the direction of $u_j$ and $v_j$, respectively. Then what we need to do is clear: we need to construct $u_j$ and $u_i$ such that they are orthogonal to $v_i$ and $v_j$, respectively.

**Eigenvectors**

To get the derivatives of the eigenvectors of a Hermitian matrix with distinct eigenvalue $\lambda_i$ and normalized eigenvector $u_i$, we differentiate
$$
\begin{align*}
\frac{\partial}{\partial \alpha}(H u_i - \lambda_i u_i) = \left(\frac{\partial H}{\partial \alpha} - \frac{\partial \lambda_i}{\partial \alpha}I \right)u_i + (H - \lambda_i I) \frac{\partial u_i}{\partial \alpha} &= 0\\
\Rightarrow \frac{\partial u_i}{\partial \alpha} = (H - \lambda_i I)^{-1} \left(\frac{\partial H}{\partial \alpha} - \frac{\partial \lambda_i}{\partial \alpha} I \right) u_i\\
\end{align*}
$$
noting that the inverse is justified because the nullspace of $H - \lambda_i I$ is spanned by $u_i$, which is the direction that is subtracted from $\frac{\partial H}{\partial \alpha}$. Hence the action of $(H - \lambda_i I)^{-1}$ is unambiguous. We may use the Moore-Penrose pseudo inverse to write:
$$
\frac{\partial u_i}{\partial \alpha} = (H - \lambda_i I)^{+} \frac{\partial H}{\partial \alpha} u_i\\
$$
