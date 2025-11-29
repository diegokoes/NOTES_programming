# CONCEPTS > DATA TRANSFER OBJECT (DTO)
## SUMMARY
> [!summary]
> It's an architecture pattern, with the goal to **decouple** the *domain models* from the *presentation layer* and also center in an object multiple parameters  in order to reduce the number of method calls.
## THEORY

They are created as [[POJO]]'s and mapped from the [[concepts_domain_model|domain models]]/[[spring_entity|entities]] through a mapper component. 

They provide **flexibility**, we can build as many as we want for a same [[concepts_domain_model| domain model]] without it having to affect the domain design.

> [!warning] BUT NOT TOO MANY **DTO'S** 
> More classes, more mappers = hard to maintain

> [!warning] Not one class for many different scenarios
> Some attributes will frequently be not used

> [!danger] **DON'T ADD BUSINESS LOGIC INSIDE THEM**
> **THEY ARE OBJECTS TO OPTIMIZE DATA TRANSFER ONLY** 

![[Pasted image 20251128174546.png]]
