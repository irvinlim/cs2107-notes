# CS2107 Lecture 5

## Types of access control
- **Discretionary access control (DAC)**: The owner of the file decides the policy.
  - e.g. Unix file system permissions (`rwx` permissions)
- **Mandatory access control (MAC)**: Access rules are decided by system-wide security policy.
  - e.g. SELinux

### Example implementation: Access control matrices

| User | f1 | f2 | f3 | f4 |
|------|----|----|----|----|
| u1   | rx | x  |    |    |
| u2   | w  |    | r  | x  |
| u3   | r  | w  |    | x  |
| u4   |    | r  | w  | r  |

## Sandboxes
Applications can be restricted in their access to operating system resources, to limit potential damage.

Examples of limits that can be imposed:
- Prohibit TCP/IP networking
- Prohibit writing/reading to FS
- Restrict to writing/reading to specific directories
- Prohibit OS services

## Bell-LaPaluda (BLP) model
Security levels are in a [total ordering](https://en.wikipedia.org/wiki/Total_orderhttps://en.wikipedia.org/wiki/Total_order) which formalizes a policy restricting information flow from a higher security level to a lower security level. Ensures **confidentiality** of data, but not integrity (see [CIA triad](lect1.md#cia-triad)).

Default levels of security:
- **T**: Top secret
- **S**: Secret
- **C**: Confidential
- **U**: Unclassified

### Security theorem
- A system is considered secure in the current state if all the current accesses are permitted by the two properties.
- A transition from one state to the next is considered secure if it goes from one secure state to another secure state.

The **basic security theorem** (of BLP) states that, if the initial state of a system is secure, and if all state transitions are secure, then the system will always be secure.

### Basic BLP properties

#### No-read-up-1 property
Formally, given a subject `s` and object (file) `o`, and I<sub>`i`</sub> which dictates the security level of item `i`,
> `s` can read `o` if and only if I<sub>`o`</sub> ≤ I<sub>`s`</sub>

In other words, a user can only read files lower or equal to his or her own security level.

#### No-write-down-1 property
Formally, given a subject `s` and object (file) `o`, and I<sub>`i`</sub> which dictates the security level of item `i`,
> `s` can write `o` if and only if I<sub>`s`</sub> ≤ I<sub>`o`</sub>

In other words, a user can only write files higher or equal to his or her own security level.

### BLP extended

### Categories
Each object is classified in one or more categories. Hence, security levels can be defined at a category level (i.e. given `(l, c)`, where `l` is the security classification, and `c` is the category).

#### Domination relation
Define a relation between two security levels `(l, c)` and `(l', c')`, as follows:
> `(l, c)` *dom* `(l', c')` if and only if l' ≤ l and c'  c

In other words, a security level `L` dominates another security level `L'` if and only if `L` is of higher classification than `L'`, and `L`'s categories must contain all of `L'`'s categories.

#### No-read-up-2 property
Formally, given a subject `s` and object (file) `o`,
> `s` can read `o` if and only if `s` *dom* `o`

In other words, a user can only read files lower or equal to his or her own security level.

#### No-write-down-2 property
Formally, given a subject `s` and object (file) `o`,
> `s` can write `o` if and only if `o` *dom* `s`

In other words, a user can only write files higher or equal to his or her own security level.

## Biba model
Direct inverse of the BLP model. Ensures **integrity** of data but not confidentiality.

An example use of the Biba model would be (also) in a military sense, where a soldier of a lower rank cannot issue orders to a higher ranked solder (i.e. no-write-up), and information from a lower ranked soldier cannot be trusted by a higher ranked soldier (i.e. no-read-down)

### No-read-down property
Formally, given a subject `s` and object (file) `o`, and I<sub>`i`</sub> which dictates the security level of item `i`,
> `s` can read `o` if and only if I<sub>`s`</sub> ≤ I<sub>`o`</sub>

In other words, a user can only read files higher or equal to his or her own security level.

### No-write-up property
Formally, given a subject `s` and object (file) `o`, and I<sub>`i`</sub> which dictates the security level of item `i`,
> `s` can write `o` if and only if I<sub>`o`</sub> ≤ I<sub>`s`</sub>

In other words, a user can only write to files lower or equal to his or her own security level.

## Chinese Wall model
A security policy that aims to address conflicts of interest between competing organisations. It is a hybrid model that addresses both **confidentiality** and **integrity**.

The underlying idea is that subjects cannot work for their client's competitors.

### Definitions
Denote `y(c)` as a function of `c` (user), that returns `c`'s company/organisation.

Denote `x(c)` as a function of `c` that returns `c`'s competitors.

### SimpleProperty
> `s` can access `c` if and only if for all `c'` that `s` can read, `y(c) ∉ x(c')` or `y(c) = y(c')`

In other words, you cannot read documents for company `y(c)` if you can read any of its competitors' documents.

### *-Property
> `s` can write to `c` if and only if `s` cannot read any `c'` where `x(c') ≠ ∅` and `y(c) ≠ y(c')`

In other words, you cannot write to a particular document, if you can read documents owned by a company whose competitor is the document owner for which you are trying to write to.