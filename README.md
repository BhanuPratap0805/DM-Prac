# DM-Prac

# 1. Create a class SET with member functions: isMember, powerSet, subset, union, intersection, complement, set difference, symmetric difference, cartesian product. Write a menu driven program to perform the above functions on an instance of the SET class.

```cpp
#include <iostream>
using namespace std;

class SET {

    int elements[100];
    int size;

public:

    SET() {
        size = 0;
    }

    void input() {

        cout << "Enter number of elements: ";
        cin >> size;

        cout << "Enter elements: ";

        for (int i = 0; i < size; i++)
            cin >> elements[i];
    }

    bool isMember(int x) {

        for (int i = 0; i < size; i++)
            if (elements[i] == x)
                return true;

        return false;
    }

    void powerSet() {

        int total = 1;

        for (int i = 0; i < size; i++)
            total *= 2;

        cout << "Power Set: { ";

        for (int i = 0; i < total; i++) {

            cout << "{ ";

            for (int j = 0; j < size; j++)
                if (i & (1 << j))
                    cout << elements[j] << " ";

            cout << "} ";
        }

        cout << "}";
    }

    bool isSubset(SET s) {

        for (int i = 0; i < s.size; i++)
            if (!isMember(s.elements[i]))
                return false;

        return true;
    }

    SET unionSet(SET s) {

        SET result;
        result.size = 0;

        for (int i = 0; i < size; i++)
            result.elements[result.size++] = elements[i];

        for (int i = 0; i < s.size; i++)
            if (!result.isMember(s.elements[i]))
                result.elements[result.size++] = s.elements[i];

        return result;
    }

    SET intersection(SET s) {

        SET result;
        result.size = 0;

        for (int i = 0; i < size; i++)
            if (s.isMember(elements[i]))
                result.elements[result.size++] = elements[i];

        return result;
    }

    SET complement(SET universal) {

        SET result;
        result.size = 0;

        for (int i = 0; i < universal.size; i++)
            if (!isMember(universal.elements[i]))
                result.elements[result.size++] = universal.elements[i];

        return result;
    }

    SET difference(SET s) {

        SET result;
        result.size = 0;

        for (int i = 0; i < size; i++)
            if (!s.isMember(elements[i]))
                result.elements[result.size++] = elements[i];

        return result;
    }

    SET symmetricDifference(SET s) {

        SET d1 = difference(s);
        SET d2 = s.difference(*this);

        return d1.unionSet(d2);
    }

    void cartesianProduct(SET s) {

        cout << "Cartesian Product: { ";

        for (int i = 0; i < size; i++)
            for (int j = 0; j < s.size; j++)
                cout << "(" << elements[i] << "," << s.elements[j] << ") ";

        cout << "}";
    }

    void display() {

        cout << "{ ";

        for (int i = 0; i < size; i++)
            cout << elements[i] << " ";

        cout << "}";
    }
};

int main() {

    SET A, B, universal;
    int choice;

    cout << "Enter Set A:\n";
    A.input();

    cout << "Enter Set B:\n";
    B.input();

    cout << "Enter Universal Set:\n";
    universal.input();

    do {

        cout << "\n1. Is Member";
        cout << "\n2. Power Set";
        cout << "\n3. Subset Check";
        cout << "\n4. Union";
        cout << "\n5. Intersection";
        cout << "\n6. Complement";
        cout << "\n7. Set Difference";
        cout << "\n8. Symmetric Difference";
        cout << "\n9. Cartesian Product";
        cout << "\n0. Exit";
        cout << "\nEnter choice: ";
        cin >> choice;

        switch (choice) {

            case 1: {
                int x;
                cout << "Enter element to check: ";
                cin >> x;
                cout << x << (A.isMember(x) ? " is a member" : " is not a member") << " of A";
                break;
            }

            case 2:
                A.powerSet();
                break;

            case 3:
                cout << "B is " << (A.isSubset(B) ? "" : "not ") << "a subset of A";
                break;

            case 4:
                cout << "A union B = ";
                A.unionSet(B).display();
                break;

            case 5:
                cout << "A intersection B = ";
                A.intersection(B).display();
                break;

            case 6:
                cout << "Complement of A = ";
                A.complement(universal).display();
                break;

            case 7:
                cout << "A - B = ";
                A.difference(B).display();
                break;

            case 8:
                cout << "Symmetric Difference = ";
                A.symmetricDifference(B).display();
                break;

            case 9:
                A.cartesianProduct(B);
                break;

            case 0:
                cout << "Exiting...";
                break;

            default:
                cout << "Invalid Choice";
        }

    } while (choice != 0);

    return 0;
}
```

---

# 2. Create a class RELATION. Use Matrix notation to represent a relation. Include member functions to check if the relation is Reflexive, Symmetric, Anti-symmetric, Transitive. Using these functions check whether the given relation is: Equivalence or Partial Order relation or None.

```cpp
#include <iostream>
using namespace std;

class RELATION {

    int matrix[10][10];
    int n;

public:

    void input() {

        cout << "Enter number of elements in set: ";
        cin >> n;

        cout << "Enter the relation matrix (0 or 1):\n";

        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                cin >> matrix[i][j];
    }

    void display() {

        cout << "Relation Matrix:\n";

        for (int i = 0; i < n; i++) {

            for (int j = 0; j < n; j++)
                cout << matrix[i][j] << " ";

            cout << endl;
        }
    }

    bool isReflexive() {

        for (int i = 0; i < n; i++)
            if (matrix[i][i] != 1)
                return false;

        return true;
    }

    bool isSymmetric() {

        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                if (matrix[i][j] != matrix[j][i])
                    return false;

        return true;
    }

    bool isAntiSymmetric() {

        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                if (i != j && matrix[i][j] == 1 && matrix[j][i] == 1)
                    return false;

        return true;
    }

    bool isTransitive() {

        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                if (matrix[i][j] == 1)
                    for (int k = 0; k < n; k++)
                        if (matrix[j][k] == 1 && matrix[i][k] == 0)
                            return false;

        return true;
    }

    void checkRelationType() {

        bool reflexive     = isReflexive();
        bool symmetric     = isSymmetric();
        bool antiSymmetric = isAntiSymmetric();
        bool transitive    = isTransitive();

        cout << "\nReflexive    : " << (reflexive     ? "Yes" : "No");
        cout << "\nSymmetric    : " << (symmetric     ? "Yes" : "No");
        cout << "\nAnti-Symmetric: " << (antiSymmetric ? "Yes" : "No");
        cout << "\nTransitive   : " << (transitive    ? "Yes" : "No");

        if (reflexive && symmetric && transitive)
            cout << "\nType: Equivalence Relation";

        else if (reflexive && antiSymmetric && transitive)
            cout << "\nType: Partial Order Relation";

        else
            cout << "\nType: None";
    }
};

int main() {

    RELATION r;

    r.input();
    r.display();
    r.checkRelationType();

    return 0;
}
```

---

# 3. Write a Program that generates all the permutations of a given set of digits, with or without repetition.

```cpp
#include <iostream>
using namespace std;

int digits[20];
int n;
int perm[20];
bool used[20];

void permuteWithout(int pos) {

    if (pos == n) {

        for (int i = 0; i < n; i++)
            cout << digits[i] << " ";

        cout << endl;
        return;
    }

    for (int i = 0; i < n; i++) {

        if (!used[i]) {

            used[i] = true;
            digits[pos] = perm[i];
            permuteWithout(pos + 1);
            used[i] = false;
        }
    }
}

void permuteWith(int pos) {

    if (pos == n) {

        for (int i = 0; i < n; i++)
            cout << digits[i] << " ";

        cout << endl;
        return;
    }

    for (int i = 0; i < n; i++) {

        digits[pos] = perm[i];
        permuteWith(pos + 1);
    }
}

int main() {

    int choice;

    cout << "Enter number of digits: ";
    cin >> n;

    cout << "Enter the digits: ";

    for (int i = 0; i < n; i++)
        cin >> perm[i];

    cout << "\n1. Permutations without repetition";
    cout << "\n2. Permutations with repetition";
    cout << "\nEnter choice: ";
    cin >> choice;

    if (choice == 1) {

        for (int i = 0; i < n; i++)
            used[i] = false;

        cout << "\nAll permutations without repetition:\n";
        permuteWithout(0);
    }

    else if (choice == 2) {

        cout << "\nAll permutations with repetition:\n";
        permuteWith(0);
    }

    else
        cout << "Invalid Choice";

    return 0;
}
```

---

# 4. For any number n, write a program to list all the solutions of the equation x₁ + x₂ + x₃ + ... + xₙ = C, where C is a constant (C<=10) and x₁, x₂, x₃,...,xₙ are nonnegative integers, using brute force strategy.

```cpp
#include <iostream>
using namespace std;

int n, C;
int x[20];

void solve(int pos, int remaining) {

    if (pos == n - 1) {

        if (remaining >= 0) {

            x[pos] = remaining;

            for (int i = 0; i < n; i++)
                cout << "x" << i + 1 << "=" << x[i] << " ";

            cout << endl;
        }

        return;
    }

    for (int val = 0; val <= remaining; val++) {

        x[pos] = val;
        solve(pos + 1, remaining - val);
    }
}

int main() {

    cout << "Enter number of variables (n): ";
    cin >> n;

    cout << "Enter constant C (<=10): ";
    cin >> C;

    if (C > 10) {
        cout << "C must be <= 10";
        return 1;
    }

    cout << "\nAll solutions of x1 + x2 + ... + x" << n << " = " << C << ":\n";

    solve(0, C);

    return 0;
}
```

---

# 5. Write a Program to evaluate a polynomial function. (For example store f(x) = 4n² + 2n + 9 in an array and for a given value of n, say n = 5, compute the value of f(n)).

```cpp
#include <iostream>
#include <cmath>
using namespace std;

int main() {

    int degree;

    cout << "Enter degree of polynomial: ";
    cin >> degree;

    double coeff[100];

    cout << "Enter coefficients from highest degree to constant term:\n";

    for (int i = 0; i <= degree; i++) {

        cout << "Coefficient of x^" << degree - i << ": ";
        cin >> coeff[i];
    }

    double n;

    cout << "Enter value of n to evaluate f(n): ";
    cin >> n;

    double result = 0;

    for (int i = 0; i <= degree; i++)
        result += coeff[i] * pow(n, degree - i);

    cout << "f(" << n << ") = " << result;

    return 0;
}
```

---

# 6. Write a Program to check if a given graph is a complete graph. Represent the graph using the Adjacency Matrix representation.

```cpp
#include <iostream>
using namespace std;

int main() {

    int v;

    cout << "Enter number of vertices: ";
    cin >> v;

    int adj[10][10];

    cout << "Enter adjacency matrix:\n";

    for (int i = 0; i < v; i++)
        for (int j = 0; j < v; j++)
            cin >> adj[i][j];

    bool isComplete = true;

    for (int i = 0; i < v; i++) {

        for (int j = 0; j < v; j++) {

            if (i == j && adj[i][j] != 0) {
                isComplete = false;
                break;
            }

            if (i != j && adj[i][j] != 1) {
                isComplete = false;
                break;
            }
        }

        if (!isComplete)
            break;
    }

    cout << "\nAdjacency Matrix:\n";

    for (int i = 0; i < v; i++) {

        for (int j = 0; j < v; j++)
            cout << adj[i][j] << " ";

        cout << endl;
    }

    cout << "\nThe graph is " << (isComplete ? "" : "not ") << "a Complete Graph";

    return 0;
}
```

---

# 7. Write a Program to check if a given graph is a complete graph. Represent the graph using the Adjacency List representation.

```cpp
#include <iostream>
using namespace std;

int main() {

    int v, e;

    cout << "Enter number of vertices: ";
    cin >> v;

    cout << "Enter number of edges: ";
    cin >> e;

    int adj[10][10];
    int degree[10];

    for (int i = 0; i < v; i++) {
        degree[i] = 0;
        for (int j = 0; j < v; j++)
            adj[i][j] = -1;
    }

    cout << "Enter edges (u v) 0-indexed:\n";

    for (int i = 0; i < e; i++) {

        int u, v2;
        cin >> u >> v2;

        adj[u][degree[u]++] = v2;
        adj[v2][degree[v2]++] = u;
    }

    cout << "\nAdjacency List:\n";

    for (int i = 0; i < v; i++) {

        cout << i << " -> ";

        for (int j = 0; j < degree[i]; j++)
            cout << adj[i][j] << " ";

        cout << endl;
    }

    bool isComplete = true;

    for (int i = 0; i < v; i++) {

        if (degree[i] != v - 1) {
            isComplete = false;
            break;
        }

        for (int j = 0; j < v; j++) {

            if (i == j) continue;

            bool found = false;

            for (int k = 0; k < degree[i]; k++) {

                if (adj[i][k] == j) {
                    found = true;
                    break;
                }
            }

            if (!found) {
                isComplete = false;
                break;
            }
        }

        if (!isComplete) break;
    }

    cout << "\nThe graph is " << (isComplete ? "" : "not ") << "a Complete Graph";

    return 0;
}
```

---

# 8. Write a Program to accept a directed graph G and compute the in-degree and out-degree of each vertex.

```cpp
#include <iostream>
using namespace std;

int main() {

    int v, e;

    cout << "Enter number of vertices: ";
    cin >> v;

    cout << "Enter number of directed edges: ";
    cin >> e;

    int inDegree[10] = {0};
    int outDegree[10] = {0};

    cout << "Enter directed edges (u v) meaning u->v, 0-indexed:\n";

    for (int i = 0; i < e; i++) {

        int u, w;
        cin >> u >> w;

        outDegree[u]++;
        inDegree[w]++;
    }

    cout << "\nVertex\tIn-Degree\tOut-Degree\n";

    for (int i = 0; i < v; i++)
        cout << i << "\t" << inDegree[i] << "\t\t" << outDegree[i] << endl;

    return 0;
}
```
