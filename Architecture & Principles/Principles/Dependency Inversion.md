#depdencyinversion

The direction of dependency within the application should be in the direction of abstraction, not implementation details. Most applications are written such that compile-time dependency flows in the direction of runtime execution, producing a direct dependency graph. That is if class A calls a method of class B and class B calls a method of class C, then compile time class A will depend on B and B will depend on C.

Applying the dependency inversion principle allows A to call methods on an abstraction that B implements, making it possible for A to call B at run time, but for B to depend on an interface controlled by A at compile time ( this inverting the typical compile-time tendency). At run time, the flow of program execution remains unchanged, but the introduction of interfaces means the different implementations of these interfaces can easily be pulled in.

**Dependency inversion** is a key part of building loosely coupled applications, since implementation details can be written to depend on and implement higher-level abstractions, rather than the other way around. The resulting applications are more testable, modular, and maintainable as a result. The practice of _dependency injection_ is made possible by following the dependency inversion principle.

![[Pasted image 20230626100704.png]]